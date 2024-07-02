---

title: Architektur
created by: Miguel Tinembart
created at: 2024-07-01 00:00:00 +0200 CEST
tags:
  - inbox

---

## Architektur

![Runner-architecture](./assets/runner-diagramm.drawio.png)

Anhand der Abbildung stellen wir folgende Komponenten fest:

- Runners welche für Github Actions zur Verfügung stehen
- Ein Maas Region und Rack Controller verwalten Bare Metal Instanzen und virtuelle Maschinen
- Die Runners übernehmen Grundlegende Deployment Task

### Abhängigkeiten
Anhand dieser Graphe ist ersichtlich, dass zuerst die Infrastruktur bereitgestellt sein muss, bevor die Applikationen wie Runner und deren Jobs ausgerollt werden können. Der zentrale Fokus der Bereitstellung in diesem Projekt befasst sich mit der Bereitstellung der nötigen Komponenten der Infrastruktur.

```mermaid
graph LR;
  MAAS[MAAS] ---> VM[VMs] ;
  VM ---> Run[Runner]
  Run[Runner] ---> TF[Terraform]
  Run ---> Ansible[Ansible]
  Run ---> etc
  subgraph Infrastruktur
    MAAS
    VM
  end
  subgraph Applikation
    Run
    Ansible
    TF
    etc
  end
```

#### Applikationen

Folgende Applikationen und Dienste definieren die Grundlage für die Infrastruktureinheit:

| Name | Funktion | 
| --- | --- |
| #Postgreql | Datenbank für MAAS Regiond |
| #MAAS-Region-Controller | Stellt die API für REST & GUI für Bereitstellung zur Verfügung |
| #MAAS-Rack-Controller | Stellt Images für Instanzen bereit. DNS, DHCP, etc | 

Diese Applikationen übernehmen in der Konfiguration für Ansible ihre eigene Rollen und erlauben so die freie Verteilung ihrer Hosts nach Funktion.

##### Design

Mit Ansible sind Rollen für die entsprechende Applikation erstellt worden. So erlaubt es sich verschiedene TEilsysteme im gleichen System oder in verteilten Systemen in Betrieb zu nehmen.

> [!warning]
> Das Playbook und deren Rollen in diesem Projekt sind nicht für echte Productionzwecke mit redundanten oder horizontal skalierenden Systemen gedacht. Sie erlaubt lediglich die Erstellung von separierten Systemen mit ihrer eigenen Verantwortung.

#### Hardware

Folgende Hardware wird eingesetzt um die folgenden Rollen einzusetzen. Diese Entscheidung wurde in Bezug auf mein Budget an physischen Rechnern gemacht.

```mermaid
graph TD;
  pgsql[Postgresql];
  regiond[Regiond Controller];
  rackd[Rackd Controller];
  subgraph Host-1
    pgsql
  end

  subgraph Host-2
    regiond
    rackd
  end
```


### Services

Folgende Services

Mit Ansible werden die MAAS Controller Resourcen bereitgestellt und konfiguriert. Durch Terraform und manueller Handarbeit zum Starten und Integrieren der Maas Instanzen, können danach VMs provisioniert werden und einer bestimmten physischen Insatnz anhand von Charakteristiken wie starke CPU oder hoher RAM muss zum Einsatz.

Nachfolgend wird beschrieben wie die verschiedenen Systeme sich aufeinander verhalten: 


Die verwendeten Systeme werden in vier Ebenen kategorisiert:

- Data Plane
- Control Plane
- Application Plane
- Compute Plane

Mit dieser Aufteilung wird sichergestellt das die einzelnen Komponenten einfach zugeordnet werden können. 

### Abhängigkeiten

![Development Environment Dependencies](./assets/Development-Environment-Dependencies.png)
