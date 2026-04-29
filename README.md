# 愛票網 iTickets 商業網站

這是愛票網電子票系統的官方介紹網站，介紹電子票券發行、自我核銷專利技術、銀行履約保證機制與三種商家合作方案。

純靜態網站，**不需要任何後端**，可直接部署到 GitHub Pages、Vercel、Netlify、Cloudflare Pages 或任何能放靜態檔案的主機。

---

## 📁 檔案結構

```
.
├── index.html              ← 主網頁（單檔，含所有 CSS / JS）
├── assets/
│   ├── redeem-screenshot.jpg   ← 自我核銷實際畫面
│   ├── og-image.png             ← 社群分享卡片（Facebook/LINE/Twitter）
│   └── apple-touch-icon.png     ← iOS 加到主畫面的 icon
├── robots.txt              ← 搜尋引擎爬蟲設定
├── sitemap.xml             ← 網站地圖（SEO）
├── vercel.json             ← Vercel 部署設定（自動套用快取與安全 header）
├── netlify.toml            ← Netlify 部署設定
├── .github/workflows/
│   └── deploy.yml          ← GitHub Pages 自動部署
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🚀 部署方式（三選一）

### 方式 A：Vercel（最推薦，最快）

1. 前往 [vercel.com](https://vercel.com) 註冊／登入（用 GitHub 帳號最快）
2. 把這個資料夾的內容上傳到一個 GitHub repo（私有或公開都可以）
3. 在 Vercel 點 **「New Project」** → 選你的 repo → 直接點「Deploy」
4. 約 30 秒後拿到 `https://你的專案名.vercel.app` 的網址

> Vercel 會自動讀 `vercel.json` 套用安全 header 與快取設定，免費方案完全夠用。

#### 綁定自訂網域

在 Vercel 的專案頁 → Settings → Domains → 輸入你的網域（如 `itickets.com.tw`）→ 跟著畫面指示改 DNS 即可。

---

### 方式 B：GitHub Pages（完全免費，適合 GitHub 重度使用者）

1. 把這個資料夾推到一個 GitHub repo（**必須是 public，或 GitHub Pro/組織方案**）
2. 進 repo → **Settings → Pages**
3. Source 選 **「GitHub Actions」**
4. 推送 `main` branch 後，`.github/workflows/deploy.yml` 會自動幫你部署
5. 拿到 `https://你的帳號.github.io/repo名稱/` 的網址

#### 綁定自訂網域

在 repo 根目錄新增 `CNAME` 檔（內容只有一行：你的網域），然後到 DNS 商把 A record 指向 GitHub Pages 的 IP（185.199.108.153 等四個）。

---

### 方式 C：Netlify（簡單、強大、免費）

1. 註冊 [netlify.com](https://netlify.com)
2. 拖曳整個資料夾到 Netlify 的部署區，或連結 GitHub repo
3. 自動部署完成

> Netlify 會讀 `netlify.toml` 套用設定。

---

## 🔧 部署前的客製化清單

部署前建議調整以下幾處：

### 1. 替換 OG 圖片中的網址（如果你有確定網域）

編輯 `index.html` 開頭的 meta 標籤，把所有 `https://itickets.example.com` 改成你的實際網域：

```html
<link rel="canonical" href="https://你的網域/">
<meta property="og:url" content="https://你的網域/">
<meta property="og:image" content="https://你的網域/assets/og-image.png">
```

也記得改 `robots.txt` 與 `sitemap.xml` 中的網址。

### 2. 接上實際的表單後端

目前合作申請表單是純前端 demo（送出後 console.log）。**部署前請務必接上真實後端**，建議三種做法：

#### 選項 A：Formspree（最簡單，免費版每月 50 封）

到 [formspree.io](https://formspree.io) 註冊 → 拿到 endpoint URL → 編輯 `index.html` 找到：

```html
<form class="contact-form" id="partnerForm" onsubmit="return submitForm(event)">
```

改成：

```html
<form class="contact-form" id="partnerForm" action="https://formspree.io/f/你的ID" method="POST">
```

並把 `onsubmit="return submitForm(event)"` 移除，或保留 success 動畫的部分（只移除 `e.preventDefault()`）。

#### 選項 B：Google Forms 串接

把表單欄位 name 對應到 Google Form 的 entry ID，submit 到 Google Form 的 formResponse URL。

#### 選項 C：自架 API

如果有自己的後端，把 `submitForm` 函式中的 `console.log` 改成 fetch POST 即可。

### 3. 填入實際的銀行名稱與月費

`index.html` 中目前用「合作銀行」「依方案而定」做為占位，等實際合作銀行名稱與月費確定後可以替換。可以搜尋以下關鍵字找到位置：

- `合作銀行`（履約保證區、FAQ）
- `依方案而定`（FAQ 第二題）
- `月費`（多處）

### 4. 圖片優化（可選）

`assets/redeem-screenshot.jpg` 已是 24KB，效能上沒問題。但如果想再優化：

```bash
# 用 sharp/imagemin 做 WebP 版本（要先 npm install）
# 或上 https://squoosh.app/ 手動壓縮
```

`og-image.png` 是 81KB，如果要縮小可轉成 WebP（但 og-image 為相容性建議保持 PNG/JPG）。

---

## 💻 本地預覽

```bash
# Python 內建 server（Python 3）
python3 -m http.server 8000

# 或用 Node
npx serve .

# 或直接打開 index.html（部分功能如 fetch 在 file:// 下不能用，但這個網站全部正常）
```

打開 http://localhost:8000 即可。

---

## 📊 SEO 與分析建議

### Google Search Console
1. 部署完成後到 [search.google.com/search-console](https://search.google.com/search-console)
2. 加入你的網域
3. 用「網域驗證」或「HTML 標籤」驗證所有權
4. 提交 `https://你的網域/sitemap.xml`

### Google Analytics（可選）

在 `index.html` 的 `</head>` 之前加上 GA4 追蹤代碼：

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXX');
</script>
```

---

## ✅ 上線檢查清單

- [ ] 已替換所有 `itickets.example.com` 為實際網域
- [ ] 表單已接上實際後端（Formspree / Google Forms / 自架 API）
- [ ] 已測試 mobile（iPhone Safari、Android Chrome）顯示正常
- [ ] 已用 [Lighthouse](https://pagespeed.web.dev/) 跑分，效能 / SEO 都 90+ 分
- [ ] 已提交 sitemap 到 Google Search Console
- [ ] 已用 [Facebook Debugger](https://developers.facebook.com/tools/debug/) 測試分享 OG 卡片正常
- [ ] （可選）已設定 Google Analytics
- [ ] （可選）已綁定自訂網域 + HTTPS

---

## 🛠 技術規格

- **純靜態 HTML / CSS / JS**，無編譯步驟、無框架、無相依套件
- **單檔 HTML**：所有 CSS 與 JS 都內嵌在 `index.html`，僅圖片為外部資源
- **字體**：Google Fonts（Noto Serif TC、Sora、JetBrains Mono），需要網路載入
- **檔案大小**：HTML 約 90KB（gzip 後約 18KB），加上圖片總計約 200KB
- **瀏覽器支援**：所有現代瀏覽器（Chrome、Safari、Firefox、Edge 最近兩年版本）

---

## 📞 維護與修改

需要新增區塊、修改文案、調整方案內容時，所有東西都在 `index.html` 中。檔案結構是：

1. `<head>` ← SEO meta、字體、結構化資料
2. `<style>` ← 全部的 CSS（用 CSS 變數，改 `:root` 的顏色就能改主題色）
3. `<body>` ← 各 section 順序：Hero → Features → 自我核銷（含 Live Preview）→ 履約保證 → 應用場景 → 方案費率 → 計費 FAQ → 聯絡 → Footer
4. `<script>` ← 動畫觸發、FAQ toggle、表單 demo

---

© 2026 愛票網 iTickets · All rights reserved.
