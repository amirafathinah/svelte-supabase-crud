# Svelte Supabase CRUD

Aplikasi CRUD untuk kontak sederhana menggunakan Svelte dan Supabase untuk tugas Web Lanjut.

## Fitur

- Menambahkan nama dan nomor telepon
- Menampilkan data kontak dalam tabel
- Mengedit data kontak
- Menghapus data kontak
- Menyimpan data ke Supabase

## Teknologi

- Svelte
- Vite
- Supabase

## Cara Menjalankan

### 1. Clone repository

```bash
git clone https://github.com/amirafathinah/svelte-supabase-crud.git
cd svelte-supabase-crud
```

### 2. Install dependencies

Pastikan Node.js dan npm sudah terinstall, lalu jalankan:

```bash
npm install
```

### 3. Siapkan database Supabase

Buat project di [Supabase](https://supabase.com). Buka **SQL Editor** dan jalankan:

```sql
create table public.kontak (
  id bigint generated always as identity primary key,
  nama text not null,
  telepon text not null,
  created_at timestamptz default now()
);

alter table public.kontak enable row level security;

create policy "allow all for anon"
on public.kontak
for all
to anon
using (true)
with check (true);
```

### 4. Buat file `.env`

Buat file `.env` di folder utama project, sejajar dengan `package.json`:

```env
VITE_SUPABASE_URL=https://project-id.supabase.co
VITE_SUPABASE_ANON_KEY=isi-publishable-atau-anon-key
```

Ganti isinya dengan **Project URL** dan **publishable key** dari dashboard Supabase. 

### 5. Jalankan aplikasi

```bash
npm run dev
```

Buka URL yang muncul di terminal
