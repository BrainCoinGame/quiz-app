<template>
  <div class="game-container">

    <!-- Header с информацией об игроке -->
    <div class="flex flex-wrap justify-between items-center w-full mb-4">
      <div class="bg-[#1E2029] rounded-lg p-2 flex-1 mr-2 mb-2">
        <p class="text-gray-400 text-xs mb-1 truncate">ID игрока</p>
        <p class="text-white text-sm">{{ playerId }}</p>
      </div>
      <div class="bg-[#1E2029] rounded-lg p-2 flex-1">
        <p class="text-gray-400 text-xs mb-1 text-center">Общий счет</p>
        <p class="text-white text-sm text-center">{{ totalScore }}</p>
      </div>
    </div>
<!-- Логотип -->
<div class="logo-container flex justify-center items-center mb-4">
      <img src="/logo.png" alt="Логотип" class="logo" />
    </div>
    <!-- Выбор темы -->
    <template v-if="!selectedTopic">
      <h1 class="text-xl font-medium text-center mb-6 text-white">Викторина</h1>
      <div class="grid grid-cols-2 sm:grid-cols-2 md:grid-cols-3 gap-3 w-full">
        <div
          v-for="topic in topics"
          :key="topic.id"
          @click="selectTopic(topic.id)"
          class="bg-[#1E2029] rounded-lg p-3 cursor-pointer transition-transform hover:scale-105 text-center"
        >
          <p class="text-white text-sm">{{ topic.name }}</p>
        </div>
      </div>
    </template>

    <!-- Викторина -->
    <template v-else-if="currentQuestionData">
      <div class="bg-[#1E2029] rounded-lg p-4 w-full h-full">
        <div class="flex items-center justify-between mb-4">
          <h2 class="text-sm font-medium text-white">{{ getTopicName }}</h2>
          <span class="text-gray-400 text-xs">
            {{ currentQuestion + 1 }} / {{ totalQuestions }}
          </span>
        </div>

        <quiz-question
          :question="currentQuestionData.question"
          :options="currentQuestionData.options"
          @answer="handleAnswer"
        />

        <!-- Кнопки в два ряда -->
        <div class="mt-4 grid grid-cols-2 gap-3">
          <button
            @click="backToTopics"
            class="bg-gray-700 text-white px-3 py-2 text-xs rounded-lg hover:bg-gray-600"
          >
            Назад
          </button>
          <div class="text-xs text-green-400 flex items-center justify-center">
            Счет: {{ score }}
          </div>
          <button
            @click="nextQuestion"
            class="bg-gray-700 text-white px-3 py-2 text-xs rounded-lg hover:bg-gray-600"
          >
            Следующий
          </button>
        </div>
      </div>
    </template>

    <!-- Завершение викторины -->
    <template v-else>
      <div class="bg-[#1E2029] rounded-lg p-4 w-full text-center">
        <h2 class="text-lg text-white mb-2">Викторина завершена!</h2>
        <p class="text-gray-300 text-sm">Ваш результат: {{ score }} из {{ totalQuestions }}</p>
        <button
          @click="backToTopics"
          class="mt-4 bg-gray-700 text-white px-3 py-2 text-xs rounded-lg hover:bg-gray-600"
        >
          Вернуться к темам
        </button>
      </div>
    </template>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import QuizQuestion from "./QuizQuestion.vue";

const playerId = ref(Math.random().toString().slice(2, 8));
const totalScore = ref(0); // Общий счет игрока
const selectedTopic = ref(null);
const currentQuestion = ref(0);
const score = ref(0); // Счет в текущей викторине

const topics = [
  { id: 1, name: "Литература" },
  { id: 2, name: "Кино" },
  { id: 3, name: "Музыка" },
  { id: 4, name: "География" },
  { id: 5, name: "Наука" },
  { id: 6, name: "Технологии" },
];

const questions = {
  1: [
    {
      question: "Какой автор написал 'Войну и мир'?",
      options: ["Толстой", "Достоевский", "Гоголь", "Чехов"],
      correct: 0,
    },
    // другие вопросы...
  ],
  // другие темы...
};

const getTopicName = computed(() => {
  return topics.find((topic) => topic.id === selectedTopic.value)?.name || '';
});

const currentQuestionData = computed(() => {
  return questions[selectedTopic.value]?.[currentQuestion.value] || null;
});

const totalQuestions = computed(() => {
  return questions[selectedTopic.value]?.length || 0;
});

const nextQuestion = () => {
  if (currentQuestion.value < totalQuestions.value - 1) {
    currentQuestion.value++;
  } else {
    score.value++;
  }
};

const handleAnswer = (index) => {
  const correct = currentQuestionData.value.correct;
  if (index === correct) {
    score.value++;
  }
};

const selectTopic = (id) => {
  selectedTopic.value = id;
  currentQuestion.value = 0;
  score.value = 0;
};

const backToTopics = () => {
  selectedTopic.value = null;
  score.value = 0;
  currentQuestion.value = 0;
};
</script>

<style scoped>
.logo-container {
  margin-bottom: 20px; /* Отступ между логотипом и другими элементами */
}

.logo {
  max-width: 150px; /* Максимальная ширина логотипа */
  height: auto; /* Автоматическая высота для сохранения пропорций */
}

.game-container {
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 8px;
  margin: 0;
  background-color: #121212;
  overflow: hidden;
}

.p-4 {
  padding: 12px;
}

.text-center {
  text-align: center;
}

.transition-transform {
  transition: transform 0.2s;
}

.hover\:scale-105:hover {
  transform: scale(1.05);
}

.grid-cols-2 {
  grid-template-columns: repeat(2, minmax(0, 1fr));
}

.gap-3 {
  gap: 0.75rem;
}

@media (max-width: 768px) {
  .game-container {
    height: 100vh;
    justify-content: flex-start;
  }

  .p-4 {
    padding: 12px;
  }

  .grid-cols-2 {
    grid-template-columns: repeat(2, 1fr);
  }

  .hover\:scale-105:hover {
    transform: none;
  }

  h1 {
    font-size: 1.25rem;
  }

  .text-sm {
    font-size: 0.875rem;
  }

  .text-xs {
    font-size: 0.75rem;
  }

  .game-container button {
    margin-top: 20px;
  }
}
</style>
