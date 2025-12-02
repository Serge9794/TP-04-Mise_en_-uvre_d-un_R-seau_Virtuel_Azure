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

  # ➤ Capture 1 – Création du groupe de ressources
  
<img width="1217" height="807" alt="1" src="https://github.com/user-attachments/assets/01abeed0-247d-45a2-b4e4-ad40f3244910" />


 # ➤ Capture 2 – Formulaire de création du VNet MainServicesVnet
 
<img width="1227" height="811" alt="2" src="https://github.com/user-attachments/assets/5417aac3-dfaa-4f85-b3e0-95b2bbef542b" />
<img width="1230" height="820" alt="2 1" src="https://github.com/user-attachments/assets/0305794f-ed97-4d56-a9ec-3e9b92714ab2" />





# ➤ Capture 3 – Création du sous-réseau AppsSubnet
<img width="1226" height="780" alt="3" src="https://github.com/user-attachments/assets/17650474-8f52-402f-a886-5c28a144e0c0" />






# ➤ Capture 4 – Création du sous-réseau DataSubnet
<img width="1258" height="802" alt="4" src="https://github.com/user-attachments/assets/7953e5c1-2e07-4c3b-b65b-d4d18b81bd93" />




# ➤ Capture 5 – Export du modèle ARM
<img width="1197" height="927" alt="5" src="https://github.com/user-attachments/assets/f59c1d15-55be-4ba4-8d8b-51e7b4454e70" />


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

# ➤ Capture 6 – Fichier template.json modifié
<img width="1285" height="942" alt="6" src="https://github.com/user-attachments/assets/8c9d918f-6e31-45c1-b17c-bbdbfc563839" />
<img width="1294" height="922" alt="6 1" src="https://github.com/user-attachments/assets/3db177c8-9579-415e-b019-40eb25f4d1af" />


# ➤ Capture 7 – Déploiement du template ARM
<img width="1230" height="798" alt="7" src="https://github.com/user-attachments/assets/d71c0fc3-30de-4910-ac9e-8fc90a23b40a" />
<img width="1214" height="812" alt="7 1" src="https://github.com/user-attachments/assets/da444e5b-0c84-4f49-a7c3-71df7340576d" />
#  ➤ Capture 8 – Résultat : VNet FactoryVnet créé
<img width="1220" height="792" alt="8" src="https://github.com/user-attachments/assets/c5d26c50-eaa3-4c24-b340-d7c968ea4e6b" />

---

# 📘 **Tâche 3 – Configurer un ASG et un NSG**

### 🎯 Objectif

Mettre en place un **ASG** pour regrouper des machines par rôle applicatif et un **NSG** pour contrôler le trafic entrant/sortant.

---

## 🔵 **3.1 – Créer un Application Security Group (ASG)**

* Nom : **asg-web-frontend**
* Région : East US

Cet ASG servira pour les VMs front-end web.
# ➤ Capture 9 – Création de l’ASG
<img width="1205" height="813" alt="9 1" src="https://github.com/user-attachments/assets/069c25d2-b555-41c9-86d5-618ffe8c38ed" />
<img width="1216" height="804" alt="9" src="https://github.com/user-attachments/assets/ef736818-21d0-4209-b48c-a0117347efcd" />



---

## 🔵 **3.2 – Créer un Network Security Group (NSG)**

* Nom : **nsg-MainServices**
* Associer au sous-réseau **AppsSubnet** (MainServicesVnet)

 # ➤ Capture 10 – Création du NSG
 <img width="1228" height="810" alt="10" src="https://github.com/user-attachments/assets/a0b353fa-0bb2-42fd-9032-df608e593510" />

 # ➤ Capture 11 – Association du NSG au sous-réseau
 <img width="1240" height="803" alt="11" src="https://github.com/user-attachments/assets/1c159e90-507b-407f-bd88-0ddf2123d983" />

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
<img width="1210" height="807" alt="12" src="https://github.com/user-attachments/assets/a8e70f81-a5be-46f0-8fae-41f71d056c6f" />

---

## 🔵 **3.4 – Ajouter une règle sortante bloquant Internet**

| Paramètre   | Valeur               |
| ----------- | -------------------- |
| Destination | Service Tag          |
| Tag         | Internet             |
| Action      | Deny                 |
| Priorité    | 4096                 |
| Nom         | DenyInternetOutbound |

# ➤ Capture 13 – Règle DenyInternetOutbound
<img width="1225" height="817" alt="13" src="https://github.com/user-attachments/assets/611630bf-6583-4f9c-9969-55f788626587" />

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

# ➤ Capture 14 – Création de la zone DNS publique et liste des serveurs DNS Azure

<img width="1215" height="802" alt="14 15" src="https://github.com/user-attachments/assets/5ca5f21d-de26-4db6-a7a1-0b130a374767" />



# ➤ Capture 15 – Enregistrement A : www

<img width="1230" height="820" alt="16" src="https://github.com/user-attachments/assets/b4a6db07-f1ca-489d-9d3e-751cc0d0b259" />


 #  ➤Tester la résolution :

```bash
nslookup www.demo-lab.net ns-1-09.azure-dns.com.
```
<img width="1120" height="787" alt="TEST" src="https://github.com/user-attachments/assets/d5e96adf-13b0-4c19-a921-38947e74aa86" />


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
# ➤ Capture 17 – Création zone DNS privée
<img width="1221" height="796" alt="17" src="https://github.com/user-attachments/assets/e8cabe11-d790-4ff8-ac35-e5f1a5f6f450" />

# ➤ Capture 18 – Private DNS VNet Link
<img width="1231" height="792" alt="18" src="https://github.com/user-attachments/assets/05b81904-f48c-4280-a578-49229ca7f5e4" />

# ➤ Capture 19 – Ajout enregistrement A (sensor-vm)
<img width="1183" height="832" alt="19" src="https://github.com/user-attachments/assets/67bc4e02-c612-4370-8f50-0ec8acddfb6e" />



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



