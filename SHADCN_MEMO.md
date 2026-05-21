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

### Konfigurasi Warna Premium (Tema Lapangan Padel)
Kami menyusun palet HSL aktif-premium bertema olahraga padel (Forest Court Green & Electric Tennis Yellow):

| Token Warna | Kegunaan | Light Mode HSL | Dark Mode HSL |
| :--- | :--- | :--- | :--- |
| `--background` | Background halaman utama | `165 20% 98.5%` (Off-white) | `165 30% 4%` (Slate-black) |
| `--foreground` | Warna teks utama | `165 45% 10%` (Deep green) | `165 20% 98%` (Crisp white) |
| `--primary` | Background tombol default / brand | `165 45% 15%` (Court Forest) | `165 40% 95%` (Contrast green-white) |
| `--primary-foreground` | Teks di atas warna primary | `76 100% 50%` (Neon Yellow) | `165 45% 10%` (Deep green) |
| `--accent` | Warna sorotan / aktif / hover | `76 100% 48%` (Vibrant yellow) | `76 100% 48%` (Vibrant yellow) |
| `--accent-foreground` | Teks di atas warna accent | `165 45% 10%` (Deep green) | `165 30% 4%` (Slate-black) |
| `--card` | Background box / card | `0 0% 100%` (White) | `165 20% 8%` (Darker green-slate) |

*Konfigurasi lengkap silakan lihat di file [src/assets/app.css](file:///Users/ryan-dev/Documents/Development/work/2026/make-ui-padel/src/assets/app.css).*

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
