# Projet-Azure
# Guide de simulation des captures d'écran du projet Azure

Bonjour,

Je vais vous donner dans ce fichier README les étapes pour simuler chaque capture d'écran présente dans mon rapport Azure. Pour chaque image (o1.png à d11.png), je vais détailler la procédure à suivre sur le portail Azure.

**Note** : 
- Création et gestion des MV -> o1.png à o5.png
- Mise en place de la haute disponibilité et l'auto-scaling dans Azure -> a1.png à a5.png
- Réseaux Virtuels Azure -> b1.png à b16.png
- Création et gestion du stockage Azure -> c1.png à c7.png
- Azure App Service -> d1 à d11

## Création et gestion des machines virtuelles (o1.png à o6.png)

### o1.png - Interface de création d'une machine virtuelle Azure

Pour reproduire cette capture d'écran :
1. Connectez-vous au [portail Azure](https://portal.azure.com)
2. Cliquez sur "Créer une ressource" dans le menu de gauche
3. Recherchez "Machine virtuelle" et sélectionnez-la
4. Cliquez sur "Créer" > "Machine virtuelle Azure"
5. Vous devriez maintenant voir l'interface de base de création d'une VM

### o2.png - Configuration du nom de la VM

Pour reproduire cette capture d'écran :
1. Dans l'interface de création de VM, sous l'onglet "Bases"
2. Assurez-vous que les champs suivants sont configurés :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : Créez-en un nouveau ou utilisez un existant
   - Nom de la machine virtuelle : Entrez "VM1-Windows"
   - Région : France Central
   - Options de disponibilité : Sélectionnez "Zones de disponibilité"
   - Zones de disponibilité : Cochez les zones 1 et 2
   - Type de sécurité : Standard
   - Image : Windows Server 2016 Datacenter--x64 Gen2
   - Taille : Standard B1s

> **Note**: Si vous rencontrez un problème avec la taille B1s et les zones de disponibilité, essayez de ne sélectionner que la Zone 1, ou optez pour une série de VM plus grande comme D2s_v3.

### o4.png - Configuration du disque OS

Pour reproduire cette capture d'écran :
1. Dans l'interface de création de VM, allez à l'onglet "Disques"
2. Pour "Type de disque du système d'exploitation", sélectionnez "SSD Standard"
3. Pour "Option de redondance", sélectionnez "Stockage localement redondant (LRS)"
4. La taille standard de 127 GiB devrait être automatiquement configurée

### o5.png - Obtention des informations de connexion RDP

Pour reproduire cette capture d'écran :
1. Une fois la VM déployée, accédez à la ressource dans le portail Azure
2. Dans le menu de gauche, cliquez sur "Vue d'ensemble"
3. En haut de la page, cliquez sur le bouton "Connecter"
4. Sélectionnez "RDP" dans les options de connexion
5. Vous verrez alors les informations de connexion RDP, y compris l'adresse IP publique

### o6.png - Erreur de connexion RDP

Cette capture montre une erreur de connexion RDP. Pour simuler cette situation :
1. Essayez de vous connecter à votre VM via RDP avant d'avoir configuré les règles de sécurité réseau
2. Vous devriez voir une erreur indiquant "Impossible de se connecter à l'ordinateur distant"

Pour résoudre ce problème et continuer :
1. Retournez au portail Azure
2. Accédez à votre VM
3. Dans le menu de gauche, sélectionnez "Mise en réseau"
4. Vérifiez qu'une règle entrante pour le port 3389 (RDP) existe
5. Si ce n'est pas le cas, ajoutez une nouvelle règle de port d'entrée pour autoriser le trafic RDP (TCP/3389)

## Ajout d'un disque de données

Après avoir établi une connexion RDP réussie, vous pouvez ajouter un disque de données :

1. Dans le portail Azure, accédez à votre VM
2. Dans le menu de gauche, sélectionnez "Disques"
3. Cliquez sur "Ajouter un disque de données"
4. Configurez un nouveau disque de 32 GiB avec un SSD Standard (LRS)
5. Enregistrez les modifications

Après avoir ajouté le disque dans Azure, connectez-vous à la VM via RDP pour l'initialiser :
1. Ouvrez "Gestion des disques" (tapez diskmgmt.msc dans la barre de recherche Windows)
2. Initialisez le nouveau disque qui apparaît comme "non initialisé"
3. Créez une nouvelle partition simple
4. Formatez le disque en NTFS
5. Assignez-lui la lettre de lecteur E:

## Modification de la taille de la VM

Pour changer la taille de votre VM :
1. Dans le portail Azure, accédez à votre VM
2. Dans le menu de gauche, sélectionnez "Taille"
3. Choisissez la taille B2s (2 vCPU, 4 GiB de RAM)
4. Confirmez le changement

> **Note**: Cette opération nécessitera le redémarrage de la VM, planifiez-la pendant une période de faible activité.

## Capture d'une image de la VM

Pour capturer une image de votre VM :
1. Dans le portail Azure, accédez à votre VM
2. Cliquez sur "Capturer" dans le menu supérieur
3. Configurez les paramètres suivants :
   - Nom de l'image : VM1-Windows-Image
   - Groupe de ressources : même groupe que la VM
   - Option "Capturer uniquement l'image" (pour conserver la VM)
4. Lancez la capture

Si vous rencontrez des problèmes avec la capture d'image, vous pouvez créer un instantané du disque OS :
1. Naviguez vers "Disques" dans le portail Azure
2. Sélectionnez le disque OS de votre VM
3. Cliquez sur "Créer un instantané"
4. Configurez un nom et un type de stockage
5. Créez l'instantané

## Surveillance de la VM

Pour configurer la surveillance de votre VM :
1. Dans le portail Azure, accédez à votre VM
2. Dans le menu de gauche, sélectionnez "Métriques"
3. Configurez des graphiques pour surveiller :
   - Utilisation du CPU
   - Utilisation de la mémoire
   - Opérations d'E/S disque
   - Trafic réseau entrant/sortant

Pour configurer des alertes :
1. Dans le menu de gauche, sélectionnez "Alertes"
2. Cliquez sur "Créer une règle d'alerte"
3. Configurez des alertes pour :
   - Utilisation du CPU > 90% pendant plus de 5 minutes
   - Espace disque disponible < 10%
   - État de la VM (non disponible)

Pour la surveillance complète de la mémoire, installez l'extension de diagnostic Azure :
1. Dans le menu de gauche de votre VM, sélectionnez "Extensions"
2. Cliquez sur "Ajouter"
3. Sélectionnez "Azure Diagnostics Extension"
4. Configurez la collecte des compteurs de performance supplémentaires

## Arrêt et suppression de la VM

Pour arrêter votre VM :
1. Accédez à votre VM dans le portail Azure
2. Cliquez sur "Arrêter" dans le menu supérieur
3. Confirmez l'opération

Pour supprimer la VM et les ressources associées :
1. Sélectionnez votre VM dans le portail Azure
2. Cliquez sur "Supprimer" dans le menu supérieur
3. Cochez les options pour supprimer également :
   - Les disques (OS et données)
   - Les interfaces réseau
   - Les adresses IP publiques
4. Confirmez la suppression

Pour vérifier que toutes les ressources ont été supprimées, vérifiez votre groupe de ressources pour vous assurer qu'aucune ressource liée à votre VM n'est encore présente.

## Mise en place de la haute disponibilité et l'auto-scaling dans Azure (a1.png à a5.png)

Cette section couvre la création d'un groupe à haute disponibilité et d'un ensemble de mise à l'échelle de machines virtuelles (VMSS) dans Azure.

### a1.png - Création d'un groupe à haute disponibilité dans le portail Azure

Pour reproduire cette capture d'écran :
1. Connectez-vous au [portail Azure](https://portal.azure.com)
2. Recherchez "Groupes à haute disponibilité" dans la barre de recherche en haut
3. Cliquez sur "Créer" pour lancer l'assistant de création
4. Configurez les paramètres suivants :
   - Abonnement : Sélectionnez votre abonnement Azure
   - Groupe de ressources : Créez un nouveau groupe appelé "TP-HA-RG"
   - Nom : "TP-AvailabilitySet"
   - Région : France Central
   - Domaines d'erreur : 2 (valeur par défaut)
   - Domaines de mise à jour : 5 (valeur par défaut)
5. Cliquez sur "Vérifier + créer" puis sur "Créer"

### a2.png - Création d'une VM Windows dans le groupe à haute disponibilité

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à "Machines virtuelles"
2. Cliquez sur "Créer" puis sélectionnez "Machine virtuelle"
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "TP-HA-RG" (le même que pour le groupe à haute disponibilité)
   - Nom de la machine virtuelle : "win-vm-1"
   - Région : France Central
   - Options de disponibilité : Sélectionnez "Groupe à haute disponibilité"
   - Groupe à haute disponibilité : "TP-AvailabilitySet" (celui que vous venez de créer)
   - Image : Windows Server 2016 Datacenter (ou l'image personnalisée si vous en avez créé une)
   - Taille : Standard D2s v3
4. Configurez les informations d'identification et les ports d'entrée
5. Continuez avec les paramètres par défaut pour les autres sections
6. Cliquez sur "Vérifier + créer" puis sur "Créer"

### a3.png - Liste des machines virtuelles créées dans le groupe à haute disponibilité

Pour reproduire cette capture d'écran :
1. Créez trois machines virtuelles dans le groupe à haute disponibilité comme indiqué ci-dessus :
   - Une première VM Windows nommée "win-vm-1" (déjà créée à l'étape précédente)
   - Une VM Linux nommée "linux-vm-1" avec Ubuntu Server 20.04 LTS et taille Standard B2s
   - Une deuxième VM Windows nommée "win-vm-2" avec la même image que la première VM Windows
2. Une fois toutes les VM créées, accédez à "Machines virtuelles" dans le portail Azure
3. Vous devriez voir la liste de toutes vos VM avec leurs états respectifs

### a4.png - Domaines d'erreur et de mise à jour pour une VM dans le groupe à haute disponibilité

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à la machine virtuelle "win-vm-1"
2. Dans le menu de gauche, sélectionnez "Disponibilité + mise à l'échelle"
3. Vous verrez alors les informations sur les domaines d'erreur et de mise à jour attribués à cette VM
4. Notez les valeurs pour le domaine d'erreur (0) et le domaine de mise à jour (0)
5. Répétez cette opération pour les autres VM pour observer leur distribution dans les différents domaines

### a5.png - Suppression du groupe de ressources et des ressources associées

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à "Groupes de ressources"
2. Sélectionnez le groupe de ressources "TP-HA-RG"
3. Cliquez sur "Supprimer le groupe de ressources" dans le menu supérieur
4. Dans la boîte de dialogue qui s'affiche, saisissez le nom du groupe de ressources "TP-HA-RG" pour confirmer la suppression
5. Cliquez sur "Supprimer"

### Création d'un ensemble de mise à l'échelle de machines virtuelles (VMSS)

Pour créer un VMSS comme dans l'exercice 3 du rapport :
1. Dans le portail Azure, recherchez "Ensembles de mise à l'échelle de machines virtuelles"
2. Cliquez sur "Créer" pour lancer l'assistant de création
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : Créez un nouveau groupe appelé "TP-VMSS-RG"
   - Nom du groupe identique de machines virtuelles : "tp-vmss"
   - Région : France Central
   - Zone de disponibilité : Sélectionnez "Zone 1"
   - Image : Ubuntu Server 20.04 LTS
   - Taille : Standard DS1_v2
   - Type d'authentification : Mot de passe
   - Configurez un nom d'utilisateur et un mot de passe sécurisés
4. Dans la section "Mise à l'échelle", configurez :
   - Nombre d'instances initial : 3
   - Stratégie de mise à l'échelle : Flexible
   - Nombre minimal d'instances : 2
   - Nombre maximal d'instances : 5
   - Nombre souhaité d'instances : 3
5. Cliquez sur "Vérifier + créer" puis sur "Créer"

### Configuration de l'auto-scaling pour le VMSS

Pour configurer l'auto-scaling une fois le VMSS créé :
1. Accédez à votre VMSS dans le portail Azure
2. Dans le menu de gauche, sélectionnez "Mise à l'échelle"
3. Sélectionnez "Mise à l'échelle automatique personnalisée"
4. Créez deux règles de mise à l'échelle :

   **Règle d'augmentation de capacité :**
   - Nom : "Scale-Out-Rule"
   - Métrique : Pourcentage CPU moyen
   - Opérateur : Supérieur à
   - Seuil : 70%
   - Durée : 10 minutes (période d'agrégation)
   - Action : Augmenter le nombre d'instances de 1
   - Délai d'attente : 5 minutes
   
   **Règle de diminution de capacité :**
   - Nom : "Scale-In-Rule"
   - Métrique : Pourcentage CPU moyen
   - Opérateur : Inférieur à
   - Seuil : 30%
   - Durée : 10 minutes (période d'agrégation)
   - Action : Diminuer le nombre d'instances de 1
   - Délai d'attente : 5 minutes

5. Enregistrez les règles d'auto-scaling

### Test de l'auto-scaling

Pour tester l'auto-scaling de votre VMSS :
1. Accédez à la section "Instances" de votre VMSS pour vérifier que les 3 instances initiales sont bien créées
2. Connectez-vous à l'une des instances via SSH
3. Exécutez le script suivant pour générer une charge CPU élevée :
   ```bash
   while true; do openssl speed; done
   ```
4. Répétez cette opération sur les trois instances
5. Observez dans le portail Azure que le VMSS détecte l'augmentation de la charge CPU
6. Après environ 10 minutes, vérifiez que le VMSS a ajouté une instance supplémentaire (passage de 3 à 4 instances)

### Suppression des ressources VMSS

Une fois vos tests terminés :
1. Accédez à "Groupes de ressources" dans le portail Azure
2. Sélectionnez le groupe de ressources "TP-VMSS-RG"
3. Cliquez sur "Supprimer le groupe de ressources"
4. Confirmez la suppression en saisissant le nom du groupe de ressources
5. Cliquez sur "Supprimer"

## Réseaux Virtuels Azure (b1.png à b16.png)

Cette section couvre la création et la configuration des réseaux virtuels dans Azure, avec un focus sur les communications entre VMs du même réseau, entre VMs de différents réseaux virtuels via le peering, et la mise en place d'une connexion VPN point-à-site.

### b1.png - Création du réseau virtuel BahaSamia-vNet1 avec deux sous-réseaux

Pour reproduire cette capture d'écran :
1. Connectez-vous au [portail Azure](https://portal.azure.com)
2. Recherchez "Réseaux virtuels" dans la barre de recherche et cliquez dessus
3. Cliquez sur "Créer" pour lancer l'assistant de création
4. Configurez les paramètres de base :
   - Abonnement : Sélectionnez votre abonnement Azure
   - Groupe de ressources : Créez un nouveau groupe appelé "vnet-RG"
   - Nom : "BahaSamia-vNet1"
   - Région : North Europe
5. Allez à l'onglet "Adresses IP"
6. Supprimez l'espace d'adressage par défaut et ajoutez "192.168.0.0/16"
7. Créez deux sous-réseaux :
   - Sous-réseau 1 : "Direction" avec l'adresse "192.168.1.0/29"
   - Sous-réseau 2 : "RH" avec l'adresse "192.168.2.0/28"
8. Cliquez sur "Vérifier + créer" puis sur "Créer"

### b2.png - Configuration des règles de sécurité pour BahaSamia-NSG1

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, recherchez "Groupes de sécurité réseau" et cliquez dessus
2. Cliquez sur "Créer" pour créer un nouveau NSG
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG" (le même que pour le réseau virtuel)
   - Nom : "BahaSamia-NSG1"
   - Région : North Europe
4. Cliquez sur "Vérifier + créer" puis sur "Créer"
5. Une fois le NSG créé, ouvrez-le et allez à "Règles de sécurité de trafic entrant"
6. Cliquez sur "Ajouter"
7. Configurez la règle SSH :
   - Source : Any
   - Plages de ports source : *
   - Destination : Any
   - Service : SSH
   - Action : Allow
   - Priorité : 1000 (ou une valeur inférieure à 1001 pour être prioritaire)
   - Nom : "Allow-SSH"
8. Cliquez sur "Ajouter"

### b3.png - Association du NSG aux sous-réseaux Direction et RH

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, ouvrez le NSG "BahaSamia-NSG1"
2. Dans le menu de gauche, cliquez sur "Sous-réseaux"
3. Cliquez sur "Associer"
4. Dans le menu déroulant "Réseau virtuel", sélectionnez "BahaSamia-vNet1"
5. Dans le menu déroulant "Sous-réseau", sélectionnez "Direction"
6. Cliquez sur "OK"
7. Répétez les étapes 3 à 6 pour associer le NSG au sous-réseau "RH"
8. Vous devriez maintenant voir les deux sous-réseaux associés au NSG

### b4.png - Création de VM1 dans le sous-réseau Direction

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à "Machines virtuelles"
2. Cliquez sur "Créer" puis sélectionnez "Machine virtuelle"
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG"
   - Nom de la machine virtuelle : "VM1"
   - Région : North Europe (même région que le réseau virtuel)
   - Image : Ubuntu Server 20.04 LTS
   - Taille : Standard B1s
4. Configurez l'authentification avec un nom d'utilisateur et une clé SSH ou un mot de passe
5. Allez à l'onglet "Mise en réseau"
6. Pour "Réseau virtuel", sélectionnez "BahaSamia-vNet1"
7. Pour "Sous-réseau", sélectionnez "Direction (192.168.1.0/29)"
8. Pour "Adresse IP publique", conservez l'option par défaut
9. Pour "Groupe de sécurité réseau de carte réseau", sélectionnez "Aucun" (car nous utilisons déjà le NSG au niveau du sous-réseau)
10. Désactivez "Supprimer automatiquement l'IP publique lorsque la machine virtuelle est supprimée"
11. Cliquez sur "Vérifier + créer" puis sur "Créer"

### b5.png - Test de ping entre VM1 et VM2 du même réseau virtuel

Pour reproduire cette capture d'écran :
1. Créez une deuxième VM nommée "VM2" en suivant les mêmes étapes que pour VM1, mais placez-la dans le sous-réseau "RH"
2. Connectez-vous à VM1 via SSH :
   ```bash
   ssh votre_nom_utilisateur@adresse_ip_publique_vm1
   ```
3. Une fois connecté à VM1, exécutez un ping vers l'adresse IP privée de VM2 :
   ```bash
   ping 192.168.2.4
   ```
4. Vous devriez voir les réponses du ping, indiquant que la communication entre les deux VMs fonctionne correctement
5. Prenez une capture d'écran du résultat du ping

### b6.png - Diagramme du réseau pour l'exercice 1

Pour reproduire cette capture d'écran :
1. Cette image est un diagramme conceptuel du réseau configuré dans l'exercice 1
2. Vous pouvez créer un diagramme similaire en utilisant un outil comme draw.io ou Microsoft Visio
3. Représentez les éléments suivants dans votre diagramme :
   - Le réseau virtuel BahaSamia-vNet1 (192.168.0.0/16)
   - Les deux sous-réseaux Direction (192.168.1.0/29) et RH (192.168.2.0/28)
   - Les deux machines virtuelles VM1 et VM2 avec leurs adresses IP privées
   - Le groupe de sécurité réseau BahaSamia-NSG1 associé aux deux sous-réseaux
   - Des flèches indiquant la communication entre VM1 et VM2

### b7.png - Création du réseau virtuel BahaSamia-vNet2 avec un sous-réseau

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à "Réseaux virtuels"
2. Cliquez sur "Créer"
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG" (même groupe que le premier réseau virtuel)
   - Nom : "BahaSamia-vNet2"
   - Région : France Central (une région différente du premier réseau virtuel)
4. Allez à l'onglet "Adresses IP"
5. Supprimez l'espace d'adressage par défaut et ajoutez "172.16.0.0/16"
6. Créez un sous-réseau nommé "Personnel" avec l'adresse "172.16.1.0/24"
7. Cliquez sur "Vérifier + créer" puis sur "Créer"

### b8.png - Création de VM3 dans le sous-réseau Personnel

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez à "Machines virtuelles"
2. Cliquez sur "Créer" puis sélectionnez "Machine virtuelle"
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG"
   - Nom de la machine virtuelle : "VM3"
   - Région : France Central (même région que BahaSamia-vNet2)
   - Image : Ubuntu Server 20.04 LTS
   - Taille : Standard B1s
4. Configurez l'authentification avec un nom d'utilisateur et une clé SSH ou un mot de passe
5. Allez à l'onglet "Mise en réseau"
6. Pour "Réseau virtuel", sélectionnez "BahaSamia-vNet2"
7. Pour "Sous-réseau", sélectionnez "Personnel (172.16.1.0/24)"
8. Pour les paramètres de sécurité, autorisez le port SSH (22)
9. Cliquez sur "Vérifier + créer" puis sur "Créer"

### b9.png - Échec du test de ping entre VM1 et VM3 avant le peering

Pour reproduire cette capture d'écran :
1. Connectez-vous à VM1 via SSH
2. Essayez de faire un ping vers l'adresse IP privée de VM3 :
   ```bash
   ping 172.16.1.4
   ```
3. Vous devriez voir que le ping échoue, car par défaut, les réseaux virtuels dans Azure sont isolés les uns des autres
4. Prenez une capture d'écran du résultat du ping (qui devrait montrer des timeouts)

### b10.png - Configuration du peering entre les deux réseaux virtuels

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, accédez au réseau virtuel "BahaSamia-vNet1"
2. Dans le menu de gauche, cliquez sur "Peerings"
3. Cliquez sur "Ajouter" pour créer un nouveau peering
4. Configurez les paramètres du peering :
   - Nom du peering de ce réseau virtuel vers le réseau virtuel distant : "vNet1-to-vNet2"
   - Réseau virtuel distant : Sélectionnez "BahaSamia-vNet2"
   - Nom du peering du réseau virtuel distant vers ce réseau virtuel : "vNet2-to-vNet1"
5. Laissez les autres paramètres par défaut
6. Cliquez sur "Ajouter"
7. Prenez une capture d'écran de la configuration du peering

### b11.png - Test de ping réussi entre VM1 et VM3 après la configuration du peering

Pour reproduire cette capture d'écran :
1. Connectez-vous à VM1 via SSH
2. Essayez à nouveau de faire un ping vers l'adresse IP privée de VM3 :
   ```bash
   ping 172.16.1.4
   ```
3. Cette fois, le ping devrait réussir, confirmant que le peering permet la communication entre les réseaux virtuels
4. Prenez une capture d'écran du résultat du ping réussi

### b12.png - Diagramme du réseau pour l'exercice 2

Pour reproduire cette capture d'écran :
1. Créez un diagramme conceptuel représentant la configuration du réseau après la mise en place du peering
2. Représentez les éléments suivants dans votre diagramme :
   - Les deux réseaux virtuels BahaSamia-vNet1 (192.168.0.0/16) et BahaSamia-vNet2 (172.16.0.0/16)
   - Tous les sous-réseaux : Direction, RH et Personnel
   - Les trois machines virtuelles VM1, VM2 et VM3 avec leurs adresses IP privées
   - Le groupe de sécurité réseau BahaSamia-NSG1
   - La connexion de peering entre les deux réseaux virtuels
   - Des flèches indiquant la communication entre VM1 et VM3

### b13.png - Création du réseau virtuel pour l'exercice 3

Pour reproduire cette capture d'écran :
1. Supprimez d'abord toutes les ressources des exercices précédents
2. Dans le portail Azure, accédez à "Réseaux virtuels"
3. Cliquez sur "Créer"
4. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG"
   - Nom : "BahaSamia-vNet1"
   - Région : France Central
5. Allez à l'onglet "Adresses IP"
6. Supprimez l'espace d'adressage par défaut et ajoutez "172.16.0.0/16"
7. Créez un sous-réseau nommé "Personnel" avec l'adresse "172.16.1.0/24"
8. Cliquez sur "Vérifier + créer" puis sur "Créer"

### b14.png - Création de la passerelle de réseau virtuel

Pour reproduire cette capture d'écran :
1. Dans le portail Azure, recherchez "Passerelles de réseau virtuel" et cliquez dessus
2. Cliquez sur "Créer"
3. Configurez les paramètres de base :
   - Abonnement : Votre abonnement Azure
   - Groupe de ressources : "vnet-RG"
   - Nom : "BahaSamia-VPN-gateway1"
   - Région : France Central (même région que le réseau virtuel)
   - Type de passerelle : VPN
   - Type de VPN : Basé sur des itinéraires
   - SKU : De base
   - Génération : Generation1
   - Réseau virtuel : "BahaSamia-vNet1"
4. Un sous-réseau de passerelle sera automatiquement créé avec l'adresse "172.16.2.0/24"
5. Configurez une nouvelle adresse IP publique nommée "VPN-ip1"
6. Cliquez sur "Vérifier + créer" puis sur "Créer"
7. Prenez note que le déploiement peut prendre jusqu'à 45 minutes

### b15.png - Exportation des certificats pour la connexion VPN

Pour reproduire cette capture d'écran :
1. Sur votre machine Windows locale, ouvrez PowerShell en tant qu'administrateur
2. Générez un certificat racine auto-signé avec PowerShell :
   ```powershell
   $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable -HashAlgorithm sha256 -KeyLength 2048 -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
   ```
3. Générez un certificat client avec PowerShell :
   ```powershell
   New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable -HashAlgorithm sha256 -KeyLength 2048 -CertStoreLocation "Cert:\CurrentUser\My" -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
   ```
4. Exportez le certificat racine au format .cer (sans la clé privée) :
   - Ouvrez le gestionnaire de certificats (certmgr.msc)
   - Naviguez jusqu'à Personnel > Certificats
   - Cliquez avec le bouton droit sur le certificat "P2SRootCert"
   - Sélectionnez "Toutes les tâches" > "Exporter"
   - Suivez l'assistant et sélectionnez "Non, ne pas exporter la clé privée"
   - Sélectionnez le format X.509 encodé en base 64 (.CER)
   - Enregistrez le fichier
5. Exportez le certificat client au format .pfx (avec la clé privée) :
   - Cliquez avec le bouton droit sur le certificat "P2SChildCert"
   - Sélectionnez "Toutes les tâches" > "Exporter"
   - Suivez l'assistant et sélectionnez "Oui, exporter la clé privée"
   - Sélectionnez le format PKCS #12 (.PFX)
   - Définissez un mot de passe pour protéger le certificat
   - Enregistrez le fichier
6. Prenez une capture d'écran du processus d'exportation

### b16.png - Configuration point-à-site de la passerelle VPN

Pour reproduire cette capture d'écran :
1. Une fois la passerelle VPN déployée, accédez-y dans le portail Azure
2. Dans le menu de gauche, cliquez sur "Configuration point-à-site"
3. Configurez les paramètres suivants :
   - Pool d'adresses : "192.168.1.0/24" (plage d'adresses pour les clients VPN)
   - Type de tunnel : OpenVPN (SSL)
   - Type d'authentification : Certificat Azure
4. Sous "Certificats racine", cliquez sur "Ajouter un certificat"
5. Donnez un nom au certificat (par exemple, "BahaSamia-Root")
6. Ouvrez le fichier .cer exporté précédemment avec un éditeur de texte et copiez tout le contenu (y compris les lignes BEGIN CERTIFICATE et END CERTIFICATE)
7. Collez ce contenu dans le champ "Données du certificat public"
8. Cliquez sur "Enregistrer"
9. Prenez une capture d'écran de la configuration point-à-site

### Test de la connexion VPN point-à-site

Pour tester la connexion VPN point-à-site :
1. Dans la configuration point-à-site de la passerelle VPN, cliquez sur "Télécharger le client VPN"
2. Sélectionnez le client approprié pour votre système d'exploitation
3. Téléchargez et installez le client VPN sur votre machine locale
4. Avant de démarrer la connexion VPN, notez votre adresse IP privée actuelle :
   ```
   ipconfig
   ```
5. Importez le certificat client (.pfx) si nécessaire
6. Lancez le client VPN et connectez-vous au réseau virtuel Azure
7. Une fois connecté, vérifiez votre nouvelle adresse IP :
   ```
   ipconfig
   ```
8. Créez une VM dans le sous-réseau "Personnel" du réseau virtuel si ce n'est pas déjà fait
9. Testez la connectivité en faisant un ping vers l'adresse IP privée de cette VM :
   ```
   ping 172.16.1.4
   ```
10. Le ping devrait réussir, confirmant que la connexion VPN fonctionne correctement

### Nettoyage des ressources

Après avoir terminé tous les exercices, nettoyez les ressources pour éviter des frais inutiles :
1. Déconnectez le client VPN
2. Dans le portail Azure, accédez à "Groupes de ressources"
3. Sélectionnez le groupe de ressources "vnet-RG"
4. Cliquez sur "Supprimer le groupe de ressources"
5. Confirmez la suppression en saisissant le nom du groupe de ressources
6. Cliquez sur "Supprimer"
7. Attendez que toutes les ressources soient supprimées

## Création et gestion du stockage Azure (c1.png à c7.png)

[Cette section sera complétée ultérieurement]

## Azure App Service (d1 à d11)

[Cette section sera complétée ultérieurement]
