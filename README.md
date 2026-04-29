# Bihter • Sağlık Takip 🐾

Bihter için kişisel sağlık takip uygulaması. Tek HTML dosyası, Firebase senkron, PWA destekli.

## Özellikler

- 🏠 **Ana ekran** — fotoğraf, otomatik yaş, günlük özet, ruh hali etiketleri, acil veteriner ara
- 💊 **İlaç takibi** — saatler, doz, stok takibi, otomatik düşüm
- 🍽️ **Yemek + iştah grafiği** — 3'lü skala (🟢🟡🔴), 7 günlük trend
- 💧 **Su takibi** — günlük ml, haftalık grafik
- 📊 **Sağlık** — kilo grafiği, tahlil sonuçları (parametre trendi + foto), aşı, hastalık geçmişi
- 📅 **Takvim** — veteriner randevuları, parazit/tımar rutinleri
- 🔔 **Tarayıcı bildirimleri** — ilaç saatleri, yemek saatleri, randevular
- 🔐 **AES-256 şifreli bulut senkron** — Firebase Firestore + Cloud ID + şifre
- 📱 **PWA** — telefona indirilebilir, çevrimdışı çalışır

## Kurulum

### 1. Firebase Projesi Oluştur

1. https://console.firebase.google.com → "Add project" → adı: `bihter` (ya da istediğin)
2. Google Analytics'i kapat (gerekli değil)
3. Sol menü → **Build → Firestore Database** → **Create database**
4. **Start in production mode** seç
5. Bölge: `eur3` (Avrupa) ya da `europe-west`
6. Oluştur

### 2. Firestore Güvenlik Kurallarını Ayarla

Firestore → **Rules** sekmesi → aşağıdaki kuralı yapıştır → **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /bihter_app/{docId} {
      allow read, write: if true;
    }
  }
}
```

> **Not:** Veriler zaten AES-256 ile şifreli olarak gönderiliyor. Firebase'deki herkes okusa bile şifre olmadan açamaz. Yine de güvenlik için bir şifren olduğundan emin ol.

### 3. Web Uygulaması Ekle

1. Firebase ana ekran → ⚙ (Project settings) → **General** sekmesi
2. **Your apps** bölümünde web simgesini </> tıkla
3. App nickname: `bihter-web` → **Register app**
4. Karşına çıkan `firebaseConfig` objesini kopyala (apiKey, authDomain, projectId vs.)

### 4. Config'i Yapıştır

`index.html` dosyasını aç, dosyanın içinde **`FIREBASE_CONFIG`** ara (yaklaşık 1700. satır), kendi config'ini yapıştır:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "bihter-xxx.firebaseapp.com",
  projectId: "bihter-xxx",
  storageBucket: "bihter-xxx.appspot.com",
  messagingSenderId: "123...",
  appId: "1:123...:web:abc..."
};
```

### 5. GitHub Pages'e Yükle

1. GitHub'da yeni bir repo oluştur (örn: `bihter`)
2. Bütün dosyaları (`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`) repoya yükle
3. Repo → **Settings → Pages** → Source: **main** branch → / (root) → **Save**
4. Birkaç dakika sonra `https://kullanıcıadın.github.io/bihter/` adresinde açılır

### 6. İlk Kullanım

1. Uygulamayı aç → **Yeni Hesap** sekmesi
2. **Bulut ID:** kendine bir takma ad gir (örn: `bihter-evim`)
3. **Şifre:** sadece sen biliyor olacaksın — KAYBETME, sıfırlama yok!
4. **Hesap Oluştur** → uygulama açılır

Aynı Bulut ID ve şifreyle başka cihazdan da giriş yaparsan veriler otomatik senkronize olur.

### 7. Telefona Yükle (PWA)

- **iPhone:** Safari'de aç → Share → "Add to Home Screen"
- **Android:** Chrome'da aç → menü → "Add to Home screen"

Anasayfaya kısayol eklenir, uygulama gibi açılır, bildirimler çalışır.

## Bildirimler

İlk açılışta bildirim izni ister. İzin verirsen:
- **İlaç saatlerinde** bildirim gelir (alındı işaretlenmediyse)
- **Yemek saatlerinde** bildirim gelir (öğün işaretlenmediyse)
- **Randevu öncesi gün** akşam 18:00'da hatırlatır

> **Önemli:** Tarayıcı/PWA arka planda çalışırken bildirim gelir. Tamamen kapatırsan gelmez. iPhone'da en güvenilir yol uygulamayı PWA olarak ana ekrana eklemek.

## Sorun Giderme

- **"Çevrimdışı" görüyorsam:** Firebase config doğru mu? İnternet var mı? Yine de uygulamada veriler korunur, çevrimiçi olunca senkronize olur.
- **Şifremi unuttum:** Veriler şifreli olduğu için sıfırlanamaz. Yedek aldıysan (Ayarlar → Veriyi Dışa Aktar) ondan kullanabilirsin. Yedeğin yoksa veri kaybı olur.
- **Bildirim gelmiyor:** Tarayıcı ayarlarından bu site için bildirimi açtığından emin ol. Telefonda PWA olarak ekle, daha güvenilir çalışır.

## Veri Yedeği

Her ay arada bir Ayarlar → **Veriyi Dışa Aktar** ile JSON yedek al. Şifrenizi unutursanız tek kurtarma yöntemidir.
