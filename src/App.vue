<script setup lang="ts">
import { ref } from 'vue'
import Navbar from '@/components/dashboard/Navbar.vue'
import SubNavbar from '@/components/dashboard/SubNavbar.vue'
import MetricCard from '@/components/dashboard/MetricCard.vue'
import FinancialTrends from '@/components/dashboard/FinancialTrends.vue'
import DepartmentsPerformance from '@/components/dashboard/DepartmentsPerformance.vue'
import FinancialStructure from '@/components/dashboard/FinancialStructure.vue'
import { Button } from '@/components/ui/button'
import { 
  CalendarRange, 
  Upload, 
  Settings2,
  Sun,
  Moon
} from 'lucide-vue-next'

// Active section tab inside Finances
const activeFinTab = ref('Overview')
const financeSubTabs = ['Overview', 'Operations', 'Source distribution', 'Insurance', 'Reports']

// Dark mode state
const isDark = ref(false)
function toggleDarkMode() {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}
</script>

<template>
  <div class="min-h-screen bg-background text-foreground font-sans antialiased pb-12 transition-colors duration-500 ease-in-out">
    <!-- Top Nav Header -->
    <Navbar />

    <!-- Sub Navbar Tab Pills -->
    <SubNavbar />

    <!-- Main Container -->
    <main class=" mx-auto px-4 md:px-8 space-y-6">
      
      <!-- Content Section Header (Title, Date Selector, Actions) -->
      <section class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 pt-2">
        <div>
          <h2 class="text-3xl font-extrabold tracking-tight text-foreground">
            Finances
          </h2>
        </div>

        <div class="flex items-center gap-2">
          <!-- Date Filter Dropdown -->
          <button 
            class="flex items-center gap-2 px-4 h-9 rounded-xl bg-card border border-border/80 hover:bg-muted text-xs font-bold text-foreground transition-all cursor-pointer shadow-sm"
          >
            <CalendarRange class="size-3.5 text-muted-foreground" />
            <span>This week</span>
          </button>

          <!-- Export/Upload Action -->
          <Button 
            variant="outline" 
            size="icon" 
            class="rounded-xl size-9 border-border/80 bg-card hover:bg-muted text-muted-foreground shadow-sm"
            aria-label="Export finance reports"
          >
            <Upload class="size-4" />
          </Button>

          <!-- Settings Action -->
          <Button 
            variant="outline" 
            size="icon" 
            class="rounded-xl size-9 border-border/80 bg-card hover:bg-muted text-muted-foreground shadow-sm"
            aria-label="Finance settings"
          >
            <Settings2 class="size-4" />
          </Button>

          <!-- Divider -->
          <div class="h-6 w-px bg-border/80 mx-1"></div>

          <!-- Dynamic Theme Switcher Float -->
          <Button 
            variant="outline" 
            size="icon" 
            class="rounded-xl size-9 border-border/80 bg-card hover:bg-muted shadow-sm transition-all"
            @click="toggleDarkMode"
            aria-label="Toggle dark mode"
          >
            <Sun v-if="isDark" class="size-4 text-amber-500" />
            <Moon v-else class="size-4 text-primary" />
          </Button>
        </div>
      </section>

      <!-- Inner Finance Navigation Subtabs -->
      <section class="border-b border-border/80 flex items-center gap-6 overflow-x-auto scrollbar-hide">
        <button 
          v-for="subTab in financeSubTabs" 
          :key="subTab"
          @click="activeFinTab = subTab"
          :class="[
            'pb-3 text-xs font-bold transition-all relative whitespace-nowrap cursor-pointer',
            activeFinTab === subTab 
              ? 'text-primary border-b-2 border-primary' 
              : 'text-muted-foreground hover:text-foreground'
          ]"
        >
          {{ subTab }}
        </button>
      </section>

      <!-- KPI Metrics Grid (4 columns) -->
      <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <MetricCard 
          title="Revenue" 
          value="$1,248,320" 
          percentage="14%" 
          trend="up" 
        />
        <MetricCard 
          title="Expenses" 
          value="$642,800" 
          percentage="6.2%" 
          trend="down" 
        />
        <MetricCard 
          title="Profit" 
          value="$605,520" 
          percentage="18.9%" 
          trend="up" 
        />
        <MetricCard 
          title="Outstanding Invoices" 
          value="$42,800" 
          percentage="11.3%" 
          trend="down" 
        />
      </section>

      <!-- Financial Trends SVG Interactive Chart -->
      <section class="w-full grid grid-cols-3 gap-2">
        <div class="col-span-2">
          <FinancialTrends />
        </div>
        <div class="dashboard-card">

        </div>
      </section>

      <!-- Bottom Grid (Departments & Financial Structure) -->
      <section class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-stretch">
        <!-- Dept performance (7/12 cols) -->
        <div class="lg:col-span-7">
          <DepartmentsPerformance class="h-full" />
        </div>

        <!-- Financial structure (5/12 cols) -->
        <div class="lg:col-span-5">
          <FinancialStructure class="h-full" />
        </div>
      </section>

    </main>
  </div>
</template>

<style>
/* Smooth typography */
html {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

/* Hide scrollbar for Chrome, Safari and Opera */
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
/* Hide scrollbar for IE, Edge and Firefox */
.scrollbar-hide {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
</style>
