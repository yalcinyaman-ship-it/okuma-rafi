# Timaş Çocuk — Dijital Okuma Rafı

Depoda üç dosya olur:
- `index.html` — site (dokunmayın, güncellemelerde üzerine yazılır)
- `config.js` — ayarlar (Firebase + Drive anahtarı burada)
- `README.md` — bu dosya

## 1. GitHub Pages
Settings → Pages → Branch: `main` / `root` → Save.

## 2. Firebase
1. console.firebase.google.com → proje oluşturun.
2. **Build → Firestore Database** → Create database (production mode, eur3).
3. **Build → Authentication** → Get started → **Email/Password**'u açın → Users → **Add user** (yönetici e-postası + şifre).
4. **Authentication → Settings → Authorized domains** → `yalcinyaman-ship-it.github.io` ekleyin.
5. **Proje ayarları → Genel → Web uygulaması ekle (</>)** → çıkan `firebaseConfig` bloğunu `config.js` içine yapıştırın.
6. **Firestore → Rules** sekmesine şunu yapıştırıp Publish:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /books/{id} {
      allow read: if true;
      allow write: if request.auth != null
        && request.auth.token.email in ['yalcinyaman@timas.com.tr'];
    }
  }
}
```

## 3. Drive API anahtarı
1. console.cloud.google.com → üstten Firebase projenizi seçin.
2. **APIs & Services → Library** → "Google Drive API" → Enable.
3. **APIs & Services → Credentials → Create credentials → API key**.
4. Anahtarı düzenleyin: *Application restrictions* → Websites → `https://yalcinyaman-ship-it.github.io/*`; *API restrictions* → Google Drive API.
5. Anahtarı `config.js` içindeki `driveApiKey` alanına yazın.

## 4. Kitap ekleme
Sitenin en altındaki kilide basın → giriş yapın → **Kitap ekle**.
Drive'da PDF'e sağ tık → Paylaş → "Bağlantıya sahip herkes" → bağlantıyı forma yapıştırın.
Kapak, PDF'in ilk sayfasından otomatik gelir.

İlk kurulumda raf boşsa yönetim çubuğundaki **Örnek kitapları yükle** ile deneme verisi ekleyebilirsiniz (sonra silebilirsiniz).

## Güncelleme
Yeni `index.html` → Add file → Upload files → Commit. `config.js`'e dokunulmaz.
