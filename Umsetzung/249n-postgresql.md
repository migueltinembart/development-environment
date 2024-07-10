---

title: Postgresql
created by: Miguel Tinembart
created at: 2024-07-02 00:00:00 +0200 CEST
tags:
  - Semesterarbeit
  - ansible
---

# Postgresql

Postgresql spielt für die Datenverwaltung in MAAS eine zentrale Rolle. Für die Bereitstellung kommt Ansible zum Zug. Eine Rolle für die Bündelung der Konfiguration wurde erstellt.

Ansible Postgresql Rolle: [Postgresql Rolle](https://github.com/migueltinembart/maas/tree/main/roles/postgres)

## Ziele

Die Rolle muss folgende Konfigurationen übernehmen können:

- [x] Anhand der Architektur des Systems die richtige Installation vornehmen (AMD64, ARM64, etc.)
- [x] Listen aufnehmen damit Tasks iterierbar sind
- [x] Benutzer erstellen und berechtigen können
- [x] Eine Datenbank anlegen
- [x] Konfigurationen an Files vornehmen (z.B. `pg_hba.conf`)
- [x] Mittels Handler bestimmte Tasks nur bei Änderungen vornehmen können.

## Umsetzung

Die Schritte für das Aufsetzen der postgresql Datenbank können mit einzelnen Code Snippets einzeln beobachtet werden.

### Architekturunabhängigkeit

Dieser Task soll das `apt` repository mit dem definierten repository-key für die Signierung in die Liste de erlaubten Repos ablegen. Mit der Variabel `ansible_architecture` kann zur Laufzeit in die Variable auf einer per Host festgelegten CPU-Architektur einen anderen Wert hinterlegen. 

Die Variable `ansible_distribution_release` setzt hier als Variabel den distributionsnamen. Bei Ubuntu 22.04 wäre dass `jammy`.

```yaml
# tasks/main.yaml

- name: Add specified version of PostgreSQL repository
  ansible.builtin.apt_repository:
    repo: >
      deb [arch={{ ansible_architecture }} signed-by=/etc/apt/keyrings/postgres.asc]
      http://apt.postgresql.org/pub/repos/apt/ {{ ansible_distribution_release }}-pgdg main
    state: present
    filename: /etc/apt/sources.list.d/pgdg

```

### Iterierbarkeit

Wenn gefordert, soll nicht immer ein Task geschrieben werden, welches dieselbe Aufgabe übernimmt wie die letzte. Darum verwende ich loops in Ansible um einen Task in mehrmals auszuführen und die Zeit für die Tasks sinkt, da weniger OVerhead generiert wird. 

```yaml
# tasks/main.yaml
- name: Install required packages
  ansible.builtin.package:
    name: "{{ item }}"
    state: present
  loop:
    - ca-certificates
    - curl
    - acl
    - python3-psycopg2
    - postgresql
```

### Handlers

Handlers erleichtern die Steuerung der Änderungen und die Reaktion darauf. Am Ende der Taskliste wird mit dem `notify:` Attribut ein Signal an einen Handler geschickt. Dieser löst nur aus wenn eine Änderung stattgefunden hat und ist für die Erstinstallation wichtig, denn nachdem die Konfiguration beim ersten Durchlauf sicher geändert wurde, sollte der `postgresql.service` neugestartet werden.

```yaml
# tasks/main.yaml
- name: Assign Priviliges to users for their repsective db
  community.postgresql.postgresql_privs:
    database: "{{ item.db }}"
    objs: "{{ item.objs | default(omit) }}"
    privs: "{{ item.privs }}"
    type: "{{ item.type | default(omit) }}"
    roles: "{{ item.user | default(omit) }}"
    state: present
  become: true
  become_user: "{{ pg_vars.postgres_db_user }}"
  with_items: "{{ pg_vars.privs | default([]) }}"
  notify:
    - Reload PostgreSQL

# handlers/main.yaml
- name: Reload PostgreSQL
  ansible.builtin.service:
    name: postgresql
    state: reloaded
```

## Nutzung

Um die Rolle nutzen zu können wird in einem site.yaml die Rolle einem play in diesem Playbook zugewiesen. Anhand der Group Vars werden die Variabeln welche standardmässig für die postgresql Hostgruppe definiert ist eingesetzt und können von Hostvars überschrieben werden. 

Ein entsprechener Eintrag für das Inventory kann wie folgt aussehen:

```yaml
# inventory/hosts.yaml
all:
  vars:
    ansible_connection: ssh
    ansible_ssh_user: ansible
    ansible_python_interpreter: /usr/bin/python3
    ansible_common_remote_group: ansible
  children:
    postgres:
      hosts:
        postgres.local:
          ansible_host: 192.168.10.2
          postgres_db_password: randomPassword
```

Ein Beispiel-Inventory steht euch hier zur Verfügung: [inventory](https://github.com/migueltinembart/maas/tree/main/inventory)

Die site.yaml dient als Einstiegspunkt für Ansible um Tasks in Folge durchzuarbeiten. Alle zusätzlichen Rollen werden einem Play zugewiesen.

```yaml
- name: Apply the PostgreSQL role
  hosts: postgres
  become: true
  vars_files:
    - group_vars/postgres.yaml
  roles:
    - postgres
```

Um schlussendlich die Rolle mit diesem Inventory durchzuspielen, kann man dies einfach mit diesem befehl:

```bash
ansible-playbook -i inventory/hosts.yaml site.yaml
```


