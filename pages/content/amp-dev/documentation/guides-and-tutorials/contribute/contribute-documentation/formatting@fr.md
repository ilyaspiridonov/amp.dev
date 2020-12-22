---
"$title": Guides et tutoriels de formatage
"$order": '3'
description: Exigences de formatage des fichiers pour amp.dev
formats:
- websites
- stories
- ads
- email
author: CrystalOnScript
---

Les guides et tutoriels sont envoyés en [Markdown](https://www.markdownguide.org/), avec une commande frontmatter supplémentaire et un formatage de shortcode.

## Emplacements de documentation

Le contenu sur amp.dev provient de deux référentiels, notamment [amp.dev](https://github.com/ampproject/amp.dev) et [AMPHTML](https://github.com/ampproject/amphtml) . Toute la documentation de référence sous les composants provient soit des modules intégrés ou des extensions AMPHTML.

- [Composants intégrés ](https://github.com/ampproject/amphtml/tree/master/builtins)
- [Composants](https://github.com/ampproject/amphtml/tree/master/extensions)
- [Cours](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/courses)
- [Exemples](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/examples)
- [Guides et tutoriels](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/guides-and-tutorials)

Plusieurs autres documents sont importés dans amp.dev depuis AMPHTML. Ils sont [répertoriés dans ce fichier](https://github.com/ampproject/amp.dev/blob/future/platform/config/imports/spec.json) . Ne mettez pas à jour ces documents dans le référentiel amp.dev,sinon vos modifications seront écrasées sur les versions suivantes!

## Frontmatter

Frontmatter est fourni en haut de chaque guide et tutoriel.

Exemple:

```yaml
$title: Include Custom JavaScript in AMP Pages
$order: 7
formats:
  - websites
author: CrystalOnScript
contributors:
  - fstanis
description: For web experiences requiring a high amount of customization AMP has created amp-script, a component that allows the use of arbitrary JavaScript on your AMP page without affecting the page's overall performance.
```

<table>
  <tr>
   <td>
    <code>$title</code>
   </td>
   <td>Le titre de votre document tel qu'il apparaîtra dans la table des matières. Mettez en majuscule la première lettre du premier mot, à l'exception de « AM P» et d'autres noms propres. Utilisez l'esperluette '&' au lieu du mot 'et'.</td>
  </tr>
  <tr>
   <td>
    <code>$order</code>
   </td>
   <td>Définissez où votre document apparaît dans la table des matières. Vous devrez peut-être modifier la commande '$order' dans d'autres documents pour qu'elle apparaisse au bon endroit.</td>
  </tr>
  <tr>
   <td>
    <code>formats</code>
   </td>
   <td>Énumérez les expériences AMP pour lesquelles votre document est pertinent. Si votre document est pertinent pour les sites Internet et les stories AMP, mais pas pour les annonces AMP ou les e-mails AMP, votre frontmatter doit être comme suit: ```yaml formats: - websites - stories```</td>
  </tr>
  <tr>
   <td>
<code>author</code>
   </td>
   <td>L'auteur, c'est vous! Utilisez votre nom d'utilisateur GitHub.</td>
  </tr>
  <tr>
   <td>
<code>contributors</code>
   </td>
   <td>Listez toutes les personnes qui ont contribué à votre document. Ce champ est facultatif.</td>
  </tr>
  <tr>
   <td>
<code>description</code>
   </td>
   <td>Rédigez une brève description de votre guide ou tutoriel. Cela aide à l'optimisation pour les moteurs de recherche, et met votre travail à disposition de ceux qui en ont besoin!</td>
  </tr>
  <tr>
   <td>
<code>tutorial</code>
   </td>
   <td>Ajoutez 'tutorial: true' au frontmatter du site Intenret pour ajouter l'icône du tutoriel à côté. Les tutoriels apparaissent au bas de leur section dans la table des matières.</td>
  </tr>
</table>

# Shortcodes

Pour une liste des shortcodes et de leurs utilisations, veuillez consulter [documentation.md sur GitHub](https://github.com/ampproject/amp.dev/blob/future/contributing/documentation.md#shortcodes) .

## Images

amp.dev est conçu avec AMP! Par conséquent, nos images doivent correspondre aux critères [`amp-img`](../../../../documentation/components/reference/amp-img.md). Le processus de conception utilise la syntaxe suivante pour convertir les images au format `amp-img` approprié.

<div class="ap-m-code-snippet"><pre>{{ image('/static/img/docs/tutorials/custom-javascript-tutorial/image1.jpg', 500, 369, layout='intrinsic', alt='Image of basic amp script tutorial starter app') }}</pre></div>

## Filtrage des sections

Certains documents peuvent être pertinents pour plusieurs formats AMP, mais certains formats peuvent nécessiter des explications ou des informations supplémentaires qui ne sont pas pertinentes pour les autres. Vous pouvez filtrer ces sections en les enveloppant dans le shortcode suivant.

<div class="ap-m-code-snippet"><pre>&amp;lsqb;filter formats="websites"]
This is only visible for [websites](?format=websites).
&amp;lsqb;/filter]

&amp;lsqb;filter formats="websites"]
This is only visible for [websites](?format=websites).
&amp;lsqb;/filter]

&amp;lsqb;filter formats="websites, email"]
This is visible for [websites](?format=websites) &amp; [email](?format=email).
&amp;lsqb;/filter]

&amp;lsqb;filter formats="stories"]
This is visible for [stories](?format=stories).
&amp;lsqb;/filter]</pre></div>

## Conseils

Vous pouvez ajouter des conseils et des légendes en enveloppant le texte dans le shortcode suivant:

<div class="ap-m-code-snippet"><pre>&amp;lsqb;tip type="default"]
Default tip
[/tip]

&amp;lsqb;tip type="important"]
Important
[/tip]

&amp;lsqb;tip type="note"]
Note
[/tip]

&amp;lsqb;tip type="read-on"]
Read-on
[/tip]</pre></div>

## Extraits de code

Placez des extraits de code dans des ensembles de trois accents graves, spécifiez la langue à la fin du premier ensemble d'accents graves.

<div class="ap-m-code-snippet"><pre>```html
  // code sample
```

```css
  // code sample
```

```js
  // code sample
```</pre></div>

Si votre code contient des accolades doubles, ce qui est souvent le cas si vous utilisez des modèles [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), vous devez envelopper la partie code:

<div class="ap-m-code-snippet"><pre>```html<br>{% raw	%}<br>  // code with double curly braces<br>{% endraw	%}<br>```</pre></div>

### Extraits de code dans les listes

Python-Markdown a certaines limites. Utilisez la syntaxe suivante lorsque vous incluez des extraits de code dans des listes:

<div class="ap-m-code-snippet"><pre>&lsqb;sourcecode:html]
      <html>
        <p>Indented content.</p>
      </html>
    &lsqb;/sourcecode]</pre></div>

## Aperçu des exemples de code

Les exemples de code peuvent avoir un aperçu et/ou un lien vers une version de [playground AMP](https://playground.amp.dev/).

<div class="ap-m-code-snippet">
  <pre>&lsqb;example preview="default: none|inline|top-frame"
          playground="default: true|false"
          imports="<custom-element-1>,<custom-element-2>,..."
          template="<custom-template>"]
  ```html
    // code sample
  ```
  &lsqb;/example]</pre>
</div>

Remarque: l'aperçu sera automatiquement transformé au format actuellement sélectionné lors de son ouverture dans le playground 🤯!

Utilisez l'attribut `preview` pour définir la façon dont l'aperçu est généré:

- **none**: aucun aperçu ne sera généré

- **inline**: l'exemple d'aperçu s'affiche au-dessus du code source. Un aperçu intégré n'est possible que pour les exemples de sites Internet normaux si le code ne contient aucun élément `head` . Utilisez cette option pour les petits exemples qui n'ont pas besoin de style ou d'autres éléments `head` (les importations ne comptent pas, car elles sont spécifiées via l'attribut `imports` ).

- **top-frame**: l'exemple d'aperçu est affiché au-dessus du code source à l'intérieur d'un iframe. Les modes d'orientation possibles sont `portrait` et `landscape` . Vous pouvez présélectionner l'orientation en spécifiant l'attribut supplémentaire:

- **orientation**: `default: landscape|portrait`

Si des éléments personnalisés sont nécessaires, spécifiez-les dans l'attribut `imports` sous forme de liste séparée par une virgule avec le nom du composant suivi par deux points et la version. Si votre code utilise [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), spécifiez la dépendance plutôt dan l'attribut `template`.

Pour les contenus d'e-mail comprenant des liens de ressources, utilisez le caractère de remplacement <code>{{server_for_email}}</code> dans la source.

### Exemple Inline

Voici un exemple d'intégration inline simple. Vous pouvez définir CSS via des styles inline:

<div class="ap-m-code-snippet"><pre>[example preview="inline" playground="true"]
    ```html
    <div style="background: red; width: 200px; height: 200px;">Hello World</div>
    ```
  [/example]</pre></div>

Vous obtenez ceci:

[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]

Avertissement: les exemples inline sont intégrés directement dans la page. Cela peut entraîner des conflits si des composants sont déjà utilisés sur la page (par exemple, `amp-consent` ).

### Aperçu Top-Frame

Utilisez l'aperçu top-frame chaque fois que vous devez spécifier des éléments d'en-tête ou définir des styles globaux dans `<style amp-custom>` .

Important: n'ajoutez aucun code passe-partout AMP dans l'en-tête car cela sera ajouté automatiquement, en fonction du format AMP. Ajoutez uniquement des éléments nécessaires à l'exemple dans l'en-tête!

<div class="ap-m-code-snippet"><pre>[example preview="top-frame"
         playground="true"]
    ```html
    <head>
      <script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
      <style amp-custom>
        body {
          background: red;
        }
      </style>
    </head>
    <body>
      <h1>Hello AMP</h1>
      <amp-youtube width="480"
        height="270"
        layout="responsive"
        data-videoid="lBTCB7yLs8Y">
      </amp-youtube>
    </body>
    ```
  [/example]</pre></div>

Vous obtenez ceci:

[example preview="top-frame"
         playground="true"]
```html
<head>
  <script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
  <style amp-custom>
    body {
      background: red;
    }
  </style>
</head>
<body>
  <h1>Hello AMP</h1>
  <amp-youtube width="480"
    height="270"
    layout="responsive"
    data-videoid="lBTCB7yLs8Y">
  </amp-youtube>
</body>
```
[/example]

### Stories AMP

Utilisez à la fois `preview="top-frame"` et `orientation="portrait"` pour prévisualiser les stories AMP.

<div class="ap-m-code-snippet"><pre>[example preview="top-frame"
         orientation="portrait"
         playground="true"]
    ```html
    <head>
      <script async custom-element="amp-story"
          src="https://cdn.ampproject.org/v0/amp-story-1.0.js"></script>
      <style amp-custom>
        body {
          font-family: 'Roboto', sans-serif;
        }
        amp-story-page {
          background: white;
        }
      </style>
    </head>
    <body>
      <amp-story standalone>
        <amp-story-page id="cover">
          <amp-story-grid-layer template="vertical">
            <h1>Hello World</h1>
            <p>This is the cover page of this story.</p>
          </amp-story-grid-layer>
        </amp-story-page>
        <amp-story-page id="page-1">
          <amp-story-grid-layer template="vertical">
            <h1>First Page</h1>
            <p>This is the first page of this story.</p>
          </amp-story-grid-layer>
        </amp-story-page>
      </amp-story>
    </body>
    ```
  [/example]</pre></div>

Vous obtenez ceci:

[example preview="top-frame"
         orientation="portrait"
         playground="true"]
```html
  <head>
    <script async custom-element="amp-story"
        src="https://cdn.ampproject.org/v0/amp-story-1.0.js"></script>
    <style amp-custom>
      body {
        font-family: 'Roboto', sans-serif;
      }
      amp-story-page {
        background: white;
      }
    </style>
  </head>
  <body>
    <amp-story standalone>
      <amp-story-page id="cover">
        <amp-story-grid-layer template="vertical">
          <h1>Hello World</h1>
          <p>This is the cover page of this story.</p>
        </amp-story-grid-layer>
      </amp-story-page>
      <amp-story-page id="page-1">
        <amp-story-grid-layer template="vertical">
          <h1>First Page</h1>
          <p>This is the first page of this story.</p>
        </amp-story-grid-layer>
      </amp-story-page>
    </amp-story>
  </body>
```
[/example]

### URL absolues pour les e-mails AMP

Voici comment nous utilisons <code>{{server_for_email}}</code> pour rendre l'URL du point de terminaison absolue si elle est intégrée dans un e-mail AMP.

<div class="ap-m-code-snippet"><pre>[example preview="top-frame" playground="true"]
    ```html
    <div class="resp-img">
      <amp-img alt="flowers"
        src="{{server_for_email}}/static/inline-examples/images/flowers.jpg"
        layout="responsive"
        width="640"
        height="427"></amp-img>
    </div>
    ```
  [/example]</pre></div>

Vous obtenez ceci:

[example preview="top-frame" playground="true"]
```html
<div class="resp-img">
  <amp-img alt="flowers"
    src="{{server_for_email}}/static/inline-examples/images/flowers.jpg"
    layout="responsive"
    width="640"
    height="427"></amp-img>
</div>
```
[/example]

### Retrait de modèles de moustache

Voici un exemple de `top-frame` utilisant un point de terminaison distant. Les modèles de moustache doivent être retirés dans les exemples à l'aide de <code>{% raw %}</code> et <code>{% endraw %}</code> :

<div class="ap-m-code-snippet">
  <pre>[example preview="top-frame"
        playground="true"
        imports="amp-list:0.1"
        template="amp-mustache:0.2"]
    ```html
    <amp-list width="auto" height="100" layout="fixed-height"
      src="{{server_for_email}}/static/inline-examples/data/amp-list-urls.json">
      <template type="amp-mustache">{% raw %}
        <div class="url-entry">
          <a href="{{url}}">{{title}}</a>
        </div>
      {% endraw %}
      </template>
    </amp-list>
    ```
[/example]</pre>
</div>

Vous obtenez ceci:

[example preview="top-frame"
         playground="true"
         imports="amp-list:0.1"
         template="amp-mustache:0.2"]
```html
<amp-list width="auto" height="100" layout="fixed-height"
  src="{{server_for_email}}/static/inline-examples/data/amp-list-urls.json">
  <template type="amp-mustache">{% raw %}
    <div class="url-entry">
      <a href="{{url}}">{{title}}</a>
    </div>
    {% endraw %}
  </template>
</amp-list>
```
[/example]

## Liens

Vous pouvez créer un lien vers d'autres pages avec une syntaxe de lien markdown standard:

```md
 [link](../../../courses/beginning-course/index.md)
```

Lorsque vous créez un lien vers une autre page sur amp.dev, la référence sera un chemin de fichier relatif au fichier cible.

### Ancres

Lien vers des sections spécifiques d'un document à l'aide d'ancres:

```md
[link to example section](#example-section)
```

Veuillez créer la cible de l'ancre à l'aide de `<a name="#anchor-name></a>` avant de créer un lien vers une section sans ancre. Il est judicieux de la placer à la fin du titre de la section.

```html
## Example section <a name="example-section"></a>
```

Vous devez utiliser uniquement des lettres, des chiffres, le tiret et le trait de soulignement dans une ancre. Veuillez utiliser des noms d'ancre courts en anglais qui correspondent au titre ou décrire la section. Assurez-vous que le nom de l'ancre est unique dans le document.

Lorsqu'une page est traduite, les noms des ancres ne doivent pas être modifiés: ils restent en anglais.

Lorsque vous créez une ancre qui sera utilisée dans un lien d'une autre page, vous devez également créer la même ancre dans toutes les traductions.

### Filtre de format AMP

La documentation, les guides, les tutoriels et les exemples de composants peuvent être filtrés par format AMP: sites Internet ou stories AMP, par exemple. Lorsque vous créez un lien vers une telle page, vous devez spécifier explicitement un format pris en charge par la cible, en ajoutant le paramètre de format au lien:

```md
 [link](../../learn/amp-actions-and-events.md?format=websites)
```

Vous pouvez omettre ce paramètre uniquement si vous êtes sûr que la cible prend en charge **tous** les formats de votre page.

### Références de composants

Un lien vers une documentation de référence de composant pointera automatiquement vers la dernière version si votre lien omet la partie version. Lorsque vous souhaitez explicitement pointer vers une version, spécifiez son nom complet:

```md
 [latest version](../../../components/reference/amp-carousel.md?format=websites)
 [explicit version](../../../components/reference/amp-carousel-v0.2.md?format=websites)
```

## Structure de document

### Titres, grands titres et sous-titres

La première lettre du premier mot dans les titres, grands titres et sous-titres est en majuscule, et le reste en minuscules. AMP et autres noms propres sont aussi en majuscules. Il n'existe pas de grand titre `Introduction`, dans la mesure où les introductions suivent le titre du document.

### Dénomination des documents

Nommez les documents en utilisant la convention de dénomination par tirets.

<table>
  <tr>
   <td>
<strong>À faire</strong>
   </td>
   <td>
<strong>À éviter</strong>
   </td>
  </tr>
  <tr>
   <td>hello-world-tutorial.md</td>
   <td>hello_world_tutorial.md</td>
  </tr>
  <tr>
   <td>website-fundamentals.md</td>
   <td>websiteFundamentals.md</td>
  </tr>
  <tr>
   <td>actions-and-events.md</td>
   <td>actionsandevents.md</td>
  </tr>
</table>
