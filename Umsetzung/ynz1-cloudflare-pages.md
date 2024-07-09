---

title: cloudflare-pages
created by: Miguel Tinembart
created at: 2024-07-06 00:00:00 +0200 CEST
tags:
  - inbox
  - terraform
  - cloudflare
---

## Verwendete Mittel

Folgende Mittel sind für die Implementation in dieser Umsetzung zum Einsatz gekommen:

- [cloudflare](https://www.cloudflare.com)
- [Terraform](./terraform.md)
- [Github Actions](./2vug-github-actions.md)
- [Tincloud Infrastructure Repo](https://github.com/migueltinembart/tincloud-infrastructure)

## Ziele

Folgende Ziele müssen erfüllt werden:

- [ ] Soll DNS Einstellungen für Cloudflare verwalten
- [ ] Soll die Pages Infrastruktur für die Dokumentation bereitstellen

## Einsatz

Im [Tincloud Infrastructure Repo](https://github.com/migueltinembart/tincloud-infrastructure) kann im Ordner //TODO das main.tf initialisiert werden. 

```hcl
# Im Ordner //TODO
terraform init -backend-config backend.hcl
```

Passe das `.tfvars`-file an und übergib es an terraform für plan oder apply

```hcl
terraform apply -var-file prod.tfvars
```

### Erstellte Resourcen


