---
"$title": مواصفات AMP HTML
order: '8'
formats:
- مواقع الويب
teaser:
  text: تُعد AMP HTML مجموعة فرعية من HTML لتصميم صفحات المحتوى مثل المقالات الإخبارية بطريقة تضمن خصائص أداء أساسية معينة.
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-html-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

تُعد AMP HTML مجموعة فرعية من HTML لتصميم صفحات المحتوى مثل المقالات الإخبارية بطريقة تضمن خصائص أداء أساسية معينة.

ونظرًا لكونها مجموعة فرعية من HTML، فإنها تضع بعض القيود على المجموعة الكاملة من العلامات والوظائف المتاحة من خلال HTML، ولكنها لا تتطلب تطوير محركات عرض جديدة: يمكن لوكلاء المستخدم الحاليين عرض AMP HTML تمامًا مثل كل HTML الأخرى.

[tip type="read-on"] إذا كنت مهتمًا اهتمامًا أساسيًا بما هو مسموح به في AMP وما هو غير مسموح به، فيمكنك مشاهدة [الفيديو التمهيدي حول قيود AMP](https://www.youtube.com/watch?v=Gv8A4CktajQ). [/tip]

إلى جانب إمكانية تحميل مستندات AMP HTML إلى خادم ويب وعرضها تمامًا مثل أي مستند HTML آخر؛ لا يلزم تكوين خاص للخادم. ومع ذلك، فقد تم تصميمها أيضًا ليتم عرضها اختياريًا من خلال أنظمة عرض AMP المتخصصة التي تعد وكيلًا لمستندات AMP. إذ تعمل هذه المستندات على عرضها من الأصل الخاص بها كما يُسمَح لها بتطبيق عمليات التحويل على المستند الذي يوفر مزايا أداء إضافية. وإليك قائمة غير كاملة من التحسينات التي قد يقوم بها نظام العرض:

- استبدال مراجع الصور بصور بحجم منفذ العرض الخاص بالعارض.
- تضمين الصور التي تظهر في جزء الطي من الصفحة.
- تضمينات متغيرات CSS.
- تحميل مسبق للمكونات الموسَّعة.
- تقليص HTML وCSS.

تستخدم AMP HTML مجموعة من العناصر المخصصة التي تمت المساهمة بها ولكن تتم إدارتها واستضافتها مركزيًا لتنفيذ وظيفية متقدمة مثل معارض الصور التي يمكن العثور عليها في مستند AMP HTML. وعلى الرغم من أنه يسمح بتصميم المستند باستخدام CSS مخصص، فإنه لا يسمح لـ JavaScript المكتوبة بواسطة المؤلف بخلاف ما يتم توفيره من خلال العناصر المخصصة للوصول إلى أهداف الأداء الخاصة به.

وباستخدام تنسيق AMP، يجعل منتجو المحتوى ذلك المحتوى الموجود في ملفات AMP متاحًا للتسجيل في الفهرس (يخضع لقيود robots.txt) وتخزينه مؤقتًا وعرضه عن طريق جهات خارجية.

## الأداء <a name="performance"></a>

الأداء المتوقع هو أحد أهداف التصميم الرئيسية لصفحات AMP HTML. وإننا نهدف في المقام الأول إلى تقليل الوقت حتى يتمكن المستخدم من استهلاك/استخدام محتوى الصفحة. وهذا يعني على نحو ملموس أنه:

- يجب تصغير طلبات HTTP اللازمة لعرض المستند وتخطيطه بالكامل.
- يجب تنزيل الموارد مثل الصور أو الإعلانات فقط إذا كان من المحتمل أن يراها المستخدم.
- يجب أن تكون المتصفحات قادرة على حساب المساحة التي يحتاجها كل مورد على الصفحة دون إحضاره.

## تنسيق AMP HTML <a name="the-amp-html-format"></a>

### نموذج مستند<a name="sample-document"></a>

[sourcecode:html]
<!DOCTYPE html>
<html ⚡>
  <head>
    <meta charset="utf-8" />
    <title>Sample document</title>
    <link rel="canonical" href="./regular-html-version.html" />
    <meta
      name="viewport"
      content="width=device-width,minimum-scale=1,initial-scale=1"
    />
    <style amp-custom>
      h1 {
        color: red;
      }
    </style>
    <script type="application/ld+json">
      {
        "@context": "http://schema.org",
        "@type": "NewsArticle",
        "headline": "Article headline",
        "image": ["thumbnail1.jpg"],
        "datePublished": "2015-02-05T08:00:00+08:00"
      }
    </script>
    <script
      async
      custom-element="amp-carousel"
      src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
    ></script>
    <script
      async
      custom-element="amp-ad"
      src="https://cdn.ampproject.org/v0/amp-ad-0.1.js"
    ></script>
    <style amp-boilerplate>
      body {
        -webkit-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -moz-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -ms-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        animation: -amp-start 8s steps(1, end) 0s 1 normal both;
      }
      @-webkit-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-moz-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-ms-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-o-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
    </style>
    <noscript
      ><style amp-boilerplate>
        body {
          -webkit-animation: none;
          -moz-animation: none;
          -ms-animation: none;
          animation: none;
        }
      </style></noscript
    >
    <script async src="https://cdn.ampproject.org/v0.js"></script>
  </head>
  <body>
    <h1>Sample document</h1>
    <p>
      Some text
      <amp-img src="sample.jpg" width="300" height="300"></amp-img>
    </p>
    <amp-ad
      width="300"
      height="250"
      type="a9"
      data-aax_size="300x250"
      data-aax_pubname="test123"
      data-aax_src="302"
    >
    </amp-ad>
  </body>
</html>
[/sourcecode]

### لغة الترميز المطلوبة <a name="required-markup"></a>

يشترط في مستندات AMP HTML

- <a name="dctp"></a>تبدأ بنوع مستند `<!doctype html>`. [🔗](#dctp)
- يحتوي على علامة المستوى الأعلى
    <span><code data-md-type="codespan"><html ⚡></code></span> (تكون
    <span><code><html amp></code></span> مقبولة كذلك).
- <a name="crps"></a>تحتوي على علامات `<head>` و`<body>` (وهي اختيارية في HTML). [🔗](#crps)
- <a name="canon"></a>تحتوي على علامة `<link rel="canonical" href="$SOME_URL">` داخل رؤوسها والتي تشير إلى إصدار HTML العادي من مستند AMP HTML أو إلى نفسها إذا لا يوجد إصدار HTML من هذا القبيل. [🔗](#canon)
- <a name="chrs"></a>تحتوي على علامة `<meta charset="utf-8">` كفرع أول لعلامة الرأس الخاصة بها. [🔗](#chrs)
- <a name="vprt"></a>تحتوي على علامة `<meta name="viewport" content="width=device-width">` داخل علامة رأسها. ويوصى أيضًا بتضمين`minimum-scale=1` و`initial-scale=1`. [🔗](#vprt)
- <a name="scrpt"></a>تحتوي على علامة `<script async src="https://cdn.ampproject.org/v0.js"></script>` داخل علامة الرأس الخاصة بها. [🔗](#scrpt)
- <a name="boilerplate"></a>تحتوي [ على رمز فقرة AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md)  (`head > style[amp-boilerplate]` و`noscript > style[amp-boilerplate]` في علامة رؤوسهم. [🔗](#boilerplate)

### بيانات التعريف<a name="metadata"></a>

يُنصح بأن تكون مستندات AMP HTML مرفقة ببيانات التعريف الموحدة: [بروتوكول الرسوم البيانية المفتوحة](http://ogp.me/)، [بطاقات Twitter](https://dev.twitter.com/cards/overview)، وغيرهما.

ونوصي أيضًا بترميز مستندات AMP HTML باستخدام [schema.org/CreativeWork](https://schema.org/CreativeWork) أو أي من أنواعها الأكثر تحديدًا مثل [schema.org/NewsArticle](https://schema.org/NewsArticle) أو [schema.org/BlogPosting](https://schema.org/BlogPosting).

### علامات HTML <a name="html-tags"></a>

يمكن استخدام علامات HTML بدون تغيير في AMP HTML. إذ تحتوي علامات معينة على علامات مخصصة مكافئة (مثل `<img>` و`<amp-img>`)  وغيرهما من العلامات المحظورة صراحة:

<table>
  <tr>
    <th width="30%">العلامة</th>
    <th>الحالة في AMP HTML</th>
  </tr>
  <tr>
    <td width="30%">script</td>
    <td>محظورة ما لم يكن النوع عبارة عن <code>application/ld+json</code>، أو  <code>application/json</code>، أو  <code>text/plain</code>، (يمكن إضافة قيم أخرى غير قابلة للتنفيذ حسب الحاجة). أما الاستثناء عبارة عن علامة النص البرمجي الإلزامي لتحميل وقت تشغيل AMP وعلامات النص البرمجي لتحميل المكونات الموسعة.</td>
  </tr>
  <tr>
    <td width="30%">noscript</td>
    <td>مسموحة، ويمكن استخدامها في أي مكان في المستند. إذا تم تحديد ذلك، فسيتم عرض المحتوى الموجود داخل العنصر <code><noscript></code> إذا قام المستخدم بتعطيل JavaScript.</td>
  </tr>
  <tr>
    <td width="30%">base</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">img</td>
    <td>استبدلت بـ <code>amp-img</code>.<br> يرجى ملاحظة: أن <code><img></code> عبارة عن <a href="https://www.w3.org/TR/html5/syntax.html#void-elements">عنصر خالٍ وفقًا لـ HTML5</a>، لذا لا يحتوي على علامة النهاية. ومع ذلك، فإن <code><amp-img></code> بها علامة نهاية <code></amp-img></code>.</td>
  </tr>
    <tr>
    <td width="30%">picture</td>
    <td>محظورة، إذ تعرض تنسيقات مختلفة للصور باستخدام السمة <a href="https://amp.dev/documentation/guides-and-tutorials/develop/style_and_layout/placeholders?format=websites">fallback</a> أو تقدم  <a href="https://amp.dev/documentation/components/amp-img#attributes"><code>srcset</code> عديدة في </a><code><amp-img></code>.</td>
  </tr>
  <tr>
    <td width="30%">video</td>
    <td>استبدلت بـ <code>amp-video</code>.</td>
  </tr>
  <tr>
    <td width="30%">audio</td>
    <td>استبدلت بـ <code>amp-audio</code>.</td>
  </tr>
  <tr>
    <td width="30%">iframe</td>
    <td>استبدلت بـ <code>amp-iframe</code>.</td>
  </tr>
    <tr>
    <td width="30%">frame</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">frameset</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">object</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">param</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">applet</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">embed</td>
    <td>محظورة.</td>
  </tr>
  <tr>
    <td width="30%">form</td>
    <td>مسموح بها، تتطلب تضمين الملحق <a href="https://amp.dev/documentation/components/amp-form">amp-form</a>.</td>
  </tr>
  <tr>
    <td width="30%">عناصر الإدخال</td>
    <td>مسموح بها في مع <a href="https://amp.dev/documentation/components/amp-form#inputs-and-fields">استثناء لبعض أنواع الإدخال</a>، وهي <code><input type=button></code>، <code><button type=image></code> غير صالحة. ويُسمح أيضًا بالعلامات ذات الصلة: <code><fieldset></code>، <code><label></code>
</td>
  </tr>
  <tr>
    <td width="30%">button</td>
    <td>مسموح بها.</td>
  </tr>
  <tr>
    <td width="30%"><code><a name="cust"></a>style</code></td>
    <td>
<a href="#boilerplate">تتطلب علامة نمط لـ amp-boilerplate</a>. إذ يُسمح بعلامة نمط إضافية واحدة في علامة الرأس لغرض التصميم المخصص. ويجب أن تحتوي علامة النمط هذه على السمة <code>amp-custom</code>. <a href="#cust">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">link</td>
    <td>مسموح بالقيم <code>rel</code> المسجلة في <a href="http://microformats.org/wiki/existing-rel-values">microformats.org</a>. إذا كانت القيمة <code>rel</code> مفقودة من قائمتنا البيضاء، <a href="https://github.com/ampproject/amphtml/issues/new">فيرجى إرسال مشكلة بذلك</a>.  فيما لا يُسمح بالقيمة <code>stylesheet</code> والقيم الأخرى مثل <code>preconnect</code>، <code>prerender</code> و<code>prefetch</code> التي تحتوي على آثار جانبية في المتصفح. وهناك حالة خاصة لإحضار صفحات الأنماط من مزودي الخطوط المدرجين في القائمة البيضاء.</td>
  </tr>
  <tr>
    <td width="30%">meta</td>
    <td>يمكن استخدام السمة <code>http-equiv</code> لقيم معينة مسموح بها؛ راجع <a href="https://github.com/ampproject/amphtml/blob/master/validator/validator-main.protoascii">مواصفات مسؤول التحقق من صحة AMP</a> للحصول على التفاصيل.</td>
  </tr>
  <tr>
    <td width="30%"><code>a</code></td>
    <td>يجب ألا تبدأ قيمة السمة <code>href</code> بـ <code>javascript:</code>. وفي حالة التعيين، يجب أن تكون قيمة السمة<code>target</code> عبارة عن <code>_blank</code>. وخلاف ذلك مسموح به. <a href="#ancr">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">svg</td>
    <td>أغلب عناصر SVG مسموح بها.</td>
  </tr>
</table>

يجب أن تستخدم تطبيقات مسؤول التحقق من الصحة قائمة بيضاء بناءً على مواصفات HTML5 مع إزالة العلامات أعلاه. راجع [إضافة علامة AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-tag-addendum.md).

### التعليقات<a name="comments"></a>

غير مسموح بتعليقات HTML الشرطية.

### سمات HTML <a name="html-attributes"></a>

غير مسموح بأسماء السمات التي تبدأ بـ `on` (مثل `onclick` أو `onmouseover`) في AMP HTML. ويُسمح بالسمة بالاسم الحرفي `on` (من دون لاحقة).

غير مسموح بالسمات المتعلقة بـ XML، مثل xmlns وxml:lang وxml:base وxml:space في AMP HTML.

غير مسموح بسمات AMP الداخلية التي تبدأ بـ `i-amp-` في AMP HTML.

### الفئات<a name="classes"></a>

غير مسموح بأسماء فئات AMP الداخلية المسبوقة بـ `-amp-` و`i-amp-` في AMP HTML.

راجع [وثائق AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-css-classes.md) للتعريف على معنى أسماء الفئات المسبوقة بـ `amp-`. إذ يُسمح باستخدام هذه الفئات ويقصد بها السماح بتخصيص بعض ميزات وقت تشغيل AMP وملحقاته.

يُسمح بجميع أسماء الفئات المختلقة الأخرى في لغة ترميز AMP HTML.

### المعرِّفات <a name="ids"></a>

غير مسموح ببعض أسماء المعرِّفات في AMP HTML، مثل المعرفات المسبوقة بـ `-amp-` و`i-amp-` التي قد تتعارض مع معرِّفات AMP الداخلية.

راجع وثائق AMP للحصول على ملحقات معينة قبل استخدام معرِّفات`amp-` و`AMP` لتفادي التعارض مع الميزات التي توفرها هذه الملحقات، مثل `amp-access`.

واعرض القائمة الكاملة لأسماء المعرِّفات غير المسموح بها من خلال البحث عن `mandatory-id-attr` [هنا](https://github.com/ampproject/amphtml/blob/master/spec/../validator/validator-main.protoascii).

### الروابط<a name="links"></a>

مخطط `javascript:` غير مسموح به.

### صفحات الأنماط<a name="stylesheets"></a>

تأتي العلامات الدلالية الرئيسية وعناصر AMP المخصصة مع أنماط افتراضية لتسهيل عملية تصميم مستند سريع الاستجابة على نحو معقول. وقد يتم إضافة خيار إلغاء الاشتراك في الأنماط الافتراضية في المستقبل.

#### @-قواعد <a name="-rules"></a>

@-قواعد التالية مسموح بها في صفحات الأنماط:

`@font-face`, `@keyframes`, `@media`, `@page`, `@supports`.

لن يُسمَح بـ `@import`، قد يتم إضافة أخرى في المستقبل.

#### صفحات أنماط المؤلف <a name="author-stylesheets"></a>

يمكن للمؤلفين إضافة أنماط مخصصة إلى مستند باستخدام علامة `<style amp-custom>` واحدة في رأس المستند أو الأنماط المضمنة.

يُسمَح بقواعد `@keyframes` في `<style amp-custom>`. على الرغم من ذلك، إذا كان عددها كبيرًا جدًا، فيوصى بوضعها في علامة `<style amp-keyframes>` الإضافية، التي يجب أن تكون موجودة في نهاية مستند AMP. وللحصول على مزيد من التفاصيل، راجع قسم [صحفتا أنماط الإطارات الأساسية](#keyframes-stylesheet) من هذا المستند.

#### المحددات<a name="selectors"></a>

تنطبق القيود التالية على المحددات في صفحات أنماط المؤلف:

##### أسماء العلامات والفئات <a name="class-and-tag-names"></a>

قد لا تبدأ أسماء الفئات والمعرِّفات والعلامات والسمات في صفحات أنماط المؤلف بالسلسلتين  `-amp-` و`i-amp-`. حيث إنها محجوزة للاستخدام الداخلي في وقت تشغيل AMP. ويترتب على ذلك أن صفحة أنماط المستخدم قد لا تشير إلى محددات CSS لفئات  `-amp-` ومعرِّفات `i-amp-` وعلامات وسمات `i-amp-`. وليس الغرض أن يتم تخصيص هذه الفئات والمعرِّفات وأسماء العلامات/السمات عن طريق المؤلفين. على الرغم من ذلك، يمكن للمؤلفين تجاوز أنماط فئات وعلامات `amp-` لأي خصائص CSS لم يتم حظرها صراحةً حسب مواصفات هذه المكونات.

لمنع استخدام محددات السمات للتحايل على قيود اسم الفئة، لا يُسمح لمحددات CSS عمومًا بأن تحتوي على رموز وسلاسل تبدأ بـ `-amp-` and `i-amp-`.

#### مهم <a name="important"></a>

غير مسموح باستخدام المؤهل `!important`. فهذا مطلب ضروري لتمكين AMP من فرض ثوابت تحجيم العناصر.

#### الخصائص <a name="properties"></a>

يسمح AMP فقط بالتحولات والرسوم المتحركة للخصائص التي يمكن تسريعها بواسطة معالج الرسوميات في المتصفحات الشائعة. وإننا ندرج حاليًا في القائمة البيضاء: `opacity`, `transform` (وأيضًا `-vendorPrefix-transform`).

في المثال التالي، يلزم إدراج `<property>` في القائمة البيضاء أعلاه.

- `transition <property>` (وأيضًا -vendorPrefix-transition)
- `@keyframes name { from: {<property>: value} to {<property: value>} }` (وأيضًا `@-vendorPrefix-keyframes`)

#### الحد الأقصى للحجم <a name="maximum-size"></a>

يُعد خطأ تحقق من الصحة في حالة ما إذا كانت صفحة أنماط المؤلف أو الأنماط المضمنة معًا أكبر من 75000 بايت.

### صفحة أنماط الإطارات الأساسية <a name="keyframes-stylesheet"></a>

بالإضافة إلى `<style amp-custom>`، يمكن للمؤلفين أيضًا إضافة العلامة `<style amp-keyframes>`، المسموح بها تحديدًا للرسوم المتحركة للإطارات الرئيسية.

وتنطبق القيود التالية على العلامة `<style amp-keyframes>`:

1. يمكن وضعها فقط كآخر عنصر تابع للعنصر `<body>` الخاص بالمستند.
2. قد تحتوي فقط على القواعد `@keyframes`، `@media`، `@supports` والدمج بينها.
3. يجب ألا تكون أكبر من 500000 بايت.

يرجع سبب وجود العلامة `<style amp-keyframes>` إلى أن قواعد الإطارات الرئيسية غالبًا ما تكون ضخمة حتى بالنسبة إلى الرسوم المتحركة المركَّبة على نحو معتدل، التي تؤدي إلى بطء تحليل CSS والرسمة الأولى للمحتوى. ولكن غالبًا ما تتجاوز هذه القواعد حد الحجم المفروض على `<style amp-custom>`. ويسمح وضع إعلانات الإطارات الرئيسية هذه في أسفل المستند في `<style amp-keyframes>` بتجاوز قيود الحجم. ونظرًا لأن الإطارات الأساسية لا تحظر العرض، فإنها تتجنب أيضًا حظر أول رسومات المحتوى لتحليلها.

مثال:

[sourcecode:html]
<style amp-keyframes>
@keyframes anim1 {}

@media (min-width: 600px) {
  @keyframes anim1 {}
}
</style>
</body>
[/sourcecode]

### الخطوط المخصصة <a name="custom-fonts"></a>

قد يقوم المؤلفون بتضمين صفحات أنماط للخطوط المخصصة. وتكون الطريقتان المعتمدتان عبارة عن علامات الرابط التي تشير إلى مزودي الخطوط المدرجين في القائمة البيضاء وتضمين `@font-face`.

مثال:

[sourcecode:html]
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Tangerine"
/>
[/sourcecode]

يمكن إدراج مزودي الخطوط في القائمة البيضاء إذا كانوا يدعمون عمليات تكامل CSS فقط ويعملون عبر HTTPS. ويُسمح حاليًا للأصول التالية بعرض الخطوط عبر علامات الارتباط:

- Fonts.com: `https://fast.fonts.net`
- Google Fonts: `https://fonts.googleapis.com`
- Font Awesome: `https://maxcdn.bootstrapcdn.com, https://use.fontawesome.com`
- [Typekit](https://helpx.adobe.com/typekit/using/google-amp.html): `https://use.typekit.net/kitId.css` (replace `kitId` accordingly)

ملحوظة مسؤولي التنفيذ: تتطلب الإضافة إلى هذه القائمة تغيير قاعدة AMP Cache CSP.

للمؤلفين الحرية في تضمين كل الخطوط المخصصة عبر تعليمات CSS `@font-face` عبر CSS المخصص. ويجب إحضار الخطوط المضمنة عبر `@font-face` عبر المخطط HTTP أو HTTPS.

## وقت تشغيل AMP <a name="amp-runtime"></a>

يمكن إدراج مزودي الخطوط في القائمة البيضاء إذا كانوا يدعمون عمليات تكامل CSS فقط ويعملون عبر HTTPS. ويُسمح حاليًا للأصول التالية بعرض الخطوط عبر علامات الارتباط:

يتم تحميل وقت تشغيل AMP عبر علامة <code><script src="https://cdn.ampproject.org/v0.js"></script></code> الإلزامية في <code><head></code> لمستند AMP.

ويمكن وضع وقت تشغيل AMP في وضع التطوير لأي صفحة. حيث سيؤدي وضع التطوير إلى تشغيل التحقق من صحة AMP على الصفحة المضمنة، مما يؤدي إلى إرسال حالة التحقق وأي أخطاء إلى وحدة تحكم مطوري JavaScript. وقد يتم تشغيل وضع التطوير من خلال إلحاق `#development=1` بعنوان URL للصفحة.

## الموارد <a name="resources"></a>

يجب تضمين الموارد مثل الصور أو مقاطع الفيديو أو الملفات الصوتية أو الإعلانات في ملف AMP HTML من خلال عناصر مخصصة مثل `<amp-img>`. إذ نطلق عليها اسم "الموارد المُدارة" لأنه يتم تحديد ما إذا كان سيتم تحميلها وعرضها للمستخدم وكذا توقيت ذلك من خلال وقت تشغيل AMP.

ولا توجد ضمانات خاصة فيما يتعلق بسلوك التحميل لوقت تشغيل AMP، ولكن يجب أن يسعى عمومًا لتحميل الموارد بسرعة كافية، بحيث يتم تحميلها في الوقت الذي يرغب المستخدم في رؤيتها إن أمكن. ويجب أن يعطي وقت التشغيل الأولوية للموارد الموجودة حاليًا في منفذ العرض ومحاولة توقع التغييرات في منفذ العرض وتحميل الموارد مسبقًا وفقًا لذلك.

فيما قد يقرر وقت تشغيل AMP في أي وقت إلغاء تحميل الموارد غير الموجودة حاليًا في منفذ العرض أو إعادة استخدام حاويات الموارد مثل iframe لتقليل الاستهلاك الإجمالي لذاكرة الوصول العشوائي.

## مكونات AMP <a name="amp-components"></a>

يستخدم AMP HTML عناصر مخصصة تسمى "مكونات AMP" لاستبدال علامات تحميل الموارد المضمنة مثل `<img>` و`<video>` ولتنفيذ ميزات ذات تفاعلات مركَّبة مثل مكتبات الفيديو أو lightbox للصور.

راجع [مواصفات مكون AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-components.md) للحصول على مزيد من التفاصيل حول المكونات المدعومة.

هناك نوعان من مكونات AMP المدعومة:

1. مضمنة
2. موسعة

تتوفر المكونات المضمنة دائمًا في مستند AMP ولها عنصر مخصص خاص مثل `<amp-img>`. ويجب تضمين المكونات الموسعة بشكل صريح في المستند.

### السمات المشتركة <a name="common-attributes"></a>

#### `layout`, `width`, `height`, `media`, `placeholder`, `fallback` <a name="layout-width-height-media-placeholder-fallback"></a>

تحدد هذه السمات تخطيط العنصر، حيث إن الهدف الرئيسي هنا عبارة عن ضمان إمكانية عرض العنصر وحجز مساحته على نحو صحيح قبل تنزيل أي من JavaScript أو الموارد عن بُعد.

راجع [نظام تخطيط AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-layout.md) لمزيد من التفاصيل حول نظام التخطيط.

#### `on` <a name="on"></a>

تُستخدَم السمة `on` لتثبيت معالجات الأحداث في العناصر. إذ تعتمد الأحداث المدعومة على العناصر.

وتكون القيمة لبناء الجملة عبارة عن لغة نموذجية بسيطة خاصة بالمجال من النموذج:

[sourcecode:javascript]
eventName:targetId[.methodName[(arg1=value, arg2=value)]]
[/sourcecode]

مثال: `on="tap:fooId.showLightbox"`

إذا تم إغفال `methodName`، فسيتم تنفيذ الطريقة الافتراضية في حالة تحديدها للعنصر. مثال: `on="tap:fooId"`

قد تقبل بعض الإجراءات الوسيطة، إذا تم توثيقها. ويتم تحديد الوسيطات بين هلالين في ترقيم `key=value`. وتكون القيم المقبولة عبارة عن:

- سلاسل بسيطة من دون علامتي اقتباس: `simple-value`;
- سلاسل بعلامتي اقتباس: `"string value"`  أو `'string value'`;
- قيم ثنائية: `true` أو `false`;
- أرقام: `11` أو `1.1`.

يمكنك الاستماع إلى أحداث متعددة في عنصر واحد من خلال فصل الأحداث بفاصلة منقوطة `;`.

Example: `on="submit-success:lightbox1;submit-error:lightbox2"`

 [اقرأ المزيد حول أحداث وإجراءات AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-actions-and-events.md).

### مكونات موسَّعة <a name="extended-components"></a>

المكونات الموسعة عبارة عن مكونات لا يتم نقلها بالضرورة مع وقت تشغيل AMP. بدلاً من ذلك، يجب تضمينها بشكل صريح في المستند.

يتم تحميل المكونات الموسعة بتضمين علامة `<script>` في الرأس الخاص بالوثيقة مثل ما يلي:

[sourcecode:html]
<script
  async
  custom-element="amp-carousel"
  src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
></script>
[/sourcecode]

ويجب أن تتضمن علامة `<script>` السمة `async` كما يجب أن تتضمن السمة `custom-element` التي تشير إلى اسم العنصر.

قد تستخدم عمليات تنفيذ وقت التشغيل الاسم لتقديم العناصر النائبة لهذه العناصر.

ويجب أن يبدأ عنوان URL للبرنامج النصي بـ `https://cdn.ampproject.org`  ويجب أن يتبع نمطًا صارمًا مؤلفًا من <code>/v\d+/[a-z-]+-(latest|\d+|\d+\.\d+)\.js</code>.

##### عنوان URL <a name="url"></a>

يتخذ عنوان URL للمكونات الموسعة الشكل:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$ELEMENT_NAME-$ELEMENT_VERSION.js
[/sourcecode]

##### تعيين الإصدار <a name="versioning"></a>

[راجع سياسة تعيين إصدار AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-versioning-policy.md).

### القوالب <a name="templates"></a>

تعرض القوالب محتوى HTML بناءً على النموذج الخاص باللغة وبيانات JSON المقدمة.

راجع [مواصفات قوالب AMP](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md) للحصول على مزيد من التفاصيل حول القوالب المدعومة.

لا يتم نقل النماذج مع وقت تشغيل AMP ويجب تنزيلها تمامًا كما هو الحال مع العناصر الموسَّعة. إذ يتم تحميل المكونات الموسَّعة بتضمين علامة `<script>` في رأس المستند مثل ما يلي:

[sourcecode:html]
<script
  async
  custom-template="amp-mustache"
  src="https://cdn.ampproject.org/v0/amp-mustache-0.2.js"
></script>
[/sourcecode]

ويجب أن تتضمن العلامة `<script>` السمة `async` كما يجب أن تتضمن السمة `custom-template` التي تشير إلى نوع القالب. فيما يجب أن يبدأ عنوان URL للبرنامج النصي بـ `https://cdn.ampproject.org` ويجب أن يتبع نمطًا صارمًا مؤلفًا من `/v\d+/[a-z-]+-(latest|\d+|\d+\.\d+)\.js`.

ويتم الإعلان عن القوالب في المستند على النحو التالي:

[sourcecode:html]
<template type="amp-mustache" id="template1">
  Hello {% raw %}{{you}}{% endraw %}!
</template>
[/sourcecode]

فيما تكون السمة `type` مطلوبة ويجب أن تشير إلى برنامج نصي `custom-template` معلن.

وتكون السمة `id` اختيارية. إذ تكتشف عناصر AMP القوالب الخاصة بها. وتتضمن السيناريوهات النموذجية عنصر AMP الذي يبحث عن `<template>` إما بين العناصر الفرعية أو المشار إليه بواسطة المعرِّف.

ويعتمد بناء الجملة داخل عنصر القالب على لغة القالب المحددة. على الرغم من ذلك، يمكن تقييد لغة القالب داخل AMP. على سبيل المثال، وفقًا لعنصر "القالب"، يجب أن تكون كل عمليات الإنتاج عبارة عن نموذج كائن مستند صالح جيد التشكيل. إذ تخضع كل مخرجات القالب أيضًا إلى التصحاح لضمان إخراج AMP صالح.

ومن أجل التعرف على بناء الجملة والقيود الخاصة بقالب ما، يرجى زيارة [وثائق القوالب](https://github.com/ampproject/amphtml/blob/master/spec/./amp-html-templates.md#templates).

##### عنوان URL <a name="url-1"></a>

يتخذ عنوان URL للمكونات الموسعة الشكل:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$TEMPLATE_TYPE-$TEMPLATE_VERSION.js
[/sourcecode]

##### تعيين الإصدار <a name="versioning-1"></a>

راجع تعيين إصدارات العناصر المخصصة لمزيد من التفاصيل.

## الأمان <a name="security"></a>

يجب ألا تتسبب مستندات AMP HTML في حدوث أخطاء عند عرضها مع سياسة أمان المحتوى التي لا تتضمن الكلمتين الرئيسيتين `unsafe-inline` و`unsafe-eval`.

وقد تم تصميم تنسيق AMP HTML بحيث تكون هذه ماهية الأمر دائمًا.

يجب أن تخضع جميع عناصر قالب AMP لمراجعة أمان AMP قبل إرسالها إلى مستودع AMP.

## SVG <a name="svg"></a>

عناصر SVG التالية هي المسموح بها في الوقت الحالي:

- basics: "g", "glyph", "glyphRef", "image", "marker", "metadata", "path", "solidcolor", "svg", "switch", "view"
- shapes: "circle", "ellipse", "line", "polygon", "polyline", "rect"
- text: "text", "textPath", "tref", "tspan"
- rendering: "clipPath", "filter", "hkern", "linearGradient", "mask", "pattern", "radialGradient", "vkern"
- special: "defs" (all children above are allowed here), "symbol", "use"
- filter: "feColorMatrix", "feComposite", "feGaussianBlur", "feMerge", "feMergeNode", "feOffset", "foreignObject"
- ARIA: "desc", "title"

إلى جانب السمات التالية:

- "xlink:href": only URIs starting with "#" are allowed
- "style"

## اكتشاف مستند AMP <a name="amp-document-discovery"></a>

توفر الآلية الموضحة أدناه طريقة موحدة للبرنامج لاكتشاف ما إذا كان إصدار AMP موجودًا لمستند معياري أم لا.

وفي حالة وجود مستند AMP يُعد تمثيلًا بديلًا لمستند معياري، يجب أن يشير المستند المعياري إلى مستند AMP عبر العلامة `link` [بالصلة"amphtml"](http://microformats.org/wiki/existing-rel-values#HTML5_link_type_extensions).

مثال:

[sourcecode:html]
<link rel="amphtml" href="https://www.example.com/url/to/amp/document.html" />
[/sourcecode]

من المتوقع أن يشير مستند AMP نفسه إلى المستند المعياري عبر علامة `link`  بالصلة "معياري".

مثال:

[sourcecode:html]
<link
  rel="canonical"
  href="https://www.example.com/url/to/canonical/document.html"
/>
[/sourcecode]

(إذا كان المورد الفردي هو AMP و المستند المعياري في الوقت نفسه، فيجب أن تشير العلاقة الأساسية إلى نفسها؛ لا يلزم وجود علاقة "amphtml".)

ولاحظ أنه للحصول على أقصى توافق مع الأنظمة التي تستهلك AMP، يجب أن يكون من الممكن قراءة علاقة "amphtml" من دون تنفيذ JavaScript. (بمعنى أنه يجب أن تكون العلامة موجودة في HTML البسيط، ولا يتم إدخالها عبر JavaScript.)
