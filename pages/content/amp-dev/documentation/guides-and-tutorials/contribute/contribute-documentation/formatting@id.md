---
"$title": "Tutorial & panduan pembuatan format"
"$order": '3'
description: Persyaratan pembuatan format berkas untuk amp.dev
formats:
- websites
- stories
- ads
- email
author: CrystalOnScript
---

Panduan dan tutorial diserahkan di [Markdown](https://www.markdownguide.org/), dengan pengaturan format kode pendek dan bagian awal tambahan.

## Lokasi dokumentasi

Konten mengenai amp.dev diambil dari dua repositori, [amp.dev](https://github.com/ampproject/amp.dev) dan [AMPHTML](https://github.com/ampproject/amphtml). Semua dokumentasi referensi di bawah komponen diambil dari AMPHTML, baik dari bawaan maupun ekstensi.

- [Komponen bawaan ](https://github.com/ampproject/amphtml/tree/master/builtins)
- [Komponen](https://github.com/ampproject/amphtml/tree/master/extensions)
- [Kursus](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/courses)
- [Contoh](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/examples)
- [Panduan & tutorial](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/guides-and-tutorials)

Ada beberapa dokumen lain yang diimpor ke amp.dev dari AMPHTML. Dokumen-dokumen tersebut [tercantum di dalam berkas ini](https://github.com/ampproject/amp.dev/blob/future/platform/config/imports/spec.json). Jangan perbarui dokumen-dokumen ini di repositori amp.dev – perubahan yang Anda buat akan ditimpa oleh buatan (build) selanjutnya!

## Bagian Awal

Bagian Awal ada di bagian atas setiap panduan dan tutorial.

Contoh:

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
   <td>Judul dokumen Anda sebagaimana akan tampil pada daftar isi. Gunakan huruf besar untuk huruf pertama kata, kecuali untuk “AMP” dan kata benda nama diri. Gunakan simbol `&`, sebagai pengganti kata `dan`.</td>
  </tr>
  <tr>
   <td>
    <code>$order</code>
   </td>
   <td>Tentukan tempat kemunculan daftar isi dokumen Anda. Anda mungkin perlu mengedit `$order` di dokumen lain agar muncul di posisi yang tepat.</td>
  </tr>
  <tr>
   <td>
    <code>formats</code>
   </td>
   <td>Cantumkan pengalaman AMP yang relevan dengan dokumen Anda. Jika dokumen Anda relevan dengan situs web AMP dan cerita AMP, tetapi tidak relevan dengan iklan AMP atau email AMPI, bagian awal Anda akan seperti yang berikut ini:     ```format yaml:           - situs web           - cerita     ```</td>
  </tr>
  <tr>
   <td>
<code>author</code>
   </td>
   <td>Penulisnya adalah Anda! Gunakan nama pengguna Anda di GitHub.</td>
  </tr>
  <tr>
   <td>
<code>contributors</code>
   </td>
   <td>Cantumkan siapa saja yang telah berkontribusi untuk dokumen Anda. Bidang ini tidak wajib diisi.</td>
  </tr>
  <tr>
   <td>
<code>description</code>
   </td>
   <td>Tuliskan deskripsi singkat tentang panduan atau tutorial Anda. Ini membantu dalam pengoptimalan mesin pencarian, mengantarkan karya Anda ke tangan mereka yang membutuhkannya!</td>
  </tr>
  <tr>
   <td>
<code>tutorial</code>
   </td>
   <td>Tambahkan `tutorial: true` ke bagian awal untuk situs web tersebut untuk menambahkan ikon tutorial di sebelahnya. Tutorial ada di bagian bawah pada posisinya di daftar isi.</td>
  </tr>
</table>

# Kode Pendek

Untuk mengetahui daftar kode singkat dan penggunaannya, silakan lihat [documentation.md di GitHub](https://github.com/ampproject/amp.dev/blob/future/contributing/documentation.md#shortcodes).

## Gambar

amp.dev dibangun dengan AMP! Oleh karena itu, gambar kita harus sesuai dengan kriteria [`amp-img`](../../../../documentation/components/reference/amp-img.md). Proses pembuatan menggunakan sintaks berikut ini untuk mengubah gambar menjadi format `amp-img` yang tepat.

<div class="ap-m-code-snippet"><pre>{{ image('/static/img/docs/tutorials/custom-javascript-tutorial/image1.jpg', 500, 369, layout='intrinsic', alt='Image of basic amp script tutorial starter app') }}</pre></div>

## Bagian pemfilteran

Beberapa dokumen mungkin relevan untuk sejumlah format AMP, namun format-format tertentu mungkin memerlukan penjelasan atau informasi lebih lanjut yang tidak relevan dengan yang lainnya. Anda dapat memfilter bagian-bagian ini dengan membungkusnya di dalam kode pendek berikut ini.

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

## Tip

Anda dapat menambahkan kiat dan informasi (callout) dengan membungkus teks di dalam kode pendek berikut ini:

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

## Snippet kode

Tempatkan snippet kode di dalam backtick tiga serangkai, tentukan bahasa pada akhir rangkaian backtick pertama.

<div class="ap-m-code-snippet"><pre>```html
  // code sample
```

```css
  // code sample
```

```js
  // code sample
```</pre></div>

Jika kode Anda mengandung kurung kurawal ganda, ini biasanya ada jika Anda menggunakan templat [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), Anda harus membungkus bagian kode:

<div class="ap-m-code-snippet"><pre>```html
{% raw	%}
  // code with double curly braces
{% endraw	%}
```</pre></div>

### Snippet kode di dalam daftar

Python-Markdown memiliki sejumlah keterbatasan. Gunakan sintaks berikut ini saat menyertakan snippet kode di dalam daftar:

<div class="ap-m-code-snippet"><pre>&lsqb;sourcecode:html]
      <html>
        <p>Indented content.</p>
      </html>
    &lsqb;/sourcecode]</pre></div>

## Pratinjau sampel kode

Sampel kode dapat memiliki pratinjau dan/atau tautan ke versi [AMP Playground](https://playground.amp.dev/).

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

Catatan: Pratinjau secara otomatis akan diubah ke format yang telah dipilih saat ini saat membukanya di playground 🤯!

Gunakan atribut `preview` untuk menentukan cara pratinjau dibuat:

- **none** (tidak ada): Pratinjau tidak akan dibuat

- **inline** (di dalam baris): Pratinjau contoh akan ditampilkan di atas kode sumber. Pratinjau inline hanya mungkin untuk contoh situs web normal jika kodenya tidak mengandung elemen `head` (wadah metadata). Gunakan pilihan ini untuk contoh-contoh kecil yang tidak memerlukan gaya apa pun atau elemen `head` lain (yang diimpor tidak termasuk karena ditentukan melalui atribut `imports`).

- **top-frame** (bingkai atas): Pratinjau contoh ditampilkan di atas kode sumber di dalam sebuah iframe. Orientasinya dapat dialihkan antara mode `portrait` (potret) dan `landscape` (lanskap). Anda dapat menetapkan pilihan oritentasi sebelumnya dengan menentukan atribut tambahan:

- **orientation** (orientasi): `default: landscape|portrait`

Jika elemen kustom diperlukan, tentukan elemen-elemen tersebut di dalam atribut `imports` sebagai daftar yang dipisahkan dengan koma bersama nama komponen yang diikuti oleh tanda titik dua dan versinya. Jika kode Anda menggunakan [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites), tentukan ketergantungannya (dependensi) pada atribut `template`.

Untuk konten email dengan tautan sumber daya, gunakan placeholder <code>{{server_for_email}}</code> di sumber.

### Sampel Inline

Berikut ini adalah sampel inline sederhana yang disematkan. Anda dapat menentukan CSS melalui gaya inline:

<div class="ap-m-code-snippet"><pre>[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]</pre></div>

Penampilannya seperti yang berikut ini:

[example preview="inline" playground="true"]
    ```html
    <div style="background: red; width: 200px; height: 200px;">Hello World</div>
    ```
  [/example]

Peringatan: sampel inline disematkan secara langsung ke dalam halaman. Hal ini dapat menimbulkan konflik jika komponsen telah digunakan di halaman tersebut (cth.: `amp-consent`).

### Pratinjau Bingkai Atas

Gunakan pratinjau bingkai atas kapan pun Anda perlu menentukan elemen tajuk (header) atau menentukan gaya global di dalam `<style amp-custom>`.

Penting: Jangan tambahkan kode boilerplate (standar) AMP apa pun ke tajuk karena ini akan ditambahkan secara otomatis, berdasarkan format AMP. Tambahkan elemen yang dibutuhkan saja sesuai sampel ke head!

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

Penampilannya seperti yang berikut ini:

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

### Cerita AMP

Gunakan `preview="top-frame"` bersama `orientation="portrait"` untuk melihat pratinjau Cerita AMP.

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

Penampilannya seperti yang berikut ini:

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

### URL Mutlak untuk Email AMP

Perhatikan cara kita menggunakan <code>{{server_for_email}}</code> untuk menjadikan endpoint (titik akhir) URL mutlak jika disematkan di dalam email AMP.

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

Penampilannya seperti yang berikut ini:

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

### Keluar dari templat mustache

Berikut ini adalah sampel `top-frame` dengan menggunakan endpoint jarak jauh. Perlu keluar dari templat mustache di dalam sampel dengan menggunakan <code>{% raw %}</code> dan <code>{% endraw %}</code>:

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

Penampilannya seperti yang berikut ini:

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

## Tautan

Anda dapat menautkan ke halaman-halaman lain dengan sintaks tautan Markdown standar:

```md
 [link](../../../courses/beginning-course/index.md)
```

Saat menautkan ke halaman lain di amp.dev, referensi akan menjadi filepath (jalur berkas) relatif ke berkas target.

### Jangkar

Tautkan ke bagian tertentu di dalam dokumen dengan menggunakan jangkar:

```md
[link to example section](#example-section)
```

Silakan buat jangkar pada target dengan menggunakan `<a name="#anchor-name></a>` sebelum menautkan ke bagian yang tidak mempunyai jangkar. Tempat yang bagus adalah pada bagian akhir baris tajuk bagian tersebut:

```html
## Example section <a name="example-section"></a>

```

Anda hanya boleh menggunakan huruf, tanda pisah, dan underscore (garis bawah) pada jangkar. Silakan gunakan nama jangkar yang singkat dalam bahasa Inggris yang sesuai dengan baris tajuk, atau uraikan bagian tersebut. Pastikan bahwa nama jangkar itu khas di dalam dokumen tersebut.

Saat sebuah Halaman diterjemahkan, nama jangkar tidak boleh berubah dan tetap dalam bahasa Inggris.

Jika Anda membuat sebuah jangkar yang akan digunakan pada sebuah tautan dari halaman lain, Anda juga harus membuat jangkar yang sama di semua terjemahan.

### Filter format AMP

Panduan, tutorial, contoh, dan dokumentasi komponen dapat difilter dengan format AMP, seperti situs web AMP atau cerita AMP. Saat menautkan ke halaman yang demikian, Anda harus secara jelas memilih suatu format, yang didukung oleh target, dengan melampirkan parameter format ke tautan:

```md
 [link](../../learn/amp-actions-and-events.md?format=websites)
```

Anda hanya boleh meniadakan parameter ini jika Anda yakin bahwa target tersebut mendukung **semua** format yang didukung oleh halaman Anda.

### Referensi komponen

Sebuah tautan ke dokumentasi referensi komponen secara otomatis akan mengarah ke versi terbaru jika tautan Anda meniadakan bagian versinya. Jika Anda ingin secara tegas mengarahkan ke suatu versi, sediakan nama lengkapnya:

```md
 [latest version](../../../components/reference/amp-carousel.md?format=websites)
 [explicit version](../../../components/reference/amp-carousel-v0.2.md?format=websites)
```

## Struktur Dokumen

### Judul, tajuk, dan subtajuk

Huruf pertama kata pada judul, tajuk, dan subtajuk menggunakan huruf besar, lalu diikuti huruf kecil. Pengecualian termasuk AMP dan kata benda nama diri lainnya. Tidak ada tajuk yang berjudul `Introduction` (Pengantar), pengantar mengikuti judul dokumen.

### Penamaan dokumen

Nama dokumen menggunakan garis pisah dalam penamaan seperti biasanya.

<table>
  <tr>
   <td>
<strong>Lakukan</strong>
   </td>
   <td>
<strong>Jangan Lakukan</strong>
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
