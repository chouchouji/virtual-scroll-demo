<template>
  <div class="container">
    <VirtualScroller :items="list" />
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import VirtualScroller from './views/VirtualScroller.vue'

const list = ref<Array<{ id: number; label: string }>>([])

const getLabel = (index: number) => {
  if (index % 4 === 0) {
    return 'It is a known fact that the right styleguide can significantly increase development speed, debugging, and the implementation of new features in legacy code. Sadly, most CSS codebases are sometimes developed without any structure or naming conventions. This leads to an unmaintainable CSS codebase in the long term.'
  }

  if (index % 3 === 0) {
    return 'The BEM approach ensures that everyone who participates in the development of a website works with a single codebase and speaks the same language. Using proper naming will prepare you for the changes in design of the website.'
  }

  if (index % 2 === 0) {
    return 'Parts of a block and have no standalone meaning. Any element is semantically tied to its block.'
  }

  return 'On smaller brochure sites, how you organize your styles isn’t usually a big concern. You get in there, write some CSS, or maybe even some SASS. You compile it all into a single stylesheet with SASS’s production settings, and then you aggregate it to get all the stylesheets from modules into a nice tidy package.However, when it comes to larger, more complex projects, how you organize your code is the key to efficiency in at least these three ways: it affects how long it takes you to write code, how much of that code you’ll have to write and how much loading your browser will have to do. This becomes especially important when you’re working with teams of themers, and when high performance is essential.'
}

onMounted(() => {
  for (let i = 0; i < 100000; i++) {
    list.value.push({
      id: i,
      label: getLabel(i)
    })
  }
})
</script>

<style scoped>
.container {
  border: 1px solid #ccc;
}
</style>
