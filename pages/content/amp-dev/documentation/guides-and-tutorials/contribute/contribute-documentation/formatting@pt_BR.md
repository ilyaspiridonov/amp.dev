---
"$title": Guias e tutoriais de formatação
"$order": '3'
description: Requisitos de formatação de arquivos para o amp.dev
formats:
- websites
- stories
- ads
- email
author: CrystalOnScript
---

Guias e tutoriais são submetidos em [Markdown](https://www.markdownguide.org/), contendo formatação adicional para frontmatter e shortcode.

## Locais da documentação

O conteúdo em amp.dev é retirado de dois repositórios, [amp.dev](https://github.com/ampproject/amp.dev) e [AMPHTML](https://github.com/ampproject/amphtml). Toda a documentação de referência relacionada a componentes é retirada de AMPHTML, ou de builtins ou de extensões.

- [Componentes embutidos ](https://github.com/ampproject/amphtml/tree/master/builtins)
- [Componentes](https://github.com/ampproject/amphtml/tree/master/extensions)
- [Cursos](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/courses)
- [Exemplos](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/examples)
- [Guias e tutoriais](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/guides-and-tutorials)

Há vários outros documentos que são importados para amp.dev a partir do AMPHTML. Eles estão [listados neste arquivo](https://github.com/ampproject/amp.dev/blob/future/platform/config/imports/spec.json). Não atualize estes documentos no repositório amp.dev - suas alterações serão sobrescritas em builds subsequentes!

## Frontmatter

O Frontmatter existe na parte superior de cada guia e tutorial.

Exemplo:

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
   <td>O título do seu documento, tal como irá aparecer no índice. Coloque em maiúsculas a primeira letra da primeira palavra, exceto "AMP" e outros substantivos próprios. Use o símbolo '&' em vez da palavra `and`.</td>
  </tr>
  <tr>
   <td>
    <code>$order</code>
   </td>
   <td>Defina em que parte do índice o seu documento aparece. Você poderá ter que editar a ordem `$order` em outros documentos para que ela apareça na posição correta.</td>
  </tr>
  <tr>
   <td>
    <code>formats</code>
   </td>
   <td>Liste as experiências AMP para as quais o seu documento é relevante. Se seu documento era relevante para sites AMP e histórias AMP, mas não para anúncios AMP ou e-mails AMP, o seu frontmatter seria assim: ```yaml formats: - websites - stories ```</td>
  </tr>
  <tr>
   <td>
<code>author</code>
   </td>
   <td>O autor é você! Use seu nome de usuário do GitHub</td>
  </tr>
  <tr>
   <td>
<code>contributors</code>
   </td>
   <td>Liste qualquer um que tenha contribuído com seu documento. Este campo é opcional.</td>
  </tr>
  <tr>
   <td>
<code>description</code>
   </td>
   <td>Escreva uma breve descrição do seu guia ou tutorial Isto ajuda na otimização do motor de busca, permitindo que seu trabalho seja encontrado por quem precisa dele!</td>
  </tr>
  <tr>
   <td>
<code>tutorial</code>
   </td>
   <td>Adicione `tutorial: true` ao frontmatter para que o site inclua o ícone do tutorial ao lado dele. Tutoriais aparecem no índice na parte inferior da sua seção.</td>
  </tr>
</table>

# Shortcodes

Para uma lista de shortcodes e seus usos, por favor consulte [documentation.md no GitHub](https://github.com/ampproject/amp.dev/blob/future/contributing/documentation.md#shortcodes).

## Imagens

amp.dev é construído com AMP! Portanto, nossas imagens devem corresponder aos critérios do [`amp-img`](../../../../documentation/components/reference/amp-img.md). O processo de build utiliza a seguinte sintaxe para converter imagens para o formato `amp-img` apropriado.

<div class="ap-m-code-snippet"><pre>{{ image('/static/img/docs/tutorials/custom-javascript-tutorial/image1.jpg', 500, 369, layout='intrinsic', alt='Image of basic amp script tutorial starter app') }}</pre></div>

## Filtragem de seções

Alguns documentos podem ser relevantes para múltiplos formatos AMP, mas certos formatos podem necessitar de explicações ou informações adicionais que não são relevantes para outros. Você pode filtrar estas seções, empacotando-as com o seguinte shortcode.

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

## Dicas

Você pode adicionar dicas e chamadas empacotando o texto no seguinte shortcode:

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

## Fragmentos de código

Insira fragmentos de código entre conjuntos de três crases; indique a linguagem ao final do primeiro conjunto de crases.

<div class="ap-m-code-snippet"><pre>```html
  // code sample
```

```css
  // code sample
```

```js
  // code sample
```</pre></div>

Se o seu código contiver chaves duplas, o que é comum se você usa modelos [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), você precisará empacotar a parte do código:

<div class="ap-m-code-snippet"><pre>```html<br>{% raw	%}<br>  // code with double curly braces<br>{% endraw	%}<br>```</pre></div>

### Fragmentos de código em listas

O Python-Markdown possui algumas limitações. Use a seguinte sintaxe ao incluir fragmentos de código em listas:

<div class="ap-m-code-snippet"><pre>&lsqb;sourcecode:html]
      <html>
        <p>Indented content.</p>
      </html>
    &lsqb;/sourcecode]</pre></div>

## Pré-visualização de amostras de código

Amostras de código podem ter uma pré-visualização e/ou link para uma versão que executa no [Playground AMP](https://playground.amp.dev/).

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

Observação: a pré-visualização será automaticamente transformada no formato selecionado ao ser aberta no playground🤯!

Use o atributo `preview` para definir como a pré-visualização é gerada:

- **none**: Nenhuma pré-visualização será gerada

- **inline**: A pré-visualização de exemplo é mostrada acima do código-fonte. Uma pré-visualização inline só é possível para exemplos de sites comuns se o código não contiver quaisquer elementos `head` Use esta opção para pequenos exemplos que não precisam de quaisquer estilos ou outros elementos `head` (importações não contam, já que são especificadas através do atributo `imports`).

- **top-frame**: a pré-visualização do exemplo é mostrada acima do código-fonte dentro de um iframe. A orientação pode ser alternada entre os modos `portrait` e `landscape`. Você poderá pré-selecionar a orientação ao especificar o atributo adicional:

- **orientation**: `default: landscape|portrait`

Se forem necessários elementos personalizados, especifique-os no atributo `imports` como uma lista separada por vírgulas com o nome do componente seguido por um sinal de dois-pontos e a versão. Se o seu código usar [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), indique a dependência no atributo `template`.

Para conteúdo de e-mail com links para recursos, use o placeholder <code>{{server_for_email}}</code> na fonte.

### Amostra inline

Aqui está uma amostra inline simples. Você pode definir CSS através de estilos inline:

<div class="ap-m-code-snippet"><pre>[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]</pre></div>

Veja um exemplo:

[example preview="inline" playground="true"]
    ```html
    <div style="background: red; width: 200px; height: 200px;">Hello World</div>
    ```
  [/example]

Importante: as amostras inline são incorporadas diretamente na página. Isto pode levar a conflitos se os componentes já estiverem sendo usados na página (por exemplo, `amp-consent`).

### Pré-visualização do frame superior

Use a pré-visualização do frame superior sempre que você precisar especificar elementos do cabeçalho ou definir estilos globais dentro de  `<style amp-custom>`.

Importante: não adicione nenhum código boilerplate do AMP ao cabeçalho, já que ele será adicionado automaticamente com base no formato AMP. Apenas adicione ao cabeçalho elementos que sejam necessários para a amostra!

<div class="ap-m-code-snippet"><pre>  [example preview="top-frame"<br>         playground="true"]<br>    ```html<br>    <head><br>      <script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script><br>      <style amp-custom><br>        body {<br>          background: red;<br>        }<br>      </style><br>    </head><br>    <body><br>      <h1>Hello AMP</h1><br>      <amp-youtube width="480"<br>        height="270"<br>        layout="responsive"<br>        data-videoid="lBTCB7yLs8Y"><br>      </amp-youtube><br>    </body><br>    ```<br>  [/example]</pre></div>

Veja um exemplo:

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

### Histórias AMP

Use `preview="top-frame"` junto com `orientation="portrait"` para pré-visualizar histórias AMP.

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

Veja um exemplo:

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

### URLs absolutas para e-mail AMP

Observe como utilizamos <code>{{server_for_email}}</code> para tornar o URL do endpoint absoluta se ela estiver incorporada em um e-mail AMP.

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

Veja um exemplo:

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

### Escapando modelos mustache

Eis uma amostra `top-frame` usando um endpoint remoto. Os modelos mustache precisam ser escapados nas amostras usando <code>{% raw %}</code> e <code>{% endraw %}</code>:

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

Veja um exemplo:

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

## Links

Você pode incluir links para outras páginas com a sintaxe comum para links em markdown:

```md
 [link](../../../courses/beginning-course/index.md)
```

Ao vincular com outra página em amp.dev, a referência será um caminho de arquivos relativo ao arquivo destino.

### Âncoras

Links para seções específicas de um documento usando âncoras:

```md
[link to example section](#example-section)
```

Por favor, crie o alvo da âncora usando `<a name="#anchor-name></a>` antes de vincular a uma seção que não tenha uma âncora. Um bom lugar é no final do cabeçalho da seção:

```html
## Example section <a name="example-section"></a>
```

Você só deve usar letras, dígitos, traço e o sublinhado em uma âncora. Por favor, use nomes de âncora curtos em inglês que correspondam com o título ou que descrevam a seção. Garanta que o nome da âncora seja único dentro do documento.

Quando uma página é traduzida os nomes de âncora não devem ser alterados e devem permanecer em inglês.

Quando você cria uma âncora que será usada em um link que tem origem em outra página, você também deve criar a mesma âncora em todas as traduções.

### Filtro de formato AMP

Documentação de componentes, guias, tutoriais e exemplos são filtráveis através do formato AMP, tais como sites AMP ou histórias AMP. Ao criar um link para uma página assim você deve especificar explicitamente um formato, que seja suportado pelo alvo, anexando ao link o parâmetro de formato:

```md
 [link](../../learn/amp-actions-and-events.md?format=websites)
```

Apenas quando você tiver certeza que o alvo suporta **todos** os formatos que sua página suporta você poderá omitir o parâmetro.

### Referências dos componentes

Links para uma documentação de referência de um componente apontarão automaticamente para a versão mais recente se seu link omitir a parte da versão. Quando voce quiser indicar a versão explicitamente, especifique o nome completo:

```md
 [latest version](../../../components/reference/amp-carousel.md?format=websites)
 [explicit version](../../../components/reference/amp-carousel-v0.2.md?format=websites)
```

## Estrutura do documento

### Títulos, cabeçalhos e subcabeçalhos

A primeira letra da primeira palavra em títulos, cabeçalhos e subcabeçalhos é posta em maiúsculas, o que segue fica em minúsculas. As expectativas incluem AMP e outros substantivos próprios. Nenhum cabeçalho tem `Introduction` como título. Introduções seguem o título do documento.

### Nomenclatura de documentos

Use a convenção de nomenclatura com traço para dar nomes aos documentos

<table>
  <tr>
   <td>
<strong>Faça</strong>
   </td>
   <td>
<strong>Não faça</strong>
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
