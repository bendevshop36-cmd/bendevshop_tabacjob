# bendev_tabacjob

Job tabac pour ESX. Le joueur récolte des feuilles, les transforme en tabac puis en cigarettes, fait des paquets et les revend. Il peut louer un camion pour se déplacer.

Script gratuit, tu peux le modifier et le partager. Merci de garder le nom de l'auteur.

## Ce qu'il y a dedans

- Récolte sur des plantes que tu places toi-même en jeu
- Deux ateliers de transformation (feuilles, roulage, paquets), avec choix de la quantité
- Vente des paquets à un PNJ
- Location de camion avec caution remboursée au retour
- Interface maison, sans dépendance à un autre menu
- Contrôles côté serveur (distance, quantités, durées, prix)
- Français et anglais

## Dépendances

- es_extended
- ox_lib
- ox_target
- oxmysql

OneSync doit être activé si tu gardes `Config.CheckDistance = true`.

## Installation

1. Mets le dossier `bendev_tabacjob` dans ton dossier `resources`.
2. Ajoute les 4 items :
   - inventaire ESX classique : importe `install/items.sql` dans ta base
   - ox_inventory : copie `install/ox_inventory.lua` dans `ox_inventory/data/items.lua`
3. Ajoute `ensure bendev_tabacjob` dans ton `server.cfg`, après les dépendances.
4. Redémarre le serveur. La table des plantes se crée toute seule.
5. Place les plantes en jeu (voir plus bas).

## Placer les plantes

Les plantes sont enregistrées en base, il n'y a rien à écrire dans la config.

- `/addplant` : ajoute une plante à ta position
- `/delplant` : supprime la plante la plus proche (5 m max)

Ces commandes sont réservées aux groupes listés dans `Config.AdminGroups` (admin et superadmin par défaut), ou à ceux qui ont la permission ace `command`.

## Configuration

Tout se règle dans `config.lua` :

- `Config.Locale` : `fr` ou `en`
- `Config.Notify` : `nui` (intégré), `esx` ou `ox`
- Récolte : item, quantité, durée, délai de repousse
- Camion : modèle, prix de la caution, durée, point de départ et de retour
- Transformations : ce qu'il faut et ce qu'on obtient, durée, position du PNJ
- Vente : item vendu et prix à l'unité
- Position, icône et couleur de chaque blip

Pour ajouter une recette, copie un bloc dans `actions` de `Config.Transfo1` ou `Config.Transfo2Paquet` et change l'`id`, les items et les quantités. L'`id` doit être unique.

## Images des objets

L'interface cherche l'image de chaque item dans l'ordre : `Config.Images.overrides`, puis `Config.Images.path` (ton inventaire), puis `html/img/`. Si rien n'est trouvé, une icône par défaut s'affiche. Le fichier doit s'appeler comme l'item, par exemple `paquet_clope.png`. Pense à régler `Config.Images.path` selon ton inventaire.

## Camion : clés et carburant

Le camion donne une plaque `TABAC` suivie de 3 chiffres. Pour brancher ton script de clés ou de carburant, modifie la fonction `onSpawn` dans `Config.Camion`.

## Traduire

Copie `locales/fr.lua` en `locales/xx.lua`, remplace `Locales['fr']` par `Locales['xx']`, traduis, puis mets `Config.Locale = 'xx'`. La partie `ui` contient les textes de l'interface.

## Remarques

- L'interface charge la police Poppins (Google Fonts) et les icônes Font Awesome (cdnjs) depuis internet. Sans connexion, l'affichage reste utilisable mais moins joli.
- Vérifie les coordonnées par défaut avant de mettre en production, en particulier la station de roulage qui est assez loin des autres points.
- Si tu changes le nom du dossier, aucune autre modification n'est nécessaire.

## Problèmes courants

- **"Il te faut ... " alors que j'ai les items** : les noms d'items de ta base doivent être exactement ceux de la config.
- **Les PNJ n'apparaissent pas** : vérifie que ox_target tourne et que tu es à moins de `Config.PedStreamDistance`.
- **Aucune plante visible** : tu n'en as pas encore ajouté, utilise `/addplant`.
- **"Tu es trop loin" partout** : OneSync n'est pas activé. Active-le ou mets `Config.CheckDistance = false`.
