# البيانات المنظمة (Schema)

**النتيجة: 45/100** — https://samad.email/app/ — 2026-10-25

## ما يعمل جيداً

- Schema بصيغة JSON-LD موجود في HTML الخام (يقرؤه الزاحف دون JavaScript)
- النوع HomeAndConstructionBusiness (فرع من LocalBusiness) مناسب، مع عنوان بريدي ومنطقة خدمة

## المشكلات

### [عالٍ] صورة Schema معطلة وحقول أساسية مفقودة

**الدليل:** image تشير إلى icon-192.png (404). لا يوجد telephone ولا email ولا logo ولا geo ولا openingHoursSpecification ولا sameAs ولا priceRange. الأهلية للظهور المعزز في المحلي تضعف.

**الإصلاح:** إضافة: telephone (+966506898111)، email، logo (ملف موجود)، image (صورة حقيقية للفريق/المشروع)، geo (خط الطول والعرض)، openingHoursSpecification (24/7 للطوارئ)، sameAs (X، Instagram، رابط Google Maps)، و hasOfferCatalog للخدمات.

### [متوسط] لا يوجد Organization/WebSite/Service schema

**الدليل:** لا يوجد Organization مع logo و sameAs، ولا WebSite، ولا Service لكل خدمة، ولا BreadcrumbList. صفحات WordPress بلا أي Schema.

**الإصلاح:** إضافة Organization + WebSite على مستوى الموقع، و Service على كل صفحة خدمة، و NewsArticle على الأخبار الحقيقية، و BreadcrumbList.
