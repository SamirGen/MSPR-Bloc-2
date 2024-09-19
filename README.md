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

Avant de procéder à l'application du modèle RFM (Récence, Fréquence, Montant), il est essentiel d'effectuer un nettoyage minutieux des données afin d'assurer la qualité et la pertinence de l'analyse. Ce processus de préparation des données est une étape cruciale, car des données mal nettoyées ou incohérentes pourraient biaiser les résultats du modèle et, par conséquent, les conclusions du projet. Voici les différentes étapes du processus de nettoyage et de préparation des données, telles qu'elles ont été appliquées.

1. Suppression des Valeurs Manquantes
Dans un premier temps, nous avons vérifié la présence de valeurs manquantes dans les colonnes critiques pour le modèle RFM. Ces colonnes comprennent :

user_id : Identifiant unique de l'utilisateur, essentiel pour agréger les comportements d'achat individuels.
event_time : Date et heure des événements, nécessaire pour calculer la récence.
event_type : Type d’événement (achat, vue de produit, ajout au panier, etc.), qui permet de différencier les actions des utilisateurs.
price : Montant associé à un événement d’achat, nécessaire pour évaluer le montant total dépensé.
La présence de valeurs manquantes dans ces colonnes compromettrait la fiabilité du modèle. Par conséquent, toutes les lignes présentant des valeurs manquantes dans ces colonnes ont été supprimées du jeu de données.

2. Gestion des Doublons
Une autre étape critique a consisté à identifier et supprimer les doublons dans les données. Les doublons peuvent se manifester lorsqu’un même événement est enregistré plusieurs fois (même user_id, product_id, event_time, etc.). Ces doublons peuvent provenir d’erreurs de log ou de processus redondants dans la collecte des données. Leur présence aurait pu fausser les résultats en sur-représentant certains achats ou interactions. Après identification, ces doublons ont été supprimés pour garantir l’unicité des événements.

3. Gestion des Valeurs Aberrantes
La gestion des valeurs aberrantes est une étape essentielle pour garantir la fiabilité des résultats du modèle RFM. Deux types d’anomalies ont été identifiés et corrigés :

Montants aberrants (price) : Des montants anormalement élevés ou bas ont été recherchés, notamment des achats à 0 € ou des valeurs exceptionnellement élevées qui pourraient résulter d’erreurs de saisie. Ces valeurs aberrantes, non représentatives du comportement d'achat standard, ont été soit corrigées, soit supprimées en fonction de leur nature.

Fréquence inhabituelle : Certains utilisateurs semblaient afficher une fréquence d'achats anormalement élevée, ce qui pouvait indiquer un bug dans la collecte des données ou une activité anormale. Une analyse plus approfondie de ces comportements a permis de déterminer s’ils devaient être exclus de l’analyse pour ne pas fausser les résultats.

4. Conversion des Formats de Données
Pour assurer l’intégrité des calculs du modèle RFM, il était nécessaire de vérifier les formats des données. Deux vérifications ont été effectuées :

Format des dates : La colonne event_time a été formatée correctement en type datetime afin de permettre le calcul précis de la récence (R), qui repose sur la date du dernier achat ou événement pertinent.

Format des montants : Les valeurs de la colonne price ont été converties en type float ou integer pour garantir une agrégation correcte lors du calcul du montant total dépensé (M). Une vérification a été effectuée pour s'assurer que ces montants n'étaient pas stockés sous forme de chaînes de caractères, ce qui aurait pu entraîner des erreurs dans les calculs.

5. Vérification de la Cohérence des Données
La dernière étape du nettoyage des données a consisté à vérifier leur cohérence. Nous avons notamment porté attention aux éléments suivants :

Cohérence entre événements : Chaque événement d'achat devait être associé à un identifiant utilisateur (user_id) et à un montant d'achat (price). Des incohérences, telles que des achats sans montant ou sans utilisateur, ont été identifiées et corrigées.

Dates des événements : Une vérification des dates a permis de s'assurer qu’aucun événement d'achat n'était enregistré dans le futur (au-delà de la période d'analyse) ou avant la date officielle de lancement du projet. Des anomalies dans les dates auraient pu indiquer des erreurs dans la collecte des données.

Conclusion
Après avoir suivi ces différentes étapes de nettoyage, les données sont prêtes pour l'agrégation et l'application du modèle RFM. Cette étape de préparation a permis de garantir la qualité des données utilisées, assurant ainsi la pertinence des clusters qui seront générés. Ce nettoyage rigoureux est une condition sine qua non pour la construction d’un modèle fiable et exploitable dans le cadre de ce projet de segmentation client.
