# 📝 SHADCN-VUE + TAILWIND V4 INTEGRATION MEMO

Dokumen ini adalah memori dan panduan referensi lengkap mengenai integrasi **shadcn-vue** dan **Tailwind CSS v4** pada project template **ui-padel**. Simpan dan tunjukkan file ini ke model AI berikutnya agar tidak perlu mengulang setup dari awal.

---

## 🚀 Tech Stack & Setup Details

- **Core Framework**: Vue 3 (Vite + TypeScript)
- **Styling**: Tailwind CSS v4 (CSS-first architecture)
- **Component Primitives**: `reka-ui` (Modern Radix-Vue rebrand)
- **Component UI**: `shadcn-vue` (v2+)

---

## 📁 File Struktur & Alias Konfigurasi

Project ini menggunakan alias `@/*` yang merujuk pada direktori `./src/*`. Alias dikonfigurasi pada:
- **Root tsconfig.json** & **tsconfig.app.json**:
  ```json
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
  ```
- **vite.config.ts**:
  ```typescript
  import tailwindcss from '@tailwindcss/vite'
  // ...
  plugins: [
    vue(),
    vueDevTools(),
    tailwindcss(), // Tailwind v4 integration
  ]
  ```

---

## 🎨 System Warna & app.css (Light & Dark Mode)

Tailwind v4 menghilangkan `tailwind.config.js` tradisional. Seluruh token tema didefinisikan secara langsung di dalam **`src/assets/app.css`** menggunakan directive `@theme`.

### Konfigurasi Warna Premium (Tema CareOps)
Kami menyusun palet HSL aktif-premium bertema operasional klinik kesehatan yang modern:

| Token Warna | Kegunaan | Light Mode HSL | Dark Mode HSL |
| :--- | :--- | :--- | :--- |
| `--background` | Background halaman utama | `240 14% 97.5%` (Light slate) | `240 10% 3.9%` (Dark black) |
| `--foreground` | Warna teks utama | `240 10% 12%` (Deep slate) | `0 0% 98%` (Crisp white) |
| `--primary` | Background tombol default / brand | `255 65% 58%` (Royal Violet) | `255 70% 65%` (Vibrant purple) |
| `--primary-foreground` | Teks di atas warna primary | `0 0% 100%` (White) | `240 10% 3.9%` (Dark black) |
| `--accent` | Warna sorotan / aktif / hover | `255 65% 58%` (Royal Violet) | `255 70% 65%` (Vibrant purple) |
| `--card` | Background box / card | `0 0% 100%` (White) | `240 10% 6%` (Deep charcoal) |

*Konfigurasi lengkap silakan lihat di file [src/assets/app.css](file:///Users/ryan-dev/Documents/Development/work/2026/make-ui-padel/src/assets/app.css).*

### 💎 Reusable Premium Card (.dashboard-card)
Untuk menghindari duplikasi class Tailwind, kami membuat class utility premium `.dashboard-card` di `app.css`. Class ini langsung memiliki styling border, rounded corners, drop shadow, dan hover transition efek yang mulus:
```css
.dashboard-card {
  background-color: hsl(var(--card));
  border: 1px solid hsl(var(--border) / 0.8);
  border-radius: 1.5rem; /* rounded-3xl */
  padding: 1.5rem; /* p-6 */
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); /* shadow-sm */
  transition: all 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

.dashboard-card:hover {
  border-color: hsl(var(--border));
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -4px rgba(0, 0, 0, 0.05); /* shadow-md */
}
```

---

## 🧩 Komponen Dashboard Modular (`src/components/dashboard/`)

Dashboard CareOps dipecah menjadi komponen modular berikut untuk kemudahan pemeliharaan:

1. **`Navbar.vue`**: Header navigasi atas berisi logo, penukar klinik (*Westside clinic*), tombol tambah lokasi, ikon pencarian, bel notifikasi aktif, dan profil avatar Alice H.
2. **`SubNavbar.vue`**: Tab navigasi horisontal berisi pilihan menu: *Dashboard, Staff, Scheduling, Finances (Aktif), Inventory, Analytics*.
3. **`MetricCard.vue`**: Komponen KPI kartu reusable untuk *Revenue, Expenses, Profit*, dan *Outstanding Invoices* lengkap dengan indikator kenaikan/penurunan (tren) yang halus.
4. **`FinancialTrends.vue`**: Area dan Line chart SVG kustom yang sepenuhnya interaktif. Memiliki garis pandu putus-putus vertikal dinamis dan popup tooltip yang menyajikan nilai tepat saat di-hover.
5. **`DepartmentsPerformance.vue`**: Tabel statistik kinerja departemen (*Surgery, Cardiology, Radiology*) dengan visualisasi alokasi berbentuk garis-garis bar vertikal hijau yang unik.
6. **`FinancialStructure.vue`**: Bar progress kapsul modern terbagi secara proporsional sesuai persentase kontribusi sumber keuangan (*Patient services, Insurance claims, Packages, Other*).

---

## 🛠️ Cara Menambah Component Baru

File `components.json` di root telah dikonfigurasi agar mendeteksi Tailwind v4 (dengan mengosongkan path `config` tailwind). Untuk menambahkan component baru dari shadcn-vue, jalankan perintah berikut di terminal:

```bash
npx shadcn-vue@latest add <nama-component>
# Contoh: menambah Dialog / Modal
npx shadcn-vue@latest add dialog
```

Component yang ditambahkan otomatis tersimpan di folder `src/components/ui/` dan siap di-import seperti:
```typescript
import { Button } from '@/components/ui/button'
```

---

## 🔄 Cara Menukar Dark/Light Mode

Dark mode dikonaktifkan dengan menambahkan class `.dark` pada root element `<html>`.
Contoh implementasi reactive di Vue 3:

```typescript
import { ref } from 'vue'

const isDark = ref(false)

function toggleDarkMode() {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}
```

---

## 📝 Catatan Tambahan untuk Model AI Baru

1. **JANGAN membuat file `tailwind.config.js`** kecuali sangat terpaksa. Seluruh modifikasi utility class / theme token harus diletakkan dalam `@theme` di file `src/assets/app.css`.
2. Pastikan file `components.json` tetap memiliki konfigurasi `"config": ""` di bawah section `"tailwind"`.
3. Gunakan utility helper class `cn()` dari `@/lib/utils` untuk menggabungkan class Tailwind secara dinamis.
