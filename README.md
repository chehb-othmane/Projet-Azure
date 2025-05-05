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

[Cette section sera complétée ultérieurement]

## Réseaux Virtuels Azure (b1.png à b16.png)

[Cette section sera complétée ultérieurement]

## Création et gestion du stockage Azure (c1.png à c7.png)

[Cette section sera complétée ultérieurement]

## Azure App Service (d1 à d11)

[Cette section sera complétée ultérieurement]
