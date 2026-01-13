
## 🧱 Panduan Setup Modern Vite + React + Tailwind + ShadCN

---

### ✅ 1. Inisialisasi Proyek Vite

```bash
npm create vite@latest
```

- Pilih `React` dan `JavaScript` (untuk TypeScript gunakan proses di dokumentasi)
    
- Masuk ke folder proyek:

```bash
cd your-project-name
```

### ✅ 2. Instal Dependensi

```bash
npm install
```

---

## 🎨 Setup TailwindCSS

### ✅ 3. Instal Tailwind dan Plugin Vite

```bash
npm install tailwindcss @tailwindcss/vite
```

### ✅ 4. (Opsional) Instal `tw-animate-css` untuk dukungan utilitas animasi

```bash
npm install tw-animate-css
```

---

### ✅ 5. Konfigurasi Tailwind di `index.css`

Buka `src/index.css`, **hapus semuanya** dan ganti dengan:

```css
@import "tailwindcss";
```


---

## 🧠 Alias Path dengan `@`

### ✅ 6. Buat atau edit `jsconfig.json` (untuk proyek JavaScript)

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

Untuk proyek **TypeScript**, gunakan `tsconfig.json` sebagai gantinya.

---

### ✅ 7. Instal tipe Node untuk konfigurasi Vite dan IntelliSense yang lebih baik

```bash
npm install -D @types/node
```

---

### ✅ 8. Ganti `vite.config.js` dengan berikut:

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";
import { fileURLToPath } from "url";
import tailwindcss from "@tailwindcss/vite";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

---

## ✨ Integrasi ShadCN UI

### ✅ 9. Inisialisasi ShadCN

```bash
npx shadcn@latest init
```

- Pilih **warna dasar** (mis. `neutral`, `blue`, dll)

Ini akan menambahkan kebutuhan di file CSS utama... Kamu bisa cek dan menyesuaikan warna sesuai pilihanmu.

Sekarang `shadcn/ui` siap digunakan dengan komponen yang sudah dibuat dengan rapi.

---

## 🎉 Selesai! Sekarang kamu bisa:

- Menggunakan alias seperti `@/components/Button` (komponen shadcn apa pun)
    
- Menggunakan utilitas Tailwind
    
- Menggunakan komponen ShadCN langsung
    
- Membangun UI yang indah dan scalable dengan struktur yang rapi


---



# 🌓 Pergantian Tema dengan shadcn/ui (React JSX)

Panduan ini membantu kamu mengintegrasikan **dark/light mode switching** di proyek React (JSX) menggunakan `shadcn/ui`.

---

## 🛠️ Prasyarat

- Proyek React (JSX)
    
- `shadcn/ui` sudah terpasang 
    
- Struktur folder yang benar (setup alias path `@/components/`) (baca dari awal untuk ini)


---

## 1. 📁 Buat Theme Provider

Buat file baru:  
`src/components/theme-provider.jsx`

Salin isi file **ThemeProvider** (theme-provider) dari repo.  
Pastikan kamu menyalin versi **JSX** (atau ubah dari TSX jika perlu).

---

## 2. 💡 Bungkus App dengan ThemeProvider

Buka file `App.jsx` lalu bungkus komponen root dengan `ThemeProvider`:

```jsx
import { ThemeProvider } from "@/components/theme-provider"

function App() {
  return (
    <ThemeProvider defaultTheme="dark" storageKey="vite-ui-theme">
      {/* Replace with your routes or layout */}
      {children}
    </ThemeProvider>
  )
}

export default App
```

> ✅ Ganti `{children}` dengan komponen atau rute yang kamu pakai (mis. `<Router />`).

---

## 3. 🎛️ Buat Komponen Theme Switcher

Kamu bisa membuat komponen **Theme Switch** sendiri, atau gunakan contoh yang disediakan.

### ✅ Example: `ThemeChange.jsx`

Buat komponen untuk toggle tema:  
`src/components/ThemeChange.jsx`
 

Berikut [contoh](https://github.com/Haisyam/ReactShadcn/blob/main/src/components/ThemeChange.jsx)

Lalu tambahkan komponen ini di tempat yang dibutuhkan.

---
## 4. Tambahkan skrip ini di file index.html untuk mencegah glitch

```html
<script>
      (function () {
        try {
          const theme = localStorage.getItem("vite-ui-theme");
          const prefersDark = window.matchMedia(
            "(prefers-color-scheme: dark)"
          ).matches;
          const resolved =
            theme === "dark" || (!theme && prefersDark)
              ? "dark"
              : theme === "light"
              ? "light"
              : prefersDark
              ? "dark"
              : "light";
          document.documentElement.classList.add(resolved);
        } catch (_) {}
      })();
</script>
```
---

## 🧪 Pemeriksaan Akhir

- ✅ Tema berganti antara light/dark
    
- ✅ Tersimpan di local storage dengan key `vite-ui-theme`
    
- ✅ Berfungsi saat reload dan berpindah halaman
