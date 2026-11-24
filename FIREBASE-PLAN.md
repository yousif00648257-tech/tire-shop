# پلانی Firebase / Firebase plan

بارودۆخ: ڕاگیراوە، بۆ دواتر پاشەکەوتکراوە (2026-11-24).
Status: paused, saved for later (2026-11-24).

## ئێستا لە کوێین / Where we are

- ئەپەکە لەسەر ئینتەرنێتە: https://yousif00648257-tech.github.io/tire-shop/
- کڕیاران دەتوانن تایەکان ببینن و داوای خزمەتگوزاری بکەن.
- ئەوەی هێشتا کار ناکات:
  - **بەشی بەڕێوەبردن:** زیادکردنی تایە و وێنە لەلایەن خاوەنەوە.
  - **داواکارییەکانی کڕیار:** ناگەنە لای خاوەن، تەنها لە مۆبایلی کڕیارەکە پاشەکەوت دەبن.
- چارەسەر: Firebase، کە بەخۆڕاییە و پێویستی بە کارتی بانکی نییە.

## هەنگاوەکانی خاوەن / Owner's steps (in the Firebase console)

1. **پرۆژە:** https://console.firebase.google.com
   - **Create a project**، ناو: `tire-shop`
   - Google Analytics بکوژێنەوە ← **Create project**
2. **ئەپی وێب:** ئایکۆنی `</>` دابگرە
   - ناو: `tire-shop`، Hosting **دامەنێ** ← **Register app**
   - کۆدی `firebaseConfig` کۆپی بکە و بۆ Claude ی بنێرە. ئەم کۆدە نهێنی نییە.
3. **Authentication:**
   - **Build → Authentication → Get started → Google → Enable → Save**
   - پاشان **Settings → Authorized domains → Add domain** و ئەمە زیاد بکە: `yousif00648257-tech.github.io`
4. **Firestore:**
   - **Build → Firestore Database → Create database**
   - Location: ئەوروپا (`europe-west`) ← **Start in production mode** ← **Create**
   - لە تابی **Rules**، یاساکانی خوارەوە دابنێ و ئیمەیڵی Gmail ی خۆت بنووسە ← **Publish**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isOwner() {
      return request.auth != null
        && request.auth.token.email == "YOUR_GMAIL@gmail.com"
        && request.auth.token.email_verified == true;
    }
    match /tires/{id} {
      allow read: if true;
      allow write: if isOwner();
    }
    match /settings/{id} {
      allow read: if true;
      allow write: if isOwner();
    }
    match /bookings/{id} {
      allow create: if request.resource.data.keys().hasAll(['name', 'phone', 'addr', 'when'])
                    && request.resource.data.size() < 30;
      allow read, update, delete: if isOwner();
    }
  }
}
```

ئیمەیڵەکە تەنها لە ناو Rules ی Firebase دا دابنێ. لە کۆدی GitHub دا دای مەنێ، چونکە ئەو پرۆژەیە Public ە.

## ئەوەی Claude دواتر دەیکات / What Claude builds next

- **چوونە ژوورەوەی خاوەن:** دوگمەیەک بۆ چوونە ژوورەوە بە Google. بەشی بەڕێوەبردن تەنها بۆ خاوەن دەردەکەوێت.
  - شێوازی ناسینەوە: ئەپەکە هەوڵ دەدات `bookings` بخوێنێتەوە. تەنها خاوەن ڕێگەی هەیە، بۆیە ئیمەیڵی خاوەن لە ناو کۆدەکەدا نانووسرێت.
- **تایەکان:** لە کۆڵێکشنی `tires` پاشەکەوت دەبن. هەموو کەسێک دەیانبینێت.
  - **وێنەکان:** بچووک دەکرێنەوە بۆ نزیکەی 800px (JPEG) و لە ناو داتابەیس پاشەکەوت دەبن، بۆیە Firebase Storage و کارتی بانکی پێویست نین.
- **داواکارییەکان:** لە کۆڵێکشنی `bookings` پاشەکەوت دەبن. لیستێکی داواکارییەکان (ناو، ژمارە، ناونیشان، کات) تەنها بۆ خاوەن دەردەکەوێت.
- **`settings/main`:** پیشاندان یان شاردنەوەی تایە نموونەییەکان.
- لە کۆتاییدا لەسەر GitHub Pages بڵاو دەکرێتەوە.

## بۆ دەستپێکردنەوە / To resume

پێی بڵێ: «با Firebase تەواو بکەین» و کۆدی `firebaseConfig` بنێرە.
Say: "Let's finish Firebase" and paste the `firebaseConfig` block.
