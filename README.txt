STUDYSPACE - FIREBASE CLOUD VERSION

Included: Firebase email/password authentication, cloud reviewer storage, shared reviewer browsing, and realtime online/offline member presence.

1. Create a Firebase project at https://console.firebase.google.com
2. Add a Web app (</>) and copy its config values.
3. In Authentication > Sign-in method, enable Email/Password.
4. In Realtime Database, create a database (locked mode).
5. Open index.html in a text editor and replace the PASTE_* placeholders in firebaseConfig with your actual web-app config.
6. Publish the Realtime Database rules below (adjust if desired):

{
  "rules": {
    "users": {
      ".read": "auth != null",
      "$uid": { ".write": "auth != null && auth.uid === $uid" }
    },
    "presence": {
      ".read": "auth != null",
      "$uid": { ".write": "auth != null && auth.uid === $uid" }
    },
    "reviewers": {
      ".read": "auth != null",
      "$reviewerId": {
        ".write": "auth != null && ((!data.exists() && newData.child('ownerId').val() === auth.uid) || (data.exists() && data.child('ownerId').val() === auth.uid && (!newData.exists() || newData.child('ownerId').val() === auth.uid)))"
      }
    }
  }
}

7. Members page supports member search, profile photos (paste a public image URL using the Profile photo button), live presence, and reviewer counts. Clicking a member opens their reviewer list.
8. Profile photo URLs are stored under users/<uid>/photoURL. Ensure the users rules permit authenticated reads and only each UID writes its profile.
9. Deploy index.html on a static host (Firebase Hosting, GitHub Pages, etc.). Firebase Authentication may require adding your deployed domain to Authentication > Settings > Authorized domains.

Notes:
- Reviewers and profile records are stored in Firebase Realtime Database, not the browser.
- Uploaded source files are read in-browser to generate the reviewer; this version stores the reviewer text and filenames, not the original file binaries.
- Test your database rules and deployment before sharing publicly.
