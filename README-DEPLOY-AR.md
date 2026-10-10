# PM System Online — Vercel و Android APK

## محتوى النسخة

- `index.html`: الواجهة الأونلاين.
- `manifest.webmanifest` و`icon.svg`: تثبيت كتطبيق PWA.
- `sw.js`: Service Worker للـ PWA بدون Cache؛ التطبيق يحتاج الإنترنت دائمًا.
- `vercel.json`: إعدادات نشر Static على Vercel.

## ما تم تعطيله

- كروت Word/PDF ومجلدات الكروت المحلية.
- الكتابة والتعديل في تقارير الصيانة اليومية.
- Local fallback لبيانات المعدات والسجلات.
- Firebase Firestore/Auth هو مصدر البيانات؛ عند فقد الاتصال تظهر شاشة تطلب إعادة الاتصال.

تبقى متاحة: المعدات، القراءات/Records، جدول الصيانة، أوامر الصيانة، المستخدمون والصلاحيات، وسجل البيانات الموجود في Firestore.

## النشر على Vercel — الطريقة الأسهل

1. أنشئ مستودع GitHub جديدًا.
2. ارفع الملفات الموجودة في هذا المجلد إلى جذر المستودع، وليس مجلدًا داخليًا.
3. من Vercel اختر **Add New Project → Import Git Repository**.
4. الإعدادات:
   - Framework Preset: `Other` أو `No Framework`
   - Build Command: اتركه فارغًا
   - Output Directory: `.` أو اترك الافتراضي
5. اضغط Deploy.
6. من Firebase Console أضف رابط Vercel إلى:
   - Authentication → Settings → Authorized domains
   - مثال: `your-project.vercel.app`
7. بعد تثبيت دومين مخصص، أضفه أيضًا إلى قائمة الدومينات المسموحة في `index.html` بدل الاعتماد على نطاق Vercel العام.

> لا تضع Service Account JSON أو أي مفتاح Firebase خاص داخل الموقع. إعداد Firebase الموجود في الواجهة هو Web config عام، وحماية البيانات الحقيقية يجب أن تكون في Firestore Security Rules وAuthentication.

## تحويله إلى APK

بعد نشره على رابط HTTPS نهائي:

1. افتح `https://www.pwabuilder.com/`.
2. ضع رابط Vercel.
3. اختر **Build My PWA → Android**.
4. نزّل حزمة Android الناتجة، ثم وقّعها بمفتاح Android خاص بك.
5. للاستخدام الشخصي يمكن تثبيت APK مباشرة بعد السماح بالتثبيت من المصدر المناسب.
6. للنشر على Google Play استخدم Android App Bundle (`.aab`) ووقّعه بمفتاح ثابت لا تفقده.

الـ APK هنا عبارة عن تطبيق Android يفتح النسخة السحابية HTTPS. لا يحتوي على نسخة بيانات محلية، لذلك لا يعمل لإضافة أو مشاهدة البيانات عند انقطاع الإنترنت.

## اختبار سريع قبل APK

- تسجيل الدخول بحساب Admin.
- فتح Equipment وRecords وMaintenance Schedule.
- تجربة إضافة قراءة/سجل من جهازين مختلفين.
- التأكد أن الحسابات تُقرأ من Firebase Auth والملفات الشخصية من Firestore.
- فصل الإنترنت والتأكد من ظهور رسالة الاتصال بدل استخدام بيانات محلية.
- التأكد من أن Cards وتقارير الصيانة اليومية للقراءة فقط/معطلة حسب المطلوب.
