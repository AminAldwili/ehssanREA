# بلوبرنت منصة البر الوقفية — تحليل كامل من الألف إلى الياء
**المصادر:** موقع `albir.ly` (الصفحات وملفات الجافاسكربت والـ CSS)، وتطبيق Android `ly.albiralwaqfiyah.app` إصدار 1.2.2 (ملف XAPK).
**الطريقة:** تحليل ثابت فقط. ما شغّلنا التطبيق، وما طلبنا خادم الـ API (`api.albir.ly`) مباشرة.
**الرموز:** ✅ دليل مباشر · 🔎 استنتاج قوي · ❓ غير مؤكد.
**التاريخ:** 3 نوفمبر 2026.

---

## 1. الهوية والملكية
| البند | القيمة |
|---|---|
| الاسم | منصة البر الوقفية ✅ |
| المالك | «إحدى مبادرات» **الهيئة العامة للأوقاف والشؤون الإسلامية** (رابط `awqaf.gov.ly`) ✅ |
| الوصف الرسمي | «منصة إلكترونية تعمل على ميكنة سبل مشاركة الأفراد والمؤسسات في دعم المشاريع الوقفية المختلفة» ✅ |
| المنفّذ التقني | **شركة العنكبوت الليبي (Libyan Spider)** — «الشريك المنفذ» ✅ |
| شركاء | **شركة معاملات للخدمات المالية** (شريك فضي، المشغّل الرسمي للموزّع الوطني عبر شبكة نمو) ✅، **المصرف الإسلامي الليبي** ✅، تطبيق **بريستو** (اتفاقية تعاون) ✅، زيارة من **البنك الإسلامي للتنمية** ✅ |
| القنوات | موقع ويب + تطبيق Android وiOS (روابط المتجرين في الكود) ✅ |
| الأرقام المعروضة | 4,195,761.96 د.ل · 49,249 مساهمة · 6,047 مساهم ✅ |
| الآية الشعار | ﴿لَن تَنَالُوا الْبِرَّ حَتَّىٰ تُنفِقُوا مِمَّا تُحِبُّونَ﴾ ✅ |

---

## 2. فكرة المنتج
المنصة **متخصصة في الوقف**: المشاريع ذات النفع الدائم (مساجد، آبار، إذاعات قرآن، طباعة مصاحف، تحلية مياه، مراكز تحفيظ، تكييف وفرش مساجد). ✅

### التصنيفات ✅
| التصنيف | التعريف (من الموقع) |
|---|---|
| **الكل** | — |
| **أساس** | مشاريع وقفية ترتبط بالأصول الوقفية الدائمة |
| **روافد** | مشاريع نفعية دائمة تقدّم خدمات مستمرة للمستفيدين |
| **صدقة جارية** | «مصرف وقفي مستدام» تُجمع فيه **فوائض المشاريع بعد اكتمالها + المساهمات المباشرة**، ويُعاد توجيهها حسب الأولويات |
| **إبراء الذمة** | مصرف لاستقبال الأموال غير المشروعة أو المشتبهة، تُصرف في المصالح العامة |

### الباقات ✅
«قائمة تحوي مجموعة من المشاريع الوقفية التي تشترك في ذات المصرف الوقفي»:
- **الباقة الأكاديمية:** رعاية طلاب الجامعات والمعاهد في التخصصات غير الشرعية.
- **باقة المساجد:** صيانة وترميم واستكمال المساجد ومراكز التحفيظ وإنشاء مساجد جديدة.
- **الباقة العلمية:** تعليم علوم الكتاب والسنة، المكتبات والمراجع، كفالة أهل العلم.
- **الباقة التعاونية:** أنشطة اجتماعية وتعاونية خيرية.

### وسوم حالة المشروع ✅
مستمر · مستعجل · مميز.

### أقسام أخرى
- **الحملات** («حملات البر»، «كل الحملات») ✅ — طبيعتها بالضبط ❓ (تجميعة مشاريع تنشرها الإدارة 🔎).
- **تقديم مشروع:** الجمهور يقترح مشروعاً وقفياً، «ليباشر فريقنا المختص، بالتنسيق مع مكتب الأوقاف، دراسة الجدوى الفنية والشرعية» ✅. يحتاج تسجيل دخول في التطبيق ✅.
- **الأخبار**، **الشركاء**، **الإحصائيات**، **الأسئلة الشائعة**، **من نحن / عن الأوقاف**، **اتصل بنا** (هاتف، بريد، واتساب) ✅.

---

## 3. المعمارية العامة
```
                 ┌──────────────────────────┐
  المتبرّع ─────►│ موقع albir.ly            │  Nuxt 3 (Vue) + SSR
                 └────────────┬─────────────┘
                              │ https://api.albir.ly/clientarea
                 ┌────────────▼─────────────┐
                 │ خادم API واحد            │  🔎 Laravel (Sanctum + Fortify)
                 │ (بيئة تجريبية:            │
                 │  api.albir.demo.ly)       │
                 └────────────▲─────────────┘
                              │ https://api.albir.ly/mobile
                 ┌────────────┴─────────────┐
  المتبرّع ─────►│ تطبيق Flutter             │  Android/iOS + Shorebird
                 └──────────────────────────┘
  خدمات خارجية: مجمّع الدفع Plutu + بوّابات ليبية · Firebase · OneSignal · Huawei Push
                Sentry (glitchtip.server.ly) · Google Analytics · Cloudflare Turnstile
```
- **خادم واحد ومدخلين:** `/clientarea` للموقع و `/mobile` للتطبيق، وبنفس أسماء المسارات تقريباً ✅.
- **بيئة تجريبية منفصلة:** `api.albir.demo.ly/mobile` ✅.
- **التطبيق قالب جاهز:** مسار البناء `ls_app_development/app/custom/albir_alwaqfiyah` وحزم خاصة غير منشورة (`core`، `ignitor`، `astra`، `communicator`، `state_management`، `extended_ui`، `local_storage`، `dynamic_forms`، `simple`) ✅ → العنكبوت الليبي تعيد استعمال نفس الإطار لعملاء آخرين 🔎.

---

## 4. التقنيات

### الموقع ✅
| المجال | التقنية |
|---|---|
| الإطار | Nuxt 3 (Vue 3) مع SSR، Vuex |
| الواجهة | UIkit (مكوّنات `uk-*`) |
| النماذج | vee-validate (`veeForm`) |
| الـ API | طبقة موحّدة: كل نداء معرّف بـ `{method, url, entity}` (72 تعريفاً) |
| الخط | SST Arabic (Roman/Bold) |
| المراقبة | Sentry ذاتي الاستضافة على `glitchtip.server.ly` |
| التحليلات | Google Analytics (`G-GLFXVN8Z88`) |
| الحماية | Cloudflare Turnstile |
| SEO | schema.org (Nuxt Schema.org)، og tags |

### الخادم 🔎
- **Laravel**: الأدلة `/auth/csrf-cookie` (Sanctum)، `/auth/tfa/register|enable|disable|recovery-codes` (Fortify)، و `?_method=patch` (أسلوب Laravel لتمرير PATCH عبر POST).
- **نموذج بيانات متجر إلكتروني:** المشروع = `product`، المساهمة = `order`، مع `cart` و `checkout` و `attributes` ✅.

### التطبيق ✅ (من أسماء حزم Dart)
| المجال | الحزم |
|---|---|
| الحالة | riverpod، hooks_riverpod، flutter_hooks، state_notifier |
| التنقّل | auto_route |
| الشبكة | dio + interceptors إضافية |
| النماذج | freezed، json_annotation، copy_with_extension، equatable، fpdart (برمجة وظيفية) |
| الفورمات | reactive_forms، reactive_phone_form_field، pin_code_fields، reactive_pin_code_fields، sms_autofill، phone_numbers_parser |
| التخزين | hive، flutter_secure_storage، shared_preferences، sqflite |
| الدفع | **plutu_payment**، webview_flutter |
| الإشعارات | firebase_messaging، OneSignal، **huawei_push**، flutter_local_notifications |
| Firebase | core، analytics، crashlytics، remote_config، app_check، auth |
| الواجهة | flex_color_scheme، skeletonizer، shimmer، infinite_scroll_pagination، cached_network_image، flutter_svg، smooth_page_indicator، sliding_up_panel، readmore، badges، dotted_border، photo_view، expandable، flexi_chip، custom_sliding_segmented_control، responsive_framework |
| HTML | flutter_widget_from_html_core (عرض محتوى الأخبار والصفحات) |
| الخرائط | google_maps_flutter، map_launcher |
| التحديث | **Shorebird** (تحديث الكود بدون المتجر)، upgrader (يطلب تحديث التطبيق) |
| متفرقات | shake، share_plus، open_filex، file_picker، image_picker، flutter_image_compress، device_info_plus، flutter_udid، package_info_plus، app_settings، permission_handler، logger |
| الحماية | `com.pairip` (فحص ترخيص Google Play)، Firebase App Check |

### بنية كود التطبيق (feature-first) ✅
```
albir_alwaqfiyah/
├── feature/
│   ├── authentication   ├── home            ├── project
│   ├── donation         ├── cart            ├── checkout
│   ├── payment          ├── contributions   ├── campaigns
│   ├── packages         ├── partners        ├── news
│   ├── pages            ├── statistics      ├── project_submission
│   ├── client_notifications ├── contact_us  ├── settings
│   ├── platform         ├── attributes      ├── main
│   ├── maintanence      └── blocked_device
├── shared/  ui · utils · domain · analytics · additional_interceptors · dev_tools
└── generated/l10n
```

---

## 5. خريطة الصفحات والشاشات

### صفحات الموقع ✅
| المسار | الصفحة |
|---|---|
| `/` | الرئيسية |
| `/donations` | مشاريع البر (القائمة) |
| `/donations/:id` | تفاصيل المشروع |
| `/donations/:id/checkout` | التبرّع المباشر لمشروع |
| `/cart` · `/cart/checkout` | سلة البر · إتمام السلّة |
| `/campaigns` · `/campaigns/:id` | الحملات · تفاصيل حملة |
| `/packages` | الباقات |
| `/partners` | الشركاء |
| `/statistics` | الإحصائيات |
| `/news` · `/news/:slug` | الأخبار · خبر |
| `/project-submission` | تقديم مشروع |
| `/faqs` · `/about-us` · `/contact-us` · `/:slug` | صفحات المحتوى |
| `/auth/login` · `/auth/signup` | الدخول · التسجيل |
| `/account` | الحساب: طلبات المساهمة، مساهماتي، الملف، الإشعارات |
| `/account/my-donations/:order/complete` | إكمال الدفع لطلب |

**قائمة التنقّل** ✅: الرئيسية، مشاريع البر، الباقات، الحملات، الشركاء، الإحصائيات، الأخبار، حول المنصة، اتصل بنا، الإشعارات، طلبات المساهمة، مساهماتي، تسجيل الدخول، تسجيل حساب جديد، سياسة الخصوصية، الشروط والأحكام.

### شاشات التطبيق ✅ (أسماء auto_route)
| المجموعة | الشاشات |
|---|---|
| الأساس | Main، Home، TabPage، Redirect |
| المشاريع | ProjectDetails، ProjectContributions، ProjectLocations (خريطة)، ProjectQueryCategories، ProjectQuerySearch، ImagesViewer |
| الحملات والباقات | Campaigns، CampaignDetails، Packages |
| التبرّع | Cart، Checkout، CardPaymentWebView، OfflinePayment، Donations، DonationDetails، Contributions |
| الحساب | Auth، Login، Register، EmailVerification، ForgotPassword، ChangePassword، Account، UpdateAccount، UpdateCorporateAccount، AccountDeletion، PhoneVerifiable، OtpSourceSelection |
| المحتوى | News، NewsDetails، Pages، FAQs، About، Partners، Statistics، ContactUsForm، ProjectSubmissionForm |
| النظام | Settings، ClientNotifications، NotificationsTroubleshooting، MaintenanceMode، BlockedDeviceMode، ForceUpdate |
| المطوّرين | DevTools، Logging، LocalHistory |

### أقسام الصفحة الرئيسية (بالترتيب) ✅
1. شريط مشاريع مميّزة (بطاقة: العنوان، وصف قصير، الهدف، المتبرّع، النسبة، «ساهم الآن»)
2. التصنيفات (الكل، أساس، روافد، صدقة جارية، إبراء الذمة)
3. **المشاريع العاجلة**
4. **المشاريع المتاحة**
5. **الباقات**
6. **المشاريع المكتملة** (مع «اكتمل يوم ...»)
7. الأخبار
8. الشركاء
9. إحصائيات وأرقام (إجمالي المبلغ، عدد المساهمات، عدد المساهمين، عدد المشاريع)
10. الآية + قاعدة الحد الأدنى + التذييل

### صفحة المشروع ✅
العنوان، الوسوم (مستمر/مستعجل/مميز)، الوصف، **التصنيف، المدينة، الباقة، المعرّف**، الهدف / تم جمع / المتبقي / النسبة، **قيمة المساهمة** (مبالغ مقترحة + حقل مخصّص)، «ساهم الآن» و«أضف إلى السلة»، **إحصائيات المشروع**: عدد المساهمات، آخر مساهمة، عدد الزيارات، تاريخ آخر زيارة.

---

## 6. نموذج البيانات

### المشروع (`product`) ✅
```
id, name, type, type_localized, image, cities[], donatable,
end_at_diff_for_humans, contribution_amounts[],
statistics {
  formatted_amount            // الهدف
  formatted_paid_amount       // تم جمع
  remaining_amount, formatted_remaining_amount   // المتبقي
  donatable_remaining_amount  // أقصى مبلغ مسموح
  overfund_amount, formatted_overfund_amount     // الزائد عن الحاجة
}
```
+ من الصفحة: التصنيف، الباقة، المعرّف (رقم من 6 خانات مثل 631932)، الوسوم، عدد الزيارات، آخر زيارة/مساهمة ✅.

### بقية الكيانات (من أسماء الـ API) ✅
| الكيان | ملاحظات |
|---|---|
| `categories` | أساس، روافد، صدقة جارية، إبراء الذمة |
| `packages` | مجموعة مشاريع بنفس المصرف الوقفي |
| `campaigns` | ❓ |
| `attributes` | خصائص للفلترة 🔎 (مثل المدينة) |
| `cart` | عناصر: مشروع + مبلغ |
| `orders` | `order_number`، `payment_method`، حالة (منها «بانتظار الدفع» `order_number_awaiting_payment`) |
| `payment-methods` | تُجلب حسب المبلغ `?amount=` |
| `user` | أفراد: `first_name, last_name, email, mobile/phone, password`؛ وجهات: `/user/profile/corporate` |
| `notifications` | إشعارات المستخدم |
| `news` | مع تصنيفات وسنوات و slug |
| `pages` | صفحات ثابتة بـ slug |
| `partners`، `banners`، `stats`، `platform`، `contribution`، `faq`، `contact` | محتوى وإعدادات |
| `project-submission` | نموذج ديناميكي 🔎 (حزمة `dynamic_forms` في التطبيق) |

---

## 7. قواعد العمل ✅
1. **الحد الأدنى للمساهمة 5 د.ل.**
2. **المبالغ المقترحة** تأتي من الخادم لكل مشروع (`contribution_amounts`). إذا غابت، الافتراضي في الكود `[5, 50, 100]`.
3. **الحد الأعلى** = المتبقي القابل للتبرّع (`donatable_remaining_amount`).
4. **إذا المتبقي أقل من 5 د.ل:** يُقبل 5 د.ل، و«سيتم تخصيص المبلغ الزائد لمشاريع أُخرى أكثر احتياجاً» (`overfund_amount`).
5. **فوائض المشاريع المكتملة تذهب لمصرف «الصدقة الجارية»** ويُعاد توزيعها.
6. **الخادم يحدّد طرق الدفع المتاحة حسب المبلغ** (`GET /payment-methods?amount=`).
7. **المشروع المكتمل** يُعرض في «المشاريع المكتملة» مع تاريخ الاكتمال.
8. **الطلب يمكن إلغاؤه** (`/orders/{id}/cancel`) أو **تغيير طريقة دفعه** (`update-payment-method`).
9. **رقم الهاتف:** المدار يبدأ بـ 091 أو 093، وليبيانا بـ 092 أو 094 ✅.
10. رسائل الأخطاء تشمل «رصيد غير كافي» و«ألا يتجاوز إجمالي المساهمة {amount}» ✅.

---

## 8. مسارات الاستخدام

### أ. التبرّع المباشر لمشروع ✅
```
صفحة المشروع → يختار مبلغ (مقترح أو مخصّص)
→ GET  /payment-methods?amount=X          (الطرق المتاحة)
→ POST /products/{id}/quick-order  { amount, payment_method, notes }
← { order_number, payment_method }
→ ينتقل إلى /account/my-donations/{order_number}/complete
   (طرق Plutu: ?submit=auto → يبدأ الدفع تلقائياً)
→ الدفع حسب الطريقة (القسم 9)
→ رسالة نجاح «تمت عملية الدفع بنجاح، جزاك الله خيراً على مساهمتك»
```

### ب. السلّة ✅
```
POST /cart (إضافة) → PATCH /cart (تعديل المبالغ) → PATCH /cart/remove → DELETE /cart (تفريغ)
→ POST /checkout { payment_method, notes, ... } ← order_number → نفس مسار الدفع
```

### ج. الحساب
- **الموقع:** تسجيل بالبريد وكلمة المرور ← تأكيد البريد (`/auth/email/verify/confirm|resend`) ← دخول. استعادة كلمة المرور. **تحقق بخطوتين** اختياري ✅.
- **التطبيق:** نفس الشي، ورمز تأكيد البريد يُرسل للبريد ✅. وفيه شاشة **اختيار طريقة استلام رمز التحقق** (SMS أو واتساب) و **تحقق رقم الهاتف** ✅ — متى تُستعمل بالضبط ❓.
- **نوعا الحساب** ✅: أفراد (بيانات شخصية)، وجهات (يسجّلها الشخص المفوّض رسمياً، للمساهمة تحت بند المسؤولية الاجتماعية).
- طلب حذف الحساب وإلغاؤه ✅.

### د. تقديم مشروع ✅
تسجيل الدخول ← نموذج المقترح ← إقرار «أقر بأن البيانات المقدمة صحيحة، وأعلم أن تقديم هذا النموذج لا يعني اعتماد المشروع» ← «تم إرسال المقترح بنجاح».

### هـ. الإشعارات (التطبيق) ✅
تسجيل الجهاز `/fcm/register`، وحذفه عند الخروج `/fcm/delete`، وشاشة «حل مشاكل الإشعارات». ثلاث قنوات: Firebase وOneSignal وHuawei Push.

### و. تحكم الخادم في التطبيق ✅
وضع **الصيانة**، و**حظر الجهاز**، و**التحديث الإجباري**، و Remote Config.

---

## 9. الدفع بالتفصيل

### الطرق المعروضة ✅
البطاقة المصرفية · سداد · ليبيانا · المدار الجديد · تداول · وان باي · إدفع لي · تحويل مصرفي · صك · نقداً.
الواجهة تقسمها: «طرق الدفع الإلكترونية» و «طرق الدفع المصرفية» ✅.

### المعرّفات والمسارات (الموقع) ✅
كل المسارات تبدأ بـ `/orders/{id}/payment/`:

| الطريقة | المعرّف | الخطوات |
|---|---|---|
| بطاقات محلية | `localbankcards` / `localbankcards_plutu` | `localbankcards/confirm` |
| بطاقات دولية | `mpgs_plutu` | `mpgs/confirm` ← تحويل لصفحة البوّابة |
| سداد | `sadad_plutu` | `sadad/verify` ← رمز SMS ← `sadad/confirm` |
| إدفع لي | `edfali_plutu` | `edfali/verify` ← رمز ← `edfali/confirm` |
| ليبيانا | `libyana_credit` | `libyana/verify` ← رمز ← `libyana/confirm` |
| المدار | `almadar_credit` | `almadar/verify` ← رمز ← `almadar/confirm` |
| تداول | `tlync_plutu` | `tlync/confirm` ← تحويل |
| يسر باي | `yussorpay_plutu` | `yussorpay/confirm` ← تحويل |
| مصرفي باي | `musrefypay_plutu` | `musrefypay/confirm` ← تحويل |
| وان باي | `onepay` | ❓ |
| غير إلكتروني | تحويل مصرفي / صك / نقد | `offline` (مع «رقم الحوالة/الصك» في التطبيق) |

- **Plutu** هو مجمّع الدفع لأغلب الطرق (اللاحقة `_plutu`، وحزمة `plutu_payment` في التطبيق) ✅.
- نمط **verify ← confirm**: الأول يرسل رمز تحقق للجوال، والثاني يؤكّد به 🔎 (نصوص التطبيق تؤكد إرسال رسالة نصية فيها رمز).

### الدفع في التطبيق ✅
- **سداد:** «أدخل رقم الهاتف المسجل في خدمة سداد وسنة الميلاد ثم اضغط تأكيد» ← رمز برسالة نصية.
- **إدفع لي:** «أدخل رقم هاتفك المشترك في خدمة ادفعلي ثم اضغط تحقق» ← رمز.
- **تداول:** «الدفع عن طريق بطاقة تداول».
- **بطاقة:** شاشة WebView (`CardPaymentWebViewRoute`).
- **غير إلكتروني:** شاشة `OfflinePaymentRoute`.
- مسار عام `/orders/{id}/payment/{payment_key}/confirm`، ومسار `/orders/{number}/payment/pay-order` لإكمال طلب قائم.
- مسارات `/api/e-topup/{sadad|edfali|tlync}/...` ✅ — معناها الدقيق ❓.

---

## 10. مرجع الـ API

### القاعدة
- الموقع: `https://api.albir.ly/clientarea`
- التطبيق: `https://api.albir.ly/mobile` (تجريبي: `https://api.albir.demo.ly/mobile`)

### القائمة الكاملة (الموقع، 72 تعريف) ✅
**GET**
`/home` `/stats` `/platform` `/banners` `/categories` `/attributes` `/packages` `/partners` `/contribution` `/products` `/products/{id}` `/campaigns` `/campaigns/{id}` `/payment-methods` `/payment-methods/{id}` `/cart` `/orders` `/orders/{id}` `/notifications` `/news` `/news/{id}` `/news/categories` `/news/years` `/pages/{id}` `/faq` `/contact` `/project-submission` `/user/profile` `/auth/csrf-cookie`

**POST**
`/auth/login` `/auth/signup` `/auth/logout` `/auth/password/forgot` `/auth/password/reset` `/auth/email/verify/confirm` `/auth/email/verify/resend` `/auth/tfa/register` `/auth/tfa/enable` `/auth/tfa/disable` `/auth/tfa/recovery-codes`
`/cart` `/checkout` `/products/{id}/quick-order`
`/orders/{id}/payment/{localbankcards|mpgs|tlync|yussorpay|musrefypay}/confirm`
`/orders/{id}/payment/{sadad|edfali|libyana|almadar}/verify` و `/confirm`
`/orders/{id}/payment/offline` `/orders/{id}/update-payment-method?_method=patch`
`/contact` `/project-submission` `/media/download` `/user/deletion-inquiry` `/user/profile/corporate?_method=patch`

**PATCH**
`/cart` `/cart/remove` `/orders/{id}/cancel` `/user/profile`

**DELETE**
`/cart` `/notifications/{id}` `/user/deletion-inquiry`

### إضافات التطبيق ✅
`/fcm/register` `/fcm/delete` `/notify-me` `/orders/{number}/payment/pay-order` `/orders/{id}/payment/{payment_key}/confirm` `/api/e-topup/{sadad|edfali|tlync}/{verify|confirm}`

---

## 11. التصميم

### الألوان (من ملفات الـ CSS والـ SVG) ✅
| الدور (🔎 من التكرار والسياق) | اللون |
|---|---|
| الأخضر الأساسي | `#18825A` |
| أخضر غامق | `#1A5E42` · `#0A4E33` · `#03452F` |
| ذهبي/أصفر (تمييز) | `#FFB936` |
| بيج (خلفيات دافئة) | `#F4ECD8` · `#EFE5CD` · `#D1C5A9` · `#DECBA5` |
| بنّي (نصوص على البيج) | `#433422` |
| داكن جداً (أيقونات التطبيق) | `#0A071C` |
| خطأ | `#DE0000` |

الهوية: **أخضر + ذهبي + بيج دافئ**، أهدأ وأكثر «تراثية» من أخضر إحسان المؤسسي 🔎.

### الخط والأيقونات ✅
- خط **SST Arabic** (Light/Roman/Medium/Bold) في الموقع والتطبيق.
- أيقونات خاصة `app_icons.otf` في التطبيق.
- أيقونات التصنيفات بحالتين (عادي/مفعّل): `all`، `asas`، `rawafed`، `running_charity`، `debt_relief`.
- أيقونات الدفع: `sadad`، `edfali`، `card`، `cash`، `bank`.
- أيقونات إحصائيات المشروع: `contributions_count`، `last_donation`، `last_visit`، `last_view`، `projects_count`، `beneficiaries`، `completed_projects`، `completed_goal`، `completed_time`.
- رسوم الحالات الفارغة: `empty_cart`، `empty_notification`، `no_result`، `no_connection`، `maintenance`.
- خلفيات: `bg.png`، `card_bg_left/right.svg`، `home_divider.png`، `home_title.png`.

### أنماط الواجهة 🔎
- بطاقة مشروع: صورة، عنوان، المدينة، المتبقي، شريط تقدّم بالنسبة، زر «ساهم الآن».
- تحميل هيكلي (skeleton) وتمرير لانهائي في التطبيق.
- لوحة منزلقة من الأسفل (sliding_up_panel) غالباً لاختيار المبلغ أو الدفع.

---

## 12. التشغيل والأمان ✅
- **مراقبة الأخطاء:** Sentry/GlitchTip ذاتي (الموقع)، Crashlytics (التطبيق).
- **التحليلات:** Google Analytics، Firebase Analytics، Huawei Analytics.
- **الحماية:** Cloudflare Turnstile (الموقع)، Firebase App Check + فحص ترخيص Play (`pairip`) (التطبيق)، حظر الأجهزة من الخادم، تحقق بخطوتين للحساب.
- **التحديث:** Shorebird (فوري)، وupgrader وForceUpdate (إجباري من المتجر).
- **أجهزة هواوي:** دعم HMS كامل (Push، Analytics) لأن بعضها بدون خدمات جوجل.
- **بيئات:** إنتاج وتجريبي منفصلين.

---

## 13. دروس لأيادي (مكمّل للبر)
**نأخذها:**
1. **مجمّع الدفع Plutu** بدل التكامل مع كل بوّابة لحالها (يغطي سداد، إدفع لي، تداول، البطاقات المحلية والدولية، يسر باي، مصرفي باي).
2. **قواعد المبالغ:** حد أدنى، مبالغ مقترحة من الخادم لكل مشروع، حد أعلى = المتبقي، والزائد يروح لمصرف عام.
3. **مصرف عام دائم** يستقبل الفوائض والتبرّعات المباشرة (عندهم «صدقة جارية»؛ عندنا مثلاً «صندوق الحالات العاجلة» أو «صدقة عامة»).
4. **الخادم يحدّد طرق الدفع حسب المبلغ.**
5. **حساب أفراد + حساب جهات** (الشركات تتبرّع تحت المسؤولية الاجتماعية).
6. **تحكم الخادم:** صيانة، تحديث إجباري، حظر جهاز.
7. **Huawei Push** إلى جانب Firebase.
8. **خادم واحد بمدخلين** (ويب وجوال) وبيئة تجريبية منفصلة.
9. **نفس تقنيات التطبيق اللي اخترناها:** riverpod، auto_route، dio، hive، secure_storage، freezed، feature-first.

**نتميّز فيها (اللي البر ما يغطيه):**
1. **الحالات الفردية** مع خصوصية المستفيد والتحقق من الحالة.
2. **الزكاة** بحساب منفصل محاسبياً وحاسبة زكاة.
3. **الإغاثة والطوارئ** بنشر سريع للحملات العاجلة.
4. **كفالة أيتام وأسر** شهرية (مع حل لمشكلة الدفع الدوري في الطرق الليبية).
5. **الدخول برقم الجوال + OTP** (البر يعتمد البريد وكلمة المرور، وهذا أثقل على المستخدم الليبي 🔎).

---

## 14. ما لم يُتحقق منه ❓
- شكل الطلبات والردود الفعلية من الـ API (يحتاج إذناً بطلب `api.albir.ly` أو مراقبة شبكة التطبيق وقت التشغيل).
- طبيعة «الحملات» بالضبط.
- معنى `e-topup` و `notify-me`.
- متى تُستعمل شاشتا اختيار طريقة رمز التحقق وتحقق رقم الهاتف، واستعمال firebase_auth.
- لوحة الإدارة (غير ظاهرة للعامة).
- نسخة iOS (حلّلنا Android فقط).
