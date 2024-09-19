# MSPR-Bloc-2

Amazing souhaite utiliser l’IA pour segmenter sa base clients selon leurs habitudes d’achat et leurs interactions avec le site (ex. : visites de pages produits, ajout/retrait du panier, achats). Cette segmentation permettra de proposer des stratégies marketing plus efficaces et de personnaliser l’expérience client pour stimuler les ventes.
L’objectif est de créer un modèle capable de catégoriser les clients en fonction de leurs comportements d’achat et de navigation sur le site. Ce modèle devra être capable d'évoluer et de s'intégrer dans l'infrastructure existante d’Amazing, en exploitant les données à grande échelle et en restant conforme aux régulations telles que le RGPD.

Nous avons mis en place un modèle de segmentation RFM (Récence, Fréquence, Montant), un outil efficace pour catégoriser les clients en fonction de leurs comportements d'achat. Ce modèle permet de segmenter les clients selon trois dimensions clés :

 Récence (R) : Mesure le temps écoulé depuis la dernière interaction du client avec le site (en particulier les achats).

 Fréquence (F) : Mesure le nombre total d'achats effectués par chaque client sur une période donnée.

 Montant (M) : Représente le total des dépenses réalisées par chaque client.

Ces trois indicateurs permettent d'établir un profil comportemental pour chaque client, facilitant la mise en place d'actions marketing ciblées.

Pour alimenter le modèle RFM, nous avons utilisé les données disponibles dans la base de données d'événements fournie par Amazing. Les colonnes principales utilisées pour la segmentation sont les suivantes :

event_time : Date et heure des événements d'achat, elle est essentielle pour mesurer la récence. En calculant la différence entre la date actuelle et la date du dernier achat ou de la dernière interaction, vous pouvez classer les clients selon le temps écoulé depuis leur dernier achat ou visite.

event_type : Filtrage des événements de type purchase pour isoler les actions d'achat.

price : Le prix des produits achetés, indispensable pour calculer le montant total dépensé par chaque client.

user_id : Identifiant unique du client, utilisé pour agréger les actions de chaque utilisateur.

user_session : Identifiant de session, facultatif dans le modèle RFM mais utile pour d'autres analyses de comportement.

Ainsi pour calculer la fréquence (F), l'identifiant de l'utilisateur (user_id) et le type d’événement (event_type) sont nécessaires pour calculer la fréquence. Vous pouvez compter le nombre total d’événements pertinents (achats ou visites) pour chaque utilisateur sur une période donnée, ce qui donne une idée de la fréquence d’interaction ou d’achat de ce client.
De plus pour calculer le montant (M), la colonne price est utilisée pour calculer le montant total dépensé par chaque client. Il est important d'utiliser le event_type pour vous assurer que vous ne comptez que les événements d'achats (et non les vues ou ajouts au panier). L'agrégation des montants pour chaque utilisateur fournira une estimation de la valeur totale dépensée.

  
