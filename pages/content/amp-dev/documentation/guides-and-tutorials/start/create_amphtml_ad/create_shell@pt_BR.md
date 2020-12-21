---
"$title": Crie a estrutura do anúncio
"$order": '0'
description: 'Usando seu editor de texto favorito, crie um arquivo HTML chamado my-amphtml-ad.html. Copie a seguinte marcação HTML para esse arquivo: ...'
---

O [HTML necessário para um anúncio AMPHTML](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md) is é uma variante do [AMPHTML necessário para uma página AMP](../../../../documentation/guides-and-tutorials/learn/spec/amphtml.md). Vamos nos familiarizar com o código necessário criando a estrutura de nosso anúncio HTML para AMP.

Usando seu editor de texto favorito, crie um arquivo HTML chamado **`my-amphtml-ad.html`**. Copie a seguinte marcação HTML para esse arquivo: ...

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

Esta marcação é para um arquivo HTML básico e válido. Observe que incluímos a tag `meta` de viewport para termos um [viewport responsivo](../../../../documentation/guides-and-tutorials/develop/style_and_layout/responsive_design.md#controlling-the-viewport).

Agora, vamos modificar o HTML para torná-lo um anúncio AMPHTML.

Na tag `<html> `,  acrescente o atributo `⚡4ads`, que identifica o documento como um anúncio AMPHTML. Alternativamente, você pode especificar o atributo `amp4ads`, que também é válido.

```html
<!doctype html>
<html ⚡4ads>
<head>
...
```

[tip type="note"] **OBSERVAÇÃO –**  Diferentemente das páginas AMP, [os anúncios AMPHTML não requerem uma tag `<link rel="canonical">`](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#amphtml-ad-format-rules). [/tip]

Anúncios AMPHTML requerem sua própria versão do runtime AMP, portanto, adicione a seguinte tag `<script>` à seção `<head>` do seu documento:

```html
<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>
```

Os criativos de anúncios AMPHTML requerem um estilo de [boilerplate](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#boilerplate) diferente e consideravelmente mais simples do que as páginas AMP. Acrescente o código a seguir à sua seção `<head>`:

```html
<style amp4ads-boilerplate>body{visibility:hidden}</style>
```

To style your AMPHTML ad, your CSS must be embedded inline in the AMPHTML document using <code><style amp-custom></style> </code>tags in the <code></code> section. As we're rendering a basic image ad, we don't require any CSS, so we won't add these tags.

[tip type="note"] **OBSERVAÇÃO –** Para anúncios AMPHTML, o tamanho máximo para uma folha de estilo inline é *20 kilobytes*. Saiba mais sobre [requisitos de CSS na especificação de anúncios AMPHTML](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#css). [/tip]

Este é o código completo para seu arquivo HTML:

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

Agora você tem um anúncio AMPHTML válido, embora um pouco vazio. Vamos criar o anúncio gráfico.
