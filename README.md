# validator-config-gpu

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)

## Description

Dépôt de gestion de la configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG au niveau du [Géoportail de l'Urbanisme (GpU)](https://www.geoportail-urbanisme.gouv.fr).

## Mises en garde

* Les schémas sont actuellement gérés avec un outil dédié (géoportail de l'urbanisme) travaillant sur le contenu `config`.
* L'organisation de ce dépôt est amenée à évoluer.
* Les issues sont les bienvenues en cas de détection d'un problème.
* Les contributions directes sur ce dépôt (pull request) ne sont pas souhaitées dans l'immédiat.

## Standards CNIG de référence

Les modèles de ce dépôt sont une traduction, dans le méta-modèle de [IGNF/validator](https://github.com/IGNF/validator), des standards publiés par le CNIG. Le texte de référence reste le standard CNIG (voir les [ressources pour la dématérialisation des documents d'urbanisme](https://cnig.gouv.fr/ressources-dematerialisation-documents-d-urbanisme-a2732.html)) : en cas d'écart entre un modèle et le standard, c'est le standard qui fait foi et l'écart est à signaler dans les issues de ce dépôt.

Les standards sont également publiés sur GitHub par le CNIG :

| Modèles de ce dépôt                          | Standard CNIG                               | Dépôt CNIG                                                                                                              |
| -------------------------------------------- | ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `cnig_PLU_*`, `cnig_PLUi_*`, `cnig_POS_*`    | Plan local d'urbanisme (PLU)                | [cnigfr/schema-plan-local-urbanisme](https://github.com/cnigfr/schema-plan-local-urbanisme)                             |
| `cnig_CC_*`                                  | Carte communale                             | [cnigfr/schema-carte-communale](https://github.com/cnigfr/schema-carte-communale)                                       |
| `cnig_PSMV_*`                                | Plan de sauvegarde et de mise en valeur     | [cnigfr/schema-plan-sauvegarde-et-mise-en-valeur](https://github.com/cnigfr/schema-plan-sauvegarde-et-mise-en-valeur)   |
| `cnig_SCoT_*`                                | Schéma de cohérence territoriale (SCoT)     | [cnigfr/schema-schema-coherence-territoriale](https://github.com/cnigfr/schema-schema-coherence-territoriale)           |
| `cnig_SUP_<CATEGORIE>_*`                     | Servitudes d'utilité publique (SUP)         | [cnigfr/schema-servitudes-utilite-publique](https://github.com/cnigfr/schema-servitudes-utilite-publique)               |

Remarques :

* L'année figurant dans le nom du modèle correspond à la version du standard (ex : `cnig_PLU_2017` pour le standard CNIG PLU v2017).
* Les modèles POS reprennent le standard PLU ; ils existent séparément pour faciliter l'automatisation de la validation dans le GpU.
* Les modèles SUP sont déclinés par catégorie de servitude (ex : `cnig_SUP_AC1_2016`) à partir d'un modèle abstrait commun (`cnig_SUP_XXX_2013`, `cnig_SUP_XXX_2016`) portant les tables communes (`SERVITUDE`, `ACTE_SUP`, `GESTIONNAIRE_SUP`, `SERVITUDE_ACTE_SUP`).
* `GPU_MEC_2025` est un modèle propre au GpU (mise en compatibilité), sans standard CNIG équivalent.

## Méta-modèle : pourquoi pas TableSchema ?

Les modèles suivent le méta-modèle de IGNF/validator, documenté dans [validator-core/src/main/resources/schema](https://github.com/IGNF/validator/blob/master/validator-core/src/main/resources/schema/README.md), et non [TableSchema](https://datapackage.org/standard/table-schema/) reconnu par [schema.data.gouv.fr](https://schema.data.gouv.fr/).

Ce méta-modèle a été aligné au mieux avec TableSchema, mais la modélisation des standards CNIG de dématérialisation des documents d'urbanisme nécessite des concepts qui n'y sont pas présents :

* **Document** : un document est une arborescence de fichiers dont le nommage obéit à des expressions régulières (ex : nom du dossier, des pièces écrites).
* **Fichiers** : chaque fichier a un type (répertoire, PDF, table, métadonnées...) et une présence obligatoire ou facultative.
* **Types de colonnes** :
  * le type `Path` modélise les colonnes de type `NOMFIC` (chemin relatif d'un fichier dans le document), sans équivalent dans TableSchema ;
  * les géométries sont typées selon Simple Feature Access (`Point`, `MultiPolygon`...), alors que TableSchema propose `geopoint` et `geojson` sans permettre de préciser le type géométrique attendu.

La maîtrise de ce méta-modèle permet en outre de le faire évoluer selon les besoins de validation d'autres standards (ex : type de fichier `multi_table` introduit pour le PCRS vecteur).

Le rapprochement avec les schémas publiés par le CNIG est suivi dans l'issue [#28](https://github.com/IGNF/validator-config-gpu/issues/28).

## Organisation du dépôt

* `config/<modèle>/` : un dossier par modèle, lu par IGNF/validator :
  * `files.json` : description du document (fichiers attendus, tables de codes, contraintes sur le nom du dossier et les métadonnées) ;
  * `types/<TABLE>.json` : description des colonnes et des contraintes de chaque table (dont les clés étrangères `foreignKeys`) ;
  * `codes/` : copie des tables de codes utilisées par le modèle ;
  * `gpu.json` : informations propres au GpU, ignorées par le validateur (voir ci-dessous).
* `codes/` : tables de codes sources, copiées dans chaque modèle à l'export.

## Principes de gestion des modèles

* Les méta-modèles sont gérés en base de données à l'aide de 5 tables :

  * `validator_document_model` : Liste des standards modélisant le contenu des archives pour les PLU, POS, CC, PSMV, SUP et SCoT
  * `validator_file_model` : Liste des fichiers attendus pour chaque standard
  * `validator_feature_type` : Modélisation des fichiers de type "table"
  * `validator_attribute_type` : Modélisation des colonnes des tables
  * `validator_static_table` : Liste des tables de codes (ex : [Prescription_PSMV_2019](codes/Prescription_PSMV_2019.csv))

* La base de données est exportée dans le dossier `config/` dans un format JSON attendu par [IGNF/validator](https://github.com/IGNF/validator). Ce dossier est la source versionnée : il permet aussi de restaurer la base de données.

* Chaque modèle comporte un fichier `gpu.json`, ignoré par le validateur, qui contient les informations de la base de données non représentées dans le format JSON du validateur (héritage entre modèles et entre tables, tables abstraites, variables `<...>` avant remplacement, activation, fichier source des tables de codes). Les modèles abstraits (ex : `cnig_SUP_XXX_2013`) ont un dossier ne contenant que `gpu.json`.

* Les fichiers de `config/` sont produits par l'export et ne doivent pas être modifiés à la main : une valeur présente dans `gpu.json` l'emporte sur celle des fichiers du validateur lors de la restauration.

* Une présentation sous forme d'URL est disponible au niveau du portail :

| Lien                                                                                 | Description                             |
| ------------------------------------------------------------------------------------ | --------------------------------------- |
| https://www.geoportail-urbanisme.gouv.fr/standard/                                   | Liste des standards                     |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017                      | CNIG PLU v2017 (HTML)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017.json                 | CNIG PLU v2017 (JSON)                   |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017#table-ZONE_URBA      | CNIG PLU v2017 - table ZONE_URBA (HTML) |
| https://www.geoportail-urbanisme.gouv.fr/standard/cnig_PLU_2017/types/ZONE_URBA.json | CNIG PLU v2017 - table ZONE_URBA (JSON) |
