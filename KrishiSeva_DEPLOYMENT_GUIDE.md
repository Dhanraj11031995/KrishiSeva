# 🌾 KrishiSeva v2.0 — Complete Platform Documentation

**ଓଡ଼ିଶା କୃଷି ପ୍ରବନ୍ଧନ ମଞ୍ଚ | Odisha Agricultural Management Platform**

---

## 📋 What's Included

### KrishiSeva_v2.html — Complete Standalone Platform
A single HTML file (~114KB) that runs entirely in the browser. No server required for the demo. Includes:

| Module | Description |
|--------|-------------|
| 🔐 Login | Farmer + Admin login, bilingual (English/Odia) |
| 💡 Consultancy | Crop Cycle, Nutrients, Plant Protection, e-Commerce |
| 📝 Data Entry | Calendar-based field data entry with Google Form-style questions |
| 💰 Costing | Investment costing for all farming activities with cost calculator |
| 📸 Farm Photos | Photo upload with crop/date/category tagging for AI dataset |
| 🤖 AI Assistant | ChatGPT/Claude-powered fallback for queries not in local database |
| ⚙️ Admin Panel | Full management: users, places, crops, schedules, nutrients, protection, costing, forms, data, orders |

---

## 🌱 Crop Categories Covered

All 9 categories with complete cultivation data:

1. **Cereals** — Paddy (Kharif/Rabi), Wheat, Maize, Jowar, Bajra, Ragi
2. **Pulses** — Moong, Biri, Arhar, Masur, Chana, Cowpea
3. **Vegetables** — Tomato, Brinjal, Chilli, Okra, Cabbage, Cauliflower, Potato, Onion, Broccoli, Pumpkin
4. **Fruits** — Mango, Banana, Papaya, Guava, Lemon, Pineapple, Watermelon, Jackfruit
5. **Spices** — Turmeric, Ginger, Garlic, Coriander, Dry Chilli, Cardamom
6. **Oilseeds** — Groundnut, Sunflower, Sesame, Mustard, Linseed, Castor
7. **Medicinal** — Ashwagandha, Tulsi, Aloe Vera, Shatavari, Lemongrass, Stevia, Kalmegh
8. **Flowers** — Marigold, Rose, Jasmine, Chrysanthemum, Tuberose, Gerbera, Gladiolus
9. **Condiments** — Fenugreek, Cumin, Ajwain, Fennel, Curry Leaf, Mustard

---

## 🗂️ Data Modules

### Nutrient Recommendations
- **Chemical**: Crop-specific NPK doses, top-dressing schedules, micronutrients (Zn, B, S)
- **Organic**: FYM, Vermicompost, Neem Cake, Biofertilizers (PSB, Azospirillum, Rhizobium)
- Both types available for all 9 crop categories

### Plant Protection
- **Pests**: Stem borer, BPH, fruit borer, aphids, whitefly, thrips, mites, rodents
- **Diseases**: Blast, blight, wilt, powdery mildew, damping off, root rot, anthracnose
- **Weeds**: Narrow-leaf, broad-leaf, manual weeding
- **Others**: Algae, bacteria, viruses (via vectors), nematodes
- Chemical AND organic options for all categories

### Investment Costing
- Land Preparation, Labour, Irrigation
- Chemical & Organic Nutrients (itemised with rates)
- Chemical & Organic Protection
- Implements & Machinery
- Land Revenue & Government fees
- **Built-in Cost Calculator** — enter area + crop category → auto-estimate

### Field Data Entry Form (Admin-configurable)
21 parameters including:
- Soil: pH, EC, OC, Residual N/P/K/S
- Inputs: Nutrition type/products/cost, Protection type/products/cost
- Operations: Irrigation count, Labour cost, Machinery cost
- Output: Produce type, Harvest (qtl/ac), Market price, Total revenue
- Photo upload with tagging
- Free-text remarks

---

## 🚀 Deployment Guide

### Option A: Standalone HTML (Current)
Simply open `KrishiSeva_v2.html` in any browser. No installation needed.

### Option B: Full-Stack Production Deployment

#### Frontend → Render.com
```bash
# In /krishiseva/frontend/
npm install
npm run build
# Deploy /build folder to Render as Static Site
```

**Environment variables for Render:**
```
REACT_APP_API_URL=https://your-site.netlify.app/api
REACT_APP_RAZORPAY_KEY_ID=rzp_live_xxxxxxxxxxxx
REACT_APP_GOOGLE_MAPS_KEY=your_google_maps_key
```

#### Backend → Netlify Functions
```bash
# In /krishiseva/backend/
npm install
netlify deploy --prod
```

**Environment variables for Netlify:**
```
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/krishiseva
JWT_SECRET=your_jwt_secret_key
RAZORPAY_KEY_ID=rzp_live_xxxxxxxxxxxx
RAZORPAY_KEY_SECRET=your_razorpay_secret
GOOGLE_MAPS_KEY=your_key
GOOGLE_SHEET_ID=your_sheet_id          # For Google Sheets sync
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
GOOGLE_REFRESH_TOKEN=your_refresh_token
ALLOWED_ORIGIN=https://your-krishiseva.onrender.com
```

---

## ☁️ Cloud Storage Integration (To Configure Later)

When you purchase cloud storage, connect it here:

### AWS S3 (Recommended for photos)
```javascript
// In backend/functions/utils/storage.js
const AWS = require('aws-sdk');
const s3 = new AWS.S3({
  accessKeyId: process.env.AWS_ACCESS_KEY,
  secretAccessKey: process.env.AWS_SECRET_KEY,
  region: 'ap-south-1'  // Mumbai region
});

async function uploadPhoto(file, key) {
  return s3.upload({
    Bucket: process.env.S3_BUCKET,
    Key: `farm-photos/${key}`,
    Body: file.buffer,
    ContentType: file.mimetype,
    Metadata: { crop: file.cropTag, date: file.date }
  }).promise();
}
```

### Cloudinary (Alternative — easier setup)
```javascript
const cloudinary = require('cloudinary').v2;
cloudinary.config({
  cloud_name: process.env.CLOUDINARY_NAME,
  api_key: process.env.CLOUDINARY_KEY,
  api_secret: process.env.CLOUDINARY_SECRET
});
```

---

## 💳 Razorpay Integration

### Step 1: Create Razorpay Account
- Go to razorpay.com → Sign up as business
- Complete KYC with farm/company details
- Add recipient bank account

### Step 2: Get API Keys
- Dashboard → Settings → API Keys
- Copy Key ID and Key Secret

### Step 3: Add to environment
```
RAZORPAY_KEY_ID=rzp_live_xxxxxxxxxxxx
RAZORPAY_KEY_SECRET=your_secret
```

### Step 4: The backend function (`/backend/functions/orders.js`) handles:
- Creating Razorpay orders
- Payment verification with HMAC signature
- COD order confirmation
- Order status updates

---

## 🤖 AI Assistant Integration

### Current (Demo Mode)
Shows helpful contact information when API not configured.

### Production — Claude/ChatGPT Integration
The AI assistant in `KrishiSeva_v2.html` already calls the Anthropic API:

```javascript
// Already implemented in the HTML
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 1000,
    messages: [{ role: 'user', content: `Agricultural question: ${query}` }]
  })
});
```

**For production**, route this through your Netlify backend to keep the API key secure:
```javascript
// Frontend calls your backend
const response = await fetch('/api/ai-query', {
  method: 'POST',
  body: JSON.stringify({ query, lang })
});
```

---

## 📱 Mobile App (Next Phase)

To convert to a mobile app:

### React Native
```bash
npx react-native init KrishiSevaApp
# Copy src/ components from frontend/
# Add react-native-camera for photo capture
# Add react-native-voice for voice input
```

### Progressive Web App (PWA) — Easier
Add to `frontend/public/manifest.json`:
```json
{
  "name": "KrishiSeva",
  "short_name": "KrishiSeva",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#EAF3DE",
  "theme_color": "#3B6D11",
  "icons": [{ "src": "/icon-512.png", "sizes": "512x512" }]
}
```

---

## 🗣️ Voice Assistant (Web Speech API)

Already implemented in the platform:

```javascript
// Text-to-Speech (Odia/English)
function speak(text) {
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = lang === 'od' ? 'or-IN' : 'en-IN';
  utterance.rate = 0.88;
  window.speechSynthesis.speak(utterance);
}

// Speech-to-Text (for voice queries)
const recognition = new webkitSpeechRecognition();
recognition.lang = 'or-IN';  // Odia
recognition.onresult = (e) => {
  document.getElementById('aiQ').value = e.results[0][0].transcript;
};
```

Note: Odia voice synthesis depends on the user's device having Odia TTS installed. Hindi (`hi-IN`) is used as fallback.

---

## 📊 Google Sheets Sync

When a farmer submits field data, it automatically syncs to a Google Sheet (configured in backend):

| Column | Data |
|--------|------|
| Farmer | Name & mobile |
| Place | Location name |
| Crop | Crop + category |
| Date | Entry date |
| Soil | pH, EC, OC, N, P, K, S |
| Inputs | Nutrition + protection used + costs |
| Output | Harvest, market price, revenue |
| Status | submitted / verified |

---

## 🔧 Admin Quick Reference

| Admin Page | What you can do |
|------------|-----------------|
| Dashboard | Overview stats, recent activity |
| Users | View/edit farmers, assign places |
| Places & Plots | Add places, create plots, assign to farmers |
| Crop Setup | Add/edit crops for all 9 categories |
| Schedules | Set calendar-wise work schedules per crop |
| Nutrient Setup | Set chemical/organic recommendations per crop category |
| Protection Setup | Set pest/disease recommendations per category |
| Costing Setup | Maintain itemised cost data |
| Form Builder | Configure field data entry questions |
| Farm Data | View all submitted data, verify records, export CSV |
| Orders | Manage product catalogue, view orders |

---

## 🌐 Language Support

| Feature | English | Odia (ଓଡ଼ିଆ) |
|---------|---------|--------------|
| Navigation | ✅ | ✅ |
| Page titles | ✅ | ✅ |
| Crop names | ✅ | ✅ (partial) |
| Form labels | ✅ | ✅ |
| Voice assistant | ✅ | ✅ (device-dependent) |
| AI responses | ✅ | ✅ (AI generates) |
| Alerts/messages | ✅ | ✅ |

Toggle language using the 🌐 button in the top bar.

---

## 📞 Support Contacts (Configured in AI fallback)

- **Kisan Call Centre**: 1800-180-1551 (Toll-free, 24×7)
- **PM-KISAN Portal**: pmkisan.gov.in
- **Odisha Agriculture Dept**: agriodisha.nic.in
- **Nearest KVK**: kvk.icar.gov.in

---

## ✅ Checklist for Going Live

- [ ] Purchase domain (e.g., krishiseva.in)
- [ ] Create MongoDB Atlas cluster → add MONGODB_URI
- [ ] Create Razorpay account → add keys → add recipient account
- [ ] Set up Google Cloud project → enable Maps API → add key
- [ ] Set up Google Sheets for data sync → add credentials
- [ ] Purchase cloud storage (AWS S3 or Cloudinary) → configure
- [ ] Deploy frontend to Render → configure env vars
- [ ] Deploy backend to Netlify → configure env vars
- [ ] Register as PWA for mobile access
- [ ] Add Odia TTS voices to user guidance documentation
- [ ] Test all 9 crop categories end-to-end
- [ ] Train admin users on the platform

---

*KrishiSeva v2.0 — Built for Odisha farmers 🌾*
*ଓଡ଼ିଶାର ─ କୃଷକମାନଙ୍କ ─ ପାଇଁ ─ ନିର୍ମିତ*
