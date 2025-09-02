# Durcissement de la sécurité du SI 🛡

## Sommaire
1. Bloquer les IP indésirables  
2. Réduire la surface d’exposition de ses services publiés sur Internet  
3. Réduire la surface d’exposition du VPN SSL  
4. Durcissement des profils Next-Gen Firewall  
5. Les différents types d’authentification forte possible (2FA)  
6. La conformité des postes nomades  

---

## 1) Bloquer les IP indésirables

Dans l’**Internet Service Database**, le pare-feu **FortiGate** répertorie les adresses IP des principaux services Web ainsi que les adresses IP malveillantes.  
On peut ainsi bloquer ces adresses IP malveillantes. ☢️☠️🏴‍☠️🔥

### Étapes :
- Ajout des adresses IP malveillantes dans une règle de filtrage
<img width="865" height="459" alt="{B44A2684-EFB4-4FE0-80BE-949C23F99CB4}" src="https://github.com/user-attachments/assets/cab7e64a-c2e5-49b8-b05c-8d17757e8445" />

- Recherche par mots-clés des adresses IP malveillantes :  
  - Scanner  
  - Tor  
  - Spamming  
  - Malicious  
  - Proxy  
  - Phishing  
  - VPN

- Configuration de la règle pour matcher tout le trafic entrant
<img width="826" height="455" alt="{42DED69A-6BE5-4BCB-A902-491CDA420FCD}" src="https://github.com/user-attachments/assets/8c590389-e489-4b41-a72f-3c066ad9b8a4" />

- Activation en CLI du paramétrage `match-vip`
<img width="335" height="55" alt="{D91B661E-55EC-4380-8218-58F167F68305}" src="https://github.com/user-attachments/assets/da6240fb-a985-41eb-b934-31de1c691e52" />

  
👉 **Effet :** disparition de vos services publiés des scanners utilisés par les cyberattaquants pour trouver des cibles.
<img width="667" height="227" alt="{781EABFE-00B6-4282-A4F5-A77A563DE181}" src="https://github.com/user-attachments/assets/5bb33bdc-76c6-44cb-b84f-32948129c62f" />


### Bonus :
- Filtrage granulaire des interfaces d’administration avec les **local-in-policy** :  
  [Documentation Fortinet 1](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Filter-ingress-traffic-going-to-the-FortiGate/ta-p/190268)  
  [Documentation Fortinet 2](https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-configure-a-local-in-policy-on-a-HA/ta-p/222005)  
- À partir de la version **7.2.4**, le *virtual patching* permet d’appliquer l’IPS sur les local-in-policy.  
  [Guide Virtual Patching](https://docs.fortinet.com/document/fortigate/7.2.4/administration-guide/393161/virtual-patching-on-the-local-in-management-interface-new)

---

## 2) Réduire la surface d’exposition de ses services publiés sur Internet

Limiter la publication des services aux **pays autorisés uniquement**. 🌐📉  
Cela réduit considérablement la probabilité qu’ils soient détectés par les cyberattaquants.

### Étapes :
- Création d’objets géographiques
<img width="615" height="401" alt="{845FE85F-A61B-4D6B-9902-046A512732E8}" src="https://github.com/user-attachments/assets/27438263-590a-450d-81cf-65b2407d4b87" />

<img width="726" height="364" alt="{C455F723-3703-4239-AE11-7A78B5463618}" src="https://github.com/user-attachments/assets/ed4ef4c6-a412-4022-b032-2159b186fb76" />

- Application dans les règles de filtrage
<img width="1176" height="223" alt="{6E249AFB-BFBE-4850-91F0-2140E06A62A4}" src="https://github.com/user-attachments/assets/158df0b9-dd33-4d10-a90b-18c7dfe10777" />


---

## 3) Réduire la surface d’exposition du VPN SSL

Le **Portail SSL du FortiGate** offre de nombreuses fonctionnalités (Tunnel VPN, publication d’applications), mais il peut être vulnérable.  
Il est donc crucial d’en limiter l’exposition. 🌐📉

### Mesures recommandées :
1. Limiter l’accès aux pays où se trouvent les utilisateurs nomades
2. Modifier le port par défaut (443 → autre)
<img width="804" height="468" alt="{E411984E-7997-4B08-B50B-93C179A88C63}" src="https://github.com/user-attachments/assets/bb59db87-0b4f-4cf6-9898-ae56c45c600e" />

3. Bloquer l’accès au Portail SSL aux sources malicieuses via l’Internet Service Database

### Technique :
- Faire écouter le portail SSL sur une **interface Loopback**
- Création de l’interface Loopback
<img width="459" height="297" alt="{AD22B065-A612-4585-8F9A-F8DCE8501177}" src="https://github.com/user-attachments/assets/a55cbc21-cd3d-46f1-afc8-1a939454d66e" />
<img width="729" height="421" alt="{1522E2BB-B692-42AD-9C0C-0DCC9492963F}" src="https://github.com/user-attachments/assets/ebc4ec2f-cfd2-4c7c-9beb-3884c56b618b" />

- Création de la Virtual IP
<img width="752" height="526" alt="{E1730E8E-92EE-4E4F-AB23-EDF1129B6B78}" src="https://github.com/user-attachments/assets/bd5e18e2-bbe5-46b9-82f2-27d03c8a68ef" />

- Création des règles de filtrage
<img width="1160" height="494" alt="{F8D175E6-47FF-4CD9-B4B5-E6D9CC28608C}" src="https://github.com/user-attachments/assets/4df054f9-987a-47ec-baa2-b05d110efcb3" />

- Modification de l’interface d’écoute du VPN SSL
<img width="846" height="456" alt="{B3405D6D-0044-43AE-B801-81BADA233C07}" src="https://github.com/user-attachments/assets/ed399e68-50d0-4839-9af9-b6a0eeaefe08" />


---

## 4) Durcissement des profils Next-Gen Firewall

Les **profils de sécurité** permettent de bloquer différents types de menaces :  
- **AntiVirus** : analyse antivirus sur le trafic réseau  
- **Intrusion Prevention (IPS)** : blocage des attaques  
- **DNS Filter** : filtrage des domaines  
- **Web Filter** : filtrage des URL  
- **Application Control** : contrôle des applications Web  
- **SSL/SSH Inspection** : analyse du trafic Web chiffré 🔎  

### Application des profils :
- **Menaces → Serveurs (WAN → LAN)** : AntiVirus, IPS  
- **Clients → Menaces (LAN → WAN)** : AntiVirus, IPS, DNS Filter, Web Filter, Application Control  

### Renforcements spécifiques :
- **FortiSandbox Cloud** pour l’analyse avancée
<img width="791" height="527" alt="{4ED86536-790D-47EA-B570-077B75A8D555}" src="https://github.com/user-attachments/assets/aafb9fa9-4314-4660-be1f-1d42b23c6823" />
<img width="625" height="542" alt="{3B303724-032D-410D-A361-58FFEAE262DC}" src="https://github.com/user-attachments/assets/30d2d453-9a4d-4c21-a288-8035280cf2b3" />

- Configuration sécurisée du profil AntiVirus
<img width="801" height="569" alt="{B677A8F2-6616-478A-B976-CFBE975B0ABE}" src="https://github.com/user-attachments/assets/992ad83e-bb05-4fbd-8fc3-9902c80b5322" />

- Configuration sécurisée du profil Intrusion Prevention
<img width="788" height="501" alt="{FEAA9973-BCB3-481B-867B-1D316244FC2F}" src="https://github.com/user-attachments/assets/7fcae9a6-e049-4782-9816-6ad294518098" />

- Configuration sécurisée du profil DNS Filter
<img width="849" height="700" alt="{803D846A-4668-4459-BD95-64BCB7FBE9FE}" src="https://github.com/user-attachments/assets/292cd83a-01ec-4e67-ba54-077306789fd6" />

- Configuration sécurisée du profil Web Filter
<img width="860" height="693" alt="{91AC6D13-9481-424C-9ED7-4A0FA78E4DD8}" src="https://github.com/user-attachments/assets/d73cd531-87de-48f1-b8f6-360b8f433529" />

- Configuration sécurisée du profil Application Control 
<img width="885" height="647" alt="{7023912B-ABE6-4569-A51A-7892624AA90B}" src="https://github.com/user-attachments/assets/08214ce1-f49c-4f23-87cc-02137ad6f979" />

---

## 5) Les différents types d’authentification forte (2FA)

### Qu’est-ce qu’une authentification forte ?
Une authentification basée sur **au moins deux facteurs** :  
- **Quelque chose que l’on sait** (mot de passe, code, schéma)  
- **Quelque chose que l’on possède** (téléphone, token OTP, carte à puce)  
- **Quelque chose que l’on est** (empreinte biométrique) 💡💭

### Solutions disponibles :
- **FortiToken (physique ou mobile)** – gérés directement par FortiGate
<img width="540" height="574" alt="{87C04743-9AE3-491D-B231-47306D603C7F}" src="https://github.com/user-attachments/assets/60bdc8ba-4ccf-45d0-a0dd-1e751c36a576" />

<img width="570" height="579" alt="{0EAD55B2-BE29-41F5-9099-FCFA19E92E96}" src="https://github.com/user-attachments/assets/2c268094-ba30-44da-8dc4-c83ea60d3495" />

<img width="572" height="476" alt="{047773AC-C9E1-487D-8FDA-E47D184DEE6A}" src="https://github.com/user-attachments/assets/43148be8-b18b-41cb-ae9d-275a5b3e758f" />

<img width="1026" height="527" alt="{BAAA75B0-E18E-4DCE-AF36-B3D41858B908}" src="https://github.com/user-attachments/assets/83dc891e-7644-464a-b4dd-a27a6355db66" />

- Authentification par **SMS ou Email**
<img width="761" height="432" alt="{9CCE3EDB-5EA5-482C-A25E-CFD6A651CC1F}" src="https://github.com/user-attachments/assets/1589a183-28f8-41b2-afc0-8c15c7857c43" />
ou en cli :
<img width="570" height="124" alt="{78F08FBB-31D7-4DD5-B3E7-35C91EABB315}" src="https://github.com/user-attachments/assets/9acb1a01-b082-45c8-a837-3b3dbfa023e0" />


- Intégration avec des **systèmes tiers (SAML : Microsoft, Google, etc.)**  


---

## 6) La conformité des postes nomades

Le **FortiGate** permet de vérifier la conformité des postes se connectant au VPN SSL. 🛂  

<img width="638" height="460" alt="{2B49A9A5-AAF7-45ED-9C93-34DE2C2077F9}" src="https://github.com/user-attachments/assets/e18cc3a2-11af-4718-8221-a4d358abd7ab" />

<img width="783" height="591" alt="{CF20E50D-34F2-4FBF-8188-29DEF14D281F}" src="https://github.com/user-attachments/assets/1297a640-13e9-4fcc-afd8-231ed2089b1e" />



---

## Pour aller plus loin

- [Documentation de durcissement FortiGate](https://docs.fortinet.com/document/fortigate/7.2.0/best-practices/555436/hardening)  
- [Communauté Fortinet](https://community.fortinet.com/)  
- [Formations gratuites Fortinet](https://training.fortinet.com/)  

---
