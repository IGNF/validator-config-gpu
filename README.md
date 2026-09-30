# validator-config-gpu

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)

## Description

Dépôt de gestion de la configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG au niveau du [Géoportail de l'Urbanisme (GpU)](https://www.geoportail-urbanisme.gouv.fr).

## Mises en garde

* Les schémas sont actuellement gérés avec un outil dédié (géoportail de l'urbanisme) travaillant sur le contenu `config`.
* L'organisation de ce dépôt est amenée à évoluer.
* Les issues sont les bienvenues en cas de détection d'un problème.
* Les contributions directes sur ce dépôt (pull request) ne sont pas souhaitées dans l'immédiat.

## Principes de gestion des modèles

* Les méta-modèles sont gérés en base de données à l'aide de 4 tables :

  * `validator_document_model` : Liste des standards modélisant le contenu des archives pour les PLU, POS, CC, PSMV, SUP et SCoT
  * `validator_file_model` : Liste des fichiers attendus pour chaque standard
  * `validator_feature_type` : Modélisation des fichiers de type "table"
  * `validator_attribute_type` : Modélisation des colonnes des tables
  * `validator_static_table` : Liste des tables de codes (ex : [Prescription_PSMV_2019](codes/Prescription_PSMV_2019.csv))

* La base de données est exportée dans le dossier `config/` dans un format JSON attendu par [IGNF/validator](https://github.com/IGNF/validator). Ce dossier est la source versionnée : il permet aussi de restaurer la base de données.

* Chaque modèle comporte un fichier `gpu.json`, ignoré par le validateur, qui contient les informations de la base de données non représentées dans le format JSON du validateur (héritage entre modèles et entre tables, tables abstraites, variables `<...>` avant remplacement, activation, fichier source des tables de codes). Les modèles abstraits (ex : `cnig_SUP_XXX_2013`) ont un dossier ne contenant que `gpu.json`.

* Une présentation sous forme d'URL est disponible au niveau du portail :

| Lien                                                                                 | Description                             |
| ------------------------------------------------------------------------------------ | --------------------------------------- |
| https://www.geoportail-urbanisme.gouv.fr/standard/                                   | Liste des standards                     |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017                      | CNIG PLU v2017 (HTML)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017.json                 | CNIG PLU v2017 (JSON)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017#table-ZONE_URBA      | CNIG PLU v2017 - table ZONE_URBA (HTML) |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017/types/ZONE_URBA.json | CNIG PLU v2017 - table ZONE_URBA (JSON) |

## Remarques

* La convergence avec [Table Schema](https://specs.frictionlessdata.io/table-schema/) pour "validator_feature_type" et "validator_attribute_type" mis en avant par [schema.data.gouv.fr](http://schema.data.gouv.fr/) pour la modélisation des tables devra être ré-étudiée. Pour l'heure, il subsiste des difficultés pour modéliser certains aspects des standards CNIG à l'aide de ce modèle.
* L'héritage entre modèles, utile pour les différentes catégories de SUP, n'existe pas dans le format JSON du validateur : il est porté par les fichiers `gpu.json`.