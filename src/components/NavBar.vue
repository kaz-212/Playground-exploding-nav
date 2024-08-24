<template>
  <header class="nav-container" @mouseenter="expand" ref="navRef">
    <nav class="nav-content" ref="navContent">
      <span class="prefix" ref="prefixRef">My</span>
      <ul class="links">
        <li class="link-container">
          <span class="visible-letter">A</span>
          <span class="full-word">About</span>
        </li>
        <li class="link-container">
          <span class="visible-letter">B</span>
          <span class="full-word">Browse</span>
        </li>
        <li class="link-container">
          <span class="visible-letter">C</span>
          <span class="full-word">Contact</span>
        </li>
      </ul>
      <span class="suffix" ref="suffixRef">'s</span>
    </nav>
  </header>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import gsap from 'gsap'

const navRef = ref(null)
const navContent = ref(null)
const prefixRef = ref(null)
const suffixRef = ref(null)

let scrollListener = null

const tl = gsap.timeline({ paused: true })

onMounted(() => {
  tl.to([prefixRef.value, suffixRef.value], { opacity: 0, duration: 0.2 })
    .to(
      navContent.value,
      {
        duration: 0.5,
        width: '50%'
      },
      '<0.1'
    )
    .to(
      '.link-container',
      {
        marginRight: '200px',
        duration: 1.2,
        ease: 'elastic.out(1,1)'
      },
      '<'
    )
    .to(
      '.full-word',
      {
        opacity: 1
      },
      '<'
    )
})

const expand = () => {
  tl.play()
  addScrollListener()
}

const collapse = () => {
  tl.reverse()
  removeScrollListener()
}

const handleScroll = () => {
  collapse()
}

const addScrollListener = () => {
  if (!scrollListener) {
    scrollListener = window.addEventListener('scroll', handleScroll, { passive: true })
  }
}

const removeScrollListener = () => {
  if (scrollListener) {
    window.removeEventListener('scroll', handleScroll)
    scrollListener = null
  }
}

onBeforeUnmount(() => {
  removeScrollListener()
})
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

  .nav-content {
    display: flex;
    width: auto;
    font-size: 1.2em;
    font-weight: bold;
    /* background-color: red; */
  }

  .links {
    display: flex;
    width: 100%;
    /* justify-content: space-between; */
    list-style: none;
    cursor: pointer;

    .link-container {
      position: relative;

      .full-word {
        position: absolute;
        left: 0;
        opacity: 0;
      }
    }
  }
}
</style>
