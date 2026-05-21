<script setup lang="ts">
import { ref, computed } from 'vue'
import { MoreHorizontal } from 'lucide-vue-next'

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

// SVG settings
const width = 1000
const height = 300
const paddingLeft = 60
const paddingRight = 40
const paddingTop = 30
const paddingBottom = 40

// Scale X helper
const getX = (index: number) => {
  const chartWidth = width - paddingLeft - paddingRight
  return paddingLeft + (index / (chartData.length - 1)) * chartWidth
}

// Scale Y helper
const maxVal = 600000
const getY = (val: number) => {
  const chartHeight = height - paddingTop - paddingBottom
  return height - paddingBottom - (val / maxVal) * chartHeight
}

// Computed coordinates for paths
const revenuePoints = computed(() => chartData.map((d, i) => ({ x: getX(i), y: getY(d.revenue) })))
const expensesPoints = computed(() => chartData.map((d, i) => ({ x: getX(i), y: getY(d.expenses) })))
const profitPoints = computed(() => chartData.map((d, i) => ({ x: getX(i), y: getY(d.profit) })))

// Smooth Bezier Curve generator
const getCurvePath = (points: { x: number; y: number }[]) => {
  const firstPoint = points[0]
  if (!firstPoint) return ''
  let d = `M ${firstPoint.x} ${firstPoint.y}`
  for (let i = 0; i < points.length - 1; i++) {
    const p0 = points[i]
    const p1 = points[i + 1]
    if (!p0 || !p1) continue
    const cpX1 = p0.x + (p1.x - p0.x) / 3
    const cpY1 = p0.y
    const cpX2 = p0.x + (2 * (p1.x - p0.x)) / 3
    const cpY2 = p1.y
    d += ` C ${cpX1} ${cpY1}, ${cpX2} ${cpY2}, ${p1.x} ${p1.y}`
  }
  return d
}

const getAreaPath = (points: { x: number; y: number }[]) => {
  const linePath = getCurvePath(points)
  if (!linePath) return ''
  const bottomY = height - paddingBottom
  const lastPoint = points[points.length - 1]
  const firstPoint = points[0]
  if (!lastPoint || !firstPoint) return ''
  return `${linePath} L ${lastPoint.x} ${bottomY} L ${firstPoint.x} ${bottomY} Z`
}

// Interactive Hover State
const activeIndex = ref<number | null>(3) // Default to "Thu" just like in the mockup image!
const hoverX = ref<number>(getX(3))

const handleMouseMove = (event: MouseEvent) => {
  const svg = event.currentTarget as SVGSVGElement
  const rect = svg.getBoundingClientRect()
  const mouseX = ((event.clientX - rect.left) / rect.width) * width
  
  // Find closest point index
  let closestIndex = 0
  let minDiff = Infinity
  for (let i = 0; i < chartData.length; i++) {
    const x = getX(i)
    const diff = Math.abs(x - mouseX)
    if (diff < minDiff) {
      minDiff = diff
      closestIndex = i
    }
  }
  
  activeIndex.value = closestIndex
  hoverX.value = getX(closestIndex)
}

const handleMouseLeave = () => {
  // Keep the last active index or standard Thu selected to match image mockup
}

const activeData = computed(() => {
  if (activeIndex.value === null) return null
  return chartData[activeIndex.value] || null
})

const formatNumber = (num: number) => {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(num)
}
</script>

<template>
  <div class="bg-card border border-border/80 rounded-3xl p-6 shadow-sm relative overflow-hidden flex flex-col justify-between">
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

        <button class="p-1 rounded-lg hover:bg-muted text-muted-foreground cursor-pointer transition-colors">
          <MoreHorizontal class="size-4" />
        </button>
      </div>
    </div>

    <!-- Chart Drawing (Fully Custom SVG) -->
    <div class="relative w-full overflow-x-auto select-none">
      <svg 
        :width="width" 
        :height="height" 
        class="w-full h-auto min-w-[700px] cursor-crosshair overflow-visible"
        @mousemove="handleMouseMove"
        @mouseleave="handleMouseLeave"
      >
        <!-- Gradients -->
        <defs>
          <linearGradient id="revenueGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="hsl(var(--primary))" stop-opacity="0.15" />
            <stop offset="100%" stop-color="hsl(var(--primary))" stop-opacity="0.0" />
          </linearGradient>
          <linearGradient id="expensesGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="hsl(45 95% 50%)" stop-opacity="0.15" />
            <stop offset="100%" stop-color="hsl(45 95% 50%)" stop-opacity="0.0" />
          </linearGradient>
          <linearGradient id="profitGrad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="hsl(215 95% 50%)" stop-opacity="0.1" />
            <stop offset="100%" stop-color="hsl(215 95% 50%)" stop-opacity="0.0" />
          </linearGradient>
        </defs>

        <!-- Y Axis Gridlines & Labels -->
        <g class="text-[10px] font-bold text-muted-foreground/60">
          <line :x1="paddingLeft" :y1="getY(0)" :x2="width - paddingRight" :y2="getY(0)" stroke="rgba(0,0,0,0.05)" stroke-dasharray="2 2" />
          <text :x="paddingLeft - 15" :y="getY(0) + 3" text-anchor="end">0</text>

          <line :x1="paddingLeft" :y1="getY(100000)" :x2="width - paddingRight" :y2="getY(100000)" stroke="rgba(0,0,0,0.05)" stroke-dasharray="2 2" />
          <text :x="paddingLeft - 15" :y="getY(100000) + 3" text-anchor="end">100k</text>

          <line :x1="paddingLeft" :y1="getY(200000)" :x2="width - paddingRight" :y2="getY(200000)" stroke="rgba(0,0,0,0.05)" stroke-dasharray="2 2" />
          <text :x="paddingLeft - 15" :y="getY(200000) + 3" text-anchor="end">200k</text>

          <line :x1="paddingLeft" :y1="getY(400000)" :x2="width - paddingRight" :y2="getY(400000)" stroke="rgba(0,0,0,0.05)" stroke-dasharray="2 2" />
          <text :x="paddingLeft - 15" :y="getY(400000) + 3" text-anchor="end">400k</text>

          <line :x1="paddingLeft" :y1="getY(600000)" :x2="width - paddingRight" :y2="getY(600000)" stroke="rgba(0,0,0,0.05)" stroke-dasharray="2 2" />
          <text :x="paddingLeft - 15" :y="getY(600000) + 3" text-anchor="end">600k</text>
        </g>

        <!-- Area Gradients -->
        <path :d="getAreaPath(revenuePoints)" fill="url(#revenueGrad)" />
        <path :d="getAreaPath(expensesPoints)" fill="url(#expensesGrad)" />
        <path :d="getAreaPath(profitPoints)" fill="url(#profitGrad)" />

        <!-- Line Paths -->
        <path :d="getCurvePath(revenuePoints)" fill="none" stroke="hsl(var(--primary))" stroke-width="2.5" />
        <path :d="getCurvePath(expensesPoints)" fill="none" stroke="hsl(35 95% 55%)" stroke-width="2.5" />
        <path :d="getCurvePath(profitPoints)" fill="none" stroke="hsl(215 95% 55%)" stroke-width="2" />

        <!-- Interactive Guides & Highlighters -->
        <g v-if="activeIndex !== null && activeData">
          <!-- Dashed Vertical Line -->
          <line 
            :x1="hoverX" 
            :y1="paddingTop" 
            :x2="hoverX" 
            :y2="height - paddingBottom" 
            stroke="rgba(0,0,0,0.15)" 
            stroke-dasharray="4 4" 
            stroke-width="1.5"
          />

          <!-- Dot markers on lines -->
          <circle :cx="hoverX" :cy="getY(activeData.revenue)" r="6" fill="hsl(var(--primary))" stroke="white" stroke-width="2" class="shadow-sm" />
          <circle :cx="hoverX" :cy="getY(activeData.expenses)" r="6" fill="hsl(35 95% 55%)" stroke="white" stroke-width="2" class="shadow-sm" />
          <circle :cx="hoverX" :cy="getY(activeData.profit)" r="5" fill="hsl(215 95% 55%)" stroke="white" stroke-width="1.5" class="shadow-sm" />
        </g>

        <!-- X Axis Labels -->
        <g class="text-xs font-bold text-muted-foreground">
          <text 
            v-for="(data, idx) in chartData" 
            :key="idx" 
            :x="getX(idx)" 
            :y="height - 15" 
            text-anchor="middle"
            :class="{ 'text-primary font-extrabold': activeIndex === idx }"
          >
            {{ data.day }}
          </text>
        </g>
      </svg>

      <!-- Tooltip Box (Exact High-Fidelity Match) -->
      <div 
        v-if="activeIndex !== null && activeData"
        class="absolute bg-slate-950 text-white rounded-2xl p-4 shadow-xl border border-white/10 z-40 transition-all duration-300 w-52 pointer-events-none"
        :style="{
          left: `${(hoverX / width) * 100}%`,
          top: `20px`,
          transform: `translateX(-50%)`
        }"
      >
        <div class="text-[10px] text-white/50 font-bold uppercase tracking-wider mb-2.5">
          {{ activeData.date }}
        </div>
        <div class="space-y-1.5 text-xs font-semibold">
          <div class="flex items-center justify-between gap-4">
            <div class="flex items-center gap-1.5">
              <span class="size-2 rounded-full bg-primary"></span>
              <span>Revenue</span>
            </div>
            <span class="font-extrabold">{{ formatNumber(activeData.revenue) }}</span>
          </div>
          <div class="flex items-center justify-between gap-4">
            <div class="flex items-center gap-1.5">
              <span class="size-2 rounded-full bg-amber-500"></span>
              <span>Expenses</span>
            </div>
            <span class="font-extrabold">{{ formatNumber(activeData.expenses) }}</span>
          </div>
          <div class="flex items-center justify-between gap-4 border-t border-white/10 pt-1.5 mt-1.5">
            <div class="flex items-center gap-1.5">
              <span class="size-2 rounded-full bg-blue-500"></span>
              <span>Profit</span>
            </div>
            <span class="font-extrabold text-blue-400">{{ formatNumber(activeData.profit) }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
