---

title: Architektur
created by: Miguel Tinembart
created at: 2024-07-01 00:00:00 +0200 CEST
tags:
  - inbox

---

### Risiken

Es besteht das Risiko dass während oder nach der Inbetriebnahme Komponenten wie Rechner, Netzwerkgeräte oder ähnliches ausfallen können. Da dies auf meiner privaten Umgebung geschieht und aus geschäftlicher Sicht keine Dringlichkeit. Kritischere Kompenten des Projekts wie diese Dokumentation, werden auf Cloudflare Pages hochgeladen um die Ausfallsicherheit zu garantieren.

## Architektur

![Development Environment Planes](./assets/Development-Environment-Planes.png)

Die verwendeten Systeme werden in vier Ebenen kategorisiert:

- Data Plane
- Control Plane
- Application Plane
- Compute Plane

Mit dieser Aufteilung wird sichergestellt das die einzelnen Komponenten einfach zugeordnet werden können. 

### Abhängigkeiten

![Development Environment Dependencies](./assets/Development-Environment-Dependencies.png)

Wenn man die Abhängigkeiten vereinfacht abbildet werden folgende Punkte klar:

- Der Entwickler muss für Änderungen an der Infrastruktur nur die Änderungen commiten und in das Repository pushen
- Die Pipeline muss sicherstellen, dass Fehler im Quellcode nicht zu einem Systemabsturz am Zielsystem führt.
- Auf die Data Plane wird von der Compute und der Application Plane geschrieben und gelesen
- Der Entwickler hat keine manuellen Änderungen am System selber gemacht

