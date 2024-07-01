---
title: Startseite
date: 2024-06-24
tags:
  - Semesterarbeit
---

# Development Environment

## Projekt

### Module

- #IAC - Infrastructure as Code
- #MAAS - Metal As A Service
- #PRJ - Projektmanangement

### Dozenten

- Armin Dörzbach
- Marcel Bernet
- Philipp Rohr

### Termine

- Abgabedatum: 10.07.2024, 17:00 Uhr

### Projektrepositories

Folgende Repositories sind Bestandteil des Projekts:

- [Maas](https://github.com/migueltinembart/maas)
- [jinja-templates](https://github.com/migueltinembart/jinja-templates)
- [tincloud-infrastructure](https://github.com/migueltinembart/tincloud-infrastructure)

Jedes Repository bietet für die spezifische Anwendung ihre eigene Dokumentations- und Anleitungsstruktur und stehen allen zur Verfügung. Einzelne Repositories vereinfachen die Komposition und ermöglichen eine modulare Arbeitsweise.

## Einleitung

Eine Development Environment dient zur Vereinfachung von Entwicklungsprozessen in einer Organisation, für persönliche Projekte und kleinere Teams. Zu einer Development Environment gehören die Infrastruktur, Tools und Programme welche es sich zum Ziel machen, dem Entwickler oder dem Entwicklerteam eine Standardpalette an Vorgängen anzubieten um Applikationen zu deployen und die nötigen Resourcen zur Verfügung zu stellen um die Deployments zu vereinfachen. Da ich von einem cloud-nativen Ansatz in jeder Hinsicht ab Infrastruktur bis hin zur Applikation profitieren möchte, habe ich es mir zur Aufgabe gemacht die Planung, Umsetzung und die Dokumentation zu diesem Projekt für meine privaten Zwecke, aber auch für zukünftige Semesterarbeiten in die eigene Hand zu nehmen.

### Problem

Meine Infrastruktur besteht aus 2 Proxmox Intanzen, auf welcher diverse VMs laufen und mit Docker verschiedene Applikationen betreibe. Die meiste Konfiguration fand damals aber von Hand statt und die Überischt meiner Infratruktur somit nur durch mich nachvollziehbar. Sollte also irgendwann eine Komponente ersestzt werden müssen, z.B. durch einen Ausfall, kann ich nicht mehr gewährleisten, dass ich dieselbe Konfiguration für meine Applikationen wiederherstellen kann ohne dabei ein Backup herbeirufen zu müssen. 

### Aufgabe

Durch die Nutzung der Praktiken welche mit Infrastructure as Code, soll die Grundlegende Konfigurationen sämtlicher Infrastruktur und Applikationen vereinfachen und nachvollziehbar sein. Der Aufbau der verschiedenen Komponenten soll unabhängig aufbaubar sein und somit in mehrere Repositories gegliedert werden. Mit Terraform sollte die Erstellung von VMs einfacher zu erstellen sein. Ansible übernimmt die Erstellung der nötigen MAAS Komponenten wie Regiond und Rackd und Pipelines verschaffen mir die Möglichkeit automatisiert verschiedene Deployments zu verwirklichen ohne dass, Fehler auf die Infrastruktur überteten

### Projektmethode

Für das Projekt wird die Scrum Methode angewandt. Für dieses Projekt habe ich auf Github ein Project erstellen lassen um den Forttschritt des Projekts im Auge zu behalten.

### Ziele

Folgende Ziele sind für dieses Projekt zu erreichen:

- Einfache Erstellung von [MAAS Controller Instanzen]()
- Verwaltbarkeit von VMs oder anderen Charakteristiken von MAAS Instanzen via Terraform
- Pushen von neuen Deployments via Pipelines
- Einfache Deployments mit cloud-init
- CloudFlare mittels Terraform in die Konfiguration einfliessen lassen
- Keine Secrets im Quellcode verwalten
- Daten verschlüsselt auf Quellcodeablegen, sofern nötig
- Dokumentation des Projekts als Static Webpage übergeben

Die entsprechenden Ziele werden, während der Dokumentation explizit erwähnt

