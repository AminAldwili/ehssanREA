# تحليل منصة البر الوقفية (albir.ly)
مصدر التحليل: صفحات الموقع وملفات الجافاسكربت المحمّلة من `albir.ly` (3 نوفمبر 2026).
لم يُطلب أي شي من خادم الـ API (`api.albir.ly`) مباشرة.

## من يملكها
- الاسم: **منصة البر الوقفية**، «إحدى مبادرات» **الهيئة العامة للأوقاف والشؤون الإسلامية** (رابط `awqaf.gov.ly` في التذييل).
- المنفّذ التقني: **شركة العنكبوت الليبي (Libyan Spider)**.
- شركاء: **شركة معاملات للخدمات المالية** (شريك فضي)، **المصرف الإسلامي الليبي**.
- لها تطبيق جوال (روابط Google Play و App Store في الكود).
- أرقامها المعروضة: 4,195,761.96 د.ل، 49,249 مساهمة، 6,047 مساهم.

## التقنية
| الطبقة | الدليل |
|---|---|
| الواجهة | Nuxt 3 (Vue) مع SSR، مكوّنات UIkit، Vuex |
| الخادم | API على `https://api.albir.ly/clientarea`. مسارات `/auth/csrf-cookie` و `/auth/tfa/*` و `?_method=patch` = نمط **Laravel (Sanctum + Fortify)** — استنتاج قوي من الأسماء |
| المراقبة | Sentry ذاتي الاستضافة (glitchtip.server.ly) + Google Analytics |
| الحماية | Cloudflare Turnstile (challenges.cloudflare.com) |
| الخط | SST Arabic |

## نموذج البيانات (مثل متجر إلكتروني)
- المشروع = **product**، والمساهمة = **order**، وفيه **cart** و **checkout**.
- حقول المشروع المستعملة: `id, name, type, type_localized, image, cities, donatable, end_at_diff_for_humans, contribution_amounts`
  و `statistics: { formatted_amount, formatted_paid_amount, remaining_amount, formatted_remaining_amount, donatable_remaining_amount, overfund_amount }`.
- صفحة المشروع تعرض: الحالة (مستمر/مستعجل/مميز)، التصنيف، المدينة، الباقة، المعرّف، الهدف، تم جمع، المتبقي، النسبة، عدد المساهمات، آخر مساهمة، عدد الزيارات.

## التصنيفات والباقات
- تصنيفات: **أساس** (مرتبطة بأصول وقفية دائمة)، **روافد** (خدمات مستمرة)، **صدقة جارية**، **إبراء الذمة**.
- «صدقة جارية» = مصرف وقفي تُجمع فيه **فوائض المشاريع المكتملة + المساهمات المباشرة**، ويُعاد توجيهها حسب الأولوية. في الرئيسية يظهر كمشروع «مشروع الصدقة الجارية» يعرض «تم جمع» بدون هدف.
- «إبراء الذمة» = مصرف للأموال المشتبهة، يُصرف في المصالح العامة.
- الباقات (مجموعة مشاريع بنفس المصرف): الأكاديمية، المساجد، العلمية، التعاونية.
- أقسام الرئيسية: المشاريع العاجلة، المتاحة، الباقات، المكتملة (مع تاريخ الاكتمال)، الأخبار، الشركاء، الإحصائيات.

## قواعد المبالغ
- الحد الأدنى **5 د.ل**.
- المبالغ المقترحة تأتي من الخادم (`contribution_amounts`)، وإذا غابت فالافتراضي في الكود `[5, 50, 100]`.
- الحد الأعلى = `donatable_remaining_amount` (المتبقي).
- إذا المتبقي أقل من 5 د.ل، يُقبل 5 د.ل و **الزائد يُخصّص لمشاريع أكثر احتياجاً** (`overfund_amount`).

## مسار التبرّع
1. **تبرّع مباشر:** `POST /products/{id}/quick-order` بالحقول `amount, payment_method, notes`.
   **أو بالسلّة:** `POST /cart` ← `PATCH /cart` ← `POST /checkout`.
2. قبل الدفع يطلب `GET /payment-methods?amount=...` (الخادم يحدّد الطرق المتاحة حسب المبلغ).
3. الخادم يرجع `order_number`، فينتقل المستخدم لصفحة `/account/my-donations/{order}/complete`.
4. الدفع حسب الطريقة:

| الطريقة | الاسم في الواجهة | المسار |
|---|---|---|
| بطاقات مصرفية محلية | البطاقة المصرفية | `localbankcards/confirm` (أو عبر Plutu) |
| بطاقات دولية (MPGS) | — | `mpgs/confirm` ← تحويل لصفحة البوّابة |
| سداد | سداد | `sadad/verify` ثم `sadad/confirm` |
| إدفع لي | إدفع لي | `edfali/verify` ثم `edfali/confirm` |
| ليبيانا (رصيد) | ليبيانا | `libyana/verify` ثم `libyana/confirm` (الرقم يبدأ 092/094) |
| المدار (رصيد) | المدار الجديد | `almadar/verify` ثم `almadar/confirm` (الرقم يبدأ 091/093) |
| تداول | تداول | `tlync/confirm` ← تحويل |
| يسر باي / مصرفي باي | — | `yussorpay/confirm` / `musrefypay/confirm` ← تحويل |
| وان باي | وان پای | (أيقونة فقط) |
| دفع مصرفي غير إلكتروني | تحويل مصرفي / صك / نقداً | `POST /orders/{id}/payment/offline` |

   كل المسارات تبدأ بـ `/orders/{id}/payment/`. أغلب الطرق تمر عبر مجمّع الدفع **Plutu** (`*_plutu`).
   نمط verify ← confirm: الأول يرسل رمز تحقق، والثاني يؤكّد الدفع به (استنتاج من الأسماء).
5. يمكن تغيير طريقة الدفع لطلب قائم (`update-payment-method`) أو إلغاؤه (`/orders/{id}/cancel`).

## الحساب والهوية
- تسجيل بالبريد وكلمة مرور (`/auth/signup`, `/auth/login`)، تأكيد البريد، استعادة كلمة المرور، **تحقق بخطوتين (TFA)**.
- حقول تظهر في الكود: `first_name, last_name, email, mobile/phone, password, password_confirmation`، وملف **مؤسسي** منفصل (`/user/profile/corporate`).
- طلب حذف الحساب وإلغاؤه.
- لا يوجد دليل على OTP بالجوال للدخول.

## خدمات أخرى
- **تقديم مشروع** من الجمهور (`/project-submission`).
- حملات (`/campaigns`)، أخبار، صفحات ثابتة، أسئلة شائعة، إشعارات، تنزيل ملفات (`/media/download`، غالباً الإيصال أو الشهادة).

## سجل الـ API الكامل (72 تعريف، القاعدة `https://api.albir.ly/clientarea`)
GET: `/home /stats /platform /banners /categories /attributes /packages /partners /products /products/{id} /campaigns /campaigns/{id} /contribution /payment-methods /payment-methods/{id} /cart /orders /orders/{id} /notifications /news /news/{id} /news/categories /news/years /pages/{id} /faq /contact /project-submission /user/profile /auth/csrf-cookie`
POST: `/auth/login /auth/signup /auth/logout /auth/password/forgot /auth/password/reset /auth/email/verify/confirm /auth/email/verify/resend /auth/tfa/register|enable|disable|recovery-codes /cart /checkout /products/{id}/quick-order /orders/{id}/payment/* /orders/{id}/update-payment-method /contact /project-submission /media/download /user/deletion-inquiry /user/profile/corporate`
PATCH: `/cart /cart/remove /orders/{id}/cancel /user/profile`
DELETE: `/cart /notifications/{id} /user/deletion-inquiry`

## ما لم يُتحقق منه
- شكل الردود الفعلية من الـ API (ما طلبنا الـ API مباشرة).
- تفاصيل خطوات كل بوّابة دفع داخلياً.
- لوحة الإدارة (غير ظاهرة للعامة).
