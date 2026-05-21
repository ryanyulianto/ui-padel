<script setup lang="ts">
import { MoreHorizontal } from 'lucide-vue-next'

interface StructureItem {
  label: string
  amount: number
  percentage: number
  colorClass: string
}

const structureItems: StructureItem[] = [
  { 
    label: 'Patient services', 
    amount: 45600, 
    percentage: 38, 
    colorClass: 'bg-primary shadow-sm shadow-primary/10' 
  },
  { 
    label: 'Insurance claims', 
    amount: 36000, 
    percentage: 30, 
    colorClass: 'bg-primary/40' 
  },
  { 
    label: 'Packages', 
    amount: 14400, 
    percentage: 12, 
    colorClass: 'bg-primary/15' 
  },
  { 
    label: 'Other', 
    amount: 12000, 
    percentage: 20, 
    colorClass: 'bg-[repeating-linear-gradient(45deg,_rgba(0,0,0,0.05)_0,_rgba(0,0,0,0.05)_2px,_transparent_2px,_transparent_6px)] dark:bg-[repeating-linear-gradient(45deg,_rgba(255,255,255,0.05)_0,_rgba(255,255,255,0.05)_2px,_transparent_2px,_transparent_6px)] border border-border/80' 
  }
]

const formatCurrency = (val: number) => {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(val)
}
</script>

<template>
  <div class="style-card flex flex-col justify-between">
    <!-- Header -->
    <div class="flex items-center justify-between mb-6">
      <h3 class="text-base font-bold tracking-tight text-foreground">Financial structure</h3>
      <button class="p-1 rounded-lg hover:bg-muted text-muted-foreground cursor-pointer transition-colors">
        <MoreHorizontal class="size-4" />
      </button>
    </div>

    <!-- Details Columns -->
    <div class="grid grid-cols-4 gap-4 mb-6">
      <div v-for="item in structureItems" :key="item.label" class="space-y-1">
        <span class="block text-[10px] text-muted-foreground font-bold tracking-wider uppercase leading-none">
          {{ item.label }}
        </span>
        <span class="block text-sm md:text-base font-extrabold text-foreground leading-tight">
          {{ formatCurrency(item.amount) }}
        </span>
      </div>
    </div>

    <!-- Segmented Capsule Progress Bars (High-Fidelity Match) -->
    <div class="space-y-2">
      <!-- ProgressBar segments container -->
      <div class="w-full h-8 flex gap-1.5 rounded-full overflow-hidden select-none">
        <div 
          v-for="item in structureItems" 
          :key="item.label"
          :class="['h-full rounded-full transition-all duration-700 ease-out hover:brightness-95 hover:scale-[1.01] cursor-pointer', item.colorClass]"
          :style="{ width: `${item.percentage}%` }"
        ></div>
      </div>

      <!-- Percentage Labels beneath the bar -->
      <div class="w-full flex gap-1.5 text-[10px] font-extrabold text-muted-foreground">
        <div 
          v-for="item in structureItems" 
          :key="item.label"
          class="transition-all duration-300"
          :style="{ width: `${item.percentage}%` }"
        >
          {{ item.percentage }}%
        </div>
      </div>
    </div>
  </div>
</template>
