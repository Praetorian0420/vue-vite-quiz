# Little Quiz

Little Quiz is a small trivia game built with Vue and Vite. It presents a short series of multiple-choice questions, gives instant feedback after each answer, tracks the player's score, and lets them replay the experience when they finish.

## About the game

The game is designed to feel light, friendly, and encouraging. Players work through ten questions covering a mix of science, geography, history, nature, literature, and art topics. Question and answer order changes with each new quiz. Each question shows:

- a category label
- the current progress count
- multiple-choice answers
- immediate correct/incorrect feedback
- a final score summary at the end

The goal is simple: answer as many questions correctly as possible and finish with a fun little brain workout.

## Recent changes

- Expanded the quiz from five questions to ten questions.
- Added questions about history, technology, literature, nature, and geography.
- Randomized the order of quiz questions for each playthrough.
- Randomized the answer choices while preserving the correct answer.
- Re-randomized questions and answers whenever the player selects “Play again.”
- Updated the quiz totals and result messages to use the current question count.

## How it was made

This app was created as a small Vue 3 project using Vite for the development workflow.

The core logic lives in `src/App.vue`, where the app defines:

- a `questions` array containing each prompt, answer choices, correct answer, and explanation
- reactive state for the current question, selected answer, and score
- computed values for quiz progress and final result messages
- functions for choosing an answer, moving to the next question, and restarting the game

The interface is built with Vue templates and styled with a polished card-based layout. Bootstrap and Bootstrap Icons are used for the visual structure and icons, which help the quiz feel clean and modern without needing a heavy custom design system.

The app architecture is intentionally simple:

- `src/main.js` mounts the Vue app
- `src/App.vue` contains the quiz logic and UI
- `src/style.css` stores the custom visual styling

## Tech stack

- Vue 3
- Vite
- Bootstrap 5
- Bootstrap Icons

## Run locally

1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the dev server:
   ```bash
   npm run dev
   ```
3. Open the local URL shown in the terminal, typically `http://localhost:5173`.

## Build for production

```bash
npm run build
```

You can then preview the production build with:

```bash
npm run preview
```

## Deploy to GitHub Pages

The deployment workflow in `.github/workflows/deploy.yml` builds the app and deploys the `dist` folder through GitHub Actions.

In the repository's Pages settings:

1. Open **Settings > Pages**.
2. Set **Source** to **GitHub Actions**.
3. Push to the `main` branch or run **Deploy to GitHub Pages** from the Actions tab.

The workflow requests `contents: read`, `actions: read`, `pages: write`, and `id-token: write`, and attempts to enable Pages automatically. If the deployment action reports a 404, configure the Pages source manually and confirm the repository allows GitHub Actions to access Pages.

## Deploy to Netlify

The repository includes a `netlify.toml` file that configures Netlify to run `npm run build` and publish the generated `dist` folder. Import the repository into Netlify and use the detected settings. Netlify builds use the site root as the asset base; the GitHub Pages workflow sets its own repository subpath.

## Project purpose

This project is a beginner-friendly example of a small interactive front-end app: a quiz game that demonstrates component state, computed values, event handling, and a clean user experience in Vue.
