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
    return '历经一年的迭代，Vant 4.0 版本已正式发布，这是 Vant 自 2017 年开源以来发布的第四个重要版本。在本次迭代中，Vant 支持了深色模式，增加五个新组件，改善工具函数 API 并重构 Picker 等组件，同时继续在轻量化和易用性方面做出改进。历经一年的迭代，Vant 4.0 版本已正式发布，这是 Vant 自 2017 年开源以来发布的第四个重要版本。在本次迭代中，Vant 支持了深色模式，增加五个新组件，改善工具函数 API 并重构 Picker 等组件，同时继续在轻量化和易用性方面做出改进。历经一年的迭代，Vant 4.0 版本已正式发布，这是 Vant 自 2017 年开源以来发布的第四个重要版本。在本次迭代中，Vant 支持了深色模式，增加五个新组件，改善工具函数 API 并重构 Picker 等组件，同时继续在轻量化和易用性方面做出改进。历经一年的迭代，Vant 4.0 版本已正式发布，这是 Vant 自 2017 年开源以来发布的第四个重要版本。在本次迭代中，Vant 支持了深色模式，增加五个新组件，改善工具函数 API 并重构 Picker 等组件，同时继续在轻量化和易用性方面做出改进'
  }

  if (index % 3 === 0) {
    return '在构建现代网络体验时，如果您希望获得快速的体验并加以维持，那么对网站进行衡量、优化和监控就显得至关重要。性能对于任何线上项目的成功都起着重要作用，'
  }

  if (index % 2 === 0) {
    return '如果华佗再世,崇洋都被医治,外邦来学汉字,激发我民族意识。马钱子、决明子、苍耳子，还有莲子；黄药子、苦豆子、川楝子，我要面子。用我的方式，改写一部历史。没什么别的事，跟着我念几个字。山药当归枸杞 GO，山药 当归 枸杞 GO，看我抓一把中药，服下一帖骄傲~'
  }

  return '并帮助您采取正确的步骤来提高性能'
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
