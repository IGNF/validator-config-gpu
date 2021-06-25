# validator-bundle

## Gestion de la configuration

Les commandes de validator-bundle permettent les conversions entre les 3 formats de configuration :

* Configuration en CSV ( validator-config-cnig/config-backup )

* Base de données ( consultation et l'édition via l'interface : <gpu-site>/admin/validator/ )

* Configuration XML : utilisée par le validator ( validator-config-cnig/config )

```bash
        restore
     ------------->        export
 CSV                BDD  ------------->  JSON
     <-------------
         backup
```

## Processus pour modifier la configuration du validateur

* 1) Importer la configuration en base

Mettre à jour la configuration actuelle :

```bash
php app/console ign_validator:config:restore --mode update
```

Ecraser la configuration actuelle !

```bash
app/console ign_validator:config:restore --mode replace
```

* 2) Modifier la configuration à l'aide de l'interface

L'interface permet de modifier les différents aspects

* 3) Sauvegarder la nouvelle configuration

```bash
app/console ign_validator:config:backup
```

* 4) Exporter la nouvelle configuration pour le validateur

```bash
app/console ign_validator:config:export
```

* 5) Commiter et pusher la nouvelle configuration
