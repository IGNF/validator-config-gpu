# Principales commandes 

Voir l'aide en mode console :

```
app/console list ign_validator
```

# Configurations du validateur

Les commandes permettent les conversions entre les 3 formats de configuration

- Configuration en CSV ( validator-deploy/config-backup )

- Base de données ( consultation et l'édition via l'interface : validator-site )

- Configuration XML : utilisée par le validator ( validator-deploy/config )

```
        restore             
     ------------->        export
 CSV                BDD  ------------->  XML
     <-------------     
         backup 
```

Remarque : La commande import, symétrique à export, existe; mais peut perdre des informations (en particulier l'héritage)


# Processus de base pour modifier la configuration du validateur

1) Importer la configuration

```
app/console ign_validator:config:restore vendor/ign/validator-deploy-2/config-backup/
```

2) Modifier la configuration à l'aide de l'interface "validator-site"
L'interface permet de modifier les différents aspects

3) Sauvegarder la nouvelle configuration

```
app/console ign_validator:config:backup vendor/ign/validator-deploy-2/config-backup/
```

4) Exporter la nouvelle configuration pour le validateur

```
app/console ign_validator:config:export
```

5) Commiter et pusher la nouvelle configuration (validator-deploy)


# Astuce pour dupliquer une configuration

1) Importer la configuration en base de données

2) Conserver uniquement les modèles de document à dupliquer

Supprimer les autres dans l'interface

3) Renommer les modèles de document dans l'interface

Par exemple cnig_DU_2013 en cnig_DU_2014

4) Sauvegarder cette configuration dans un répertoire temporaire

```
mkdir temp
app/console ign_validator:config:backup temp
```

5) Resetter les identifiants dans cette sauvegarde

```
app/console ign_validator:config:reset-backup-ids temp
```

6) Importer cette sauvegarde qui dupliquera le modèle

```
app/console ign_validator:config:restore temp
```

7) Réimporter le modèle complet

```
app/console ign_validator:config:restore vendor/ign/validator-deploy/config-backup
```

Remarque : Ce restore rétablira les données d'origine

8) Vérifier le modèle dans l'interface et exporter le modèle complet avec la nouvelle configuration

```
app/console ign_validator:config:backup vendor/ign/validator-deploy/config-backup
```