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
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Recherche par mots-clés des adresses IP malveillantes :  
  - Scanner  
  - Tor  
  - Spamming  
  - Malicious  
  - Proxy  
  - Phishing  
  - VPN  
- Configuration de la règle pour matcher tout le trafic entrant
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Activation en CLI du paramétrage `match-vip`
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
  
👉 **Effet :** disparition de vos services publiés des scanners utilisés par les cyberattaquants pour trouver des cibles.
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------

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
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Application dans les règles de filtrage
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------

---

## 3) Réduire la surface d’exposition du VPN SSL

Le **Portail SSL du FortiGate** offre de nombreuses fonctionnalités (Tunnel VPN, publication d’applications), mais il peut être vulnérable.  
Il est donc crucial d’en limiter l’exposition. 🌐📉

### Mesures recommandées :
1. Limiter l’accès aux pays où se trouvent les utilisateurs nomades
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
3. Modifier le port par défaut (443 → autre)  
4. Bloquer l’accès au Portail SSL aux sources malicieuses via l’Internet Service Database

### Technique :
- Faire écouter le portail SSL sur une **interface Loopback**

---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------  
- Création de l’interface Loopback
  
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Création de la Virtual IP
  
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Création des règles de filtrage
  
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------
- Modification de l’interface d’écoute du VPN SSL
  
---------------------------------------------------------------------------IMAGE-------------------------------------------------------------------------------

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
