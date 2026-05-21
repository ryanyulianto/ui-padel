<script setup lang="ts">
import { ref } from 'vue'
import { 
  Search, 
  Bell, 
  ChevronDown, 
  Plus, 
  Building2,
  Menu,
  Activity
} from 'lucide-vue-next'
import { Button } from '@/components/ui/button'

const currentClinic = ref('Westside clinic')
const notificationCount = ref(4)

const clinics = ['Westside clinic', 'Eastside clinic', 'Central clinic']
const showClinicDropdown = ref(false)
const showProfileDropdown = ref(false)
</script>

<template>
  <header class="w-full bg-card border-b border-border/80 px-4 md:px-8 py-3.5 flex items-center justify-between sticky top-0 z-50 shadow-sm transition-all duration-300">
    <!-- Brand / Logo & Clinic Switcher -->
    <div class="flex items-center gap-6">
      <!-- CareOps Logo -->
      <div class="flex items-center gap-2 cursor-pointer group">
        <div class="size-9 rounded-lg bg-primary flex items-center justify-center text-primary-foreground shadow-md shadow-primary/20 transition-all duration-300 group-hover:scale-105 group-hover:rotate-6">
          <!-- Unique four-leaf medical logo -->
          <Activity class="size-5" />
        </div>
        <span class="text-lg font-extrabold tracking-tight text-foreground">
          Care<span class="text-primary font-black">Ops</span>
        </span>
      </div>

      <!-- Divider line -->
      <div class="hidden sm:block h-6 w-px bg-border/80"></div>

      <!-- Clinic Dropdown Selector -->
      <div class="relative hidden sm:block">
        <button 
          @click="showClinicDropdown = !showClinicDropdown"
          class="flex items-center gap-2 px-3 py-1.5 rounded-xl bg-secondary/50 hover:bg-secondary border border-border/40 text-xs font-semibold text-foreground transition-all duration-200 cursor-pointer"
        >
          <Building2 class="size-3.5 text-muted-foreground" />
          <span>{{ currentClinic }}</span>
          <ChevronDown class="size-3 text-muted-foreground transition-transform duration-200" :class="{ 'rotate-180': showClinicDropdown }" />
        </button>

        <!-- Dropdown Menu -->
        <div v-if="showClinicDropdown" class="absolute left-0 mt-1.5 w-48 rounded-xl bg-card border border-border/80 shadow-lg py-1.5 z-50">
          <button 
            v-for="clinic in clinics" 
            :key="clinic"
            @click="currentClinic = clinic; showClinicDropdown = false"
            class="w-full text-left px-4 py-2 text-xs font-medium hover:bg-muted text-foreground transition-all"
          >
            {{ clinic }}
          </button>
        </div>
      </div>

      <!-- Add Location Button -->
      <button 
        class="hidden md:flex items-center gap-1.5 text-xs font-bold text-primary hover:text-primary/80 transition-all cursor-pointer"
      >
        <Plus class="size-3.5" />
        <span>Add location</span>
      </button>
    </div>

    <!-- Actions (Search, Notification, Profile) -->
    <div class="flex items-center gap-2 md:gap-4">
      <!-- Search Trigger -->
      <Button 
        variant="ghost" 
        size="icon" 
        class="rounded-xl size-9 hover:bg-muted text-muted-foreground transition-all duration-200 active:scale-95"
        aria-label="Search dashboard"
      >
        <Search class="size-4.5" />
      </Button>

      <!-- Notifications with Badge -->
      <div class="relative">
        <Button 
          variant="ghost" 
          size="icon" 
          class="rounded-xl size-9 hover:bg-muted text-muted-foreground transition-all duration-200 active:scale-95"
          aria-label="View notifications"
        >
          <Bell class="size-4.5" />
        </Button>
        <span 
          v-if="notificationCount > 0"
          class="absolute -top-0.5 -right-0.5 size-4 rounded-full bg-primary text-primary-foreground text-[9px] font-extrabold flex items-center justify-center shadow-sm pointer-events-none"
        >
          {{ notificationCount }}
        </span>
      </div>

      <!-- Divider -->
      <div class="h-6 w-px bg-border/80"></div>

      <!-- Profile Avatar dropdown -->
      <div class="relative">
        <button 
          @click="showProfileDropdown = !showProfileDropdown"
          class="flex items-center gap-3 p-1 rounded-full md:pr-3 md:bg-secondary/40 hover:bg-secondary/80 border border-transparent hover:border-border/30 transition-all duration-200 cursor-pointer"
        >
          <img 
            src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&q=80&w=150" 
            alt="Alice H." 
            class="size-8 rounded-full object-cover border border-primary/20 shadow-sm"
          />
          <div class="hidden md:block text-left">
            <span class="block text-xs font-bold text-foreground leading-tight">Alice H.</span>
            <span class="block text-[10px] text-muted-foreground font-semibold leading-none">Super admin</span>
          </div>
          <ChevronDown class="hidden md:block size-3.5 text-muted-foreground transition-transform" :class="{ 'rotate-180': showProfileDropdown }" />
        </button>

        <!-- Dropdown Menu -->
        <div v-if="showProfileDropdown" class="absolute right-0 mt-2 w-48 rounded-xl bg-card border border-border/80 shadow-lg py-1.5 z-50">
          <div class="px-4 py-2 border-b border-border/40 md:hidden">
            <span class="block text-xs font-bold text-foreground leading-tight">Alice H.</span>
            <span class="block text-[10px] text-muted-foreground font-semibold">Super admin</span>
          </div>
          <button 
            @click="showProfileDropdown = false"
            class="w-full text-left px-4 py-2 text-xs font-medium hover:bg-muted text-foreground transition-all"
          >
            My Profile
          </button>
          <button 
            @click="showProfileDropdown = false"
            class="w-full text-left px-4 py-2 text-xs font-medium hover:bg-muted text-foreground transition-all"
          >
            Settings
          </button>
          <button 
            @click="showProfileDropdown = false"
            class="w-full text-left px-4 py-2 text-xs font-medium hover:bg-muted text-destructive transition-all"
          >
            Logout
          </button>
        </div>
      </div>
    </div>
  </header>
</template>
