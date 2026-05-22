# 🚀 Astrology Today V6 - Deploy Rehberi
# BU REHBERİ ADIM ADIM UYGULA

## ✅ ÖNCE BUNLARI YAP

### 1. GitHub Repo Oluştur
- GitHub'a git → New Repository → `astrology-today`
- Public seç → Create repository

### 2. ZIP'i İndir ve Aç
- ASTROLOGY-V6-DEPLOY-PAKETI.zip indir
- İçindekileri bir klasöre çıkar

### 3. Dosyaları Doğru Yere Koy

Çıkan dosyaları şu yapıya göre düzenle:

```
astrology-today/                    ← Ana klasör
├── content/
│   ├── monthly/          ← 12 aylık .md dosyası
│   ├── weekly/           ← 48 haftalık .md dosyası
│   ├── daily/            ← 372 günlük .md dosyası
│   └── blog/             ← Boş klasör (sonra doldurursun)
├── pages/
│   ├── index.js          ← pages-index.js'i buraya kopyala
│   ├── gunluk/
│   │   └── [date]/
│   │       └── [sign].js ← pages-gunluk-date-sign-final.js'i buraya kopyala
│   └── sitemap.xml.js    ← pages-sitemap-xml.js'i buraya kopyala
├── lib/
│   ├── horoscopes.js     ← lib-horoscopes-v2.js'i buraya kopyala
│   └── staticPaths.js    ← lib-staticPaths.js'i buraya kopyala
├── public/
│   └── admin/
│       ├── index.html    ← Admin paneli (varsa)
│       └── config.yml    ← admin/config.yml'i buraya kopyala
│   └── robots.txt        ← robots.txt'i buraya kopyala
├── next.config.js        ← next.config.js'i buraya kopyala
├── package.json          ← package.json'i buraya kopyala
├── netlify.toml          ← netlify.toml'u buraya kopyala
└── .github/
    └── workflows/
        └── daily-build-v2.yml  ← GitHub Actions
```

### 4. next.config.js Düzenle (KRİTİK!)

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',
  images: {
    unoptimized: true  // Netlify static export için ZORUNLU
  },
  trailingSlash: true
}
module.exports = nextConfig
```

### 5. package.json Kontrol Et

```json
{
  "name": "astrology-today",
  "version": "6.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "gray-matter": "^4.0.3"
  }
}
```

### 6. GitHub'a Yükle

```bash
cd astrology-today
git init
git add .
git commit -m "V6 initial deploy"
git branch -M main
git remote add origin https://github.com/KULLANICIADIN/astrology-today.git
git push -u origin main
```

---

## 🚀 NETLIFY DEPLOY

### 1. Netlify'a Git
- [app.netlify.com](https://app.netlify.com) → Sign up (GitHub ile)
- "Add new site" → "Import an existing project"
- GitHub → `astrology-today` repo'sunu seç

### 2. Build Ayarlarını Kontrol Et

| Ayar | Değer |
|------|-------|
| Build command | `npm run build` |
| Publish directory | `out` |
| Node version | 20 |

**NOT:** Eğer "Essential Next.js" plugin hatası alırsan:
- `netlify.toml` içinde `NETLIFY_NEXT_PLUGIN_SKIP = "true"` olduğundan emin ol
- VEYA Netlify dashboard → Plugins → "Essential Next.js" → Uninstall

### 3. Environment Variables Ekle

Site settings → Environment variables:
- `NODE_VERSION` = `20`
- `NETLIFY_NEXT_PLUGIN_SKIP` = `true` (static export kullanıyorsan)

### 4. Deploy Et
- "Deploy site" butonuna tıkla
- İlk build 3-5 dakika sürebilir
- Hata alırsan build log'larını kontrol et

---

## 🔧 SIK KARŞILAŞILAN HATALAR

### HATA: "Image Optimization using Next.js default loader is not compatible with next export"
**Çözüm:** `next.config.js` içinde `images.unoptimized = true` ekle

### HATA: "Page not found" deploy sonrası
**Çözüm:** 
- Publish directory `out` olduğundan emin ol
- `netlify.toml` içinde `publish = "out"` olduğundan emin ol
- Trailing slash ayarını kontrol et

### HATA: "getStaticPaths" ile ilgili hata
**Çözüm:** Dynamic route'lar (`[date]`, `[sign]`) için `getStaticPaths` fonksiyonu eksik veya hatalı. `lib/staticPaths.js`'i kontrol et.

---

## 🌐 ALAN ADI BAĞLAMA (Deploy sonrası)

1. Netlify Dashboard → Domain settings
2. "Add custom domain" → `astrologytoday.com`
3. "Netlify DNS" seç
4. Verilen 4 nameserver'ı alan adı sağlayıcına gir
5. SSL otomatik kurulur

---

## ✅ DEPLOY SONRASI KONTROL LİSTESİ

- [ ] Site `netlify.app` adresinde açılıyor
- [ ] Ana sayfa 12 burç kartını gösteriyor
- [ ] `/gunluk/2026-05-22/koc/` sayfası açılıyor
- [ ] Geçmiş tarihlere tıklanınca o günün yorumu geliyor
- [ ] `/admin` adresinde Decap CMS giriş ekranı var
- [ ] `/sitemap.xml` adresinde XML görünüyor
- [ ] `robots.txt` erişilebilir

---

## 🔄 GÜNLÜK OTOMATİK BUILD (Deploy sonrası ayarla)

1. GitHub repo → Settings → Secrets and variables → Actions
2. New repository secret:
   - `NETLIFY_AUTH_TOKEN` → Netlify'dan al (User settings → Applications → Personal access tokens)
   - `NETLIFY_SITE_ID` → Netlify site settings → General → Site details → Site ID
3. GitHub Actions otomatik çalışacak (her gün sabah 6'da)

---

**Hazır! Bir sorun olursa build log'larını ve hata mesajını paylaş.** 🚀
