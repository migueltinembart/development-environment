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

- [ ] Soll allgemeine DNS Einstellungen für Cloudflare verwalten
- [ ] Soll die Pages Infrastruktur für die Dokumentation bereitstellen

## Umsetzung

### DNS Records

Um DNS Records mit Cloudflare erstellen zu können, kann die [cloudflare_record](https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs/resources/record) verwendet werden. Um mehrere Records erstellen zu können, kann über eine `map` von records iteriert werden.

Im Beispiel unten sieht man wie für Entra ID bestimmte DNS Records erstellt werden. 

```tf

resource "cloudflare_record" "entra_records" {
  for_each = var.records
  zone_id  = cloudflare_zone.tincloud_tech.id
  name     = each.value.name
  value    = each.value.value
  type     = each.value.type
  priority = each.value.priority
  ttl      = each.value.ttl
  proxied  = false
}
```
### Pages Projekt erstellen



### Einsatz

Im [tincloud Infrastructure Repository](https://github.com/migueltinembart/tincloud-infrastructure) kann im Ordner `global/shared` das main.tf initialisiert werden. 

```hcl
# global/shared
cd global/shared
terraform init -backend-config backend.hcl
```

Passe das `.tfvars`-file an und übergib es an terraform für plan oder apply

```hcl
terraform apply -var-file prod.tfvars
```

### Resourcen

Folgende Resourcen werden benötigt um Pages auf Cloudflare deployen zu können:

- [cloudflare_pages_project](https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs/resources/pages_project)
- [cloudflare_pages_domain](https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs/resources/pages_domain)
- [cloudflare_record](https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs/resources/record)
- [cloudflare_api_token](https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs/resources/api_token)

