# 🗑️ Bin-Wise

**Live app:** https://bin-wise.web.app

Bin-Wise is a real-time AI waste classifier built for Australian recycling. Snap a photo of an item and it tells you which disposal stream it belongs in — according to the guidelines of your state (VIC, NSW, QLD, WA, SA, TAS).

Made for an English project. Scans tracked and counted, clean recyclers rise up the [community leaderboard](https://bin-wise.web.app).

## How it works

1. **In-browser computer vision** — a TensorFlow.js + MobileNet v2 model runs entirely client-side, so image classification happens on your device with no upload
2. **State-specific guidance** — the model's top predictions are sent to the [Gemini API](https://ai.google.dev), which translates them into the correct waste stream for your selected state
3. **Firebase** — Auth (email/password) for accounts, Firestore for per-user scan counts and the leaderboard, with image-hash de-duplication to keep scans honest

## Features

- 📷 Live camera or any photo upload
- 🗺️ Guidelines for all six Australian states
- 🏆 "Scan Champions" community leaderboard
- 👤 Accounts with per-user scan counts
- 🔥 Visual impact slider showing sorted vs. unsorted waste consequences

## Project layout

```
public/          # the entire web app (no framework, vanilla HTML/CSS/JS)
  index.html     # scanner UI + leaderboard
  script.js      # TF.js classification, Gemini call, Firebase logic
  config.js      # API keys (gitignored — not committed)
  config.js.example  # copy to config.js and fill in your own keys
```

## Run locally

```bash
git clone https://github.com/grantdong1-maker/bin-wise.git
cd bin-wise/public
cp config.js.example config.js   # add your Gemini + Firebase keys
python3 -m http.server 8000      # then open http://localhost:8000
```

A Gemini API key is only needed for the disposal-guidance step; the classifier runs without one.

> ⚠️ Note: `config.js` is loaded in the browser, so keys are visible to visitors. If you deploy something similar, restrict your Gemini key by referrer and rotate it regularly — or proxy the call through a Cloud Function.

## Tech

- Vanilla HTML/CSS/JS (no framework)
- TensorFlow.js ([MobileNet v2](https://github.com/tensorflow/tfjs-models/tree/master/mobilenet))
- Google Gemini API
- Firebase Hosting, Auth & Firestore
