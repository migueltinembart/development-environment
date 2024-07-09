---

title: Maas Controller
created by: Miguel Tinembart
created at: 2024-07-03 00:00:00 +0200 CEST
tags:
  - ansible
  - semesterarbeit
---

## Maas Controller

Die folgenden Controller werden für MAAS benötigt und dementsprechend wurden Rollen in Ansible erstellt.

- [MAAS Region Controller](https://github.com/migueltinembart/maas/tree/main/roles/regiond)
- [MAAS Rack Controller](https://github.com/migueltinembart/maas/tree/main/roles/rackd)

### Ziele

Die Rollen müssen folgende Kriterien erfüllen:

- [ ] Anhand der Architektur des Systems die richtige Installation vornehmen (AMD64, ARM64, etc.)
- [ ] Listen aufnehmen damit Tasks iterierbar sind
- [ ] Benutzer erstellen und berechtigen können
- [ ] Eine Datenbank anlegen
- [ ] Konfigurationen an Files vornehmen (z.B. pg_hba.conf)
- [ ] Mittels Handler bestimmte Tasks nur bei Änderungen vornehmen können.


