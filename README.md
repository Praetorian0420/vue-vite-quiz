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

## Project purpose

This project is a beginner-friendly example of a small interactive front-end app: a quiz game that demonstrates component state, computed values, event handling, and a clean user experience in Vue.
