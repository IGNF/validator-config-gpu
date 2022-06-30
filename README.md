# validator-config-gpu

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)

## Description

Dépôt de gestion de la configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG au niveau du [Géoportail de l'Urbanisme (GpU)](https://www.geoportail-urbanisme.gouv.fr).

## Mises en garde

* Les schémas sont actuellement gérés avec un outil dédié travaillant sur le contenu `config-backup`.
* L'organisation de ce dépôt est amenée à évoluer.
* Les issues sont les bienvenues en cas de détection d'un problème.
* Les contributions directes sur ce dépôt (pull request) ne sont pas souhaitées dans l'immédiat.

## Principes de gestion des modèles

* Les méta-modèles sont gérés en base de données à l'aide de 4 tables :

  * [validator_document_model](config-backup/validator_document_model.csv) : Liste des standards modélisant le contenu des archives pour les PLU, POS, CC, PSMV, SUP et SCoT
  * [validator_file_model](config-backup/validator_file_model.csv) : Liste des fichiers attendus pour chaque standard
  * [validator_feature_type](config-backup/validator_feature_type.csv) : Modélisation des fichiers de type "table"
  * [validator_attribute_type](config-backup/validator_attribute_type.csv) : Modélisation des colonnes des tables
  * [validator_static_table](config-backup/validator_static_table.csv) : Liste des tables de codes (ex : [PrescriptionPSMVType2019](codes/PrescriptionPSMVType2019.csv))

* La base de données est exportée en CSV dans le dossier `config-backup/` pour permettre la gestion des versions à l'aide de GIT.

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

* La convergence avec [Table Schema](https://specs.frictionlessdata.io/table-schema/) pour "validator_feature_type" et "validator_attribute_type" mis en avant par [schema.data.gouv.fr](http://schema.data.gouv.fr/) pour la modélisation des tables devra être ré-étudiée. Pour l'heure, il subsiste des difficultés pour modéliser certains aspects des standards CNIG à l'aide de ce modèle.
* La gestion des modèles directement en JSON devra elle aussi être envisagés. Pour l'heure, le modèle BDD/CSV offre une notion d'héritage non présente en JSON qui facilite la gestion des standards pour les différentes catégories de SUP.

