# Projet VueJs et Vuetify
#### M1 TD03 BRONDINO Thomas et BONTEMPS Cédric

##Fonctionnalités attendue
Au cours de ce projets, afin de poursuivre les travaux réalisés lors des précédents td les fonctionnalités suivantes étaient attendues : <br>
- Refaire l'interface graphique en utilisant des composants graphiques d'une bibliothèques, <br>
- Afficher plus d'informations que le nom et la cuisine (localisation par exemple)
- Ajouter un bouton "détails" dans chaque ligne du tableau 
- Dans le composant RestaurantDetail vous afficherez les informations détaillées sur le restaurant. Une photo (prise au hasard sur le web), une vidéo (optionnel), une carte OpenStreetMap ou GoogleMap avec le restaurant visible dessus (à partir de sa longitude et latitude).
En option : 
- Créer des classes CarteDesPlats et Menu qui serviront à générer aléatoirement une carte. Les plats auront une photo, une description et un prix. 
- Gestion des commandes à distance. On peut passer une commande : "je prendrai un menu standard et deux plats à la carte : la terrine de poisson et le cassoulet de Castelnaudary". On pourra voir sur la droite le récapitulatif de la commande. 
- Ensuite No Limit si vous voulez vous amuser, faites ce que vous sentez...
## Fonctionnalités réalisées
Parmi les fonctionnalités attendues, notre implémentation dispose des éléments suivants :
- Nous avons utilisé Vuetify comme composant graphique pour notre application web.
- Ensuite pour chaque restaurant, on retrouve dans le tableau des icônes permettant de modifier, supprimer ou afficher la vue de détails d'un restaurant. On y retrouve également la localisation.
- Dans le détail de notre composant on retrouve une carte de localisation obtenus à partir des coordonnées du restaurant, ainsi qu'un calendrier et une horloge afin de réserver en ligne un créneau pour ce restaurant.
## Problèmes rencontrés
Au fur et à mesure du projet, les majeurs problèmes que nous avons rencontrés sont les suivants : 
- Utiliser correctement et insérer correctement les éléments dans notre vue de détail, notamment la map qui ne s'affichait directement, nous devons nous rendre au moins une fois dans la vue de détails pour que les fois suivantes la carte s'affiche correctement.
- Mettre en place l'utilisation de plusieurs composants : Nous avons pu correctement partager les méthodes et fonctions entre nos composants mais avons eu quelques soucis pour partager les données et nous avons donc été contraint de nous limiter.
## Utilisation et conclusion
Notre application web dispose donc de chacune des fonctionnalités de base demandée tel que l'ajout, la modification ou la suppression de restaurant. <br>
De plus notre vue de détail affiche la map permettant de se rendre à ce restaurant, cependant, celle-ci ne s'affiche directement, nous devons nous rendre au moins une fois dans la vue de détails pour que les fois suivantes la carte s'affiche correctement. <br>
Enfin dans un système de réservation a été mis en place, dans la vue de détail nous pouvons sélectionner une date et une heure pour réserver une table à un restaurant.
## Lien de la video
https://youtu.be/6QRoalrcH9w
