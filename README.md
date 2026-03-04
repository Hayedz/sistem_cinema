# sistem_cinema
# 🎬 DB_CINEMA – Sistem Basis Data Perfilman

Database ini dibuat menggunakan **MariaDB 10.4.32** melalui XAMPP.
Tujuan proyek ini adalah mengimplementasikan konsep **Relational Database** dan operasi **SQL JOIN** pada sistem manajemen data film.

---

## 📌 Deskripsi Sistem

`db_cinema` merupakan database sederhana yang mengelola:

* 🎥 Data Sutradara
* 🎭 Data Genre
* 🎞️ Data Film
* 🔗 Relasi antar tabel menggunakan Foreign Key

Sistem ini dibuat untuk memahami:

* Struktur tabel relasional
* Implementasi foreign key
* Penggunaan INNER JOIN, LEFT JOIN, dan RIGHT JOIN

---

# 🏗️ Struktur Database

## 1️⃣ Database

```sql
CREATE DATABASE db_cinema;
USE db_cinema;
```

---

## 2️⃣ Tabel `sutradara`

```sql
CREATE TABLE sutradara (
    id_sutradara INT PRIMARY KEY AUTO_INCREMENT,
    nama_sutradara VARCHAR(100),
    negara_asal VARCHAR(50),
    tahun_lahir INT
);
```

Menyimpan informasi sutradara film.

---

## 3️⃣ Tabel `genre`

```sql
CREATE TABLE genre (
    id_genre INT PRIMARY KEY AUTO_INCREMENT,
    nama_genre VARCHAR(50),
    keterangan TEXT
);
```

Menyimpan kategori film.

---

## 4️⃣ Tabel `film`

```sql
CREATE TABLE film (
    id_film INT PRIMARY KEY AUTO_INCREMENT,
    judul_film VARCHAR(150),
    tahun_rilis INT,
    durasi_menit INT,
    id_sutradara INT,
    id_genre INT,
    FOREIGN KEY (id_sutradara) REFERENCES sutradara(id_sutradara),
    FOREIGN KEY (id_genre) REFERENCES genre(id_genre)
);
```

### 🔗 Relasi

* 1 Sutradara → Banyak Film (1:M)
* 1 Genre → Banyak Film (1:M)
* 1 Film → 1 Sutradara
* 1 Film → 1 Genre

---

# 📥 Contoh Data

## Sutradara

* Christopher Nolan
* Joko Anwar
* Bong Joon-ho
* Steven Spielberg
* Greta Gerwig
* Denis Villeneuve
* Martin Scorsese

## Film

* Inception
* Parasite
* Barbie
* Dune
* The Irishman
* Jurassic Park
* Pengabdi Setan

---

# 🔗 Implementasi JOIN

## 🟢 INNER JOIN

```sql
SELECT film.judul_film,
       sutradara.nama_sutradara,
       genre.nama_genre,
       film.tahun_rilis
FROM film
INNER JOIN sutradara
    ON film.id_sutradara = sutradara.id_sutradara
INNER JOIN genre
    ON film.id_genre = genre.id_genre;
```

**Penjelasan:**
Menampilkan hanya data yang memiliki relasi di semua tabel.

---

## 🟡 LEFT JOIN

```sql
SELECT sutradara.nama_sutradara, film.judul_film
FROM sutradara
LEFT JOIN film
ON sutradara.id_sutradara = film.id_sutradara;
```

**Penjelasan:**
Menampilkan semua sutradara, meskipun belum memiliki film.

---

## 🔵 RIGHT JOIN

```sql
SELECT genre.nama_genre, film.judul_film
FROM genre
RIGHT JOIN film
ON genre.id_genre = film.id_genre;
```

**Penjelasan:**
Menampilkan semua film, meskipun tidak memiliki genre.

---

# 📊 Perbedaan JOIN

| JOIN       | Fungsi                             |
| ---------- | ---------------------------------- |
| INNER JOIN | Data yang cocok di kedua tabel     |
| LEFT JOIN  | Semua data kiri + yang cocok kanan |
| RIGHT JOIN | Semua data kanan + yang cocok kiri |

---

