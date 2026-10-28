# validator-config-gpu

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)
[![Site](https://github.com/IGNF/validator-config-gpu/actions/workflows/pages.yml/badge.svg)](https://ignf.github.io/validator-config-gpu/)

Configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG dans le [Géoportail de l'Urbanisme](https://www.geoportail-urbanisme.gouv.fr).

> [!WARNING]
> Le GpU installe ce dépôt via Composer (ignf/validator-config-gpu, version figée par tag) et peut donc utiliser une version antérieure : la version en service est consultable sur le site du Géoportail de l'Urbanisme.

---

## Consulter les modèles

Un démonstrateur interactif est disponible pour parcourir l'ensemble des modèles :
**[validator-config-gpu](https://ignf.github.io/validator-config-gpu/)**

---

## Utiliser les modèles

URL du modèle (Utilisé par l'[API de validation](https://demo-validator.ign.fr) : 

> https://ignf.github.io/validator-config-gpu/config/<modèle>/files.json 


Exemple d'utilisation en ligne de commande (CLI) :

```bash
  java -jar validator-cli.jar document_validator \
    --model https://ignf.github.io/validator-config-gpu/config/cnig_PLU_2017/files.json \
    --input <dossier_du_document> ...
```

---

## Standards CNIG

Les modèles traduisent les standards CNIG dans le méta-modèle de IGNF/validator. 

Les standards originaux (PDF) officiels sont publiés sur la page Ressources pour la dématérialisation des documents d'urbanisme du CNIG. En cas d'écart, le standard fait foi (merci de le signaler dans les issues du dépôt).

Le CNIG publie également des schémas de référence sur GitHub :

| Modèles de configuration | Schéma CNIG officiel (GitHub) |
| :--- | :--- |
| cnig_PLU_*, cnig_PLUi_*, cnig_POS_* | Plan local d'urbanisme |
| cnig_CC_* | Carte communale |
| cnig_PSMV_* | Plan de sauvegarde et de mise en valeur |
| cnig_SCoT_* | Schéma de cohérence territoriale |
| cnig_SUP_<CATEGORIE>_* | Servitudes d'utilité publique |

* Les modèles POS reprennent le standard PLU, mais sont séparés pour faciliter l'automatisation dans le GpU.
* GPU_MEC_2025 (mise en compatibilité) est un modèle propre au GpU et ne constitue pas un standard officiel du CNIG.

> [!NOTE]
> Le méta-modèle est documenté dans le dépôt IGNF/validator. Il s'inspire de TableSchema tout en y ajoutant les exigences du CNIG (arborescence de fichiers nommés par regex, type Path NOMFIC, géométries typées).

---

## Organisation du dépôt

```
config/
├── variables.json           # Variables d'affichage des regex (<MAILLAGE>, <YYYYMMDD>...)
└── <modèle>/
    ├── files.json           # Document (fichiers attendus, contraintes de nommage, métadonnées)
    ├── types/<TABLE>.json   # Colonnes et contraintes de chaque table
    └── codes/               # Tables de codes
```

---

## Modifier les modèles

Les fichiers du dossier config/ ne doivent pas être modifiés à la main. Utilisez l'éditeur dédié : **[IGNF/validator-config-editor](https://github.com/IGNF/validator-config-editor/)**.
