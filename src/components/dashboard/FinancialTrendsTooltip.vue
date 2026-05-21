<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  title?: string
  data: {
    name: string
    color: string
    value: any
  }[]
}>()

const formatNumber = (num: number) => {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(num)
}

// Filter out non-numeric metadata keys (like the "day" field) to avoid NaN errors
const filteredData = computed(() => {
  return props.data.filter(item => typeof item.value === 'number' && item.name)
})
</script>

<template>
  <div class="bg-slate-950 text-white rounded-2xl p-4 shadow-xl border border-white/10 w-52 pointer-events-none select-none">
    <div class="text-[10px] text-white/50 font-bold uppercase tracking-wider mb-2.5">
      {{ title }}
    </div>
    <div class="space-y-1.5 text-xs font-semibold">
      <div v-for="(item, key) in filteredData" :key="key" class="flex justify-between items-center">
        <div class="flex items-center gap-1.5">
          <span class="size-2 rounded-full" :style="{ backgroundColor: item.color }"></span>
          <span class="capitalize text-white/90">{{ item.name }}</span>
        </div>
        <span class="font-extrabold text-white">{{ formatNumber(item.value) }}</span>
      </div>
    </div>
  </div>
</template>
