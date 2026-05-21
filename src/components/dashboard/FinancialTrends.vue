<script setup lang="ts">
import { MoreHorizontal } from 'lucide-vue-next'
import { CurveType } from '@unovis/ts'
import AreaChart from '@/components/ui/chart-area/AreaChart.vue'
import FinancialTrendsTooltip from '@/components/dashboard/FinancialTrendsTooltip.vue'

interface ChartData {
  day: string
  date: string
  revenue: number
  expenses: number
  profit: number
}

const chartData: ChartData[] = [
  { day: 'Mon', date: 'Mon Feb 1, 2026', revenue: 150000, expenses: 95000, profit: 55000 },
  { day: 'Tue', date: 'Tue Feb 2, 2026', revenue: 210000, expenses: 110000, profit: 100000 },
  { day: 'Wed', date: 'Wed Feb 3, 2026', revenue: 180000, expenses: 140000, profit: 40000 },
  { day: 'Thu', date: 'Thu Feb 4, 2026', revenue: 548300, expenses: 175500, profit: 375800 },
  { day: 'Fri', date: 'Fri Feb 5, 2026', revenue: 310000, expenses: 125000, profit: 185000 },
  { day: 'Sat', date: 'Sat Feb 6, 2026', revenue: 420000, expenses: 190000, profit: 230000 },
  { day: 'Sun', date: 'Sun Feb 7, 2026', revenue: 390000, expenses: 130000, profit: 260000 }
]

// Formatter for Y Axis (matches 0, 100k, 200k, 400k, 600k layout)
const yFormatter = (tick: number | Date) => {
  const num = typeof tick === 'number' ? tick : 0
  if (num === 0) return '0'
  return `${num / 1000}k`
}

// Formatter for X Axis (converts full date index back to day code: Mon, Tue...)
const xFormatter = (tick: number | Date) => {
  const index = typeof tick === 'number' ? tick : 0
  return chartData[index]?.day || ''
}
</script>

<template>
  <div class="style-card relative overflow-hidden flex flex-col justify-between">
    <!-- Header of Chart Section -->
    <div class="flex items-center justify-between mb-6">
      <h3 class="text-base font-bold tracking-tight text-foreground">Financial trends</h3>
      
      <!-- Legends & Menu -->
      <div class="flex items-center gap-6">
        <div class="flex items-center gap-4 text-xs font-bold">
          <div class="flex items-center gap-1.5">
            <span class="size-2 rounded-full bg-primary"></span>
            <span class="text-muted-foreground">Revenue</span>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="size-2 rounded-full bg-amber-500"></span>
            <span class="text-muted-foreground">Expenses</span>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="size-2 rounded-full bg-blue-500"></span>
            <span class="text-muted-foreground">Profit</span>
          </div>
        </div>

        <button class="p-1 rounded-lg hover:bg-muted text-muted-foreground cursor-pointer transition-colors" aria-label="More chart options">
          <MoreHorizontal class="size-4" />
        </button>
      </div>
    </div>

    <!-- Chart Drawing (Official AreaChart using Unovis) -->
    <div class="w-full overflow-x-auto select-none scrollbar-hide">
      <div class="min-w-[700px] pr-2">
        <AreaChart
          :data="chartData"
          index="date"
          :categories="['revenue', 'expenses', 'profit']"
          :colors="['hsl(var(--primary))', 'hsl(35 95% 55%)', 'hsl(215 95% 55%)']"
          :y-formatter="yFormatter"
          :x-formatter="xFormatter"
          :show-legend="false"
          :custom-tooltip="FinancialTrendsTooltip"
          :curve-type="CurveType.MonotoneX"
          :margin="{ left: 0, right: 0, top: 10, bottom: 0 }"
          class="h-[300px] w-full"
        />
      </div>
    </div>
  </div>
</template>
