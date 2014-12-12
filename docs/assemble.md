
Assemble
===============================================================================

[Assemble](http://assemble.io/) est un moteur de génération de page HTML statique. Il utilise des gabarits au format [Handlebars](http://handlebarsjs.com/) en y rajoutant un ensemble de [helpers](https://github.com/assemble/handlebars-helpers) bien pratiques.


Configuration
-------------------------------------------------------------------------------

Assemble est configuré pour nos besoins de la façon suivante:


### Gabarits globaux

Les layouts sont placés dans le dossier `/src/html/layout`. Il y en a deux par défaut:

* `default.hbs`
  Il s'agit du gabarit de base appliqué à toutes les pages par défaut.
* `documentation.hbs`
  Il s'agit du gabarit appliqué aux pages de documentation

Rien n’empêche d'en rajouter d'autres si un projet le nécessite.


### Gabarits partiels

Les gabarits partiels pour inclusion sont placés dans le dossier `/src/html/inc`. A noter que tous les gabarits doivent avoir l'extension `.hbs`

L'inclusion des gabarits est fait via le helper `> nom-du-gabarit`. Le nom du gabarit étant le nom du fichier sans l'extension `.hbs`.

> **NOTE:** _Les différents gabarits peuvent être organiser dans des sous-dossiers. Dans ce cas il faut tout de même s'assurer que les nom des gabarits sont tous différent. Il n'est pas possible d'avoir un gabarit avec le même nom dans deux dossier différents._


### Pages maîtres

Les pages maîtres sont des gabarits spéciaux qui représentent une page du projet et vont agréger un ou plusieurs gabarits partiels. Ces pages maîtres sont placé dans le dossier `/src/html/pages` et peuvent être organisées en sous-dossiers.

Toutes ses pages seront listées automatiquement dans l'index HTML du projet et les méta données placées dans l'en-tête YAML de chaque page seront utilisées pour alimenter ce même index (voir ci-après).


### Données dynamiques

Les données qui peuvent être injectées dans les gabarits sont disponibles dans des fichiers JSON placés dans `/src/html/data`

* `data.json`
  Données accessible dans tous les gabarits. Par défaut, il contient les données de configuration de la page d'index (voir ci-après)
* `dev/env.json`
  Données accessible uniquement lors du build de développement.
* `prod/env.json
  Données accessible uniquement lors du build de production.

Selon les besoins du projet, il est possible de rajouter autant de fichiers JSON que nécessaire. **ATTENTION:** _les sous-dossier autre que `dev` et `prod`ne seront pas pris en compte._


Méta données et page d'index
-------------------------------------------------------------------------------

La page d'index du projet est généré automatiquement à partir de deux sources de données:

### Données global

Le fichier `package.json` contient un certain nombre de données à propos du projet qui seront directement injectées dans l'index du projet:

* `title`: L'intitulé du projet
* `description`: La description du projet
* `logo`: L'URL du logo à afficher sur la page d'index


### Données de pages

Chaque page maître doit contenir un certain nombre de méta-données dans sont en-tête YAML. Toutes ces méta-données seront affichées dans la page d'index du projet.

#### Données obligatoire

* `title`: Le titre de la page
* `description`: La description de la page

#### Données facultatives

L'index va afficher un tableau résumant les informations de la page. Pour cela il dispose d'un certain nombres de colonnes utilisables.

Les colonnes disponibles sont configurés dans le fichier `/src/html/data/data.json`. Une colonne configurée avec la valeur `false` ne sera jamais affichée.
Chaque colonne qui doit être affiché doit être configuré avec un objet JSON ayant les paramètre suivant:

* `label`: Le titre de l'en-tête de colonne ou de groupe.
* `url`: Une URL _optionnelle_ pour créer un lien sur l'en-tête de colonne.
* `length`: Pour les groupes, indique le nombre de colonne dans le groupe.

Pour chaque colonne configurée, il est possible de spécifier une valeur correspondante pour chaque page maître directement dans sont en-tête YAML.

#### Exemple

Soit le fichier `data.json` suivant:

```javascript
{
  "INDEX": {
    "HTML": {"label": "Validation"},
    "CSS": false
  }
}
```

Et un fichier `ma-page.hbs` avec l'en-tête YAML suivant:

```yaml
---
title: Ma page
description: Exemple de meta-données

META:
  HTML: OK
  CSS : KO
---
```

L'index contiendra le tableau HTML suivant:

```xml
<table>
  <head>
    <tr>
      <th scope="col">Page</th>
      <th scope="col">Validation</th>
    </tr>
  </head>
  <tbody>
    <tr>
      <td>
        <a href="pages/ma-page.html">Exemple de page</a>
        <br>
        <small>Exemple de meta-données</small>
      </td>
      <td>OK</td>
    </tr>
  </tbody>
</table>
```

Pour un exemple plus complet, voir la page `test.hbs` et le fichier `data.json`de ce Starter Kit