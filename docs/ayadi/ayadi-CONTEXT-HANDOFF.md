# أيادي — ملف نقل السياق (Context Handoff)
انسخ هذا كاملاً والصقه في بداية المحادثة الجديدة (كلاود سيشن). يلخّص كل ما أُنجز.

---

## 0. التعليمات للمساعد في المحادثة الجديدة
أنا أمين، مبتدئ في الباك-إند تماماً، وأتعلّم بالتدريس خطوة بخطوة (درس → سؤال تحقّق → الدرس التالي).
كلّمني بالعربية (لهجة ليبية مفهومة)، وبأسلوب مبسّط مع تشبيهات. لا تفترض معرفة سابقة بالباك-إند.
نبني مشروع "أيادي" على كلاود سيشن. ابدأ من حيث انتهينا (القسم 7 بالأسفل).

## 1. المشروع
- **أيادي:** تطبيق تبرّعات لليبيا، لـ **جهة حكومية ليبية** (الاسم لم يُحدَّد بعد). **ليس** لوزارة/هيئة الأوقاف.
- **الغرض:** مشروع بعلامة مستقلة. مرجع التصميم والوظائف: تطبيق "إحسان" السعودي (حُلّل بالهندسة العكسية). لا نسخ شعار/أصول إحسان ولا تكامل نفاذ الحكومي.
- **المستخدم مبتدئ:** الأولوية للتعلّم مع البناء.

## 1ب. الموقع من منصة البر الوقفية (قرار جديد — يتقدّم على أي شي يخالفه في الملفات الأخرى)
- **منصة البر الوقفية** (albir.ly + تطبيق) هي منصة **الهيئة العامة للأوقاف** الليبية، وتغطي **المشاريع الوقفية** (مساجد، آبار، إذاعات قرآن، طباعة مصاحف). حُلّلت في `albir-analysis.md` و `albir-app-analysis.md`.
- **أيادي مكمّل للبر، مو منافس.** يغطي اللي البر ما يغطيه:
  1. **حالات فردية:** علاج، سداد ديون، إيجار، فواتير لأشخاص محدّدين.
  2. **الزكاة:** حاسبة زكاة، زكاة الفطر، وصرفها لمستحقيها.
  3. **إغاثة وطوارئ:** كوارث، نازحين، سلال غذائية.
  4. **كفالة أيتام وأسر:** كفالات شهرية دورية.
- **قرارات لازم تُحسم مع أمين في بداية الجلسة A** (قبل تعديل SRS):
  - **الزكاة:** أموالها لازم تنفصل محاسبياً عن باقي التبرّعات (تُصرف لمصارف الزكاة فقط).
  - **الحالات الفردية:** خصوصية المستفيد (ما يظهر اسمه/صورته)، ومن يتحقق من الحالة، والمستندات المطلوبة، ولمين تُصرف الفلوس (حساب الجهة ولا حساب المستفيد).
  - **الكفالة الشهرية:** طرق الدفع الليبية (سداد، إدفع لي، رصيد الجوال) تعتمد على رمز تحقق في كل عملية، فالخصم التلقائي الشهري غالباً غير ممكن. البديل: تعهّد كفالة + تذكير شهري يدفع فيه المتبرّع بنفسه. يُتحقق من مزوّد الدفع.
  - **الدفع:** البر يستعمل مجمّع الدفع **Plutu** (حزمة `plutu_payment` في Flutter) ليغطي سداد وإدفع لي وتداول والبطاقات المحلية وليبيانا والمدار مرة وحدة. يستحق الدراسة بدل التكامل مع كل بوّابة لحالها.
- **تحديث SRS مطلوب في الجلسة A:** نطاق المرحلة الأولى القديم (حملات عامة فقط، والزكاة خارج النطاق) لم يعد صحيحاً.

## 2. نتيجة تحليل إحسان (المرجع التقني — Flutter)
- الإطار: Flutter. التنقّل: auto_route. الحالة: provider. الشبكة: dio.
- تخزين: hive + sqflite. التوكِن: flutter_secure_storage.
- Firebase (Messaging/Analytics/Crashlytics) + Google Maps + Rive (أنيميشن) + PDFium (إيصالات PDF).
- المصادقة: OTP بالجوال + access_token/refresh_token (Bearer).
- الحماية: RootBeer/rjsniffer/toolChecker (كشف Root/تلاعب).
- أكثر من 70 شاشة؛ محاور: تبرّع عام/سريع، حملات، زكاة (حاسبة)، وقف، أضاحي، أيتام، تبرّع بالدم، دوري، إهداء.

## 3. القرارات المعمارية (نهائية)
- **البنية:** تطبيق عميل (Flutter، Android أولاً) + لوحة أدمن ويب + خادم API واحد.
- **الخادم (backend):** Laravel (monolith نظيف، ليس microservices). Sanctum للتوكِنات، Filament للوحة الأدمن، Spatie للصلاحيات.
- **الدفع:** بوّابة ليبية (مرشّح قوي: مجمّع Plutu، أو معاملات/سداد مباشرة) — نحتاج وثائق المزوّد وحساب تاجر باسم الجهة لاحقاً. الآن نستخدم FakeGateway للتطوير.
- **قاعدة البيانات:** SQLite للتطوير، MySQL/PostgreSQL للنشر.
- **العملة:** الدينار الليبي (LYD). اللغات: عربي (أساسي) + إنجليزي.

## 4. آلية الأموال (مهم)
- التحصيل مركزي في حساب **الجهة الحكومية** (حساب التاجر لدى البوّابة).
- كل حملة تحمل حقل beneficiary_account (+ beneficiary_bank) يحدّده الأدمن عند الإنشاء = وجهة الصرف.
- (قرار مبدئي) رقم الحساب حر يكتبه الأدمن؛ يمكن لاحقاً تحويله لقائمة بنوك معتمدة.
- كل تبرّع يُسجَّل: المبلغ + الحملة + حساب المستفيد المقصود + الحالة. تحقّق الدفع من جهة الخادم فقط.

## 5. مبادئ علّمناها (المستخدم فهمها)
- الباك-إند = المطبخ؛ التطبيق = الصالة؛ قاعدة البيانات = المخزن؛ API = النادل.
- قاعدة البيانات = جداول مترابطة بالـ id (JOIN).
- Endpoint = فعل (GET/POST/PUT/DELETE) + عنوان؛ الرد JSON + رمز حالة (200/201/400/401/404/500).
- المصادقة: OTP مرة → توكِن (سوار) يُرسل في ترويسة Authorization: Bearer مع كل طلب.
- البنية: Route → Controller → Service → Model. افصل المنطق في Service.
- **Never trust the client:** كل تحقّق حاسم في الباك-إند.
- PHP لغة تُنزَّل كـ package (مثل Python)، لا تحتاج Apache/XAMPP؛ Laravel يجي معه سيرفر مدمج (php artisan serve).

## 6. حالة بيئة المستخدم (ويندوز، جاهزة)
مثبّت ويعمل: Git، Node/npm، VS Code، PHP 8.5.12، Composer 2.10.3 (عبر Laravel Herd، PHP في ~/.config/herd/bin).
القرار: العمل على **كلاود سيشن** (ليس لوكال). تطبيقات Flutter/Android Studio تُؤجَّل للمرحلة ب.

## 7. ما تم بناؤه (أعِد بناءه في الجلسة الجديدة — Laravel)
أنشأنا خادم Laravel باسم ayadi-api، فعّلنا API+Sanctum، SQLite، وأول ميزة (campaigns) تعمل:
GET /api/campaigns و GET /api/campaigns/{id} (اختُبرت ونجحت، 200 و 404).

### أوامر الإنشاء:
```
composer create-project laravel/laravel ayadi-api
cd ayadi-api
php artisan install:api --no-interaction
touch database/database.sqlite   # وفي .env: DB_CONNECTION=sqlite (واحذف بقية سطور DB_)
php artisan make:model Campaign -m
# عدّل migration + model + controller + routes (أدناه) ثم:
php artisan migrate
php artisan serve
```

### database/migrations/*_create_campaigns_table.php (داخل up()):
```php
Schema::create('campaigns', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('description')->nullable();
    $table->decimal('goal_amount', 12, 2);
    $table->decimal('collected_amount', 12, 2)->default(0);
    $table->string('status')->default('published');
    $table->string('beneficiary_account')->nullable();
    $table->string('beneficiary_bank')->nullable();
    $table->timestamps();
});
```

### app/Models/Campaign.php:
```php
protected $fillable = [
    'title','description','goal_amount','collected_amount',
    'status','beneficiary_account','beneficiary_bank',
];
```

### app/Http/Controllers/CampaignController.php:
```php
public function index() {
    return response()->json(Campaign::where('status','published')->get());
}
public function show($id) {
    $campaign = Campaign::find($id);
    if (!$campaign) return response()->json(['error'=>'الحملة غير موجودة'], 404);
    return response()->json($campaign);
}
```

### routes/api.php:
```php
Route::get('/campaigns', [CampaignController::class, 'index']);
Route::get('/campaigns/{id}', [CampaignController::class, 'show']);
```

## 8. الخطوة التالية المقترحة (الدرس 7)
POST /api/campaigns (إنشاء حملة) → ثم المصادقة (OTP) → ثم التبرّع (Donation + Service + FakeGateway) → ثم الإيصال PDF.
علّم أولاً، ثم نفّذ، بنفس أسلوب الدرس/السؤال.

## 9. مخرجات سابقة بحوزة المستخدم
- ayadi-blueprint.md (مخطّط معماري من تحليل إحسان).
- ayadi-SRS.md (وثيقة متطلبات المرحلة الأولى).
