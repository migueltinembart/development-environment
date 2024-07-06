---

title: Github Actions
created by: Miguel Tinembart
created at: 2024-07-03 00:00:00 +0200 CEST
tags:
  - inbox

---

# Github Actions

## Verwendete Mittel

Folgende Mittel sind für die Implementation in dieser Umsetzung zum Einsatz gekommen:

- [tincloud-infrastructure repo](https://github.com/migueltinembart/tincloud-infrastructure)
- [Terraform](./terraform.md)
- [Cloud-init](https://cloudinit.readthedocs.io/en/latest/)

## Ziele

Folgende Ziele müssen erfüllt werden:

- [ ] Die Runner für Github Actions müssen auf meiner Infrastruktur laufen
- [ ] Die zusätzlichen Runner sollten möglichst einfach hinzugefüght werden können
- [ ] Gute Erreichbarkeit besitzen für die Pulls aller Runs

## Einsatz

Terraform kann über MAAS neue Instanzen provisionieren, DNS-Einstellungen übernehmen und viele weitere Resourcen erstellen. Um mit dem Provider schnell einsatzfähig zu sein, sollte ein API-Schlüssel für den Benutzer der MAAS Instanz erstellt werden. 

Danach kann im File `main.tf` im Provider Block folgende Variabeln spezifiziert werden:

```hcl
terraform {
  required_providers {
    maas = {
      source = "maas/maas"
      version = "2.2.0"
    }
  }
}

provider "maas" {
  api_url  = var.maas_api_url
  api_key  = var.maas_api_key
  
}
```

Entweder übergibt man die Variabeln in einem `.tfvars`-file oder via Kommandozeile wie folgt mit: 

```terraform
terraform plan -var maas_api_url=https://path.to.maas -var maas_api_key=keysforapi
```

## Resourcen

Folgende Resourcen werden benötigt um funktionierende MAAS-Instanzen herzustellen. 

- [maas_instance](https://registry.terraform.io/providers/dan-sullivan/maas/latest/docs/resources/maas_instance)
- [maas_dns_domain](https://registry.terraform.io/providers/dan-sullivan/maas/latest/docs/resources/maas_dns_domain)


