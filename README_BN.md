# ChatFlow — Firebase real-time chat

এই প্যাকেজে আছে:
- `index.html` — Messenger-স্টাইল responsive UI, নিজের নাম ও প্রোফাইল ছবি পরিবর্তনের সুবিধা
- `firestore.rules` — Firestore security rules
- `storage.rules` — ছবি/ভিডিও storage rules

## 1. Firebase project তৈরি
1. https://console.firebase.google.com/ এ যান এবং একটি project তৈরি করুন।
2. Project settings → General → Your apps → Web app (</>) তৈরি করুন।
3. Firebase config কপি করুন।
4. `index.html`-এ `const firebaseConfig = { ... }` অংশে `PASTE_...` মানগুলো নিজের config দিয়ে বদলান।

## 2. Authentication চালু
Firebase Console → Build → Authentication → Get started → Sign-in method → Email/Password enable করুন।

## 3. Firestore চালু ও rules প্রকাশ
1. Build → Firestore Database → Create database.
2. Rules tab-এ `firestore.rules` ফাইলের সম্পূর্ণ contents পেস্ট করে Publish করুন।

## 4. Storage চালু ও rules প্রকাশ
1. Build → Storage → Get started.
2. Rules tab-এ `storage.rules` ফাইলের contents পেস্ট করে Publish করুন।
3. Firebase Storage-এর billing/plan requirements Firebase Console-এ দেখে নিন; কিছু নতুন project-এ Storage ব্যবহারে billing account লাগতে পারে।

## 5. Hosting-এ প্রকাশ (প্রস্তাবিত)
`index.html` ফাইলটি সরাসরি file:// দিয়ে খুলবেন না—Firebase module imports/auth এর জন্য HTTPS hosting ব্যবহার করুন।
সহজ উপায়:
- Firebase Console → Build → Hosting → Get started; অথবা
- Firebase CLI দিয়ে project-এর public folder-এ `index.html` রেখে deploy করুন।

Firebase CLI ব্যবহার করলে:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# public directory হিসেবে public দিন, Single-page app: No
# public/index.html-এ এই index.html রাখুন
firebase deploy
```

## 6. দুইটি মোবাইলে পরীক্ষা
1. Hosting URL দুইটি আলাদা মোবাইলে খুলুন।
2. আলাদা ইমেইল দিয়ে দুইটি account register করুন।
3. প্রথম ফোনে “নতুন চ্যাট” → দ্বিতীয় ফোনে নিবন্ধিত ইমেইল লিখুন।
4. দুই পাশেই চ্যাট তৈরি/দেখা যাবে। মেসেজ, ছবি, ভিডিও রিয়েল-টাইমে আপডেট হবে।

## গুরুত্বপূর্ণ সীমা ও নিরাপত্তা
- এটি একটি starter app, official Messenger নয়।
- Firebase config-এর `apiKey` সাধারণত web config-এর অংশ; তবে Security Rules অবশ্যই publish করুন এবং Firebase Console-এ App Check যোগ করার কথা ভাবুন।
- Email/Password auth ব্যবহার করা হয়েছে। password কখনো Firestore-এ সংরক্ষণ করবেন না।
- বর্তমান user directory-তে signed-in ব্যবহারকারীরা email profile পড়তে পারে, যাতে ইমেইল দিয়ে চ্যাট শুরু করা যায়। বড়/পাবলিক সার্ভিসে email enumeration কমাতে Cloud Functions-এর মতো trusted backend দিয়ে invite flow তৈরি করা উত্তম।
- মেসেজ delete/edit, read receipts, push notifications, blocking/reporting, abuse controls ও end-to-end encryption এই starter-এ নেই।
- এই UI-তে chat history Firestore-এ থাকে এবং uploaded media Firebase Storage-এ থাকে।
