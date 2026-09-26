# Istibyan Survey Studio

A bilingual (English/Arabic) questionnaire app that runs on GitHub Pages:

- Creators sign in with Google, build questionnaires (AI-drafted or from a template), and publish them.
- Every questionnaire gets its own respondent link and QR code to send to the sample.
- Responses are stored in Firebase Firestore. Only the owner can see them.
- The results dashboard includes demographics, Likert statistics, Cronbach's alpha, ANOVA group comparisons, AI themes for open answers, challenges and recommendations, and a one-click PDF report.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `config.js` | Your Firebase settings (edit this) |
| `firestore.rules` | Database security rules (paste into Firebase) |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |

## Setup (about 15 minutes)

### 1. Create the Firebase project (free Spark plan)

1. Go to <https://console.firebase.google.com> → **Add project**.
2. **Build → Authentication → Get started**, then enable two sign-in providers:
   - **Google** (for creators)
   - **Anonymous** (for respondents, so each one can submit once without an account)
3. **Build → Firestore Database → Create database** (production mode, any region).
4. **Firestore → Rules**: replace the contents with `firestore.rules` from this folder and click **Publish**.
5. **Project settings → General → Your apps → Web (`</>`)**: register an app and copy the `firebaseConfig` object.
6. Open `config.js` and replace `firebase: null` with that object.

### 2. Publish on GitHub Pages

1. Create a GitHub repository and upload the files in this folder to its root.
2. **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)`** → Save.
3. Your app will be at `https://<your-username>.github.io/<repo-name>/`.

### 3. Allow your GitHub Pages domain to sign in

Firebase console → **Authentication → Settings → Authorized domains → Add domain** → `<your-username>.github.io`.

## Using it

1. Open the site and sign in with Google.
2. **New questionnaire** (or **Start from example**) → fill in the title, description and subject summary → choose demographics.
3. **Generate questionnaire with AI**, or build a template draft → review on **Questions**.
4. **Share** → **Publish** → copy the link, share on WhatsApp or email, or download the QR code.
5. **Results** shows live responses. Switch to **Demo data** to preview the analysis before real answers arrive, then **Export PDF report**.

Each creator only sees their own questionnaires. Respondent links look like `.../#/r/<id>`.

## AI features (optional)

Click the ✦ button and paste an Anthropic API key (from <https://console.anthropic.com>).
The key is saved only in that browser's local storage and is sent only to Anthropic's API.
Usage is billed to that key. Without a key, the app uses its built-in template and
rule-based analysis.

The AI calls use the official Anthropic JavaScript SDK with the `claude-opus-5` model.
Refusal fallback is on (`fallbacks: "default"`), so a declined request is retried
server-side on Anthropic's recommended model.

## Local mode

If `config.js` has `firebase: null`, the app runs in local mode: questionnaires and
responses stay in that browser, and respondent links only work on the same device.
This is useful for trying the app before setting up Firebase.
