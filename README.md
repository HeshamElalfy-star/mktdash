# MktDash — Marketing Dashboard

داش بورد تسويق لايف احترافي متصل بـ:
- **Facebook Ads** + **Instagram Ads**
- **Google Ads**
- **TikTok Ads**

---

## تشغيل المشروع

```bash
npm install
npm run dev
```

افتح http://localhost:3000 — هيشتغل بـ Demo Data فوراً.

---

## ربط APIs حقيقية

### 1. إنشاء ملف `.env.local`

```env
# Facebook / Instagram
FACEBOOK_ACCESS_TOKEN=EAAxxxxx
FACEBOOK_AD_ACCOUNT_ID=act_123456789
FACEBOOK_APP_ID=123456789
FACEBOOK_APP_SECRET=abc123def456

# Google Ads
GOOGLE_ADS_DEVELOPER_TOKEN=ABCDEF_TOKEN
GOOGLE_ADS_CLIENT_ID=xxx.apps.googleusercontent.com
GOOGLE_ADS_CLIENT_SECRET=GOCSPX-xxx
GOOGLE_ADS_REFRESH_TOKEN=1//xxx
GOOGLE_ADS_CUSTOMER_ID=1234567890

# TikTok
TIKTOK_ACCESS_TOKEN=xxx
TIKTOK_APP_ID=123456
TIKTOK_ADVERTISER_ID=7123456789

# Settings
NEXT_PUBLIC_REFRESH_MS=30000
NEXT_PUBLIC_CURRENCY=ج.م
```

---

### 2. Facebook & Instagram Ads API

1. اذهب إلى https://developers.facebook.com/apps
2. أنشئ App → أضف "Marketing API"
3. **Graph API Explorer** → اختر الـ App → Permissions:
   - `ads_read`, `ads_management`, `read_insights`, `business_management`
4. Generate Access Token → حوّله لـ Long-Lived Token:
```
GET https://graph.facebook.com/oauth/access_token
  ?grant_type=fb_exchange_token
  &client_id={APP_ID}
  &client_secret={APP_SECRET}
  &fb_exchange_token={SHORT_TOKEN}
```
5. Ad Account ID: https://business.facebook.com → Settings → Ad Accounts

---

### 3. Google Ads API

1. https://console.cloud.google.com → أنشئ مشروع
2. فعّل "Google Ads API"
3. أنشئ OAuth 2.0 Client (Web application)
4. احصل على Developer Token: Google Ads → Tools → API Center
5. OAuth flow للـ Refresh Token:
```bash
# استخدم OAuth Playground أو google-auth-library
https://developers.google.com/oauthplayground
```

---

### 4. TikTok Ads API

1. https://ads.tiktok.com/marketing_api/apps → أنشئ App
2. احصل على Access Token من الـ Authorization Flow
3. Advertiser ID من URL الـ Ads Manager

---

## هيكل المشروع

```
src/
├── app/
│   ├── api/
│   │   ├── fb/route.ts          ← Facebook + Instagram API
│   │   ├── google-ads/route.ts  ← Google Ads API
│   │   ├── tiktok/route.ts      ← TikTok Ads API
│   │   └── overview/route.ts    ← يجمع كل القنوات
│   ├── page.tsx                 ← الصفحة الرئيسية
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── charts/
│   │   ├── MultiLineChart.tsx   ← خط الأداء عبر الزمن
│   │   ├── SpendDonut.tsx       ← توزيع الإنفاق
│   │   └── BarComparison.tsx    ← مقارنة القنوات
│   ├── tables/
│   │   ├── CampaignsTable.tsx   ← جدول الحملات (sort + filter)
│   │   ├── BudgetTable.tsx      ← الميزانية مع progress bars
│   │   └── ReportTable.tsx      ← تقرير قابل للتصدير CSV
│   └── ui/
│       ├── KpiCard.tsx          ← بطاقات KPI مع flash animation
│       ├── Sidebar.tsx          ← القائمة الجانبية
│       ├── ChannelCard.tsx      ← بطاقة كل منصة
│       └── SettingsPanel.tsx    ← شاشة الإعدادات وربط APIs
├── hooks/
│   └── useDashboard.ts          ← Polling hook كل 30 ثانية
└── lib/
    ├── types.ts                 ← كل الـ TypeScript types
    ├── demo.ts                  ← Demo data generator
    └── formatters.ts            ← أدوات التنسيق
```

---

## الصفحات

| الصفحة | المحتوى |
|--------|---------|
| نظرة عامة | KPIs + Channel Cards + Charts |
| الحملات | جدول كامل مع فلترة وترتيب |
| الأداء | CTR / ROAS / Conversions / CPM |
| الميزانية | Progress bars لكل قناة |
| التقارير | تقرير شامل قابل للتصدير CSV |
| الإعدادات | دليل ربط كل API |
