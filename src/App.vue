<script setup>
import { ref } from 'vue'
import QuestionBlock from '@/components/QuestionBlock.vue'

const topics = [
  {
    title: 'Story',
    help: 'Is the plot engaging and well-constructed?  Are there any moments where the game feels too slow or rushed? Do all plot points and events make sense within the story\'s world? Are the characters well-developed and multi-dimensional? What are the central themes of the story? How emotionally engaging is the story? How well-written is the dialogue and narration?'
  },
  {
    title: 'Gameplay',
    help: 'How intuitive and responsive are the controls? How well-designed are the levels or environments? Do they provide a good balance of challenge and reward?'
  },
  {
    title: 'Graphics',
    help: 'What kind of graphics style does the game have? Does the game look realistic? Does the game feel smooth and satisfying to play? Is the user interface intuitive and easy to navigate?'
  },
  {
    title: 'Audio',
    help: 'Did you like the background music? Sound effects? Sounds of the environment? Did you play without music?'
  },
  {
    title: 'Replay value',
    help: 'Does the game offer different modes or challenges? What incentives are there for replaying the game (e.g., multiple endings, unlockables)?'
  },
  {
    title: 'Difficulty',
    help: 'Was the game too easy? Too difficult? Just perfect? Is it possible to adjust the difficulty level?'
  },
  {
    title: 'Price',
    help: 'Is the price justified? Did you buy the game on sale?'
  },
  {
    title: 'Accessibility',
    help: 'What accessibility options are available (e.g., subtitles, colorblind modes)? How inclusive is the game for players with different needs? Were there big spiders or other triggers?'
  }
]

const summary = ref('')
const copyPasteArea = ref('')

const form = ref([
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' },
  { rating: 3, notes: '' }
])

const generateRating = (rating) => {
  let out = []
  for (let i = 0; i < rating; i++) {
    out.push('⭐')
  }
  for (let i = 0; i < 5 - rating; i++) {
    out.push('➖')
  }
  return out.join('')
}

const generateText = () => {
  let output = summary.value + '\n\n[table noborder=1]\n'

  for (const i in topics) {
    if (form.value[i].rating == null) continue

    output += '[tr]\n'
    output += `[td]${topics[i].title}[/td]\n`
    output += `[td]${generateRating(form.value[i].rating)}[/td]\n`
    output += `[td]${form.value[i].notes}[/td]\n`
    output += '[/tr]\n'
  }

  output += '[/table]'

  console.log(output)

  copyPasteArea.value = output

  navigator.clipboard.writeText(output).then(
    () => {
      alert('Review copied to clipboard')
    },
    () => {
      alert('Failed to copy to clipboard, text is now in the textarea below')
    }
  )
}
</script>

<template>
  <div class="max-w-3xl mx-auto my-12 text-gray-600 dark:text-gray-400">
    <div class="flex flex-col text-center w-full mb-12">
      <h1 class="sm:text-4xl text-3xl font-medium title-font mb-4 text-gray-900 dark:text-white">
        Easy Compact Steam Review Template
      </h1>
      <p class="lg:w-2/3 mx-auto leading-relaxed text-base">
        Want to be lazy but hate those checkbox reviews? Well this should generate bit more compact
        review template. I also added some helper questions because I think words are more helpful
        than few emojis.<br>
        <a href="https://github.com/Kehet/Easy-Compact-Steam-Review-Template" class="underline text-yellow-500">
          Fork the project on GitHub.
        </a>
      </p>
    </div>

    <div class="flex flex-col gap-y-12">
      <div>
        <label for="summary" class="grow text-2xl mb-2 text-gray-900 dark:text-white">Summary</label>

        <p class="mb-3">
          How enjoyable is the overall gameplay experience? What sets this game apart from others in
          its genre? What are the most fun or engaging parts of the game?
        </p>

        <textarea
          id="summary"
          v-model="summary"
          rows="4"
          class="w-full bg-gray-100 dark:bg-gray-800 bg-opacity-50 rounded-sm border border-gray-300 dark:border-gray-700 focus:border-yellow-500 focus:bg-white dark:focus:bg-gray-900 focus:ring-2 focus:ring-yellow-200 dark:focus:ring-yellow-900 text-base outline-hidden text-gray-700 dark:text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
        ></textarea>
      </div>

      <QuestionBlock
        v-for="(topic, i) in topics"
        :key="i"
        :id="`question-${i}`"
        :title="topic.title"
        :help="topic.help"
        v-model:rating="form[i].rating"
        v-model:notes="form[i].notes"
      />

      <div class="p-2 w-full">
        <button
          @click="generateText"
          class="flex mx-auto text-black bg-yellow-500 border-0 py-2 px-8 focus:outline-hidden hover:bg-yellow-600 rounded-sm text-lg"
        >
          Copy review
        </button>
      </div>

      <div class="p-2 w-full flex flex-col gap-y-3" v-show="copyPasteArea">
        <label for="copy-paste-area" class="grow text-2xl text-gray-900 dark:text-white">
          Copy formatted review here
        </label>

        <textarea
          id="copy-paste-area"
          readonly
          v-model="copyPasteArea"
          rows="4"
          class="w-full bg-gray-100 dark:bg-gray-800 bg-opacity-50 rounded-sm border border-gray-300 dark:border-gray-700 focus:border-yellow-500 focus:bg-white dark:focus:bg-gray-900 focus:ring-2 focus:ring-yellow-200 dark:focus:ring-yellow-900 text-base outline-hidden text-gray-700 dark:text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
        ></textarea>
      </div>

    </div>
  </div>
</template>
