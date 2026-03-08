# Ziua Elei 🎂

Aplicație web pentru capturarea amintirilor de la ziua Elei.

## Setup

### 1. Clonează repository-ul
```bash
git clone https://github.com/USER/REPO.git
cd REPO
```

### 2. Configurează Firebase
```bash
cp firebase-config.example.js firebase-config.js
```
Deschide `firebase-config.js` și completează cu datele tale din [Firebase Console](https://console.firebase.google.com):
```js
window.firebaseConfig = {
    apiKey: "...",
    authDomain: "...",
    ...
};
```

> ⚠️ `firebase-config.js` este în `.gitignore` și **nu se urcă niciodată pe GitHub**.

### 3. Firebase Console — setări necesare

**Authentication** → Sign-in method → Anonymous → Activat

**Storage Rules** (pentru acces public la ziua petrecerii):
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /amintiri/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

**Storage CORS** (rulează în Google Cloud Shell):
```bash
cat > cors.json << 'EOF'
[{"origin":["https://USER.github.io"],"method":["GET","POST","PUT","DELETE","HEAD"],"maxAgeSeconds":3600,"responseHeader":["Content-Type","Authorization","Content-Length"]}]
EOF
gsutil cors set cors.json gs://PROIECT.firebasestorage.app
```

### 4. GitHub Pages
Settings → Pages → Source: `main` branch → `/` (root)

Site va fi disponibil la `https://USER.github.io/REPO`
