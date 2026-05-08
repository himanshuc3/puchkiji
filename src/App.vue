<script lang="ts">
import Fireworks from './components/fireworks/index.vue'
import gsap from 'gsap'
import sarmooh from './assets/sarmooh.mp3'

const MADAM_STATUS: { [key: string]: 0 | 1 | 2 } = {
  YES: 0,
  NO: 1,
  BROKEN_HEART: 2
}

let animation = gsap.timeline({ repeat: 20 })

const audio = new Audio(sarmooh)
let playing = false
function playAudio() {
  if (playing) {
    return
  }

  try {
    audio.play()
    console.log(audio)
    playing = true
  } catch (err) {
    console.log(err)
  }
}

function init() {
  gsap.set('.staggers', { autoAlpha: 1 })
  animation
    .from('.staggers div', { y: 40, opacity: 0, stagger: 2 })
    .to('.staggers div', { y: -40, opacity: 0, stagger: 2 }, 2)


  document.addEventListener('click', playAudio)
}

export default {
  mounted() {
    init()
  },
  beforeDestroy() {
    document.removeEventListener('click', playAudio)
  },
  components: {
    Fireworks
  },
  data() {
    return {
      noTexts: [
        'No',
        'Are you sure?',
        'Are you pawsitive?',
        'Absolutely sure pumpkin?',
        'Madam ji, think about it',
        'If you say no, me sad',
        'Me very very sad',
        'Me very very very sad',
        'Me very very very very sad',
        "Ok fine, I'll stop asking",
        'Just kidding, PLEASE SAY YES',
        "Don't be a thief of joy!",
        "You're breaking my heart :("
      ],
      fireworks: new Array(5),
      noIndex: 0,
      status: MADAM_STATUS.NO,
      MADAM_STATUS
    }
  },

  methods: {
    onConfirm(e: any) {
      e.preventDefault()
      this.status = MADAM_STATUS.YES
    },
    onCancel(e: any) {
      e.preventDefault()
      if (this.noIndex == this.noTexts.length - 1) {
        this.status = MADAM_STATUS.BROKEN_HEART
        return
      }
      this.noIndex++
    }
  },
  computed: {
    noText() {
      return this.noTexts[this.noIndex % this.noTexts.length]
    },
    buttonSize() {
      return {
        fontSize: this.noIndex * 15 + 16 + 'px'
      }
    }
  }
}
</script>

<template>
  <div id="root-askherout">
    <!-- <audio id="audio" ref="audio" src="/media/cc0-audio/t-rex-roar.mp3">
    </audio> -->
    <Fireworks v-if="status == MADAM_STATUS.YES" v-for="firwork in fireworks" />
    <div v-if="status == MADAM_STATUS.NO" class="askingher">
      <img class="title-gif" src="https://gifdb.com/images/high/cute-love-bear-roses-ou7zho5oosxnpo6k.gif" alt="" />
      <h1 class="title">
        Will you
        <div class="parent-staggers">
          <div class="staggers">
            <div>want to go for <span class="yellow">slow walks with me 🧑‍🤝‍🧑</span></div>
            <div>want more <span class="yellow">chai-sutta chronicles ☕</span></div>
            <div>give me a <span class="yellow">second chance 🤞</span></div>
          </div>
        </div>
      </h1>
      <div class="cta-container">
        <button @click="onConfirm" :style="buttonSize">
          <a href="https://www.yes.com">Yes</a>
        </button>
        <button @click="onCancel">
          <a href="https://www.no.com">{{ noText }}</a>
        </button>
      </div>
    </div>

    <div v-else-if="status == MADAM_STATUS.BROKEN_HEART" class="final">
      <img src="https://media.tenor.com/hV-KbIYraLcAAAAi/milk-and-mocha-bear.gif" alt="" />
      <h2>You really wanted to click no HUH 💔</h2>
      <h2 class="greyed-sub">
        Paste the line in chat:<br />
        "I Donut care about you." <br />
        & I'll understand you are Robert Frost <br />
        and I am the road not taken.
      </h2>
    </div>
    <div v-else class="final">
      <img src="https://media.tenor.com/gUiu1zyxfzYAAAAi/bear-kiss-bear-kisses.gif" alt="" />
      <h2>Ok yay, I want us time 💑</h2>
      <h2 class="greyed-sub">I miss you "Good mornings!", college rants and religious affirmations.</h2>
    </div>
  </div>
</template>

<style scoped>
.askingher,
#root-askherout {
  width: 500px;
  display: flex;
  margin: auto;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#audio {
  position: absolute;
}

.askingher {
  width: 100%;
}

h1,
h2 {
  color: black;
  text-align: center;
}

.title-gif {
  height: 200px;
}

.cta-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.valentine {
  color: rgba(200, 200, 200, 0.8);
  text-decoration: line-through;
}

button,
input[type='submit'],
input[type='reset'] {
  background: none;
  color: inherit;
  border: none;
  padding: 0;
  font: inherit;
  cursor: pointer;
  outline: inherit;
  border-radius: 5px;
  color: white;
  padding: 0.5rem 1rem;
}

.yellow {
  color: #256f56;
}

.poster-color {
  color: #c66053;
}

button {
  margin: 5px;
}

button a {
  font-weight: bold;
  color: white;
}

/* button a:hover {
  background-color: inherit; */
/* } */

.parent-staggers {
  position: relative;
}

.staggers {
  perspective: 200px;
  visibility: hidden;
  height: 100%;
  display: inline-block;
}

.staggers div {
  position: absolute;
  display: inline-block;
  left: 50%;
  top: 50%;
  font-weight: 600;
  font-size: 2rem;
  transform: translate(-50%, -50%);
  white-space: nowrap;
}

.final {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.final img {
  height: 200px;
  width: auto;
  object-fit: contain;
}

.final h2 {
  font-weight: bold;
  font-size: 1.75rem;
  margin-top: 20px;
}

.final .greyed-sub {
  /* color: rgba(200, 200, 200, 0.8); */
  color: #9ca1de;
  font-size: 1.5rem;
  font-weight: normal;
}

#root-askherout button:first-child {
  background-color: rgb(34, 197, 94);
  margin-right: 20px;
}

button:nth-child(2) {
  background-color: rgb(239, 68, 68);
}

@media screen and (max-width: 800px) {
  .staggers div {
    font-size: 1.5rem;
  }
}
</style>
