# TP-04-Mise_en_-uvre_d-un_R-seau_Virtuel_Azure
Ce TP consiste à concevoir et déployer un réseau virtuel Azure sécurisé. Il inclut la création d’un VNet avec des sous-réseaux, la configuration de plages d’adresses IP privées conformes  , la mise en place de groupes de sécurité réseau (NSG) pour contrôler le trafic entrant et sortant .


---

# 🧪 **TP 04 – Mise en œuvre d’un Réseau Virtuel Azure**

## 📘 **Introduction**

Dans ce laboratoire, je mets en place les briques réseau essentielles de mon environnement Azure.
L’objectif principal est d’apprendre à créer, structurer et sécuriser des réseaux virtuels tout en intégrant des composants DNS.

Ce TP fait partie d’une série dédiée à la **maîtrise des réseaux Azure**.
À travers ce module, j’apprends :

* La création de réseaux virtuels et de sous-réseaux
* L’utilisation de modèles ARM pour automatiser les déploiements
* La configuration des **NSG (Network Security Groups)** et **ASG (Application Security Groups)**
* L’utilisation d’Azure DNS (zones publiques et privées)


# 🚀 **Scénario**

Mon organisation accélère son déploiement sur Azure et prévoit une croissance rapide des services.
Il devient nécessaire d’adopter une architecture réseau cohérente, segmentée et évolutive.

Dans ce TP, je déploie :

* Un réseau virtuel principal : **MainServicesVnet**
* Un réseau de production : **FactoryVnet**

Chacun disposant d’un adressage propre, sans chevauchement, pour assurer une bonne évolutivité.


# 📂 **Sommaire**

* [Tâche 1 – Créer un réseau virtuel via le portail Azure](#-tâche-1--créer-un-réseau-virtuel-via-le-portail-azure)
* [Tâche 2 – Créer un VNet via un modèle ARM](#-tâche-2--créer-un-vnet-via-un-modèle-arm)
* [Tâche 3 – Configurer un ASG et un NSG](#-tâche-3--configurer-un-asg-et-un-nsg)
* [Tâche 4 – Configurer Azure DNS (public & privé)](#-tâche-4--configurer-azure-dns-public--privé)
* [Nettoyage](#-nettoyage)

---

# 📘 **Tâche 1 – Créer un réseau virtuel via le portail Azure**

### 🎯 Objectif

Apprendre à créer manuellement un réseau virtuel, définir son adressage, créer des sous-réseaux et exporter le modèle ARM généré.

### 🔧 Étapes

1. Accéder au portail Azure.

2. Créer un groupe de ressources : **network-lab-rg**

3. Créer le VNet **MainServicesVnet**

   * Adresse : `10.50.0.0/16`

4. Ajouter deux sous-réseaux :

   | Sous-réseau | Adresse    | CIDR |
   | ----------- | ---------- | ---- |
   | AppsSubnet  | 10.50.10.0 | /24  |
   | DataSubnet  | 10.50.20.0 | /24  |

5. Exporter et télécharger le modèle ARM généré :

   * `template.json`
   * `parameters.json`

     ➤ Capture 1 – Création du groupe de ressources


➤ Capture 2 – Formulaire de création du VNet MainServicesVnet





➤ Capture 3 – Création du sous-réseau AppsSubnet





➤ Capture 4 – Création du sous-réseau DataSubnet



➤ Capture 5 – Export du modèle ARM




# 📘 **Tâche 2 – Créer un VNet via un modèle ARM**

### 🎯 Objectif

Comprendre comment modifier un modèle ARM existant pour automatiser la création d’un second réseau virtuel.

### 🔧 Étapes

1. Modifier le template exporté pour créer **FactoryVnet**.

2. Effectuer les changements suivants :

   **Nom du VNet**

   * MainServicesVnet → FactoryVnet

   **Adresse réseau**

   * `10.50.0.0/16` → `10.60.0.0/16`

3. Modifier les sous-réseaux :

   | Ancien nom | Nouveau nom       | Nouvelle adresse |
   | ---------- | ----------------- | ---------------- |
   | AppsSubnet | ProductionSubnet1 | 10.60.20.0/24    |
   | DataSubnet | ProductionSubnet2 | 10.60.21.0/24    |

4. Modifier aussi `parameters.json`.

5. Déployer via **Deploy a custom template**.

6. Vérifier la création du réseau **FactoryVnet** et de ses sous-réseaux.

   ➤ Capture 6 – Fichier template.json modifié



Modifier le template exporté


➤ Capture 7 – Déploiement du template ARM





➤ Capture 8 – Résultat : VNet FactoryVnet créé



6. Vérifier la création du réseau FactoryVnet


---

# 📘 **Tâche 3 – Configurer un ASG et un NSG**

### 🎯 Objectif

Mettre en place un **ASG** pour regrouper des machines par rôle applicatif et un **NSG** pour contrôler le trafic entrant/sortant.

---

## 🔵 **3.1 – Créer un Application Security Group (ASG)**

* Nom : **asg-web-frontend**
* Région : East US

Cet ASG servira pour les VMs front-end web.
➤ Capture 9 – Création de l’ASG



---

## 🔵 **3.2 – Créer un Network Security Group (NSG)**

* Nom : **nsg-MainServices**
* Associer au sous-réseau **AppsSubnet** (MainServicesVnet)

  ➤ Capture 10 – Création du NSG



➤ Capture 11 – Association du NSG au sous-réseau



---

## 🔵 **3.3 – Ajouter une règle entrante basée sur l’ASG**

| Paramètre | Valeur                     |
| --------- | -------------------------- |
| Source    | Application security group |
| ASG       | asg-web-frontend           |
| Ports     | 80, 443                    |
| Protocole | TCP                        |
| Action    | Allow                      |
| Priorité  | 100                        |
| Nom       | AllowWebASG                |

**➤ Capture 12 – Règle AllowWebASG**


---

## 🔵 **3.4 – Ajouter une règle sortante bloquant Internet**

| Paramètre   | Valeur               |
| ----------- | -------------------- |
| Destination | Service Tag          |
| Tag         | Internet             |
| Action      | Deny                 |
| Priorité    | 4096                 |
| Nom         | DenyInternetOutbound |

➤ Capture 13 – Règle DenyInternetOutbound
---

# 📘 **Tâche 4 – Configurer Azure DNS (public & privé)**

## 🎯 Objectif

Découvrir la gestion DNS sur Azure : zone publique, zone privée, liens réseau, enregistrements DNS.

---

## 🌐 **4.1 – Zone DNS publique**

1. Créer une zone DNS : **demo-lab.net**

2. Noter les serveurs DNS Azure.

3. Ajouter un enregistrement A :

   | Nom | Type | TTL | IP       |
   | --- | ---- | --- | -------- |
   | www | A    | 1   | 10.2.2.4 |

➤ Capture 14 – Création de la zone DNS publique
➤ Capture 15 – Liste des serveurs DNS Azure
➤ Capture 16 – Enregistrement A : www

4. Tester la résolution :

```bash
nslookup www.demo-lab.net <Azure-DNS-Server>
```

---

## 🔒 **4.2 – Zone DNS privée**

1. Créer la zone : **private.demo-lab.net**

2. Créer un lien réseau :

   * Nom : **factory-dns-link**
   * Réseau associé : **FactoryVnet**

3. Ajouter un enregistrement A :

   | Nom       | IP       |
   | --------- | -------- |
   | sensor-vm | 10.2.2.4 |
➤ Capture 17 – Création zone DNS privée
➤ Capture 18 – Private DNS VNet Link
➤ Capture 19 – Ajout enregistrement A (sensor-vm)


---

# 🧹 **Nettoyage**

### Via PowerShell

```powershell
Remove-AzResourceGroup -Name network-lab-rg
```

### Via Azure CLI

```bash
az group delete --name network-lab-rg
```

---

# 🏁 **Conclusion**

Ce TP m’a permis de développer mes compétences réseau sur Azure en manipulant :

* Les réseaux virtuels et sous-réseaux
* Les modèles ARM
* Les ASG/NSG pour la sécurité réseau
* Azure DNS (zones publiques et privées)

Il constitue une base solide pour les TP réseau suivants (Peering, Bastion, Firewall, etc.).

---



