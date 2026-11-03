# تحليل تطبيق منصة البر الوقفية (Android 1.2.2)
الحزمة: `ly.albiralwaqfiyah.app` · الإصدار 1.2.2 (464) · minSdk 29 · targetSdk 36 · نسخة armeabi-v7a.
المصدر: ملف XAPK اللي رفعه أمين. التحليل ثابت (بدون تشغيل التطبيق).
يكمّل هذا التحليل ملف `albir-analysis.md` (تحليل الموقع).

## الصورة العامة
- **Flutter** (Dart AOT في `libapp.so`)، مع **Shorebird** لتحديث الكود بدون المتجر.
- مبني على **إطار داخلي لشركة العنكبوت الليبي**: مسار البناء في الكود `ls_app_development/app/custom/albir_alwaqfiyah`، وحزم خاصة غير منشورة: `core`، `ignitor`، `astra`، `communicator`، `state_management`، `extended_ui`، `local_storage`، `dynamic_forms`، `simple`. يعني التطبيق «تخصيص» فوق قالب يعيدون استعماله لعملاء مختلفين (استنتاج من المسار والأسماء).
- يتكلم مع **نفس الخادم** اللي يخدم الموقع، بس بمدخل مختلف: `https://api.albir.ly/mobile` (الموقع: `/clientarea`). وفيه بيئة تجريبية `https://api.albir.demo.ly/mobile`.

## التقنيات (من أسماء حزم Dart داخل الكود)
| المجال | الحزم |
|---|---|
| الحالة | riverpod / hooks_riverpod / flutter_hooks |
| التنقّل | auto_route |
| الشبكة | dio (+ interceptors إضافية) |
| النماذج والبيانات | freezed، json_annotation، copy_with_extension، fpdart، equatable |
| الفورمات | reactive_forms، phone_form_field، pin_code_fields، sms_autofill |
| التخزين | hive، flutter_secure_storage، shared_preferences، sqflite |
| الدفع | **plutu_payment** (حزمة Plutu)، webview_flutter |
| الإشعارات | firebase_messaging، OneSignal، huawei_push (أجهزة هواوي بدون خدمات جوجل)، flutter_local_notifications |
| Firebase | core، analytics، crashlytics، remote_config، app_check، auth |
| الواجهة | flex_color_scheme، skeletonizer، shimmer، infinite_scroll_pagination، cached_network_image، flutter_svg، smooth_page_indicator، sliding_up_panel، readmore، badges |
| الخرائط | google_maps_flutter، map_launcher |
| أخرى | upgrader (يطلب تحديث التطبيق)، shake، share_plus، open_filex، file_picker، image_picker، device_info_plus، flutter_udid، responsive_framework |
| الحماية | `com.pairip` (فحص ترخيص Google Play) |

## بنية الكود (feature-first)
`feature/` لكل ميزة، و `shared/` للمشترك:
authentication · home · project · donation · cart · checkout · payment · contributions · campaigns · packages · partners · news · pages · statistics · project_submission · client_notifications · contact_us · settings · platform · attributes · maintanence · blocked_device
و `shared/ui`، `shared/utils`، `shared/domain`، `shared/analytics`، `shared/additional_interceptors`، `generated/l10n`.

## ميزات تخص التطبيق (مو موجودة في الموقع)
- **تسجيل جهاز الإشعارات:** `/fcm/register` و `/fcm/delete`.
- **`/notify-me`:** غالباً تنبيه عند توفر شي (المعنى ما ظهر بالضبط).
- **`/orders/{number}/payment/pay-order`:** إكمال دفع طلب قائم.
- **`/api/e-topup/...`** لسداد وإدفع لي وتداول: مسار دفع ثاني بنفس الطرق (المعنى الدقيق غير مؤكد).
- **صيانة** و **جهاز محظور** (شاشات خاصة يتحكم فيها الخادم).
- **هزّ الجوال** (حزمة shake) — الاستعمال غير مؤكد.

## الدفع داخل التطبيق
- **سداد:** «أدخل رقم الهاتف المسجل في خدمة سداد وسنة الميلاد ثم اضغط تأكيد» ← رمز تحقق برسالة نصية ← تأكيد.
- **إدفع لي:** «أدخل رقم هاتفك المشترك في خدمة ادفعلي ثم اضغط تحقق» ← رمز ← تأكيد.
- **تداول:** «الدفع عن طريق بطاقة تداول».
- **دفع غير إلكتروني:** خانة «رقم الحوالة/الصك».
- باقي الطرق عبر `/orders/{id}/payment/{payment_key}/confirm` (غالباً صفحة البوّابة داخل WebView).
- رسالة «رصيد غير كافي» موجودة.

## الحساب
- **نوعين:** حساب **أفراد** (بيانات شخصية)، وحساب **جهات** (يسجّله الشخص المفوّض من الجهة، للمساهمة تحت بند المسؤولية الاجتماعية).
- الدخول بالبريد وكلمة المرور، والتحقق من البريد برمز يُرسل إليه.
- فيه «يرجى اختيار طريقة استلام رمز التحقق» مع أيقونات SMS وواتساب، بس ما اتضح وين يُستعمل بالضبط.
- تقديم مشروع يحتاج تسجيل دخول، ويُدرس «بالتنسيق مع مكتب الأوقاف».

## الأصول والتصميم
- خط **SST Arabic** (نفس الموقع)، وأيقونات خاصة `app_icons.otf`.
- أيقونات التصنيفات بحالتين (عادي/مفعّل): الكل، أساس، روافد، صدقة جارية (`running_charity`)، إبراء الذمة (`debt_relief`).
- أيقونات طرق الدفع: سداد، إدفع لي، بطاقة، نقد، مصرف.
- أيقونات إحصائيات المشروع: عدد المساهمات، آخر مساهمة، آخر زيارة، المستفيدين، المكتملة.

## ما لم يُتحقق منه
- شكل طلبات وردود الـ API الفعلية (يحتاج فك ترجمة Dart أو مراقبة الشبكة وقت التشغيل).
- استعمال firebase_auth بالضبط (ممكن للتحقق من رقم الجوال، بدون دليل).
- معنى `e-topup` و `notify-me` بدقة.
