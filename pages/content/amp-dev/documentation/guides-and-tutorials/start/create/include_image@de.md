---
"$title": Füge ein Bild hinzu
"$order": '2'
description: Die meisten HTML Tags können direkt in AMP HTML verwendet werden, aber bestimmte Tags wie z. B. <img> werden durch äquivalente oder leicht verbesserte …
author: pbakaus
contributors:
- bpaduch
---

Die meisten HTML Tags können direkt in AMP HTML verwendet werden, aber bestimmte Tags wie z. B. `<img>` werden durch äquivalente oder leicht verbesserte benutzerdefinierte AMP HTML Tags ersetzt (einige problematische Tags sind vollständig untersagt, siehe [HTML Tags in der Spezifikation](../../../../documentation/guides-and-tutorials/learn/spec/amphtml.md#html-tags)).

Um zu demonstrieren, wie zusätzliches Markup aussehen könnte, zeigen wir hier den Code, mit dem ein Bild in eine Seite eingebettet wird:

[sourcecode:html]
<amp-img src="welcome.jpg" alt="Welcome" height="400" width="800"></amp-img>
[/sourcecode]

[tip type="read-on"] <strong>ERFAHRE MEHR:</strong> Um zu erfahren, warum wir Tags wie <code><img></code> durch <a><code><amp-img></code></a> ersetzen und wie viele solcher Tags verfügbar sind, sieh dir den Link <a>Füge Bilder & Video hinzu</a> an. [/tip]
