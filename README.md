# Wazuh Supervision Lab

Projet de supervision sécurité basé sur **Wazuh**, déployé via Docker. Ce lab implémente des règles de détection personnalisées pour surveiller des événements critiques sur un serveur Ubuntu cible.

---

## Objectifs

- Déployer Wazuh Manager + Dashboard via Docker
- Connecter un agent Wazuh sur une VM Ubuntu cible
- Créer des règles personnalisées pour détecter :
  - Les attaques par force brute SSH
  - La création de nouveaux utilisateurs système

# Création des règles personnalisées via le Dashboard Wazuh


## 1. Accéder à l'éditeur de règles

Dans le menu principal du Dashboard :

```
Menu → Management → Rules
```

---

## 2. Règles de détection SSH — fichier `local_rules.xml`

Ouvrir le fichier `local_rules.xml` dans l'éditeur.

Deux règles ont été configurées dans le groupe `local,syslog,sshd` :

**Règle 100001** — Echec d'authentification SSH depuis une IP ciblée :

```xml
<rule id="100001" level="5">
  <if_sid>5716</if_sid>
  <srcip>1.1.1.1</srcip>
  <description>sshd: authentication failed from IP 1.1.1.1.</description>
  <group>authentication_failed,pci_dss_10.2.4,pci_dss_10.2.5,</group>
</rule>
```

**Règle 100002** — Force brute SSH (5 échecs en 120 secondes depuis la même IP) :

```xml
<rule id="100002" level="10" frequency="5" timeframe="120">
  <if_matched_sid>5716</if_matched_sid>
  <same_source_ip />
  <description>sshd: Brute force attack detected.</description>
  <group>authentication_failures,pci_dss_10.2.4,pci_dss_10.2.5,</group>
</rule>
```

Cliquer sur **Save** puis confirmer le redémarrage du Manager.

![Règles force brute SSH dans local_rules.xml](../screenshots/rules/01-local-rules-ssh.png)

---

## 3. Règle de détection de création d'utilisateur — fichier `Détection_création_d'utilisateur`

Créer un nouveau fichier de règles nommé `Détection_création_d'utilisateur` dans l'éditeur.

Une règle a été configurée dans le groupe `user_management,personnalisé` :

**Règle 100004** — Création d'un nouvel utilisateur système :

```xml
<rule id="100004" level="10">
  <if_sid>5902</if_sid>
  <match>Nouvel utilisateur</match>
  <description>Nouvel utilisateur créé sur le système</description>
  <mitre>
    <id>T1136</id>
  </mitre>
  <group>user_management</group>
</rule>
```

Cliquer sur **Save** puis confirmer le redémarrage.

![Règle création utilisateur](../screenshots/rules/02-regle-creation-utilisateur.png)

---

## 4. Vérifier les alertes dans Security Events

Après les tests, naviguer dans :

```
Menu → Security Events
```

Filtrer par `rule.id : 100002` pour la force brute SSH ou `rule.id : 100004` pour la création d'utilisateur.

![Alertes Security Events](../screenshots/alerts/01-alertes-security-events.png)

---

## Rappel — Règles configurées

| ID     | Fichier                          | Description                          | Niveau | 
|--------|----------------------------------|--------------------------------------|--------|
| 100001 | local_rules.xml                  | Echec SSH depuis IP 1.1.1.1          | 5      | 
| 100002 | local_rules.xml                  | Force brute SSH (5 échecs / 2 min)   | 10     | 
| 100004 | Détection_création_d'utilisateur | Nouvel utilisateur créé sur le système | 10   | 

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
