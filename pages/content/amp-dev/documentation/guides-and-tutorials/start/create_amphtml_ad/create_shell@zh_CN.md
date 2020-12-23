---
"$title": 为广告创建外壳
"$order": '0'
description: 使用您常用的文本编辑器，创建一个名为 my-amphtml-ad.html 的 HTML 文件。将以下 HTML 标记复制到该文件中：…
---

[AMPHTML 广告所需的 HTML](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md) 是 [AMP 网页所需的 AMPHTML](../../../../documentation/guides-and-tutorials/learn/spec/amphtml.md) 的变体。让我们通过创建 AMPHTML 广告的外壳来熟悉所需的代码。

使用您常用的文本编辑器，创建一个名为 **`my-amphtml-ad.html`** 的 HTML 文件。将以下 HTML 标记复制到该文件中：

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

此标记适用于基本的有效 HTML 文件。请注意，我们添加了 `meta` 视口标记，因此将生成[自适应视口](../../../../documentation/guides-and-tutorials/develop/style_and_layout/responsive_design.md#controlling-the-viewport)。

现在，让我们修改 HTML，使其成为 AMPHTML 广告。

在 `<html>` 标记中添加 `⚡4ads` 属性，该属性可将文档标识为 AMPHTML 广告。或者，您也可以指定 `amp4ads` 属性，同样有效。

```html
<!doctype html>
<html ⚡4ads>
<head>
...
```

[tip type="note"] **注**：与 AMP 网页不同，[AMPHTML 广告不需要 `<link rel="canonical">` 标记](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#amphtml-ad-format-rules)。[/tip]

AMPHTML 广告需要其自己的 AMP 运行时版本，因此请在文档的 `<head>` 部分中添加以下 `<script>` 标记：

```html
<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>
```

AMPHTML 广告素材需要采用与 AMP 网页不同且更简单的[样板](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#boilerplate)样式行。请在您的 `<head>` 部分中添加以下代码：

```html
<style amp4ads-boilerplate>body{visibility:hidden}</style>
```

To style your AMPHTML ad, your CSS must be embedded inline in the AMPHTML document using <code><style amp-custom></style> </code>tags in the <code></code> section. As we're rendering a basic image ad, we don't require any CSS, so we won't add these tags.

[tip type="note"] **注**：对于 AMPHTML 广告，内嵌样式表的最大大小为 *20 KB*。详细了解 [AMPHTML 广告规范中的 CSS 要求](../../../../documentation/guides-and-tutorials/learn/a4a_spec.md#css)。[/tip]

以下为 HTML 文件的完整代码：

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

您已创建了有效的 AMPHTML 广告，尽管其中尚未提供丰富的内容。接下来，让我们创建图片广告。
