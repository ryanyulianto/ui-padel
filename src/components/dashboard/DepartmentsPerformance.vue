<script setup lang="ts">
import { ref } from 'vue'
import { MoreHorizontal, Stethoscope, Heart, Activity } from 'lucide-vue-next'

interface DeptPerformance {
  name: string
  icon: any
  iconColor: string
  iconBg: string
  utilization: number
  allocation: number
  earned: number
}

const departments = ref<DeptPerformance[]>([
  { 
    name: 'Surgery', 
    icon: Stethoscope, 
    iconColor: 'text-teal-600 dark:text-teal-400',
    iconBg: 'bg-teal-50 dark:bg-teal-950/30',
    utilization: 84, 
    allocation: 72, 
    earned: 74800 
  },
  { 
    name: 'Cardiology', 
    icon: Heart, 
    iconColor: 'text-rose-600 dark:text-rose-400',
    iconBg: 'bg-rose-50 dark:bg-rose-950/30',
    utilization: 80, 
    allocation: 54, 
    earned: 68800 
  },
  { 
    name: 'Radiology', 
    icon: Activity, 
    iconColor: 'text-sky-600 dark:text-sky-400',
    iconBg: 'bg-sky-50 dark:bg-sky-950/30',
    utilization: 64, 
    allocation: 48, 
    earned: 53324 
  }
])

const formatCurrency = (val: number) => {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(val)
}

// Generate vertical tick marks representing allocation percentage
const maxTicks = 18
const getActiveTickCount = (pct: number) => {
  return Math.round((pct / 100) * maxTicks)
}
</script>

<template>
  <div class="dashboard-card flex flex-col justify-between">
    <!-- Header -->
    <div class="flex items-center justify-between mb-6">
      <h3 class="text-base font-bold tracking-tight text-foreground">Departments performance</h3>
      <button class="p-1 rounded-lg hover:bg-muted text-muted-foreground cursor-pointer transition-colors">
        <MoreHorizontal class="size-4" />
      </button>
    </div>

    <!-- Stats Table/Grid -->
    <div class="w-full overflow-x-auto">
      <table class="w-full text-left border-collapse min-w-[450px]">
        <thead>
          <tr class="border-b border-border/40 text-[10px] uppercase font-bold text-muted-foreground tracking-wider pb-3">
            <th class="pb-3 font-extrabold">Department</th>
            <th class="pb-3 font-extrabold">Utilization</th>
            <th class="pb-3 font-extrabold">Allocation</th>
            <th class="pb-3 font-extrabold text-right">Earned</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-border/30 text-xs font-semibold text-foreground">
          <tr v-for="dept in departments" :key="dept.name" class="group hover:bg-muted/10 transition-colors">
            <!-- Dept Name & Icon -->
            <td class="py-3.5 pr-4">
              <div class="flex items-center gap-3">
                <div :class="['size-8 rounded-lg flex items-center justify-center transition-all duration-300 group-hover:scale-105', dept.iconBg, dept.iconColor]">
                  <component :is="dept.icon" class="size-4" />
                </div>
                <span class="font-bold text-foreground">{{ dept.name }}</span>
              </div>
            </td>
            
            <!-- Utilization -->
            <td class="py-3.5 px-2">
              <div class="flex items-center gap-1.5 text-primary">
                <span class="inline-block size-1.5 rounded-full bg-primary animate-pulse"></span>
                <span class="font-extrabold">{{ dept.utilization }}%</span>
              </div>
            </td>

            <!-- Allocation (Beautiful Striped Vertical Indicator) -->
            <td class="py-3.5 px-2">
              <div class="flex items-center gap-2">
                <span class="text-muted-foreground/80 font-bold min-w-[38px] text-[11px]">({{ dept.allocation }}%)</span>
                <!-- Vertical ticks block -->
                <div class="flex gap-[2px]">
                  <div 
                    v-for="tick in maxTicks" 
                    :key="tick"
                    :class="[
                      'w-[2.5px] h-3.5 rounded-full transition-all duration-500',
                      tick <= getActiveTickCount(dept.allocation)
                        ? 'bg-emerald-500 dark:bg-emerald-400'
                        : 'bg-muted/60 dark:bg-muted/30'
                    ]"
                  ></div>
                </div>
              </div>
            </td>

            <!-- Earned -->
            <td class="py-3.5 pl-4 text-right font-extrabold text-foreground">
              {{ formatCurrency(dept.earned) }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
