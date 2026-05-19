# Wazuh Supervision Lab

Projet de supervision sécurité basé sur **Wazuh**, déployé via Docker. Ce lab implémente des règles de détection personnalisées pour surveiller des événements critiques sur un serveur Ubuntu cible.

---

## Objectifs

- Déployer Wazuh Manager + Dashboard via Docker
- Connecter un agent Wazuh sur une VM Ubuntu cible
- Créer des règles personnalisées pour détecter :
  - Les attaques par force brute SSH
  - La création de nouveaux utilisateurs système



---

## Contenu du dépôt

```
wazuh-supervision-lab/
├── README.md
├── docs/
│   ├── installation-wazuh-docker.md     # Installation du serveur Wazuh via Docker
│   └── installation-wazuh-agent.md      # Installation de l'agent Wazuh
├── rules/
│   └── custom_rules.xml                 # Règles personnalisées (force brute SSH, création d'utilisateur)
├── screenshots/
│   ├── rules/                           # Captures des règles dans le dashboard
│   ├── alerts/                          # Captures des alertes generées
│   └── tests/                           # Captures des scénarios de test
└── tests/
    └── test-scenarios.md                # Scénarios de test documentés
```

---

## Règles personnalisées

| ID Règle | Description                        | Niveau |
|----------|------------------------------------|--------|
| 100001   | Tentatives de connexion SSH échouées multiples (force brute) | 10 |
| 100002   | Création d'un nouvel utilisateur système détectée | 8  |

---

## Documentation

- [Installation Wazuh via Docker](https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html)
- [Installation de l'agent Wazuh](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html)

---

## Technologies utilisées

- Wazuh 4.x
- Docker / Docker Compose
- Ubuntu Server 22.04 LTS
- OpenSSH

---
