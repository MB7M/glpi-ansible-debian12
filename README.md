# Déploiement automatisé de GLPI 10.0.15 avec Ansible – Cloud privé Proxmox VE

Ce projet propose une solution automatisée pour déployer **GLPI 10.0.15** sur **Debian 12**, à l’aide d’**Ansible**, dans un environnement virtualisé sous **Proxmox VE**.

Chaque étape (installation, configuration, sécurisation HTTPS) est orchestrée par des playbooks indépendants et versionnés.

---

## Schéma d’architecture

![Schéma réseau - Ansible](captures/schema-ansible-glpi.png)

---

## Architecture  

- **Hyperviseur** : Proxmox VE
- **Réseau** : 172.168.1.0/24 (bridge)
- **Domaine local** : mbits.lan

### Machines virtuelles :

| Nom VM        | Rôle                     | OS        | IP             | RAM | Disque |
|---------------|--------------------------|-----------|----------------|-----|--------|
| SRV-ANSIBLE   | Hôte de gestion Ansible  | Debian 12 | 172.168.1.48   | 2Go | 32Go   |
| SRV-GLPI      | Serveur cible GLPI       | Debian 12 | 172.168.1.47   | 2Go | 32Go   |

---

## Documentation technique

La documentation complète est disponible dans le dossier `/docs` :

- [01_install_ansible.md](docs/01_install_ansible.md) – Installation d’Ansible sur Debian 12
- [02_config_mariadb.md](docs/02_config_mariadb.md) – Configuration complète de MariaDB pour GLPI
- [03_deploiement_glpi_cli.md](docs/03_deploiement_glpi_cli.md) – Installation automatisée de GLPI via CLI
- [04_certificat_ssl.md](docs/04_certificat_ssl.md) – Génération de certificat SSL et configuration Apache2

---

## Automatisation

Les playbooks suivants sont exécutés depuis SRV-ANSIBLE pour configurer automatiquement le serveur GLPI :

- [install-glpi-deps.yml](playbooks/install-glpi-deps.yml) : Installation d’Apache, PHP 8.2 et extensions requises
- [mariadb-config.yml](playbooks/mariadb-config.yml) : Configuration de MariaDB (base `glpi`, utilisateur `mbits`)
- [download-glpi.yml](playbooks/download-glpi.yml) : Téléchargement et extraction de l’archive GLPI
- [install-glpi-cli.yml](playbooks/install-glpi-cli.yml) : Installation automatisée de GLPI via `bin/console`
- [glpi-ssl.yml](playbooks/glpi-ssl.yml) : Génération du certificat SSL auto-signé et activation du VirtualHost HTTPS

---

## Structure du dépôt

```
glpi-ansible-debian12/
├── inventory/
│   └── hosts
│
├── playbooks/
│   ├── install-glpi-deps.yml
│   ├── mariadb-config.yml
│   ├── download-glpi.yml
│   ├── install-glpi-cli.yml
│   └── glpi-ssl.yml
│
├── docs/
│   ├── 01_install_ansible.md
│   ├── 02_config_mariadb.md
│   ├── 03_deploiement_glpi_cli.md
│   └── 04_certificat_ssl.md
│
├── captures/
│   └── schema-ansible-glpi.png
├── .gitignore
└── README.md
```
---

## Objectifs atteints

- Déploiement automatisé de GLPI 10.0.15 sur Debian 12
- Installation silencieuse via interface CLI
- Configuration MariaDB, Apache2, PHP 8.2
- Génération et intégration d’un certificat SSL auto-signé

---

## Références

- [GLPI – Site officiel](https://glpi-project.org/)
- [Ansible Documentation](https://docs.ansible.com/)
- [MariaDB Knowledge Base](https://mariadb.com/kb/en/)

