<script setup>
import { computed, ref } from 'vue'

const questions = [
  {
    category: 'Nature',
    prompt: 'What is the largest planet in our solar system?',
    answers: ['Earth', 'Saturn', 'Jupiter', 'Neptune'],
    correctAnswer: 2,
    explanation: 'Jupiter is the largest planet in our solar system.',
  },
  {
    category: 'Geography',
    prompt: 'Which city is the capital of Japan?',
    answers: ['Kyoto', 'Tokyo', 'Osaka', 'Sapporo'],
    correctAnswer: 1,
    explanation: 'Tokyo is the capital and most populous city of Japan.',
  },
  {
    category: 'Science',
    prompt: 'What do plants absorb from the atmosphere?',
    answers: ['Oxygen', 'Nitrogen', 'Helium', 'Carbon dioxide'],
    correctAnswer: 3,
    explanation: 'Plants absorb carbon dioxide and use it during photosynthesis.',
  },
  {
    category: 'Art & culture',
    prompt: 'Who painted the Mona Lisa?',
    answers: ['Vincent van Gogh', 'Claude Monet', 'Leonardo da Vinci', 'Pablo Picasso'],
    correctAnswer: 2,
    explanation: 'Leonardo da Vinci painted the Mona Lisa in the early 16th century.',
  },
  {
    category: 'The natural world',
    prompt: 'What is the fastest land animal?',
    answers: ['Cheetah', 'Pronghorn', 'Lion', 'Greyhound'],
    correctAnswer: 0,
    explanation: 'A cheetah can sprint faster than any other land animal.',
  },
]

const questionIndex = ref(0)
const selectedAnswer = ref(null)
const score = ref(0)

const currentQuestion = computed(() => questions[questionIndex.value])
const isComplete = computed(() => questionIndex.value >= questions.length)
const progress = computed(
  () => Math.round(((questionIndex.value + (selectedAnswer.value === null ? 0 : 1)) / questions.length) * 100),
)
const resultMessage = computed(() => {
  if (score.value === questions.length) return 'Perfect score! You really know your stuff.'
  if (score.value >= 3) return 'Great work! You know a little about a lot.'
  return 'Nice try! Every question is a chance to learn something new.'
})

function chooseAnswer(index) {
  if (selectedAnswer.value !== null || isComplete.value) return

  selectedAnswer.value = index
  if (index === currentQuestion.value.correctAnswer) score.value += 1
}

function answerClass(index) {
  if (selectedAnswer.value === null) return 'answer-option'
  if (index === currentQuestion.value.correctAnswer) return 'answer-option is-correct'
  if (index === selectedAnswer.value) return 'answer-option is-incorrect'
  return 'answer-option is-unselected'
}

function nextQuestion() {
  if (selectedAnswer.value === null) return

  questionIndex.value += 1
  selectedAnswer.value = null
}

function restartQuiz() {
  questionIndex.value = 0
  selectedAnswer.value = null
  score.value = 0
}
</script>

<template>
  <div class="page-shell">
    <header class="topbar">
      <a class="brand" href="#" aria-label="Little Quiz home" @click.prevent="restartQuiz">
        <span class="brand-mark"><i class="bi bi-lightning-charge-fill" aria-hidden="true"></i></span>
        <span>little<span class="brand-accent">quiz</span></span>
      </a>
      <span class="topbar-note"><i class="bi bi-stars" aria-hidden="true"></i> A little fun for your brain</span>
    </header>

    <main class="quiz-content">
      <section class="intro">
        <span class="eyebrow"><span class="eyebrow-dot"></span> YOUR MOMENT OF CURIOSITY</span>
        <h1>A little quiz.<br /><span>A lot of fun.</span></h1>
        <p>Five quick questions to give your brain a happy little stretch.</p>
      </section>

      <section v-if="!isComplete" class="quiz-card" aria-label="Quiz">
        <div class="quiz-card-top">
          <div class="question-count">
            <span class="count-current">{{ questionIndex + 1 }}</span>
            <span>of {{ questions.length }} questions</span>
          </div>
          <div class="score-pill" aria-live="polite">
            <i class="bi bi-stars" aria-hidden="true"></i>
            {{ score }} <span>points</span>
          </div>
        </div>

        <div
          class="progress quiz-progress"
          role="progressbar"
          :aria-valuenow="progress"
          aria-valuemin="0"
          aria-valuemax="100"
          :aria-label="`Quiz progress: ${progress}%`"
        >
          <div class="progress-bar" :style="{ width: `${progress}%` }"></div>
        </div>

        <div class="question-area">
          <span class="category-tag">{{ currentQuestion.category }}</span>
          <h2>{{ currentQuestion.prompt }}</h2>
          <p class="question-hint">Choose one answer to keep going.</p>
        </div>

        <div class="answer-list" role="group" :aria-label="`Answer choices for: ${currentQuestion.prompt}`">
          <button
            v-for="(answer, index) in currentQuestion.answers"
            :key="answer"
            type="button"
            class="answer-option"
            :class="answerClass(index)"
            :aria-pressed="selectedAnswer === index"
            :disabled="selectedAnswer !== null"
            @click="chooseAnswer(index)"
          >
            <span class="answer-letter">{{ String.fromCharCode(65 + index) }}</span>
            <span class="answer-text">{{ answer }}</span>
            <i
              v-if="selectedAnswer !== null && index === currentQuestion.correctAnswer"
              class="bi bi-check-circle-fill answer-result-icon"
              aria-hidden="true"
            ></i>
            <i
              v-else-if="selectedAnswer === index"
              class="bi bi-x-circle-fill answer-result-icon"
              aria-hidden="true"
            ></i>
          </button>
        </div>

        <div
          v-if="selectedAnswer !== null"
          class="answer-feedback"
          :class="selectedAnswer === currentQuestion.correctAnswer ? 'feedback-correct' : 'feedback-incorrect'"
          role="status"
          aria-live="polite"
        >
          <i
            class="bi"
            :class="selectedAnswer === currentQuestion.correctAnswer ? 'bi-emoji-smile-fill' : 'bi-lightbulb-fill'"
            aria-hidden="true"
          ></i>
          <span>
            <strong>{{ selectedAnswer === currentQuestion.correctAnswer ? 'That’s right!' : 'Not quite!' }}</strong>
            {{ currentQuestion.explanation }}
          </span>
        </div>

        <div class="card-footer-actions">
          <span v-if="selectedAnswer === null" class="action-hint">Your score is looking good!</span>
          <span v-else class="action-hint">Ready for the next one?</span>
          <button class="btn btn-primary next-button" type="button" :disabled="selectedAnswer === null" @click="nextQuestion">
            {{ questionIndex === questions.length - 1 ? 'See my results' : 'Next question' }}
            <i class="bi bi-arrow-right" aria-hidden="true"></i>
          </button>
        </div>
      </section>

      <section v-else class="quiz-card results-card" aria-live="polite">
        <div class="results-icon"><i class="bi bi-trophy-fill" aria-hidden="true"></i></div>
        <span class="category-tag">QUIZ COMPLETE</span>
        <h2>You did it!</h2>
        <p class="results-message">{{ resultMessage }}</p>
        <div class="final-score">
          <span class="final-score-number">{{ score }}</span>
          <span class="final-score-total">/ {{ questions.length }}<br />points</span>
        </div>
        <div
          class="progress quiz-progress results-progress"
          role="progressbar"
          :aria-valuenow="Math.round((score / questions.length) * 100)"
          aria-valuemin="0"
          aria-valuemax="100"
          :aria-label="`Final score: ${score} out of ${questions.length}`"
        >
          <div class="progress-bar" :style="{ width: `${(score / questions.length) * 100}%` }"></div>
        </div>
        <button class="btn btn-primary restart-button" type="button" @click="restartQuiz">
          <i class="bi bi-arrow-counterclockwise" aria-hidden="true"></i>
          Play again
        </button>
      </section>

      <footer class="page-footer">
        <span>Made for curious minds</span>
        <span aria-hidden="true">·</span>
        <span>One question at a time <i class="bi bi-heart-fill footer-heart" aria-hidden="true"></i></span>
      </footer>
    </main>
  </div>
</template>
