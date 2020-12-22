---
"$title": إدارة حالة مستخدم غير مصادق عليه باستخدام AMP
order: '2'
formats:
- websites
teaser:
  text: "**فهرس المحتويات**"
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-managing-user-state.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2017 The AMP HTML Authors. All Rights Reserved.

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

**فهرس المحتويات**

- [Background](#background)
- [Implementation guide](#implementation-guide)
    - [Before getting started](#before-getting-started)
    - [Task 1: For non-AMP pages on the publisher origin, set up an identifier and send analytics pings](#task1)
    - [Task 2: For AMP pages, set up an identifier and send analytics pings by including Client ID replacement in amp-analytics pings](#task2)
    - [Task 3: Process analytics pings from pages on the publisher origin](#task3)
    - [Task 4: Process analytics pings from AMP cache or AMP viewer display contexts and establish identifier mappings (if needed)](#task4)
    - [Task 5: Using Client ID in linking and form submission](#task5)
- [Strongly recommended practices](#strongly-recommended-practices)

حالة المستخدم هي مفهوم مهم على شبكة الإنترنت اليوم. ضع في اعتبارك حالات الاستخدام التالية التي يتم تمكينها من خلال إدارة حالة المستخدم:

- ينشئ تاجر **عربة تسوق** مفيدة تعرض للمستخدم نفس العناصر أثناء زيارته الثانية التي أضافها إلى سلة التسوق أثناء زيارته الأولى منذ عدة أسابيع. تزيد مثل هذه التجربة من فرصة شراء المستخدم لهذا العنصر من خلال التأكد من بقائه على دراية بالعنصر الذي فكر في شراءه في الماضي.
- ناشر أخبار يمكنه تخصيص **المقالات الموصى** بها للقارئ استنادًا إلى زيارات القارئ المتكررة لمقالات الناشر، مما يساعد في الحفاظ على تفاعل القارئ واكتشاف المزيد من المحتوى.
- يقوم مطور مواقع إلكترونية الذي يشغل أي نوع من المواقع بجمع **تحليلات**، التي من خلالها يمكن معرفة ما إذا كانت مشاهدتا الصفحة تنتمي إلى نفس الشخص الذي شاهد صفحتين أو شخصين مختلفين شاهد كل منهما صفحة واحدة. حيث يساعد الحصول على هذه الروية في معرفة كيفية أداء الموقع، وفي النهاية، كيفية تحسينه.

تم تصميم هذه المقالة لمساعدتك على أن تكون أكثر نجاحًا في **إدارة حالة المستخدم غير المصادق عليه في AMP**، وهي طريقة لتوفير رحلة مستخدم سلسة حتى لو لم يتخذ المستخدم إجراءً لتقديم هويته، مثل تسجيل الدخول. بعد مراجعة البعض من التحديات والاعتبارات في التعامل مع هذا الموضوع، يوضح هذا الدليل الطرق التي يتم بها دعم حالة المستخدم بواسطة AMP ويقدم توصيات حول كيفية التعامل مع التنفيذ الفني.

## معلومات عامة <a name="background"></a>

يستحق موضوع حالة المستخدم اهتمامًا خاصًا في AMP لأنه يمكن عرض صفحات AMP في سياقات متعددة مثل موقعك الإلكتروني أو في بحث Google أو في تطبيق تابع لجهة خارجية. ويخلق هذا تحديات في إدارة حالة المستخدم عندما يتنقل المستخدمون بينهم.

### عرض السياقات لصفحات AMP <a name="display-contexts-for-amp-pages"></a>

يمكنك التفكير في AMP كتنسيق محتوى محمول يتيح تحميل المحتوى بسرعة في أي مكان. يمكن عرض مستندات AMP عبر ثلاثة سياقات جديرة بالملاحظة:

- مصدر الناشر
- ذاكرة AMP للتخزين المؤقت
- عارض AMP

<table>
  <tr>
    <th width="20%">السياق</th>
    <th width="20%">هل يمكن عرض الصفحات التي لا تدعم AMP من هنا؟</th>
    <th width="20%">هل يمكن عرض صفحات AMP من هنا؟</th>
    <th>نموذج عنوان URL</th>
  </tr>
  <tr>
    <td>مصدر الناشر</td>
    <td>نعم</td>
    <td>نعم</td>
    <td><code>https://example.com/article.amp.html</code></td>
  </tr>
   <tr>
    <td>ذاكرة AMP للتخزين المؤقت</td>
    <td>لا</td>
    <td>نعم</td>
    <td><code>https://example-com.cdn.ampproject.org/s/example.com/article.amp.html</code></td>
  </tr>
   <tr>
    <td>عارض AMP</td>
    <td>لا</td>
    <td>نعم</td>
    <td><code>https://google.com/amp/s/example.com/article.amp.html</code></td>
  </tr>
</table>

دعونا نفحص كل من هذه المواقف عن كثب.

**السياق #1: مصدر الناشر.** يتم نشر صفحات AMP بحيث يتم استضافتها في الأصل من موقع الناشر ويمكن الوصول إليها عبره، على سبيل المثال، على `https://example.com` قد يجد المرء `https://example.com/article.amp.html`.

Publishers can choose to publish exclusively in AMP, or to publish two versions of content (that is, AMP content "paired" with non-AMP content). The "paired" model requires some [particular steps](https://amp.dev/documentation/guides-and-tutorials/optimize-and-measure/discovery) to ensure the AMP versions of pages are discoverable to search engines, social media sites, and other platforms. Both publishing approaches are fully supported; it's up to the publisher to decide on which approach to take.

> **ملاحظة:**
> Due to the "paired" publishing model just described, the publisher’s origin (in the example above, `https://example.com`) is a context in which **both AMP and non-AMP content can be accessed**. Indeed, it’s the only context in which this can happen because AMP caches and AMP viewers, described below, only deliver valid AMP content.

**السياق #2: ذاكرة AMP للتخزين المؤقت.** يمكن تخزين ملفات AMP مؤقتًا في السحابة بواسطة ذاكرة تخزين مؤقت لطرف ثالث لتقليل الوقت الذي يستغرقه المحتوى للوصول إلى جهاز المستخدم المحمول.

باستخدام تنسيق AMP، يجعل منتجو المحتوى المحتوى في ملفات AMP متاحًا للتخزين المؤقت من خلال أطراف ثالثة. في ظل هذا النوع من الإطار، يستمر الناشرون في التحكم في محتواهم (من خلال النشر إلى مصدرهم كما هو مفصل أعلاه)، ولكن يمكن للمنصات تخزين المحتوى مؤقتًا أو نسخه للحصول على سرعة توصيل مثالية للمستخدمين.

بشكل تقليدي، المحتوى الذي يتم تقديمه بهذه الطريقة يتوّلد من نطاق مختلف. على سبيل المثال، تستخدم [ذاكرة التخزين المؤقت Google AMP](https://developers.google.com/amp/cache/overview) `https://cdn.ampproject.org` لتقديم المحتوى، مثل `https://example-com.cdn.ampproject.org/s/example.com/article.amp.html`.

**السياق #3: عارض AMP.** تم تصميم تنسيق AMP لدعم التضمين في أجهزة عرض AMP التابعة لأطراف ثالثة. يتيح ذلك درجة عالية من التعاون بين ملف AMP وتجربة العارض، والتي تشمل فوائدها: التحميل المسبق الذكي والآمن للمحتوى وعرضه مسبقًا والإمكانيات المبتكرة مثل التمرير السريع بين صفحات AMP الكاملة.

تمامًا مثل حالة ذاكرة AMP للتخزين المؤقت، توقع أن يكون نطاق عارض AMP مختلفًا أيضًا عن مصدر الناشر. على سبيل المثال، تتم استضافة عارض بحث Google على `https://google.com` ويقوم بتضمين إطار iframe الذي يطلب محتوى الناشر من ذاكرة التخزين المؤقت Google AMP.

### السياقات المتعددة تعني إدارة حالات متعددة <a name="multiple-contexts-means-multiple-state-management"></a>

يجب أن يكون الناشرون على استعداد لإدارة حالة المستخدم لكل سياق عرض على حدة. توفر ميزة [معرّف العميل](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md#client-id) في AMP، والتي تستفيد من ملفات تعريف الارتباط أو التخزين المحلي للمحافظة على الحالة، الدعم اللازم لصفحات AMP للحصول على معرّف ثابت وسبه مستعار للمستخدم. من وجهة نظر التنفيذ، يتم استخدام ملفات تعريف الارتباط أو التخزين المحلي، وتتخذ AMP القرار الذي يجب استخدامه بناءً على سياق العرض. يتأثر هذا الاختيار بالجدوى الفنية لإدارة هذه الحالة التي تشمل مئات أو آلاف الناشرين.

ومع ذلك، يمكن أن ينتهي الأمر بناشري صفحات AMP بسهولة (عن غير قصد) إلى تصميم رحلات مستخدمين تتضمن سياقات متعددة. دعنا نلقي نظرة مجددًا على نظرتنا السابقة الخاصة بحالة استخدام عربة التسوق ونضيف بعض التفاصيل إليها لنصنع **قصة مستخدم** كاملة:

> *في اليوم الأول، تكتشف المستخدمة صفحة AMP من شركة Example Inc. عبر بحث Google. يُحمِّل بحث Google صفحات AMP في عارض AMP. أثناء عرض الصفحة، تضيف المستخدمة أربعة عناصر إلى عربة التسوق الخاصة بها ولكنها لا تقوم بإتمام الدفع. بعد أسبوعين، في اليوم الخامس عشر، تتذكر المستخدمة العناصر الأربعة التي كانت تفكر في شرائها وتقرر الآن أن الوقت قد حان لشرائها. يصلون إلى الصفحة الرئيسية لشركة Example Inc. على `https://example.com` مباشرةً (وهي صفحة رئيسية لا تدعم AMP) ويكتشفون أن عناصرهم الأربعة لا تزال محفوظة في عربة التسوق.*

في هذا السيناريو، تتلقى المستخدمة تجربة عربة تسوق متسقة على الرغم من انتقالها من سياق عارض AMP إلى سياق مصدر الناشر—ومع مرور بعض الوقت بين هذه الأحداث. هذه التجربة معقولة جدًا، وإذا كنت تصمم تجربة تسوق، فيجب أن تتوقع دعمها، فكيف تحقق ذلك؟

**To enable this and any experience involving user state, all contexts the user traverses must share their individually-maintained state with each other.** "Perfect!", you say, with the idea to share the cookie values with user identifiers across these contextual boundaries. One wrinkle: even though each of these contexts displays content controlled by the same publisher, they each see the other as a third-party because each context lives on different domains.

<amp-img alt="AMP's ability to be displayed in many contexts means that each of those contexts has its own storage for identifiers" layout="responsive" src="https://github.com/ampproject/amphtml/raw/master/spec/img/contexts-with-different-storage.png" width="1030" height="868">
  <noscript><img alt="تعني إمكانية عرض AMP في العديد من السياقات أن لكل من هذه السياقات مساحة تخزين خاصة به للمعرفات" src="https://github.com/ampproject/amphtml/raw/master/spec/img/contexts-with-different-storage.png"></noscript></amp-img>

كما سترى في المناقشة التالية، فإن كونك في موقع الطرف الثالث عند التفاعل مع ملفات تعريف الارتباط قد يمثل تحديات، اعتمادًا على كيفية تكوين إعدادات متصفح المستخدم. على وجه الخصوص، إذا تم حظر ملفات تعريف ارتباط الطرف الثالث في موقف معين، فسيؤدي ذلك إلى منع إمكانية مشاركة المعلومات عبر السياقات. من ناحية أخرى، إذا تم السماح بعمليات ملفات تعريف الارتباط للجهات الثالثة، فيمكن عندئذٍ مشاركة المعلومات.

## دليل التنفيذ <a name="implementation-guide"></a>

يقدم هذا القسم توصيات لإدارة حالة المستخدم. ويتم تقديم المهام أدناه في هيئة تدرج وتسلسل، ولكن يمكن النظر إليها إجمالاً في جزأين:

**Chunk #1: Fundamental implementation:** Tasks 1-4 are essential toward getting the basics working. They rely on a minimal set of features needed to get the job partially done: AMP’s Client ID substitution, reading and writing of cookies, and maintaining a backend mapping table. Why "partially"? Because the steps conveyed in these tasks rely on reading and writing cookies and because the browser’s cookie settings may prevent this in certain circumstances, this set of tasks is likely to be insufficient for fully managing user state in all scenarios.

بعد وضع الأساسات، نقوم بعد ذلك بزيارة موضوع بنطاق أضيق من حالات الاستخدام ولكنه يقدم حلاً كاملاً لحالات الاستخدام هذه.

**الجزء #2: استخدام معرّف العميل في الربط وتقديم النموذج:** في المهمة 5، ستتعلم الاستفادة من اجتياز الربط و/أو تقديم النموذج لتمرير معلومات معرّف عميل AMP عبر الحدود السياقية حيث ينتقل المستخدم من صفحة واحدة مباشرةً إلى أخرى.

> **تحذير:**
> ينصح دليل التنفيذ التالي باستخدام ملفات تعريف الارتباط والعمل بها. تأكد من مراجعة قسم [الممارسات الموصى بها بشدة](#strongly-recommended-practices) للحصول على اقتراحات مهمة يجب وضعها في الاعتبار.

### قبل البدء <a name="before-getting-started"></a>

في مقدمة جولة الدليل الفني أدناه، لنفترض أنك ستربط **حالة المستخدم** **بمعرّف** ثابت يمثل المستخدم. على سبيل المثال، قد يبدو المعرّف في هذا الشكل `n34ic982n2386n30`. من طرف الخادم، تقوم بعد ذلك بربط `n34ic982n2386n30` بأي مجموعة من معلومات حالة المستخدم، مثل محتوى عربة التسوق، أو قائمة المقالات التي تمت قراءتها مسبقًا، أو البيانات الأخرى بناءً على حالة الاستخدام.

<amp-img alt="A single identifier could be used to manage user state for many use cases" layout="responsive" src="https://github.com/ampproject/amphtml/raw/master/spec/img/identifiers-for-use-cases.png" width="1276" height="376">
  <noscript><img alt="يمكن استخدام معرّف واحد لإدارة حالة المستخدم للعديد من حالات الاستخدام" src="https://github.com/ampproject/amphtml/raw/master/spec/img/identifiers-for-use-cases.png"></noscript></amp-img>

من أجل الوضوح في باقي هذا المستند، سنقوم باستدعاء سلاسل مختلفة من الأحرف والتي تكون معرّفات بأسماء أكثر قابلية للقراءة مسبوقة بعلامة الدولار (`$`):

[sourcecode:text]
n34ic982n2386n30 ⇒ $sample_id
[/sourcecode]

**حالة الاستخدام الخاصة بنا:** سنعمل خلال هذا الدليل على مثال مصمم لتحقيق تتبع بسيط لمشاهدات الصفحة (أي التحليلات) حيث نريد أن ننتج أدق حساب ممكن للمستخدمين. هذا يعني أنه حتى إذا كان المستخدم يصل إلى محتوى ناشر معين من سياقات مختلفة (بما في ذلك التنقل بين صفحات AMP والصفحات التي لا تدعم AMP)، فإننا نريد أن يتم احتساب هذه الزيارات نحو فهم فريد للمستخدم كما لو كان يتصفح صفحات الناشر التقليدية التي لا تدعم AMP.

**الافتراض الذي يشير إلى توّفر قيم ملفات تعريف الارتباط الثابتة:** نفترض أيضًا أن المستخدم يستخدم نفس الجهاز والمتصفح والتصفح غير الخاص/المتخفي، من أجل ضمان الحفاظ على قيم ملفات تعريف الارتباط وإتاحتها عبر جلسات المستخدم بمرور الوقت. إذا لم يكن الأمر كذلك، فلا ينبغي توقع نجاح هذه التقنيات. إذا كان هذا مطلوبًا، فابحث في إدارة حالة المستخدم استنادًا إلى هوية المستخدم المصادق عليها (أي عند تسجيل الدخول).

**يمكن توسيع المفاهيم الواردة أدناه لتشمل حالات استخدام أخرى:** على الرغم من أننا نركز فقط على حالة استخدام التحليلات، يمكن إعادة صياغة المفاهيم التي تم المدرجة أدناه لحالات الاستخدام الأخرى التي تتطلب إدارة حالة المستخدم عبر السياقات.

<a id="task1"></a>

### المهمة 1: بالنسبة للصفحات التي لا تدعم AMP الموجودة في مصدر الناشر، عليك بإعداد معرّف وإرسال رسائل فحص التحليلات <a name="task-1-for-non-amp-pages-on-the-publisher-origin-set-up-an-identifier-and-send-analytics-pings"></a>

لنبدأ بتهيئة التحليلات للصفحات التي لا تدعم AMP التي يتم عرضها من مصدر الناشر. يمكن تحقيق ذلك بعدة طرق، بما في ذلك استخدام حزمة تحليلات مثل Google Analytics أو Adobe Analytics أو عن طريق كتابة تنفيذ مخصص.

إذا كنت تستخدم حزمة تحليلات من أحد الموردين، فمن الأرجح أن تهتم الحزمة بإعداد ملفات تعريف الارتباط وبث رسائل الفحص عبر كود التكوين وواجهات برمجة التطبيقات (API). إذا كانت هذه هي الحالة، يجب عليك قراءة الخطوات أدناه للتأكد من توافقها مع نهج التحليلات الخاص بك ولكن توّقع أنك لن تحتاج إلى إجراء أي تغييرات كجزء من إكمال هذه المهمة.

تُقدم بقية هذه المهمة إرشادات إذا كنت تبحث عن إعداد تحليلاتك الخاصة.

##### إعداد معرّف باستخدام ملفات تعريف الارتباط الخاصة بالطرف الأول <a name="set-up-an-identifier-using-first-party-cookies"></a>

إذا كانت لديك صفحات لا تدعم AMP يتم عرضها من مصدر النشر الخاص بك، فقم بإعداد معرّف ثابت ومستقر لاستخدامه في هذه الصفحات. هذا عادة ما [يتم تنفيذه مع ملفات تعريف ارتباط الطرف الأول](https://en.wikipedia.org/wiki/HTTP_cookie#Tracking).

For the purposes of our example, let’s say you’ve set a cookie called `uid` ("user identifier") that will be created on a user’s first visit. If it’s not the user’s first visit, then read the value that was previously set on the first visit.

هذا يعني أن هناك وضعان لحالة الصفحات التي لا تدعم AMP على مصدر الناشر:

**الحالة #1: الزيارة الأولية.** عند الوصول لأول مرة على الصفحة التي لا تدعم AMP، لن يكون هناك ملف تعريف ارتباط. إذا قمت بالتحقق من ملف تعريف الارتباط قبل تعيين أحدها، فسترى أنه لا يوجد قيم تم تعيينها في ملف تعريف الارتباط المقابل لـ `uid`:

[sourcecode:bash]
> document.cookie
  ""
[/sourcecode]

في وقت ما من التحميل الأولي، يجب تعيين ملف تعريف الارتباط، بحيث إذا قمت بذلك بمجرد تحميل الصفحة، سترى أن هناك قيمة تم تعيينها:

[sourcecode:bash]
> document.cookie
  "uid=$publisher_origin_identifier"
[/sourcecode]

**الحالة #2: زيارة غير أولية.** سيكون هناك مجموعة ملفات تعريف الارتباط. وبالتالي، إذا فتحت وحدة تحكم المطور على الصفحة، فسترى:

[sourcecode:bash]
> document.cookie
  "uid=$publisher_origin_identifier"
[/sourcecode]

##### إرسال رسائل فحص التحليلات <a name="send-analytics-pings"></a>

بمجرد إعداد معرّف، يمكنك الآن دمجه في رسائل فحص اتصال التحليلات لبدء تتبع مشاهدات الصفحة.

سيعتمد التنفيذ المحدد على التكوين الذي تريده، ولكن بشكل عام سوف تتطلع إلى إرسال رسائل فحص (طلبات) إلى خادم التحليلات الخاص بك، والتي تتضمن بيانات مفيدة داخل عنوان URL الخاص بالطلب نفسه. فيما يلي مثال يشير أيضًا إلى كيفية تضمين قيمة ملف تعريف الارتباط داخل الطلب:

[sourcecode:http]
https://analytics.example.com/ping?type=pageview&user_id=$publisher_origin_identifier
[/sourcecode]

لاحظ أنه في المثال أعلاه، تتم الإشارة إلى مُعرّف المستخدم بواسطة معلمة استعلام محددة، `user_id`:

[sourcecode:text]
user_id=$publisher_origin_identifier
[/sourcecode]

The use of "`user_id`" here should be determined by what your analytics server expects to process and is not specifically tied to what you call the cookie that stores the identifier locally.

<a id="task2"></a>

### المهمة 2: بالنسبة لصفحات AMP، إعداد معرّف وإرسال رسائل فحص التحليلات من خلال تضمين بديل لمعرّف العميل في رسائل فحص تحليلات AMP <a name="task-2-for-amp-pages-set-up-an-identifier-and-send-analytics-pings-by-including-client-id-replacement-in-amp-analytics-pings"></a>

بالانتقال الآن إلى صفحات AMP، دعنا نلقي نظرة على كيفية إنشاء معرّف للتحليلات ونقله. سيكون هذا ساريًا بغض النظر عن السياق الذي يتم تقديم صفحة AMP فيه، لذلك يغطي هذا أي صفحة AMP في مصدر الناشر، يتم عرضها عبر ذاكرة AMP للتخزين المؤقت، أو يتم عرضها في عارض AMP.

Through usage of features that require Client ID, AMP will do the "under the hood" work to generate and store client ID values and surface them to the features that require them. One of the principal features that can use AMP’s Client ID is [amp-analytics](https://amp.dev/documentation/components/amp-analytics), which happens to be exactly what we’ll need to implement our analytics use case example.

في صفحات AMP، أنشئ رسالة فحص تحليلات AMP تحتوي على معرّف العميل:

<table>
  <tr>
    <td width="40%"><strong>يبدو تكوين تحليلات AMP بالشكل التالي:</strong></td>
    <td width="60%"><code>https://analytics.example.com/ping?type=pageview&user_id=${clientId(uid)}</code></td>
  </tr>
  <tr>
    <td><strong>يبدو ما يمر عبر الشبكة بالشكل التالي:</strong></td>
    <td>
<code>https://analytics.example.com/ping?type=pageview&user_id=$amp_client_id</code><p><em>في هذه الحالة، يتم استبدال <code>${clientId(uid)}</code> بقيمة فعلية تنشئها AMP في تلك اللحظة أو ستُعاد بناءً على ما خزنه متصفح المستخدم محليًا بالفعل</em></p>
</td>
  </tr>
</table>

لاحظ حقيقة أن المعلمة التي تم تمريرها إلى بديل معرّف العميل، `${clientId(uid)`، تكون `uid`. كان هذا اختيارًا متعمدًا يطابق نفس اسم ملف تعريف الارتباط المستخدم في مصدر الناشر كما هو موضح في [المهمة 1](#task1). لتحقيق التكامل الأكثر سلاسة، يجب عليك تطبيق نفس التقنية.

فيما يتعلق ببقية تنفيذ تحليلات AMP، راجع وثائق [تكوين تحليلات AMP](https://amp.dev/documentation/guides-and-tutorials/optimize-measure/configure-analytics/) لمزيد من التفاصيل حول كيفية إعداد طلبات amp-analytics أو لتعديل طلبات مورد التحليلات الخاص بك. يمكن تعديل رسالة الفحص بشكل إضافي لنقل البيانات الإضافية التي تحددها مباشرة أو من خلال الاستفادة من [بدائل AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md) الأخرى.

> **من المفيد أن تعلم مايلي:**
> Why did we use of the name `uid` for the parameter passed to the Client ID feature? The parameter that the `clientId(...)` substitution takes is used to define scope. You can actually use the Client ID feature for many use cases and, as a result, generate many client IDs. The parameter differentiates between these use cases and so you use it to specify which use case you would like a Client ID for. For instance, you might want to send different identifiers to third parties like an advertiser and you could use the "scope" parameter to achieve this.

On the publisher origin, it’s easiest to think of "scope" as what you call the cookie. By recommending a value of `uid` for the Client ID parameter here in [Task 2](#task2), we align with the choice to use a cookie called `uid` in [Task 1](#task1).

<a id="task3"></a>

### المهمة 3: معالجة رسائل فحص التحليلات من الصفحات الموجودة في مصدر الناشر <a name="task-3-process-analytics-pings-from-pages-on-the-publisher-origin"></a>

نظرًا للإعداد الذي تم إجراؤه في المهمتين 1 و 2، عندما يصل شخص ما إلى إصدار AMP (من أي سياق) أو الإصدار الذي لا يدعم AMP في مصدر الناشر، ستستخدم رسائل فحص التحليلات نفس المعرّف. باتباع الإرشادات الواردة في [المهمة 2](#task2) لاختيار "نطاق" معرف العميل الذي كان يحمل نفس اسم ملف تعريف الارتباط الذي استخدمته في [المهمة 1](#task1)، فإن AMP تعيد استخدام ملف تعريف الارتباط نفسه.

وهذا موضح في الجدول أدناه:

<table>
  <tr>
    <td width="40%">تبدو رسالة فحص التحليلات القادمة من <strong>صفحة لا تدعم AMP في مصدر الناشر</strong> كما يلي</td>
    <td width="60%"><code>https://analytics.example.com/ping?type=pageview&user_id=$publisher_origin_identifier</code></td>
  </tr>
  <tr>
    <td>تبدو رسالة فحص التحليلات القادمة من <strong>صفحة AMP في مصدر الناشر</strong> كما يلي</td>
    <td>
<code>https://analytics.example.com/ping?type=pageview&user_id=$publisher_origin_identifier</code><br><em>في هذه الحالة، ينطبق الأمر نفسه! من خلال اختيار قيمة نطاق <code>uid</code>، فإن القيمة المخفية لملف تعريف الارتباط <code>uid</code>، والتي تكون <code>$publisher_origin_identifier</code>، سيتم استخدامها.</em>
</td>
  </tr>
</table>

<a id="task4"></a>

### <a name="task-4-process-analytics-pings-from-amp-cache-or-amp-viewer-display-contexts-and-establish-identifier-mappings-if-needed">المهمة 4: عرض معالجة رسائل اختبار التحليلات من ذاكرة AMP للتخزين المؤقت أو عارض AMP لسياقات وتخطيطات المعرّفات (إذا لزم الأمر)</a>

عندما أعددنا رسائل فحص التحليلات في [المهمة 2](#task2) لنقل البيانات من صفحات AMP المعروضة من خلال ذاكرة AMP للتخزين المؤقت أو عارض AMP، فقد أحدثنا أيضًا مشكلة. كما نُوقش سابقًا، تختلف كل سياقات ذاكرة AMP للتخزين المؤقت وعارض AMP عن سياق مصدر الناشر، وبجانب هذا الأمر تظهر طريقة مختلفة للحفاظ على المعرّفات. لمعالجة رسائل الفحص هذه من أجل تجنب حدوث مشكلات مثل زيادة عدد المستخدمين، سنتخذ بعض [الخطوات](#implementation-steps) لمحاولة التوفيق بين المعرّفات بقدر الإمكان.

للمساعدة في شرح الخطوات التي نتخذها، من المفيد أولاً إعادة النظر بالضبط في كيفية ظهور مشكلة زيادة عدد المستخدمين.

#### مراجعة المشكلة <a name="reviewing-the-problem"></a>

ضع في اعتبارك التدفق التالي:

1. يزور أحد المستخدمين **صفحة AMP في سياق عرض عارض AMP**، مثل `https://google.com/amp/s/example.com/article.amp.html`. نظرًا لأن عارض AMP لا يمتلك حق الوصول إلى ملف تعريف الارتباط `uid` في مصدر الناشر، يتم إنشاء قيمة عشوائية بقيمة `$amp_client_id` لتحديد هوية المستخدم.
2. ثم يزور المستخدم نفسه **صفحة على مصدر الناشر `https://example.com`**. كما هو موضح في [المهمة 3](#task3)، ثم يتم تعريف المستخدم بـ `$publisher_origin_identifier`0.

إليك نقطتين، حيث يقع كل من (1) و (2) في مصادر (أو سياقات) مختلفة. لهذا السبب، لا توجد حالة مشتركة ويختلف `$amp_client_id` عن `$publisher_origin_identifier`. إذن، ما هو التأثير؟ التأثير (1) هو جلسة مشاهدة صفحة واحدة تبدو وكأن مستخدم واحد قام بها، إما بالنسبة للتأثير (2) هو جلسة مشاهدة صفحة واحدة أخرى يبدو أنها واردة من مستخدم آخر. **{nbsp}بشكل مُبسط، على الرغم من استمرار تفاعل المستخدم مع محتوى `https://example.com`، فإننا نزيد عدد المستخدمين ويبدو أن المستخدم في التأثير (1) يرتد (حيث يقوم بزيارة واحدة للصفحة). **

#### استراتيجية الحل <a name="solution-strategy"></a>

لمعالجة مشكلة زيادة العدد، يجب عليك استخدام الاستراتيجية التالية، والتي تعتمد فعاليتها على ما إذا كان مسموحًا بقراءة ملفات تعريف ارتباط الطرف الثالث أو كتابتها:

- **التسوية الفورية للمعرّف: إذا كان بإمكانك الوصول إلى ملفات تعريف ارتباط مصدر الناشر أو تغييرها**، فاستخدم أو أنشئ معرّف مصدر الناشر وتجاهل أي معرّف في طلب التحليلات. ستتمكن من ربط النشاط بين السياقين بنجاح.
- **Delayed identifier reconciliation: If you cannot access or change the publisher origin identifier (i.e. the cookies)**, then fall back to the AMP Client ID that comes within the analytics request itself. Use this identifier as an "**alias**", rather than using or creating a new publisher origin identifier (cookie), which you cannot do (because of third party cookie blocking), and add the alias to a **mapping table**. You will be unsuccessful in immediately linking activity between the two contexts, but by using a mapping table you may be able to link the AMP Client ID value with the publisher origin identifier on a future visit by the user. When this happens, you will have the needed information to link the activity and reconcile that the page visits in the different contexts came from the same user. Task 5 describes how to achieve a complete solution in specific scenarios where the user traverses from one page immediately to another.

#### خطوات التنفيذ <a name="implementation-steps"></a>

على الخادم، تحقق من معرّف مصدر الناشر الحالي

اقرأ ملفات تعريف الارتباط المرسلة كجزء من طلب التحليلات. في مثالنا، هذا يعني التحقق من ملف تعريف الارتباط الخاص بـ `uid` من example.com.

- إذا تمت قراءة قيمة `uid` بنجاح، فاستخدمها لتسجيل بيانات التحليلات أو (**معرّف سجل التحليلات**). بسبب [ المهمة 1 ](#task1)، نعلم أن قيمة هذا المعرّف هي `$publisher_origin_identifier`. مع إنشاء معرّف سجل التحليلات، يمكننا التخطي إلى قسم [ تخزين البيانات](#data-storage).
- إذا لم تتم قراءة قيمة `uid` بنجاح، فتابع الخطوات التالية التي تتضمن جدول التعيين.

##### جدول التعيين <a name="mapping-table"></a>

سيربط جدول التعيين الخاص بنا قيم معرّف عميل AMP التي تظهر في رسائل فحص التحليلات بمعرفات مصدر الناشر على النحو التالي:

<table>
  <tr>
    <th width="50%"><strong>معرّف المستخدم في مصدر الناشر</strong></th>
    <th width="50%"><strong>User ID on AMP page that’s NOT on publisher origin ("alias")</strong></th>
  </tr>
  <tr>
    <td>يأتي من معرّف مصدر الناشر أو يتم إنشاؤه كقيمة محتملة إذا تعذر الوصول إلى معرّف مصدر الناشر.</td>
    <td>يأتي من معرّف عميل AMP</td>
  </tr>
</table>

فور التأكد من أنك لم تنجح في قراءة معرّف مصدر الناشر، تحقق مما إذا كان معرّف عميل AMP المضمن في رسالة فحص التحليلات مُستخدمًا بالفعل في التعيين. للقيام بذلك، استشر أولاً طلب تحليلات AMP الوارد للحصول على قيمة معرّف العميل. على سبيل المثال، من هذا الطلب:

[sourcecode:http]
https://analytics.example.com/ping?type=pageview&user_id=$amp_client_id
[/sourcecode]

نستخرج الجزء الغامق المقابل لمعرّف عميل AMP: `$amp_client_id`.

Next, examine the mapping table to try and find the same value in the "alias" column:

<table>
  <tr>
    <th width="50%"><strong>معرّف المستخدم في مصدر الناشر</strong></th>
    <th width="50%"><strong>User ID on AMP page that’s NOT on publisher origin ("alias")</strong></th>
  </tr>
  <tr>
    <td><code>$existing_publisher_origin_identifier</code></td>
    <td><code>$amp_client_id</code></td>
  </tr>
</table>

في المثال أعلاه، نجد سجلاً موجودًا بالفعل. تصبح القيمة التي نجدها مقترنة بمعرّف عميل AMP هي معرّف سجل التحليلات. وبالتالي، هذا هو `$existing_publisher_origin_identifier`. مع إنشاء معرف سجل التحليلات، يمكننا التخطي إلى قسم [تخزين البيانات](#data-storage).

خلافًا لذلك، إذا لم يتم العثور على معرّف عميل AMP في التعيين، فسنحتاج إلى إنشاء تعيين:

1. إنشاء **معرف أصل ناشر محتمل**. دعنا نسمي هذا `$prospective_identifier` في الأمثلة التالية. يجب إنشاء هذه القيمة وفقًا لكيفية إعداد القيمة في مصدر الناشر، كما هو موضح في [المهمة 1](#task1) أعلاه.
2. بعد ذلك، حاول [تعيين](https://en.wikipedia.org/wiki/HTTP_cookie#Setting_a_cookie) معرف مصدر الناشر المحتمل كملف تعريف ارتباط على مصدر الناشر. سينجح هذا إذا كان من الممكن كتابة ملفات تعريف ارتباط الطرف الثالث، وإلا فسوف تفشل المحاولة.
3. بعد ذلك، احتفظ بزوج {prospective publisher origin identifier, AMP Client ID}.

يُفضي التعيين الذي أنشأناه إلى الشكل التالي:

<table>
  <tr>
    <th><strong>معرف المستخدم في أصل الناشر</strong></th>
    <th><strong>User ID on AMP page that’s NOT on publisher origin ("alias")</strong></th>
  </tr>
  <tr>
    <td>
<code>$prospective_identifier</code> (يتم إنشاؤه في الوقت المناسب عند تلقي رسالة الفحص)</td>
    <td> <code>$amp_client_id</code> (يأتي من رسالة فحص التحليلات)</td>
  </tr>
</table>

سنستخدم معرّف مصدر الناشر المحتمل كمعرّف سجل التحليلات لأن هذه هي القيمة المرتبطة بالحالة في مصدر الناشر. في هذه الحالة، سيكون `$prospective_identifier`، حيث سيظهر في الصورة في قسم [تخزين البيانات](#data-storage) فيما يلي.

##### تخزين البيانات <a name="data-storage"></a>

الآن بعد أن اكتشفت معرّف سجل التحليلات، يمكنك بالفعل تخزين معلومات حالة المستخدم (بيانات التحليلات في هذه الحالة) التي تم تحديدها بواسطة هذا المعرف:

[sourcecode:text]
{analytics record identifier, analytics data ...}
[/sourcecode]

<a id="task5"></a>

### المهمة 5: استخدام معرّف العميل في الربط وتقديم النموذج <a name="task-5-using-client-id-in-linking-and-form-submission"></a>

بشكل عام، عند عدم السماح بقراءة ملفات تعريف ارتباط الطرف الثالث وكتابتها، ستكون هناك مواقف يكون فيها من المستحيل إدارة حالة المستخدم بفعالية كاملة. في المهام من 1 إلى 4، تساعد الخطوات التي اتخذناها بطريقتين: (1) أنها توفر حلاً فعالاً تمامًا عندما يُسمح بقراءة ملفات تعريف ارتباط الطرف الثالث وكتابتها، و (2) قاموا بإعداد نظامنا للاستفادة من أي فرصة نهائية للتوفيق بين معرفات السياقات المتداخلة إذا كانت التسوية الفورية مستحيلة بسبب إعدادات ملفات تعريف الارتباط في المتصفح.

في هذه المهمة، سنغطي تحسينًا إضافيًا يساعد عندما يتنقل المستخدم عبر السياقات من صفحة إلى صفحة أخرى إما **عن طريق الارتباط أو عمليات إرسال النموذج**. في هذه المواقف، ومع أعمال التنفيذ الموضحة أدناه، من الممكن إعداد مخطط فعال بالكامل لإدارة حالة المستخدم عبر السياقات.

<amp-img alt="Links can be used to pass the identifier information of one context into another (linked) context" layout="responsive" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-form-identifier-forwarding.png" width="866" height="784">
  <noscript><img alt="يمكن استخدام الروابط لتمرير معلومات المعرف الخاصة بسياق ما إلى سياق آخر (مرتبط)" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-form-identifier-forwarding.png"></noscript></amp-img>

##### استخدام ميزات بديلة <a name="using-substitution-features"></a>

المنهج الخاص بنا سيستفيد من فئتين من [بدائل AMP المختلفة](https://github.com/ampproject/amphtml/blob/master/spec/./amp-var-substitutions.md).

**To update outgoing links to use a Client ID substitution:** Define a new query parameter, `ref_id` ("referrer ID"), which will appear within the URL and indicate the **originating context’s identifier** for the user. Set this query parameter to equal the value of AMP’s Client ID substitution:

[sourcecode:html]
<a
  href="https://example.com/step2.html?ref_id=CLIENT_ID(uid)"
  data-amp-replace="CLIENT_ID"
></a>
[/sourcecode]

**حل بديل لتمرير معرّف العميل إلى الروابط الخارجية:** حدد معلمة البحث الجديدة `ref_id` كجزء من سمة بيانات `data-amp-addparams` وبالنسبة للاستعلامات التي تحتاج إلى توفير بديل للمعلمة، قم بتوفير هذه التفاصيل كجزء من `data-amp-replace`. باستخدام هذا المنهج، سيبدو عنوان URL نظيفًا وستتم إضافة المعلمات المحددة في `data-amp-addparams` ديناميكيًا

[sourcecode:html]
<a
  href="https://example.com/step2.html"
  data-amp-addparams="ref_id=CLIENT_ID(uid)"
  data-amp-replace="CLIENT_ID"
></a>
[/sourcecode]

لتمرير معلمات استعلام متعددة من خلال `data-amp-addparams` قم بفصل كل من `&` بهذا الشكل

[sourcecode:html]
<a
  href="https://example.com/step2.html"
  data-amp-addparams="ref_id=CLIENT_ID(uid)&pageid=p123"
  data-amp-replace="CLIENT_ID"
></a>
[/sourcecode]

**لتحديث مدخلات النماذج لاستخدام بديل معرّف العميل:** حدد اسمًا لحقل الإدخال، مثل `orig_user_id`. ثم حدد `default-value` لحقل النموذج لتكون قيمة بديل معرّف عميل AMP:

[sourcecode:html]
<input
  name="ref_id"
  type="hidden"
  value="CLIENT_ID(uid)"
  data-amp-replace="CLIENT_ID"
/>
[/sourcecode]

By taking these steps, the Client ID is available to the target server and/or as a URL parameter on the page the user lands on after the link click or form submission (the **destination context**). The name (or "key") will be `ref_id` because that’s how we’ve defined it in the above implementations and will have an associated value equal to the Client ID. For instance, by following the link (`<a>` tag) defined above, the user will navigate to this URL:

[sourcecode:http]
https://example.com/step2.html?ref_id=$amp_client_id
[/sourcecode]

<amp-img alt="Example of how an identifier in an AMP viewer context can be passed via link into a publisher origin context" layout="responsive" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-identifier-forwarding-example-1.png" width="1038" height="890">
  <noscript><img alt="مثال على كيفية تمرير معرّف في سياق عارض AMP عبر رابط إلى سياق أصل الناشر" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-identifier-forwarding-example-1.png"></noscript></amp-img>

عندما يصل المستخدم إلى صفحة تحتوي على قيمة `ref_id` إما كمعلمة عنوان URL أو في رأس الصفحة، تتاح لنا الفرصة لمعالجة المعرّف `ref_id` جنبًا إلى جنب مع معرّف يتم عرضه عبر الصفحة نفسها (أي قيمة ملف تعريف الارتباط). من خلال تضمين كلاهما في رسائل فحص التحليلات، يمكن لخادم التحليلات الخاص بك العمل مع كلتا القيمتين في وقت واحد، ومع العلم أنهما مرتبطان، يعكسلن هذه العلاقة في الواجهة الخلفية الخاصة بك. تُقدم الخطوة التالية تفاصيل حول كيفية القيام بذلك.

##### استخراج معلمات استعلام عنوان URL <a name="extracting-url-query-parameters"></a>

باستخدام ميزات البديل، نقوم بإعداد تدفق تنقل الرابط أو تدفق إرسال النموذج الذي يعرض المعلومات، وتحديداً معرّف العميل، إلى الخادم المستهدف و/أو كمعلمة عنوان URL يمكن قراءته على العميل بمجرد إكمال المستخدم للتنقل.

إذا تم عرض المعلومات على الخادم فقط، على سبيل المثال عبر نموذج POST، فيمكنك متابعة معالجة المعلومات وتقديم الصفحة الناتجة. عند معالجة مثل هذه البيانات، يرجى ملاحظة الخطوات المتعلقة [مصادقة المعلمة](#parameter-validation) الموضحة بالتفصيل أدناه.

إذا كانت المعلومات متاحة عبر عنوان URL وترغب في معالجتها، فهناك طريقتان يمكنك استخدامهما:

- المعالجة أثناء إعادة التوجيه (معالجة من جانب الخادم)
- المعالجة على صفحة الوصول (المعالجة من جانب العميل)

**المعالجة أثناء إعادة التوجيه (معالجة من جانب الخادم)**

للمعالجة أثناء إعادة التوجيه، تعامل مع الطلب على الخادم واستخرج المعلمة (المعلمات) ذات الصلة. يرجى الإحاطة بالخطوات المتعلقة [بمصادقة المعلمة](#parameter-validation) الموضحة بالتفصيل أدناه. قم بمعالجة البيانات، جنبًا إلى جنب مع قيم ملفات تعريف الارتباط التي تحتوي على معرفات أخرى ذات صلة، ثم أعد التوجيه إلى عنوان URL لا يحتوي على المعلمات.

**المعالجة على صفحة الوصول (المعالجة من جانب العميل)**

للمعالجة على صفحة الوصول، سيختلف الأسلوب اعتمادًا على ما إذا كانت هذه الصفحة داعمة لـ AMP أو غير ذلك.

<amp-img alt="Example of how to construct an analytics ping that contains an identifier from the previous context provided via URL and an identifier from the current context" layout="responsive" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-identifier-forwarding-example-2.png" width="1326" height="828">
  <noscript><img alt="مثال على كيفية إنشاء تحليل ping يحتوي على معرف من السياق السابق المقدم عبر عنوان URL ومعرف من السياق الحالي" src="https://github.com/ampproject/amphtml/raw/master/spec/img/link-identifier-forwarding-example-2.png"></noscript></amp-img>

*Updates to AMP page:* Use the Query Parameter substitution feature in your amp-analytics configuration to obtain the `ref_id` identifier value within the URL. The Query Parameter feature takes a parameter that indicates the "key" of the desired key-value pair in the URL and returns the corresponding value. Use the Client ID feature as we have been doing to get the identifier for the AMP page context.

[sourcecode:http]
https://analytics.example.com/ping?type=pageview&orig_user_id=${queryParam(ref_id)}&user_id=${clientId(uid)}
[/sourcecode]

عندما يتم نقل هذا عبر الشبكة، سيتم استبدال القيم الفعلية:

[sourcecode:http]
https://analytics.example.com/ping?type=pageview&orig_user_id=$referrer_page_identifier&user_id=$current_page_identifier
[/sourcecode]

متابعة من خلال الأمثلة أعلاه، لدينا:

[sourcecode:text]
$referrer_page_identifier is $amp_client_id
$current_page_identifier is $publisher_origin_id
[/sourcecode]

لذلك فإن رسائل الفحص هي في الواقع:

[sourcecode:http]
https://analytics.example.com/ping?type=pageview&orig_user_id=$amp_client_id&user_id=$publisher_origin_id
[/sourcecode]

نوصي بالتحقق من صحة قيم معلمات الاستعلام باستخدام الخطوات الموضحة في قسم [التحقق من صحة المعلمة](#parameter-validation) أدناه.

*تحديثات لصفحة ليست AMP:* بالمثل، على صفحة ليست AMP يتم عرضها من أصل الناشر، استخرج وأرسل قيمة `ref_id` المضمنة في عنوان URL. تحقق من صحة القيمة باتباع الخطوات الموضحة في قسم [التحقق من صحة المعلمة](#parameter-validation) أدناه. بعد ذلك، أنشئ اختبارات فحص البيانات التحليلية التي ستتضمن كلاً من `orig_user_id` مشتق من `ref_id{/ code4} و<code data-md-type="codespan">user_id` استنادًا إلى قيمة معرف ملف تعريف ارتباط الطرف الأول.

<blockquote>
<p><strong>هام:</strong></p>
<p>إذا اخترت معالجة المعلمات من جانب العميل على الصفحة المقصودة، فيجب أن تزيل الصفحة المقصودة معلومات المعرف من عناوين URL بمجرد أن يتم التقاط المعرف.</p>
<p>قبل إزالة المعلمات، تأكد من أن التعليمات البرمجية الأخرى التي يجب تشغيلها لقراءتها إما:</p>
<ul>
  <li>تعمل قبل حدوث الإزالة؛ أو</li>
  <li>يمكنها الوصول إلى مكان حيث قامت التعليمة البرمجية التي قرأت وأزالت المعلمات بتخزين البيانات</li>
</ul>
<p>لإجراء ذلك على صفحتك التي ليست بتنسيق AMP، ضمِّن JavaScript التالية، والتي ستزيل جميع معلمات الاستعلام من عنوان URL:</p>
<pre>var href = location.href.replace(/\?[^{{'[% raw %]'}}#]{{'{% endraw %}'}}+/, '');<br>history.replaceState(null, null, href);</pre>
<p>قم بتعديل هذا حسب الحاجة لإزالة عدد أقل من معلمات الاستعلام.</p>
</blockquote>

##### معالجة معرّفات متعددة في رسالة فحص التحليلات <a name="processing-multiple-identifiers-in-an-analytics-ping"></a>

على عكس [المهمة 4](#task4) حيث قمنا بتكوين رسالة فحص التحليلات لاحتواء قيمة معرّف واحدة فقط، مع الخطوات التي اتخذناها حتى الآن في المهمة 5، أصبح لدينا الآن اثنان - `orig_user_id` و`user_id`. سنغطي بعد ذلك كيفية معالجة هذين المعرّفين اللذين يشكّلان جزءًا من رسالة فحص التحليلات الواردة.

قبل المضي قدمًا، تأكد من تدوين الخطوات الموضحة في [التحقق من صحة المعلمة](#parameter-validation) أدناه وتأكد من أنك على استعداد للثقة في كل من القيم المشار إليها بواسطة `orig_user_id` و`user_id`.

Check if either of the values corresponding to the inbound analytics ping are present in your mapping table. In our example above, the first pageview happens on an AMP page that’s NOT on the publisher origin followed by the second pageview that happens on the publisher origin. As a result, the values for the analytics ping query parameters will look like this:

**الحالة رقم 1: ترتيب المعرّف عند إرسال رسالة فحص التحليلات من الصفحة الموجودة في أصل الناشر**

<table>
  <tr>
    <th width="20%"></th>
    <th width="40%"><strong>معرف المستخدم في أصل الناشر</strong></th>
    <th width="40%"><strong>User ID on AMP page that’s NOT on publisher origin ("alias")</strong></th>
  </tr>
  <tr>
    <td><strong>كيف يتم التعبير عنها في رسالة فحص التحليلات</strong></td>
    <td><code>user_id=$publisher_origin_id</code></td>
    <td><code>orig_user_id=$amp_client_id</code></td>
  </tr>
  <tr>
    <td><strong>مفتاح المعلمة</strong></td>
    <td><code>user_id</code></td>
    <td><code>orig_user_id</code></td>
  </tr>
  <tr>
    <td><strong>قيمة المعلمة</strong></td>
    <td><code>$publisher_origin_id</code></td>
    <td><code>$amp_client_id</code></td>
  </tr>
</table>

يرجى ملاحظة أن المعرف القادم من مشاهدة الصفحة الأولى يتوافق مع العمود الموجود في أقصى اليمين وأن المعرف القادم من مشاهدة الصفحة الثانية موجود في العمود الأوسط، وفقًا لكيفية إنشاء مثالنا أعلاه.

إذا بدأ المستخدم بدلاً من ذلك في صفحة يتم عرضها من أصل الناشر ثم انتقل بعد ذلك إلى صفحة AMP ليست موجودة في أصل الناشر، فسيتم عكس مفاتيح المعلمات، ولكن الطريقة المقابلة التي نشير بها إلى القيم لن تكون (على سبيل المثال، يشير `$amp_client_id` دائمًا إلى معرف مخزن في صفحة AMP ليست موجودة في أصل الناشر):

**الحالة رقم 2: ترتيب المعرّف عندما يتم إرسال رسالة فحص التحليلات من صفحة AMP ليست موجودة في أصل الناشر**

<table>
  <tr>
    <th width="20%"> </th>
    <th width="40%"><strong>معرّف المستخدم في مصدر الناشر</strong></th>
    <th width="40%"><strong>User ID on AMP page that’s NOT on publisher origin ("alias")</strong></th>
  </tr>
  <tr>
    <td><strong>كيف يتم التعبير عنها في رسالة فحص التحليلات</strong></td>
    <td><code>orig_user_id=$publisher_origin_id</code></td>
    <td><code>user_id=$amp_client_id</code></td>
  </tr>
  <tr>
    <td><strong>مفتاح المعلمة</strong></td>
    <td><code>orig_user_id</code></td>
    <td><code>user_id</code></td>
  </tr>
  <tr>
    <td><strong>قيمة المعلمة</strong></td>
    <td><code>$publisher_origin_id</code></td>
    <td><code>$amp_client_id</code></td>
  </tr>
</table>

When you are searching the mapping table, take note of which situation applies and search for values within the columns of the mapping table where you expect them to appear. For instance, if the analytics ping is being sent from a page on the publisher origin (Case #1), then check for values keyed by `user_id` in the mapping table column "User ID on publisher origin" and check for values keyed by `orig_user_id` in the column "User ID on AMP page that’s NOT on publisher origin (‘alias’)".

إذا لم تتمكن من تحديد موقع قيمة المعرف المستخدمة في جدول التعيينات، فقم بإنشاء تعيين جديد:

- If the analytics request comes from a page on your publisher origin, then you should choose the value corresponding to `uid` to be the analytics record identifier; choose the value of `orig_uid` to be the "alias".
- If the analytics request does not come from a page on your publisher origin, then you should choose the value corresponding to `uid` to be an "alias" value in the mapping table. Then, proceed with the remaining instructions in [Task 4](#task4) to create a prospective publisher origin identifier and attempt to set this value as a cookie on the origin.

##### التحقق من صحة المعلمة <a name="parameter-validation"></a>

Values contained in a URL can be maliciously changed, malformed, or somehow otherwise not be the values that you expect to be there. This is sometimes called cross site request forgery. Just as it is important to ensure that the analytics pings that your analytics server receives are coming from pages that you expect to be sending analytics pings, when you are "forwarding" on values that were part of the URL, be sure to validate the referrer to ensure you can trust these values.

على سبيل المثال، في الخطوات المذكورة أعلاه، أنشأنا عنوان URL التالي، المخصص للمستخدم للنقر عليه والانتقال إلى الصفحة المقابلة:

[sourcecode:http]
https://example.com/step2.html?orig_user_id=$amp_client_id
[/sourcecode]

ومع ذلك، فمن الممكن تمامًا أن يقوم المستخدم أو أحد المهاجمين بتغيير عنوان URL هذا ليكون:

[sourcecode:http]
https://example.com/step2.html?orig_user_id=$malicious_value
[/sourcecode]

تريد التأكد من أنك لا تعالج سوى مثيلات `$amp_client_id` وتجنب استخدام مثيلات `$malicious_value`.

**الخطوات المقترحة للتحقق من صحة القيم اللتي تم استلامها عبر معلمات طلب بحث عنوان URL:** تأكد من مطابقة مرجع الصفحة المقصودة لعنوان URL الذي تتوقع رؤيته. يجب أن يكون هذا عادةً ما رأيته يحمل قيمة معرّف سبق رؤيتها في طلب صالح لمشاركة الموارد عبر المصادر (CORS). نوُصي بقبول مثل هذه المعرفات المعروفة فقط.

على إحدى الصفحات التي لا تدعم AMP، تحقق من `مرجع المستند` مباشرةً من جانب العميل أو مرِّر القيمة كجزء من اختبار ping للتحليلات لتتمكّن من التحقق من صحة جانب الخادم. إذا كانت قيمة المرجع واحدة يمكنك الوثوق بها، فيمكنك قبول القيم التي نشأت من عنوان URL للصفحة المقصودة ومعالجتها، مثل `orig_user_id` في المثال الوارد أعلاه.

على إحدى صفحات AMP، استخدم المتغير البديل [مرجع المستند](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md#document-referrer) substitution لتمرير قيمة المرجع كجزء من اختبار ping للتحليلات. المعالجة من جانب الخادم هي الخيار الوحيد المتوفر. للتوضيح، في ما يلي اختبار ping للتحليلات يمكن أن ترسله الصفحة المقصودة والذي يحتوي على (1) قيمة معرّف العميل للصفحة الحالية، (2) قيمة تم تمريرها من خلال عنوان URL الذي أعددناه ليكون قيمة معرّف العميل في الإحالة الصفحة و(3) معلومات المرجع نفسها للتحقق من قيمة (2): `https://analytics.example.com/ping?type=pageview&orig_user_id=${queryParam(ref_id)}&user_id=${clientId(uid)}&referrer=${documentReferrer}`

إذا كان لا يمكنك الوثوق بالمرجع، ارفض أي قيم يتم توفيرها عبر معلمات URL ولا تستخدمها.

## الممارسات الموُصى بها بشدة <a name="strongly-recommended-practices"></a>

### الاحتفاظ باقتران واحد فقط <a name="keep-just-one-association"></a>

**يجب الاحتفاظ باقتران واحد فقط بين المعرّفات من أي سياقين. ** إذا تمت مشاهدة معرّف عميل AMP الذي ربطته سابقًا بملف تعريف ارتباط أو معرّف مستخدم آخر صادر عنك مع ملف تعريف ارتباط جديد أو معرّف مستخدم جديد تصدره، فيجب عليك حذف جميع الحالات التي احتفظت بها مقابل ملف تعريف الارتباط ومعرّف السابق للمستخدم.

ستساعد هذه الخطوات على ضمان التوافق مع توقعات المستخدمين بشأن الخصوصية. كما هو مذكور بالتفصيل في الأقسام السابقة، غالبًا ما تتضمن إدارة حالة المستخدم في AMP تخزين وربط معرّفات مختلفة عبر سياقات متعددة حيث يتم عرض محتوى AMP. **يجب عدم إساءة استخدام هذا الموقف مطلقًا لإعادة تكوين البيانات أو إجراء تتبُّع لا يتوقعه المستخدم أو لم تكشف عنه بوضوح، على سبيل المثال، بعد حذف المستخدم لملفات تعريف الارتباط الخاصة به لمواقعك.**

### مراعاة ملفات تعريف الارتباط وحذف التخزين المحلي <a name="respect-cookie-and-local-storage-deletions"></a>

**يجب أن تراعي جميع عناصر التحكُّم في الخصوصية السارية والمتوفرة للمستخدم، بما في ذلك أي عناصر تحكُّم من هذا القبيل، مما يؤدي إلى القدرة على حذف جميع ملفات تعريف الارتباط والتخزين المحلي.** لا يجب استخدام معرّف عميل AMP أو البنية الأساسية لصفحات AMP في أي وقت [لإعادة تكوين معرّف محذوف](https://en.wikipedia.org/wiki/Zombie_cookie) بعد أن يحذف المستخدم صراحةً جانبًا واحدًا من علاقة المعرّف.

### الامتثال للقوانين واللوائح المحلية <a name="comply-with-local-laws-and-regulations"></a>

**قد يتطلب ربط ملفات تعريف الارتباط و/أو المعرفّات من نطاقين أو أكثر تحديث سياسة الخصوصية أو تقديم إفصاحات إضافية للمستخدم أو الحصول على موافقة المستخدم النهائي في بعض نطاقات السلطة.** يجب أن يحلّل كل ناشر استخدام معرّف عميل AMP، الذي يستخدم ملفات تعريف الارتباط أو التخزين المحلي كوسيلة للتخزين الدائم لتقديم معرّف ثابت، فيما يتعلق بجميع القوانين واللوائح السارية فيما يتعلق بجمع البيانات وتخزينها ومعالجتها وإعلام المستخدم بها.
