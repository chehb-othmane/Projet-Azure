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

[Cette section sera complétée ultérieurement]

## Création et gestion du stockage Azure (c1.png à c7.png)

[Cette section sera complétée ultérieurement]

## Azure App Service (d1 à d11)

[Cette section sera complétée ultérieurement]
