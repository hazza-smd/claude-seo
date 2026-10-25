# SEO التقني (Technical SEO)

**النتيجة: 55/100** — https://samad.email/app/ — 2026-10-25

## ما يعمل جيداً

- HTTPS مفعّل، ووقت استجابة الخادم سريع (~110–120 مللي ث في Lighthouse)
- robots.txt يسمح بالزحف ويشير إلى خريطة الموقع
- meta robots = index,follow و canonical ذاتي صحيح على /app/
- lang="ar" و dir="rtl" معرّفان بشكل صحيح، و viewport سليم
- لا يوجد CLS على الجوال (0)

## المشكلات

### [حرج] المحتوى يُعرض من جهة العميل فقط (Client-Side Rendering)

**الدليل:** HTML الخام لـ /app/ يحتوي 3 كلمات و 0 عناوين و 0 روابط. كل المحتوى (الخدمات، المشاريع، الأخبار، الشركاء) يُجلب من Supabase عبر JavaScript بعد التحميل. جوجل يستطيع العرض لكن بتأخير وبموارد محدودة، وأغلب زواحف الذكاء الاصطناعي (GPTBot، ClaudeBot، PerplexityBot) لا تنفّذ JavaScript فترى صفحة فارغة.

**الإصلاح:** تقديم المحتوى الأساسي (H1، الخدمات، من نحن، بيانات التواصل) في HTML الأولي: إما عرض مسبق (prerender/SSG) عند كل تحديث في Supabase، أو تضمين نسخة ثابتة داخل HTML ثم تحديثها بالـ JS.

### [عالٍ] صفحة /app/ يتيمة وغير موجودة في خريطة الموقع

**الدليل:** wp-sitemap.xml لا يتضمن /app/، والصفحة الرئيسية https://samad.email/ لا تحتوي أي رابط إلى /app/. اكتشاف الصفحة يعتمد على الروابط الخارجية فقط.

**الإصلاح:** إضافة /app/ إلى خريطة الموقع (أو خريطة مستقلة يُشار إليها في robots.txt) وربطها من القائمة الرئيسية ومن تذييل موقع WordPress.

### [عالٍ] حظر GPTBot بالخطأ 429 على /app/

**الدليل:** طلبات بـ User-Agent الخاص بـ GPTBot تحصل على 429 (Too Many Requests) بشكل متكرر على /app/، بينما تحصل على 200 على الصفحة الرئيسية. Googlebot و ClaudeBot و PerplexityBot و Bingbot حصلت على 200. يبدو أنها حماية من الاستضافة (Hostinger/LiteSpeed).

**الإصلاح:** مراجعة إعدادات حماية البوتات في hPanel/LiteSpeed والسماح لـ GPTBot و OAI-SearchBot إن كان الظهور في ChatGPT مطلوباً. التحقق من سجلات الخادم لعناوين GPTBot الحقيقية.

### [عالٍ] روابط/موارد مكسورة (404)

**الدليل:** https://samad.email/app/icon-192.png و icon-512.png تعيد 404 (مستخدمة في og:image و Schema و manifest و apple-touch-icon وإشعارات sw.js). /favicon.ico يعيد 404. في الصفحة الرئيسية: project_8.png و project_9.png تعيد 404.

**الإصلاح:** رفع الملفات المفقودة أو تصحيح المسارات، ثم التحقق بأداة Rich Results Test و Facebook Sharing Debugger.

### [متوسط] حجم HTML كبير جداً (1.37 ميجابايت) بسبب صور base64 مضمّنة

**الدليل:** حوالي 588 كيلوبايت من HTML هي صور مضمّنة بصيغة base64 (PNG/SVG)، و ~106 كيلوبايت من JavaScript غير مستخدم داخل الصفحة. كل زيارة تُنزّل كل ذلك ولا يمكن تخزين الصور مؤقتاً بشكل منفصل.

**الإصلاح:** نقل الصور إلى ملفات خارجية بصيغة WebP/AVIF مع Cache-Control طويل، وفصل JavaScript الخاص بلوحة التطبيق (تسجيل الدخول، الحجوزات) عن صفحة الهبوط العامة.

### [متوسط] غياب ترويسات الأمان

**الدليل:** لا توجد Strict-Transport-Security ولا X-Content-Type-Options ولا Referrer-Policy ولا Permissions-Policy. سياسة CSP الحالية تحتوي فقط upgrade-insecure-requests.

**الإصلاح:** إضافة HSTS (max-age=31536000; includeSubDomains) و X-Content-Type-Options: nosniff و Referrer-Policy: strict-origin-when-cross-origin عبر .htaccess أو hPanel.

### [متوسط] كشف أسماء المستخدمين ومعلومات WordPress

**الدليل:** /wp-json/wp/v2/users يكشف اسم المستخدم الإداري (it@smd.com.sa)، وصفحة المؤلف /author/itsmd-com-sa/ مفهرسة، و /readme.html متاح، وترويسة X-Powered-By تكشف إصدار PHP 8.3.35.

**الإصلاح:** تعطيل نقطة users في REST API للزوار، وإعادة توجيه أو منع فهرسة صفحات المؤلف والتصنيف Uncategorized، وحذف readme.html، وإخفاء X-Powered-By.

### [منخفض] النسخة الإنجليزية من /app/ غير قابلة للفهرسة

**الدليل:** زر EN يغيّر اللغة من جهة العميل على نفس الرابط، فلا يوجد عنوان URL مستقل للمحتوى الإنجليزي ولا hreflang. /en/app/ يعيد 404.

**الإصلاح:** إن كان الجمهور الإنجليزي مهماً: إنشاء /en/app/ بمحتوى معروض مسبقاً مع hreflang متبادل (ar, en, x-default).
