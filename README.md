# validator-config-gpu

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)
[![Site](https://github.com/IGNF/validator-config-gpu/actions/workflows/pages.yml/badge.svg)](https://ignf.github.io/validator-config-gpu/)

Configuration de [IGNF/validator](https://github.com/IGNF/validator) pour la validation des standards CNIG dans le [Géoportail de l'Urbanisme (GpU)](https://www.geoportail-urbanisme.gouv.fr).

Les issues sont les bienvenues ; les pull requests ne sont pas souhaitées dans l'immédiat.

## Utiliser les modèles

La branche `main` est publiée sur GitHub Pages à chaque push :

* https://ignf.github.io/validator-config-gpu/ : consultation des modèles ;
* `https://ignf.github.io/validator-config-gpu/config/<modèle>/files.json` : modèle au format du validateur, utilisé par l'API de validation ([IGNF/validator-api](https://github.com/IGNF/validator-api)).

```bash
java -jar validator-cli.jar document_validator \
    --model https://ignf.github.io/validator-config-gpu/config/cnig_PLU_2017/files.json \
    --input <dossier du document> ...
```

Le GpU installe ce dépôt via Composer ([ignf/validator-config-gpu](https://packagist.org/packages/ignf/validator-config-gpu), version figée par tag) et peut donc utiliser une version antérieure : la version en service est visible sur https://www.geoportail-urbanisme.gouv.fr/standard/.

## Standards CNIG

Les modèles traduisent les standards CNIG dans le méta-modèle de IGNF/validator. Les standards originaux (PDF) sont publiés sur la page [Ressources pour la dématérialisation des documents d'urbanisme](https://cnig.gouv.fr/ressources-dematerialisation-documents-d-urbanisme-a2732.html) du CNIG : en cas d'écart, le standard fait foi, merci de le signaler dans les issues. Le CNIG publie aussi des schémas sur GitHub :

| Modèles                                   | Schéma CNIG (GitHub)                    |
| ----------------------------------------- | --------------------------------------- |
| `cnig_PLU_*`, `cnig_PLUi_*`, `cnig_POS_*` | [Plan local d'urbanisme](https://github.com/cnigfr/schema-plan-local-urbanisme) |
| `cnig_CC_*`                               | [Carte communale](https://github.com/cnigfr/schema-carte-communale) |
| `cnig_PSMV_*`                             | [Plan de sauvegarde et de mise en valeur](https://github.com/cnigfr/schema-plan-sauvegarde-et-mise-en-valeur) |
| `cnig_SCoT_*`                             | [Schéma de cohérence territoriale](https://github.com/cnigfr/schema-schema-coherence-territoriale) |
| `cnig_SUP_<CATEGORIE>_*`                  | [Servitudes d'utilité publique](https://github.com/cnigfr/schema-servitudes-utilite-publique) |

* L'année du nom correspond à la version du standard (`cnig_PLU_2017` : PLU v2017).
* Les modèles POS reprennent le standard PLU, séparés pour l'automatisation dans le GpU.
* `GPU_MEC_2025` (mise en compatibilité) est propre au GpU.

Le méta-modèle est [documenté dans IGNF/validator](https://github.com/IGNF/validator/blob/master/validator-core/src/main/resources/schema/README.md). Il est proche de [TableSchema](https://datapackage.org/standard/table-schema/) mais ajoute ce que les standards CNIG exigent : arborescence de fichiers nommés par regex, type `Path` (`NOMFIC`), géométries typées. Rapprochement avec les schémas CNIG : issue [#28](https://github.com/IGNF/validator-config-gpu/issues/28).

## Organisation

* `config/<modèle>/files.json` : document (fichiers attendus, contraintes de nommage, métadonnées) ;
* `config/<modèle>/types/<TABLE>.json` : colonnes et contraintes de chaque table ;
* `config/<modèle>/codes/` : tables de codes ;
* `config/<modèle>/gpu.json` (facultatif) : informations propres au GpU, ignorées par le validateur ;
* `config/variables.json` : variables d'affichage des regex (`<MAILLAGE>`, `<YYYYMMDD>`…), ignorées par le validateur.

Chaque modèle est autonome (pas d'héritage). `config/` est la source versionnée qui permet de restaurer la base du GpU.

## Modifier les modèles

Les fichiers de `config/` ne doivent pas être modifiés à la main : utiliser l'éditeur [IGNF/validator-config-editor](https://github.com/IGNF/validator-config-editor).

```bash
npm ci            # Node.js ≥ 22
npm run edit      # éditeur local sur http://127.0.0.1:5174/
npm run check     # validation de tous les modèles
npm run site      # site statique dans _site/
```
