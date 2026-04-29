# soc-lab-detection
LAB SOC - Détection d'attaques (Brute force, Latéral mouvement, Privilège escalation) avec Splunk et Active Directory

Attaque Brute Force

Cette capture montre plusieurs tentatives de connexion échouées (Event ID 4625) sur le contrôleur de domaine.

Cela indique une possible attaque de type brute force ciblant plusieurs comptes.

![Brute Force](brute-force-4625.png.png)

## Détection brute force (Splunk)

Recherche utilisée :

index=main EventCode=4625
| stats count by Nom_du_compte, "Adresse du réseau source"

Cette analyse montre plusieurs tentatives échouées depuis une même IP (192.168.1.37).

Cela indique une attaque brute force ciblant plusieurs comptes.

![Splunk 4625](splunk-4625.png.png)

## Analyse des connexions réussies (Event ID 4624)

Une analyse des événements 4624 (connexions réussies) a été effectuée afin de vérifier si certaines tentatives de brute force ont abouti.

Recherche utilisée :

index=main EventCode=4624 "Adresse du réseau source"=192.168.1.*
| stats count by Nom_du_compte, "Adresse du réseau source"

Résultat :

Des connexions réussies ont été observées depuis plusieurs adresses IP, notamment :
- 192.168.1.37 (également observée dans les échecs 4625)
- 192.168.1.106

Des comptes sensibles ont été utilisés :
- Administrator
- ANONYMOUS LOGON

### Interprétation

La présence de connexions réussies après de multiples échecs peut indiquer :
- une attaque brute force réussie
- ou une activité légitime à vérifier

### Actions recommandées

- Vérifier l'origine des connexions
- Surveiller les comptes administrateurs
- Mettre en place des alertes Splunk
