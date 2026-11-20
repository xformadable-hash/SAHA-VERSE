# SAHA ACADEMY — Version 2 setup

This package contains a bilingual HSC notes-library website with Firebase Authentication, Cloud Firestore metadata, and Cloud Storage file uploads.

## Important: one-time Firebase setup is still required
The website cannot connect to your cloud account until you create a Firebase project and add its web-app configuration. Do not share your Firebase account password or private service-account keys.

## 1. Create a Firebase project
1. Open https://console.firebase.google.com/ and sign in with your Google account.
2. Create a project named `saha-academy` (or another available name).
3. In Project settings → General → Your apps, add a Web app.
4. Copy the Firebase web configuration values.

## 2. Add your Firebase config
Open `index.html` in a text editor. Find `const firebaseConfig = { ... }` near the bottom and replace all `PASTE_...` placeholders with the values from your Firebase web app config.
The web config's API key is intended for identifying your Firebase project; actual access control is enforced by Firebase Security Rules.

## 3. Enable Authentication
1. Firebase Console → Build → Authentication → Get started.
2. Enable **Email/Password** sign-in.
3. Add your own admin email under Authentication → Users (use Add user).
4. Use a strong, unique password. Do not put the password in the website code.

## 4. Create Firestore and Storage
1. Build → Firestore Database → Create database.
2. Build → Storage → Get started.
3. Choose the region closest to your expected users and keep the same region where practical.

## 5. Publish the security rules
In Firestore → Rules, replace the rules with the contents of `firestore.rules`, then Publish.
In Storage → Rules, replace the rules with the contents of `storage.rules`, then Publish.
These rules allow public reading of published notes but restrict note/file changes to accounts whose UID has an admin record.

## 6. Grant your account admin permission (critical)
1. Authentication → Users: copy the UID for your admin account.
2. Firestore → Data: create a collection named `admins`.
3. Add a document whose document ID is exactly that UID.
4. Add a field such as `role` (string) = `admin`.
5. Do not create an admin-management feature in the public website. The rules intentionally block client-side writes to `admins`.

Only the project owner should grant admin membership in Firebase Console. Never give admin access to an account you do not control.

## 7. Deploy the website
The easiest beginner-friendly option is Firebase Hosting:
1. Install Node.js and the Firebase CLI on a computer.
2. In this folder, run `firebase login`.
3. Run `firebase init hosting`, select your existing project, and set the public directory to this folder (or a `public` folder containing `index.html`).
4. Configure it as a single-page app: **No** rewrite is needed for this static site.
5. Run `firebase deploy --only hosting`.

You can also use another static host that serves `index.html` over HTTPS. Do not open the file directly with `file://`; Firebase modules need a web server.

## Features
- Admin email/password sign-in.
- Admin-only PDF/image upload (15 MB max).
- Subjects: বাংলা, English, পৌরনীতি ও সুশাসন, ভূগোল, ইতিহাস, অর্থনীতি.
- Chapter, title, category and description metadata.
- Public search, filters, open and download.
- Admin-only delete.
- Firestore stores note metadata; Cloud Storage stores the actual files.

## Notes / limitations
- This is a starter implementation, not a fully audited production system.
- Firebase billing may be required for Cloud Storage and hosting features depending on current Firebase plan and usage. Review Firebase pricing before enabling billing.
- Public read means anyone with the website or file URL can read/download published notes. Upload rights remain admin-only.
- For a high-stakes public deployment, add App Check, rate limiting/abuse monitoring, backups, and review security rules.
- The page currently supports PDF, JPG, PNG and WEBP uploads up to 15 MB. It does not yet include editing notes, multiple-file uploads, or student accounts.
