---
"$title": Enable experimental features
"$order": '3'
description: Les composants expérimentaux AMP sont des fonctionnalités publiées qui ne sont pas encore prêtes pour une large utilisation, et sont donc protégées par un statut expérimental.
formats:
- sites Web
- stories
- annonces
---

[AMP experimental components](https://github.com/ampproject/amphtml/tree/master/tools/experiments) are released features not yet ready for wide use, so they are protected by an **experimental** status.

Developers and users can opt-in to using these features before they are fully released. But they should be used with caution, as they may contain bugs or have unexpected side effects.

[tip type="important"] Il existe un risque que certaines expériences ne soient jamais livrées en tant que fonctionnalités sur le Projet AMP. [/tip]

{% set experimental_components = g.docs ('/ content / amp-dev / documentation / components / reference') | selectattr ('experimental') | list%} {% if experimental_components | length%} Voici une liste de composants qui sont actuellement en état expérimental et sont prêts à être testés par les développeurs pour les premiers commentaires des utilisateurs:

<ul>{% pour le composant dans composants_expérimentaux %}<li> <a href="%7B%7B%20component.url.path%20%7D%7D">{{component.title}}</a>
</li> {% endfor%}</ul> {% endif %}

## Opt into the AMP Dev Channel

AMP Dev CHannel est un moyen d'activer l'utilisation sur un navigateur d'une version plus récente des bibliothèques AMP JS.

La version AMP Dev Channel **peut être moins stable** et contenir des fonctionnalités non disponibles pour tous les utilisateurs. Activez cette option si vous souhaitez aider à tester de nouvelles versions d'AMP, signaler des bogues ou créer des documents qui nécessitent une nouvelle fonctionnalité qui n'est pas encore disponible pour tout le monde.

L'activation de Dev Channel est idéale pour:

- test and play with new features not yet available to all users.
- use in quality assurance (QA) to ensure that your site is compatible with the next version of AMP.

Si vous trouvez un problème qui ne semble se produire que dans la version Dev Channel d'AMP, [signalez un problème](https://github.com/ampproject/amphtml/issues/new) avec une description. Incluez toujours une URL vers une page qui reproduit le problème.

To opt your browser into the AMP Dev Channel, go to [the AMP experiments page](https://cdn.ampproject.org/experiments.html) and activate the "AMP Dev Channel" experiment. To get notified about important/breaking changes about AMP, subscribe to the [amphtml-announce](https://groups.google.com/forum/#!forum/amphtml-announce) mailing list.

## Enable an experimental component

#### Served from cdn.ampproject.org

For content served from `https://*.cdn.ampproject.org`, go to `/experiments.html` on a Google AMP Cache subdomain and enable (or disable) any experimental component by toggling them on (or off).

Par exemple, pour activer les composants expérimentaux sur les pages AMP mises en cache dont l'origine source est `www.example.com`, allez sur `www-example-com.cdn.ampproject.org/experiments.html`.

L'activation de composants expérimentaux est enregistrée sur `localStorage` et active uniquement le composant expérimental sur les pages AMP fournies depuis le domaine actuel.

#### Served from other domains

For content served from non-CDN domains, experiments can be toggled in the devtools console using:

```js
AMP.toggleExperiment('experiment')
```

Any AMP file that includes experimental features will fail [AMP validation](validation-workflow/validate_amp.md). Remove these experimental components for production-ready AMP documents.

## Enable an experiment for a particular document

Document can choose to opt in a certain experiments. To do that, place a meta tag of the `amp-experiments-opt-in` name in the head of the HTML document before your AMP script (`https://cdn.ampproject.org/v0.js`). Its content value is a comma-separated string of experiment IDs to opt in.

```html
<head>
  ...
  <meta name="amp-experiments-opt-in" content="experiment-a,experiment-b">
  <!-- The meta tag needs to be placed before the AMP runtime script.-->
  <script async src="https://cdn.ampproject.org/v0.js"></script>
  ...
</head>
```

Ce faisant, les contenus expérimentaux spécifiés seront activés pour tous les visiteurs du document. Cependant, tous les contenus expérimentaux ne sont pas activables par le document. Pour obtenir la liste complète des composants expérimentaux autorisés, consultez l'attribut `allow-doc-opt-in` dans le fichier [`prod-config.json`](https://github.com/ampproject/amphtml/blob/master/build-system/global-configs/prod-config.json) du projet. Notez que l'activation par le document peut être annulé par la désactivation par l'utilisateur.

## Essais d'origine

[Les essais d'origine](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/explainer.md) permettent aux développeurs d'utiliser une fonctionnalité expérimentale en production et de fournir des commentaires essentiels.

Traditionnellement, une fonctionnalité en mode expérimental peut être utilisée dans le développement, mais ne peut pas être poussée en production. Avec les essais d'origine, les développeurs intéressés peuvent choisir de tester une fonctionnalité expérimentale en production, avec les attentes suivantes:

- Le test a une durée limitée.
- La fonctionnalité subira probablement quelques modifications après les essais d'origine.

Les essais d'origine offrent la possibilité de mettre en œuvre et de bénéficier d'une nouvelle fonctionnalité avant qu'elle ne soit totalement opérationnelle. La fonctionnalité vivra sur le site du développeur, et ne sera pas surveillé par un composant expérimental, et les commentaires peuvent directement influencer la direction de la fonctionnalité.

{% set trial_components = g.docs('/content/amp-dev/documentation/components/reference')|selectattr('origin_trial')|list %} {% if trial_components|length %} Les composants de la liste suivante peuvent être actuellement testés via un essai d'origine:

<ul>{% pour le composant dans composants_d'essai %}<li> <a href="%7B%7B%20component.url.path%20%7D%7D">{{component.title}}</a>
</li> {% endfor%}</ul> {% endif %}

### Activer un essai d'origine

Ajutez la balise `<meta>` suivante dans la section `<head>` de chaque page qui utilise l'expérience d'essai d'origine:

```html
<meta name="amp-experiment-token" content="{copy your token here}">
```

Remarque: `"amp-experiment-token"` est la chaîne littérale, `"amp-experiment-token"`, et non le jeton lui-même (qui entre dans l'attribut du contenu), ni le nom du composant expérimental.
