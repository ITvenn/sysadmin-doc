# Durcissement de la sécurité du SI 🛡

**Auteurs :**
- Vincent HAMEL – Administrateur systèmes et réseaux  

---

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
- Blocage des catégories *Security Risk* et *Unrated* (avec possibilité d’exceptions)  
- Application Control : blocage des catégories/applications indésirables  

---

## 5) Les différents types d’authentification forte (2FA)

### Qu’est-ce qu’une authentification forte ?
Une authentification basée sur **au moins deux facteurs** :  
- **Quelque chose que l’on sait** (mot de passe, code, schéma)  
- **Quelque chose que l’on possède** (téléphone, token OTP, carte à puce)  
- **Quelque chose que l’on est** (empreinte biométrique) 💡💭

### Solutions disponibles :
- **FortiToken (physique ou mobile)** – gérés directement par FortiGate  
- Authentification par **SMS ou Email**  
- Intégration avec des **systèmes tiers (SAML : Microsoft, Google, etc.)**  

👉 Guide FortiToken : [Réassignation des tokens](https://community.fortinet.com/t5/FortiAuthenticator/Technical-Tip-Migrating-users-and-FortiTokens-to-another/ta-p/193723)

---

## 6) La conformité des postes nomades

Le **FortiGate** permet de vérifier la conformité des postes se connectant au VPN SSL. 🛂  

### Contrôles possibles :
- Présence/absence d’un fichier  
- Entrée de registre  
- Processus lancé  

### Avancé :
- Création de politiques de conformité via CLI (`config vpn ssl web host-check-software`)  
- Gestion évoluée avec **FortiClient EMS Cloud (ZTNA, EPP/APT, Managed)**  
  - [Fiche FortiClient](https://www.fortinet.com/content/dam/fortinet/assets/data-sheets/og-forticlient.pdf)  
- Solutions **FortiTrust** (IAM, ZTNA, SASE)  
  - [Fiche FortiTrust](https://www.fortinet.com/content/dam/fortinet/assets/data-sheets/og-fortitrust.pdf)

---

## Pour aller plus loin

- [Documentation de durcissement FortiGate](https://docs.fortinet.com/document/fortigate/7.2.0/best-practices/555436/hardening)  
- [Communauté Fortinet](https://community.fortinet.com/)  
- [Formations gratuites Fortinet](https://training.fortinet.com/)  

---
