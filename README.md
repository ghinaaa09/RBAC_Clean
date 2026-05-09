# api_rbac_praktek

Project API sederhana untuk latihan authentication dan role-based access control (RBAC) menggunakan Bun, Express, Prisma, dan MySQL.

## Menjalankan Project

Install dependency:

```bash
bun install
```

Buat file `.env` dari `.env.example`, lalu sesuaikan koneksi database:

```env
PORT=4000
DATABASE_URL="mysql://root:password@localhost:3306/rbac"
JWT_SECRET="ganti-dengan-secret-yang-aman"
```

Jalankan migrasi dan seed:

```bash
npx prisma migrate deploy
bun prisma/seed.ts
```

Jalankan server:

```bash
bun run index.ts
```

Server akan berjalan di:

```text
http://localhost:4000
```

## Data Role

Seed akan membuat 2 role:

- `ADMIN`
- `USER`

Karena `roleId` bisa berbeda di setiap database, cek nilainya langsung dari database atau Prisma Studio:

```bash
npx prisma studio
```

Lalu lihat tabel `Role` dan ambil `id` untuk `ADMIN` atau `USER`.

## Endpoint untuk Postman

### 1. Register

`POST http://localhost:4000/api/auth/register`

Contoh body untuk user biasa:

```json
{
  "email": "user@mail.com",
  "password": "1234567",
  "roleId": "ISI_ROLE_ID_USER"
}
```

Contoh body untuk admin:

```json
{
  "email": "admin@mail.com",
  "password": "1234567",
  "roleId": "ISI_ROLE_ID_ADMIN"
}
```

### 2. Login

`POST http://localhost:4000/api/auth/login`

```json
{
  "email": "user@mail.com",
  "password": "1234567"
}
```

Jika berhasil, response akan berisi token:

```json
{
  "token": "bearer_token"
}
```

### 3. Test Authentication

`GET http://localhost:4000/api/test`

Di Postman:

- buka tab `Authorization`
- pilih `Bearer Token`
- isi dengan token dari endpoint login

Contoh response:

```json
{
  "message": "Auth success",
  "user": {
    "id": "user-id",
    "role": "USER",
    "iat": 1778300294,
    "exp": 1778386694
  }
}
```

### 4. Test RBAC Admin

`GET http://localhost:4000/api/admin`

Pakai token dari login.

Jika token milik admin:

```json
{
  "message": "Welcome Admin"
}
```

Jika token milik user biasa:

```json
{
  "message": "No access"
}
```

## Ringkasan Alur Test

1. Jalankan database MySQL.
2. Isi `.env`.
3. Jalankan `npx prisma migrate deploy`.
4. Jalankan `bun prisma/seed.ts`.
5. Jalankan `bun run index.ts`.
6. Ambil `roleId` dari tabel `Role`.
7. Register user atau admin.
8. Login untuk mendapatkan token.
9. Test `/api/test`.
10. Test `/api/admin`.
