<script setup lang="ts">
import { ref } from 'vue'
import { 
  LayoutGrid, 
  Users, 
  Calendar, 
  CircleDollarSign, 
  Package, 
  BarChart3 
} from 'lucide-vue-next'

const activeTab = ref('Finances')

const tabs = [
  { name: 'Dashboard', icon: LayoutGrid },
  { name: 'Staff', icon: Users },
  { name: 'Scheduling', icon: Calendar },
  { name: 'Finances', icon: CircleDollarSign },
  { name: 'Inventory', icon: Package },
  { name: 'Analytics', icon: BarChart3 }
]
</script>

<template>
  <nav class="w-full bg-transparent px-4 md:px-8 py-4 flex items-center gap-2 overflow-x-auto scrollbar-hide">
    <button 
      v-for="tab in tabs" 
      :key="tab.name"
      @click="activeTab = tab.name"
      :class="[
        'flex items-center gap-2 px-4 py-2 rounded-full text-xs font-bold transition-all duration-300 ease-out active:scale-95 cursor-pointer whitespace-nowrap',
        activeTab === tab.name 
          ? 'bg-primary/10 text-primary border border-primary/20 shadow-sm' 
          : 'bg-card text-muted-foreground border border-border/40 hover:bg-card/85 hover:text-foreground'
      ]"
    >
      <component :is="tab.icon" class="size-4" />
      <span>{{ tab.name }}</span>
    </button>
  </nav>
</template>

<style scoped>
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
