<template>
  <div
    ref="container"
    class="infinite-list-container"
    :style="{
      height: `${height}px`
    }"
    @scroll="handleScroll()"
  >
    <div class="infinite-list-phantom" :style="{ height: `${listHeight}px` }"></div>
    <div
      ref="list"
      class="infinite-list"
      :style="{ transform: `translate3d(0, ${startOffset}px, 0)` }"
    >
      <div class="infinite-list-item" v-for="item in visibleData" :key="item.id">
        {{ item.label }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, onUpdated, ref, watchEffect } from 'vue'

const CACHE_COUNT = 5

const props = defineProps({
  items: {
    type: Array<{ id: number; label: string }>,
    default: () => []
  },
  itemSize: {
    type: Number,
    default: 50
  },
  height: {
    type: Number,
    default: 400
  },
  estimateHeight: {
    type: Number,
    default: 60
  }
})

const startOffset = ref(0)
const start = ref(0)
const lastStart = ref(0)
const end = ref(0)
const container = ref<null | HTMLElement>(null)
const list = ref<null | HTMLElement>(null)
const cachedData = ref<
  {
    index: number
    height: number
    top: number
    bottom: number
  }[]
>([])

const listHeight = computed(() => cachedData.value[props.items.length - 1]?.bottom || 0)
const visibleCount = computed(() => Math.ceil(props.height / props.estimateHeight))
const visibleData = computed(() =>
  props.items.slice(start.value, Math.min(end.value, props.items.length - 1))
)

const initCachedData = () => {
  props.items.forEach((_value, index) => {
    cachedData.value.push({
      index,
      height: props.estimateHeight,
      top: index * props.estimateHeight,
      bottom: (index + 1) * props.estimateHeight
    })
  })
}

const updateCachedData = () => {
  const nodes = [...list.value!.children]
  nodes.forEach((node, index) => {
    const currentIndex = start.value + index
    const height = node.getBoundingClientRect().height
    cachedData.value[currentIndex].height = height
    cachedData.value[currentIndex].bottom = height + cachedData.value[currentIndex].top
    cachedData.value[currentIndex].top = cachedData.value?.[currentIndex - 1]?.bottom || 0
  })
}

const getCurrentIndex = (scrollTop: number) => {
  let start = 0
  let end = cachedData.value.length - 1

  while (start <= end) {
    const mid = Math.floor((start + end) / 2)
    const data = cachedData.value[mid]

    if (data.top <= scrollTop && data.bottom >= scrollTop) {
      return mid
    } else if (data.top > scrollTop) {
      end = mid - 1
    } else if (data.bottom < scrollTop) {
      start = mid + 1
    }
  }
}

const handleScroll = () => {
  let scrollTop = container.value!.scrollTop

  start.value = getCurrentIndex(scrollTop) || 0

  if (lastStart.value === start.value) {
    return
  }

  if (start.value > CACHE_COUNT) {
    start.value -= CACHE_COUNT
  }

  lastStart.value = start.value

  end.value = start.value + visibleCount.value + CACHE_COUNT

  startOffset.value = cachedData.value[start.value].top
}

watchEffect(() => {
  initCachedData()
})

onMounted(() => {
  start.value = 0
  end.value = start.value + visibleCount.value + CACHE_COUNT
})

onUpdated(() => {
  if (props.items.length > 0 && list.value) {
    updateCachedData()
  }
})
</script>

<style scoped>
.infinite-list-container {
  overflow: auto;
  position: relative;
}

.infinite-list-phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}

.infinite-list {
  left: 0;
  right: 0;
  top: 0;
  position: absolute;
}

.infinite-list-item {
  padding: 10px;
  overflow: hidden;
  text-align: center;
  color: #555;
  border: 1px solid #ccc;
  box-sizing: border-box;
}
</style>
