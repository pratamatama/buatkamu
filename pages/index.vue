<script lang="ts" setup>
const texts = [
  'Bentar, lagi loading... Sabar yaah :)',
  'Oke, semuanya udah siappp ^^',
  'Hey kamu!',
  'Iya kamu..',
  'Jangan sedih terus..',
  'Aku tau kehilangan orang yang kamu sayang itu sakit.',
  'Kecewa itu wajar...',
  'Aku paham, dan mungkin kamu butuh waktu buat sendiri..',
  "It's okay.. Terkadang, menyendiri itu lebih baik..",
  'Tapi jangan sampe kamu sedih berlarut-larut...',
  'Yahh... Meskipun aku bukan siapa-siapa, aku cuma pengen kamu tau.',
  'Banyak orang di luar sana yang lebih menghargai kamu.',
  'Yang punya keinginan lebih kuat buat bahagiain kamu.',
  'Meskipun yah.. Nggak sebanding kalo dibandingin sama yang dulu.',
  'Mereka yang bakal terus berusaha buat ngehibur kamu.',
  'Walaupun mereka tahu mereka bukan orang yang humoris dan bercandanya garing.',
  'Tapi setidaknya, bisa ngurangin sedikit rasa sedih, juga sakitmu.',
  'Jadi.. Tetep semangat! ^^',
  'Terus percaya kalau hari esok akan jadi hari yang lebih baik..',
  'Semangat yaa..',
  'Aku bakal terus dukung kamu.',
  'Sampai tiba waktunya kamu siap buat kita diskusi bareng lagi.',
  '&#128538;',
  'Dari aku, #buatkamuu.',
  'Maaf yaa kalo jelek, aku nggak bisa desain &#128514;',
]

const audio = ref<HTMLAudioElement | null>(null)

const audioMuted = ref(false)

const playAudio = () => {
  if (audio.value) audio.value.play()
}

const muteAudio = () => {
  if (audio.value) {
    audio.value.muted = !audio.value.muted
    audioMuted.value = !audioMuted.value
  }
}

const timeout = ref<NodeJS.Timer | null>(null)
const clearTimer = () => {
  if (timeout.value) clearTimeout(timeout.value)
}

onMounted(() => {
  audio.value = new Audio('/music.mp3')
  audio.value.addEventListener('ended', playAudio)
  clearTimer()
  timeout.value = setTimeout(next, 3000)
})

onUnmounted(() => {
  clearTimer()
  audio.value?.pause()
  audio.value?.removeEventListener('ended', playAudio)
})

const index = ref(0)

const text = computed(() => texts[index.value])

const theEnd = computed(() => index.value === texts.length - 1)

const next = () => {
  if (index.value === 1) {
    console.log('playing audio...')
    playAudio()
  }
  if (!theEnd.value) index.value++
}

const footerText = computed(() =>
  theEnd.value
    ? 'Horee! Teks nya udah abis, tetep semangat yaaa~ (^.^)/'
    : 'Tap layarnya buat baca teks selanjutnya :)'
)
</script>

<template>
  <div
    class="bg-pink-200 text-pink-600 text-center h-screen w-screen"
    :class="{ 'cursor-pointer': !theEnd, 'cursor-not-allowed': theEnd }"
  >
    <div
      @click="next"
      class="relative h-full flex items-center justify-center p-6"
    >
      <div class="container mx-auto">
        <p class="text-3xl mb-4" v-html="text" />
      </div>
    </div>
    <div v-if="index >= 1" class="relative">
      <div class="absolute bottom-10 left-1/2 transform -translate-x-1/2">
        <button @click="muteAudio" class="mb-4 p-4">
          <MuteIcon v-if="audioMuted" />
          <UnmuteIcon v-else />
        </button>
        <p class="text-sm text-center" v-html="footerText" />
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
</style>
