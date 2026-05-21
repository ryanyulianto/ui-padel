<script setup lang="ts">
defineProps<{
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
</script>

<template>
  <div class="bg-slate-950 text-white rounded-2xl p-4 shadow-xl border border-white/10 w-52 pointer-events-none select-none">
    <div class="text-[10px] text-white/50 font-bold uppercase tracking-wider mb-2.5">
      {{ title }}
    </div>
    <div class="space-y-1.5 text-xs font-semibold">
      <div v-for="(item, key) in data" :key="key" class="flex justify-between items-center">
        <div class="flex items-center gap-1.5">
          <span class="size-2 rounded-full" :style="{ backgroundColor: item.color }"></span>
          <span class="capitalize text-white/90">{{ item.name }}</span>
        </div>
        <span class="font-extrabold text-white">{{ formatNumber(item.value) }}</span>
      </div>
    </div>
  </div>
</template>
