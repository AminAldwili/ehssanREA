# أيادي — مخطّط بناء تطبيق تبرّعات (مرجع: تحليل إحسان 4.0.1)

> نتيجة هندسة عكسية لتطبيق إحسان (Flutter) باستخدام REA + JADX + Ghidra.
> الغرض: بناء تطبيق **أيادي** الخاص بعلامته وخوادمه الخاصة. لا تُنسخ شعارات إحسان
> ولا أصوله ولا تكامل نفاذ الحكومي؛ تُؤخذ الأفكار والمعمارية والوظائف فقط.

## 1. التقنية المستخدمة (مؤكّدة من التحليل)

| الطبقة | إحسان يستخدم | توصية أيادي |
|---|---|---|
| الإطار | Flutter (Dart AOT، `libapp.so`) | Flutter 3.x |
| التنقّل | `auto_route` (أسماء الشاشات تنتهي بـ Route) | auto_route |
| الحالة (State) | `provider` | provider أو riverpod |
| شبكة HTTP | `dio` | dio + retrofit |
| تخزين محلي | `hive` + `sqflite` | hive (سريع) + sqflite |
| تخزين آمن للتوكِن | `flutter_secure_storage` | نفسه |
| الإشعارات | Firebase Messaging + `flutter_local_notifications` | نفسه |
| التحليلات/الأعطال | Firebase Analytics + Crashlytics | نفسه |
| الخرائط/الموقع | Google Maps + GMS Location | نفسه |
| الأنيميشن | Rive (`librive_native.so`, ملفات .riv) | Rive |
| عرض PDF (الإيصالات) | PDFium (`libjniPdfium.so`) | `pdfx` أو `syncfusion_pdfviewer` |
| تسجيل صوتي | `record` (بحث صوتي) | record + speech_to_text |
| WebView (بوابة الدفع) | `flutter_inappwebview` | نفسه |
| مشاركة | `share_plus` | نفسه |
| منتقي الصور | `image_picker` | نفسه |
| الحماية | RootBeer + rjsniffer + toolChecker (كشف Root/تلاعب) | RootBeer أو freerasp |

## 2. الصلاحيات المطلوبة (من الـ Manifest)
`INTERNET`, `ACCESS_FINE/COARSE_LOCATION`, `POST_NOTIFICATIONS`, `RECORD_AUDIO`
(البحث الصوتي), `READ_CONTACTS` (إهداء تبرّع لجهة اتصال), `READ_EXTERNAL_STORAGE`,
`VIBRATE`, `RECEIVE_BOOT_COMPLETED`, `FOREGROUND_SERVICE`, `WAKE_LOCK`,
`c2dm RECEIVE` (FCM).

## 3. المصادقة (Auth)
- تعتمد **OTP عبر الجوال** + **Bearer / access_token** (مؤكّد من النصوص:
  `/user/otp/validate`, `access_token`, `Bearer`, `flutter_secure_storage`).
- إحسان يربط نفاذ (هوية وطنية) — **حكومي حصري**. في أيادي:
  - التسجيل برقم جوال + OTP.
  - توثيق الهوية اختياري لاحقاً عبر مزوّد تجاري (Uqudo / IDMeta) إن احتجته.
- التدفّق: إدخال رقم → إرسال OTP → تحقّق → استلام access_token + refresh →
  حفظه في secure_storage → تمريره في ترويسة كل طلب.

## 4. خريطة الشاشات (مستخرجة — أكثر من 70 شاشة)
**الأساسية:** Splash → Onboarding → Register/OTP → Home → Search.

**محاور التبرّع:**
- تبرّع عام / سريع / حملات (Campaigns): إنشاء حملة، حملتي، تقارير الحملة.
- الزكاة: ZakatCalculator (حاسبة: نقد/ذهب/أسهم/صناديق)، زكاة الفطر.
- الوقف (Waqf): وحدات وقفية، شهادة وقف، وقف القرآن، تخصيص المجال/المنطقة.
- الأضاحي (Adahi): طلب أضحية، تفاصيل الطلب، طلباتي.
- الأيتام ودور الرعاية (Orphanages/Organizations).
- التبرّع بالدم (Blood Donation): حجز موعد، تفاصيل الموعد.
- التبرّع الدوري (Periodic Donation).
- الإهداء (Gifts): إضافة مُهدى إليهم، إيصال هدية.
- تبرّع SMS، تنقية الأسهم (StockPurification).

**المستخدم:** UserProfile, PersonalInfo, SavedCards, DonationsLog, DonationsCart,
Refund, Invoice, Notifications, Settings, Accessibility, Language, AccountDeletion.

**محتوى:** EhsanJourney (رحلة المتبرّع + أوسمة/نقاط), Statistics, Partners,
ShariaCommittee, FAQ, About, Privacy, SLA, Suggestions, Contact.

**حماية:** RootNotSupported (يُغلق التطبيق على الأجهزة المكسورة الحماية).

## 5. نموذج الـ API (مستخرج من الخوادم الحقيقية — للمحاكاة في خادمك)
الخوادم: `api.ehsan.sa` (API), `cms.ehsan.sa` (محتوى), S3 (صور).
نماذج مسارات نبنيها بنظير في أيادي:
```
POST /api/user/otp/validate
GET  /api/donationProject                 قائمة المشاريع
GET  /api/donationProject/completedInitiatives
GET  /api/donation-initiative/top-initiatives
POST /api/campaign-donation               تبرّع لحملة
GET  /api/usercampaign/myCampaigns
GET  /api/donation/basicDonationDetails/{id}
GET  /api/donation/invoiceDetails/{id}
POST /api/donationTransactionDonors/save
GET  /api/zakat-alfiter/current
GET  /api/waqf/user-donation
GET  /api/orphanages/GetAllOrphans
GET  /api/user_dashboard_amounts | /user_dashboard_points
POST /api/notification/save-user-token    ربط FCM token
GET  /api/payment-options/
POST /api/payment/authenticate            بدء الدفع (3-D Secure عبر WebView)
GET  /api/donation/validateAllowRefund/{id}
```
> كلها أفكار بنية؛ تُعاد كتابتها بالكامل على خادمك (Node/NestJS أو Laravel).

## 6. الدفع
- بوّابة دفع عبر **WebView** (PaymentWebviewRoute, `/payment/authenticate`,
  صفحات success/failed/error/deniedByRisk). نمط 3-D Secure قياسي.
- في السعودية: مدى / Apple Pay / بطاقات عبر مزوّد مثل HyperPay أو Moyasar أو Tap.
- حفظ البطاقات (SavedCards) = توكِن من مزوّد الدفع، لا تُخزَّن أرقام البطاقات.

## 7. خطة بناء أيادي (مراحل)
1. **الأساس:** مشروع Flutter + auto_route + dio + provider + secure_storage + DI.
2. **الهوية:** تسجيل OTP + إدارة التوكِن + انترسبتر تجديد تلقائي.
3. **الواجهة الرئيسية:** Home + قائمة المشاريع + بحث + تفاصيل مشروع.
4. **التبرّع الأساسي:** سلة + دفع عبر Moyasar/HyperPay + إيصال PDF.
5. **المحاور:** زكاة (حاسبة) → وقف → حملات → دوري → إهداء، تدريجياً.
6. **الإشعارات:** FCM + local notifications + ربط التوكِن بالخادم.
7. **الحماية والجودة:** كشف Root (freerasp)، Crashlytics، Analytics، i18n (عربي/إنجليزي).

## 8. ملاحظات قانونية
- أيادي علامة مستقلة؛ لا تستخدم اسم/شعار/أيقونات إحسان ولا ملفات .riv الخاصة به.
- الخطوط (IBM Plex Arabic) مفتوحة المصدر — يجوز استخدامها. AdelleSans و Maqroo تجارية — استبدلها.
- لا تكامل مع أنظمة حكومية (نفاذ/يقين) دون ترخيص رسمي.
- أنشئ حساباتك الخاصة: Firebase، مزوّد الدفع، الخرائط.
