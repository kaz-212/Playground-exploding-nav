<template>
  <div class="nav-container" @mouseenter="expand" @mouseleave="collapse" ref="navRef">
    <div class="nav-content">
      <span class="prefix" ref="prefixRef">My</span>
      <div class="letter-container">
        <span class="letter" ref="aRef">
          <span class="visible-letter">A</span>
          <span class="full-word">About</span>
        </span>
        <span class="letter" ref="bRef">
          <span class="visible-letter">B</span>
          <span class="full-word">Browse Work</span>
        </span>
        <span class="letter" ref="cRef">
          <span class="visible-letter">C</span>
          <span class="full-word">Contact</span>
        </span>
      </div>
      <span class="suffix" ref="suffixRef">'s</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'

const navRef = ref(null)
const prefixRef = ref(null)
const suffixRef = ref(null)
const aRef = ref(null)
const bRef = ref(null)
const cRef = ref(null)

const tl = gsap.timeline({ paused: true })

onMounted(() => {
  tl.to([prefixRef.value, suffixRef.value], { opacity: 0, duration: 0.3 }).to(
    [aRef.value, bRef.value, cRef.value],
    {
      width: 'auto',
      duration: 0.5,
      stagger: 0.1
    },
    '-=0.2'
  )
})

// const expand = () => {
tl.play()
// }

const collapse = () => {
  tl.reverse()
}
</script>

<style scoped lang="scss">
.nav-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 50px;
  background-color: #141414;
  color: #feedbf;
  font-family: Arial, sans-serif;
  z-index: 1000;
  display: flex;
  align-items: center;
  padding: 0 20px;
}

.nav-content {
  display: flex;
  align-items: center;
  font-size: 1.2em;
  font-weight: bold;
}

.letter-container {
  display: flex;
}

.letter {
  position: relative;
  margin: 0 5px;
  cursor: pointer;
  overflow: hidden;
  display: inline-block;
}

.visible-letter {
  display: inline-block;
}

.full-word {
  position: absolute;
  left: 0;
  top: 0;
  white-space: nowrap;
  opacity: 0;
  width: 0;
  overflow: hidden;
}
</style>
