# validator-config-gpu

## Description

Dépôt de gestion de la configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG au niveau du Géoportail de l'Urbanisme.

## Principes

* Les méta-modèles sont gérés en base de données à l'aide de 4 tables :

  * `validator_document_model` : Liste des standards modélisant le contenu des archives pour les PLU,POS,CC,PSMV,SUP et SCoT
  * `validator_file_model` : Liste des fichiers attendus pour chaque standard
  * `validator_feature_type` : Modélisation de table pour les fichiers de type "table"
  * `validator_attribute_type` : Modélisation des colonnes des tables

* La base de données est exportée en CSV dans le dossier `config-backup/` pour un versionnement à l'aide de GIT

* La base de données est exportée dans le dossier `config/` dans un format JSON attendu par [IGNF/validator](https://github.com/IGNF/validator).

* Une présentation sous forme d'URL est disponible au niveau du portail :

| Lien                                                                                 | Description                             |
| ------------------------------------------------------------------------------------ | --------------------------------------- |
| https://www.geoportail-urbanisme.gouv.fr/standard/                                   | Liste des standards                     |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017                      | CNIG PLU v2017 (HTML)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017.json                 | CNIG PLU v2017 (JSON)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017#table-ZONE_URBA      | CNIG PLU v2017 - table ZONE_URBA (HTML) |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017/types/ZONE_URBA.json | CNIG PLU v2017 - table ZONE_URBA (JSON) |

## Remarques

* La convergence avec [table-schema](https://specs.frictionlessdata.io/table-schema/) mise en avant par [schema.data.gouv.fr](http://schema.data.gouv.fr/) pour la modélisation des tables devra être étudiée. Pour l'heure, il subsiste des difficultés pour modéliser certains aspects des standards CNIG à l'aide de ce modèle.

* La gestion des modèles directement en JSON devra elle aussi être envisagés. Pour l'heure, le modèle BDD/CSV offre une notion d'héritage non présente en JSON qui facilite la gestion des standards pour les différentes catégories de SUP.

