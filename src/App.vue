<script setup lang="ts">
import { ref } from 'vue'
import { Button } from '@/components/ui/button'
import { 
  Sun, 
  Moon, 
  Sparkles, 
  Calendar, 
  Clock, 
  MapPin, 
  Users, 
  Trophy, 
  ChevronRight, 
  PlayCircle 
} from 'lucide-vue-next'

const isDark = ref(false)

function toggleDarkMode() {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}

// Demo court booking state
const selectedTime = ref('18:00')
const bookingDate = ref('Hari ini, 21 Mei')
const duration = ref('90 Menit')

const timeSlots = ['16:00', '17:30', '19:00', '20:30']
</script>

<template>
  <div class="min-h-screen bg-background text-foreground font-sans transition-colors duration-500 ease-in-out selection:bg-accent selection:text-accent-foreground p-4 md:p-8 flex flex-col items-center justify-center">
    
    <!-- Premium Header -->
    <header class="w-full max-w-4xl flex items-center justify-between mb-8">
      <div class="flex items-center gap-3">
        <div class="size-10 rounded-xl bg-primary flex items-center justify-center text-primary-foreground shadow-lg shadow-primary/10">
          <Trophy class="size-5" />
        </div>
        <div>
          <h1 class="text-xl font-bold tracking-tight">VISTA <span class="text-accent font-extrabold">PADEL</span></h1>
          <p class="text-xs text-muted-foreground font-medium">Premium Court Club & Arena</p>
        </div>
      </div>

      <!-- Quick Theme Switcher -->
      <Button 
        variant="outline" 
        size="icon" 
        class="rounded-xl border-border bg-card hover:bg-muted transition-all duration-300 active:scale-95 shadow-sm"
        @click="toggleDarkMode"
        aria-label="Toggle dark mode"
      >
        <Sun v-if="isDark" class="size-[1.2rem] text-accent transition-transform rotate-0 scale-100" />
        <Moon v-else class="size-[1.2rem] text-primary transition-transform rotate-0 scale-100" />
      </Button>
    </header>

    <!-- Main Dynamic Card -->
    <main class="w-full max-w-4xl grid grid-cols-1 md:grid-cols-12 gap-6 items-stretch">
      
      <!-- Left Column: Court Presentation (Glassmorphic & Rich Aesthetics) -->
      <section class="md:col-span-7 bg-card border border-border rounded-3xl p-6 md:p-8 flex flex-col justify-between relative overflow-hidden shadow-xl shadow-primary/5 group">
        <!-- Visual Accent Gradient -->
        <div class="absolute -top-24 -left-24 size-48 rounded-full bg-accent/20 blur-3xl group-hover:bg-accent/30 transition-all duration-500"></div>
        <div class="absolute -bottom-24 -right-24 size-48 rounded-full bg-primary/20 blur-3xl"></div>

        <div class="relative z-10 space-y-6">
          <div class="flex items-center gap-2">
            <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-semibold bg-accent/15 text-accent-foreground border border-accent/20 animate-pulse">
              <Sparkles class="size-3" /> Tersedia Sekarang
            </span>
          </div>

          <div class="space-y-2">
            <h2 class="text-3xl font-extrabold tracking-tight md:text-4xl leading-tight">
              Arena Utama <br />
              <span class="text-transparent bg-clip-text bg-gradient-to-r from-primary to-accent font-black">PANORAMA COURT</span>
            </h2>
            <p class="text-muted-foreground text-sm leading-relaxed max-w-md">
              Nikmati fasilitas Padel premium berstandar internasional dengan lapangan panorama tanpa pilar, rumput sintetis berdensitas tinggi, dan sistem pencahayaan LED anti-silau.
            </p>
          </div>

          <!-- Quick Specs Grid -->
          <div class="grid grid-cols-3 gap-3 pt-2">
            <div class="p-3 bg-muted/50 rounded-2xl border border-border/50 text-center">
              <span class="block text-xs text-muted-foreground font-medium mb-1">Tipe Lapangan</span>
              <span class="text-sm font-bold">Panoramic</span>
            </div>
            <div class="p-3 bg-muted/50 rounded-2xl border border-border/50 text-center">
              <span class="block text-xs text-muted-foreground font-medium mb-1">Pemain</span>
              <span class="text-sm font-bold">Double (4)</span>
            </div>
            <div class="p-3 bg-muted/50 rounded-2xl border border-border/50 text-center">
              <span class="block text-xs text-muted-foreground font-medium mb-1">Rekomendasi</span>
              <span class="text-sm font-bold">Semua Level</span>
            </div>
          </div>
        </div>

        <div class="relative z-10 pt-8 flex items-center justify-between border-t border-border/40 mt-8">
          <div class="flex items-center gap-3 text-sm font-medium">
            <MapPin class="size-4 text-accent" />
            <span>Kuningan, Jakarta Selatan</span>
          </div>
          <a href="#" class="inline-flex items-center gap-1 text-xs font-semibold text-accent hover:underline">
            Lihat Lokasi <ChevronRight class="size-3" />
          </a>
        </div>
      </section>

      <!-- Right Column: Interactive Reservation Panel -->
      <section class="md:col-span-5 bg-card border border-border rounded-3xl p-6 md:p-8 flex flex-col justify-between shadow-xl shadow-primary/5">
        <div class="space-y-6">
          <h3 class="text-lg font-bold tracking-tight">Atur Sesi Bermain</h3>

          <!-- Details Setup -->
          <div class="space-y-4">
            <!-- Date selection mockup -->
            <div class="flex items-center justify-between p-3 bg-muted/40 rounded-2xl border border-border/40">
              <div class="flex items-center gap-3">
                <Calendar class="size-4 text-muted-foreground" />
                <div>
                  <span class="block text-[10px] uppercase tracking-wider text-muted-foreground font-bold">Tanggal</span>
                  <span class="text-sm font-semibold">{{ bookingDate }}</span>
                </div>
              </div>
              <Button variant="ghost" size="sm" class="text-xs hover:bg-muted text-primary font-semibold rounded-lg">Ubah</Button>
            </div>

            <!-- Duration selection mockup -->
            <div class="flex items-center justify-between p-3 bg-muted/40 rounded-2xl border border-border/40">
              <div class="flex items-center gap-3">
                <Clock class="size-4 text-muted-foreground" />
                <div>
                  <span class="block text-[10px] uppercase tracking-wider text-muted-foreground font-bold">Durasi</span>
                  <span class="text-sm font-semibold">{{ duration }}</span>
                </div>
              </div>
              <Button variant="ghost" size="sm" class="text-xs hover:bg-muted text-primary font-semibold rounded-lg">Ubah</Button>
            </div>
          </div>

          <!-- Time slot selection -->
          <div class="space-y-3">
            <label class="text-xs font-bold uppercase tracking-wider text-muted-foreground">Pilih Waktu</label>
            <div class="grid grid-cols-2 gap-2">
              <button 
                v-for="slot in timeSlots" 
                :key="slot"
                @click="selectedTime = slot"
                :class="[
                  'p-3 rounded-xl border text-sm font-semibold text-center transition-all duration-300 active:scale-95 cursor-pointer',
                  selectedTime === slot 
                    ? 'bg-accent text-accent-foreground border-accent shadow-lg shadow-accent/20' 
                    : 'bg-muted/30 border-border/80 hover:bg-muted/80'
                ]"
              >
                {{ slot }}
              </button>
            </div>
          </div>
        </div>

        <div class="space-y-4 pt-6 mt-6 border-t border-border/40">
          <div class="flex items-center justify-between">
            <div>
              <span class="block text-[10px] uppercase tracking-wider text-muted-foreground font-bold">Total Pembayaran</span>
              <span class="text-xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-foreground to-foreground/80">Rp 250.000</span>
            </div>
            <div class="text-right">
              <span class="inline-block text-[10px] px-2 py-0.5 rounded bg-accent/20 text-accent-foreground font-semibold">1x Sesi</span>
            </div>
          </div>

          <!-- shadcn-vue Button usage! -->
          <Button 
            class="w-full h-12 rounded-xl text-primary-foreground bg-primary hover:bg-primary/95 text-sm font-bold shadow-lg shadow-primary/20 transition-all duration-300 active:scale-[0.98] group flex items-center justify-center gap-2 cursor-pointer"
          >
            <PlayCircle class="size-4 text-accent transition-transform group-hover:scale-110" />
            Konfirmasi & Bayar
          </Button>
        </div>
      </section>

    </main>

    <!-- Footer -->
    <footer class="w-full max-w-4xl text-center mt-12 text-xs text-muted-foreground font-medium">
      <p>© 2026 Vista Padel Club. Powered by shadcn-vue & Tailwind CSS v4.</p>
    </footer>
  </div>
</template>

<style>
/* Clean custom styles for clean transitions */
html {
  font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
</style>
