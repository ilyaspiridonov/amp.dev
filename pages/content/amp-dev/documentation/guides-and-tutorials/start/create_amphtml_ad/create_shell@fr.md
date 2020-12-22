---
"$title": "Créer la coquille d'annonce"
"$order": '0'
description: 'À l''aide de votre éditeur de texte préféré, créez un fichier HTML nommé my-amphtml-ad.html. Copiez le balisage HTML suivant dans ce fichier: ...'
---

Le [code HTML requis pour une annonce AMPHTML](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md) est une variante du code [AMPHTML requis pour une page AMP](../../../../documentation/guides-and-tutorials/learn/spec/amphtml.md). Familiarisons-nous avec le code requis en créant la coquille de notre annonce AMPHTML.

À l'aide de votre éditeur de texte préféré, créez un fichier HTML nommé **`my-amphtml-ad.html`**. Copiez le balisage HTML suivant dans ce fichier:

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>My amphtml ad</title>
  <meta name="viewport" content="width=device-width">
</head>
<body>
</body>
</html>
```

Ce balisage concerne un fichier HTML de base valide. Notez que nous avons inclus la balise de fenêtre `meta` afin d'avoir une [fenêtre interactive](../../../../documentation/guides-and-tutorials/develop/style_and_layout/responsive_design.md#controlling-the-viewport).

Maintenant, modifions le code HTML pour en faire une annonce AMPHTML.

Dans la balise `<html>`, ajoutez l'attribut `⚡4ads`, qui identifie le document en tant qu'annonce AMPHTML. Vous pouvez également spécifier l'attribut `amp4ads`, qui est également valide.

```html
<!doctype html>
<html ⚡4ads>
<head>
...
```

[tip type="note"] **REMARQUE–**  Contrairement aux pages AMP, [les annonces AMPHTML ne nécessitent pas une balise `<link rel="canonical">`](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#amphtml-ad-format-rules). [/tip]

Les annonces AMPHTML nécessitent leur propre version du runtime AMP. Ajoutez donc la balise `<script>` suivante à la section `<head>` de votre document:

```html
<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>
```

Les créations publicitaires AMPHTML nécessitent un [modèle](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#boilerplate) de ligne de style différent et considérablement plus simple que les pages AMP. Ajoutez le code suivant à votre section `<head>`:

```html
<style amp4ads-boilerplate>body{visibility:hidden}</style>
```

To style your AMPHTML ad, your CSS must be embedded inline in the AMPHTML document using <code><style amp-custom></style> </code>tags in the <code></code> section. As we're rendering a basic image ad, we don't require any CSS, so we won't add these tags.

[tip type="note"] **REMARQUE –** Pour les annonces AMPHTML, la taille maximale pour une feuille de style intégrée est de *20 ko*. Plus de détails sur les [exigences CSS dans les spécifications d'annonce AMPHTML](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#css). [/tip]

Voici le code complet de votre fichier HTML:

```html
<!doctype html>
<html ⚡4ads>
<head>
  <meta charset="utf-8">
  <title>My amphtml ad</title>
  <meta name="viewport" content="width=device-width">
  <script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>
  <style amp4ads-boilerplate>body{visibility:hidden}</style>
</head>
<body>
</body>
</html>
```

Vous avez maintenant une annonce AMPHTML valide, quoique plutôt vide. Créons l'annonce illustrée.
