---
title: Startseite
date: 2024-06-24
tags:
  - Semesterarbeit
---

# Development Environment

## Einleitung

Eine Development Environment dient zur Vereinfachung von Entwicklungsprozessen in einer Organisation, für persönliche Projekte und kleinere Teams. Zu einer Development Environment gehören die Infrastruktur, Tools und Programme welche es sich zum Ziel machen, dem Entwickler oder dem Entwicklerteam eine Standardpalette an Vorgängen anzubieten um Applikationen zu deployen und die nötigen Resourcen zur Verfügung zu stellen um die Deployments zu vereinfachen. Da ich von einem cloud-nativen Ansatz in jeder Hinsicht ab Infrastruktur bis hin zur Applikation profitieren möchte, habe ich es mir zur Aufgabe gemacht die Planung, Umsetzung und die Dokumentation zu diesem Projekt für meine privaten Zwecke, aber auch für zukünftige Semesterarbeiten in die eigene Hand zu nehmen.

### Ziel der Projektarbeit

Endziel der Projektarbeit soll es sein eine Infrastruktur mittels Infrastructure-as-Code Praktiken in Betrieb zu nehmen. Teil der Infrastruktur umschliesst folgende Kompenten. 

- Hypervisor
- Container Engine
- Netzwerk
- Github-Runners für Pipelines

Zur Realisierung der meisten Punkte in diesem Gebiet wird MAAS eingesetzt. Zentraler Punkt der Projektarbeit soll das Aufzeigen der Schritte sein, um mit Ansible eine funktionierende MAAS Installation durchführen zu können. Mit MAAS kann können bestehende Rechner verwendet werden, welche dann als Hypervisor für virtuelle Maschinen eingesetzt werden können. Um danach die virtuellen Maschinen auf den verschiedenen Hypervisormaschinen zu provisionieren, soll terraform in Verbindung mit einer Pipeline auf Basis von Github Action mit self-hosted Runnern eingesetzt werden.

### Übersicht

Folgende Unterprojekte sind als eigenständige Repositories bearbeitet worden und gehören genauso zum Teil der Semesterarbeit:

- [Maas](https://github.com/migueltinembart/maas)
- [jinja-templates](https://github.com/migueltinembart/jinja-templates)
- [tincloud-infrastructure](https://github.com/migueltinembart/tincloud-infrastructure)

Jedes Repository bietet für die spezifische Anwendung ihre eigene Dokumentations- und Anleitungsstruktur und stehen allen zur Verfügung. Einzelne Repositories vereinfachen die Komposition und ermöglichen eine modulare Arbeitsweise. 

## Projektablauf


