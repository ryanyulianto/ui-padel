<script setup lang="ts">
import { TrendingUp, TrendingDown } from 'lucide-vue-next'

defineProps<{
  title: string
  value: string
  percentage: string
  trend: 'up' | 'down'
  trendLabel?: string
}>()
</script>

<template>
  <div class="bg-card border border-border/80 rounded-2xl p-5 shadow-sm hover:shadow-md hover:border-border transition-all duration-300 relative overflow-hidden group">
    <!-- Card header title -->
    <div class="text-xs font-bold uppercase tracking-wider text-muted-foreground mb-3 group-hover:text-primary transition-colors">
      {{ title }}
    </div>

    <!-- Main Value and Trend info -->
    <div class="space-y-4">
      <div class="text-2xl md:text-3xl font-extrabold tracking-tight text-foreground">
        {{ value }}
      </div>

      <!-- Trend row -->
      <div class="flex items-center gap-2">
        <!-- Trend Badge -->
        <span 
          class="inline-flex items-center gap-1 px-2 py-0.5 rounded-full text-[10px] font-extrabold border bg-emerald-50 text-emerald-600 border-emerald-200 dark:bg-emerald-950/20 dark:text-emerald-400 dark:border-emerald-900/30"
        >
          <TrendingUp v-if="trend === 'up'" class="size-3" />
          <TrendingDown v-else class="size-3" />
          <span>{{ percentage }}</span>
        </span>
        <span class="text-[11px] text-muted-foreground font-semibold">
          {{ trendLabel || 'per last week' }}
        </span>
      </div>
    </div>
  </div>
</template>
