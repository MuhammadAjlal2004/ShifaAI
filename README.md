# ShifaAI — Multilingual Health Navigation Prototype

ShifaAI is a hackathon prototype that structures synthetic or user-entered health notes written in English, Roman Urdu, Urdu, or Sindhi, then suggests a broad local service category and a directory entry for Hyderabad, Sindh.

> **Safety notice:** This prototype is not a medical device, diagnosis, or replacement for a clinician. AI can misunderstand symptoms and translations. Severe or persistent chest pain, serious difficulty breathing, fainting/unresponsiveness, or sudden signs of stroke require immediate emergency care; do not wait for an AI response. The hospital directory is only a starting point; check the linked facility details, current services, and availability before travel.

## Stack

- React + Vite
- Tailwind CSS
- lucide-react
- Google Gemini API (`gemini-3.8-flash` by default), accessed by a Netlify Function
- Netlify for hosting and server-side function execution

## Run locally

1. Install Node.js (Node 20 or newer recommended) and project dependencies:

   ```bash
   npm install
   ```

2. Copy `.env.example` to `.env` and set a real API key obtained from [Google AI Studio](https://aistudio.google.com/apikey):

   ```dotenv
   GEMINI_API_KEY=your_real_key_here
   GEMINI_MODEL=gemini-3.8-flash
   ```

   Do not use a `VITE_` prefix for this key. Do not commit `.env`. The included `.gitignore` excludes it.

3. Start the app **with Netlify CLI**, so the frontend and function are served together:

   ```bash
   npm run dev:netlify
   ```

   Open the local URL printed by Netlify CLI (normally `http://localhost:8888`). Running `npm run dev` starts Vite only; the Netlify function route will not be available from Vite alone.

4. Run the production frontend build:

   ```bash
   npm run build
   ```

## Deploy to Netlify

1. Import the project repository into Netlify.
2. In the site's environment-variable settings, add `GEMINI_API_KEY` with scope/access for Functions. Optionally add `GEMINI_MODEL` to override the default model.
3. Deploy or trigger a new deployment after changing environment variables.
4. Open the deployed URL, select a sample or enter synthetic notes, and run AI triage. The app should show a live result only after a real Gemini response; it displays an error instead of substituting fake clinical output when the request fails.
5. Check the site from an incognito window and ensure no API key appears in frontend source or browser network URLs.

## How AI is used

The Netlify Function forwards patient notes to the configured Gemini model and requests structured JSON. The browser validates the returned fields and displays the clinical overview, multilingual explanation, timeline, and broad department suggestion. The hospital list itself is a local curated directory; AI does not invent facility records. This is a prototype and its output is not clinically validated.

## Directory sources

- NICVD Hyderabad Satellite Centre: https://nicvd.org/national-institute-of-cardiovascular-diseases-hyderabad
- Liaquat University Hospital: https://www.luh.gos.pk/
- Isra University contact page: https://isra.edu.pk/contact
- Aga Khan Maternal & Child Care Centre, Hyderabad: https://hospitals.aku.edu/pakistan/facility/akh-hyderabad

Facility matching does not calculate patient distance, confirm opening hours, or guarantee service availability. Verify details with the facility before travelling. A future location-aware version should request consent and calculate distances from verified coordinates rather than displaying preset distances.

## Demo data and privacy

The sample patient notes are fictional demonstrations. Do not enter identifiable real patient records. Notes sent for analysis are transmitted to Google's Gemini API by the server-side function. Do not present the app as a validated medical triage system.
