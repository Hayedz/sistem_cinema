#sistem_cinema
# 🎬 DB_CINEMA

### Relational Database Implementation using MariaDB

Proyek ini merupakan implementasi sistem basis data sederhana bertema perfilman menggunakan **MariaDB 10.4.32** melalui XAMPP.

Database ini mendemonstrasikan:

* Pembuatan tabel relasional
* Implementasi Foreign Key
* Operasi `INNER JOIN`, `LEFT JOIN`, dan `RIGHT JOIN`
* Pengelolaan data film, sutradara, dan genre

---

# 🏗️ 1️⃣ Struktur Database

## 📌 Database

```sql
CREATE DATABASE db_cinema;
USE db_cinema;
```

---

## 🎥 Tabel: `sutradara`

```sql
CREATE TABLE sutradara (
    id_sutradara INT PRIMARY KEY AUTO_INCREMENT,
    nama_sutradara VARCHAR(100),
    negara_asal VARCHAR(50),
    tahun_lahir INT
);
```

### 📋 Data Sutradara

| id_sutradara | nama_sutradara    | negara_asal     | tahun_lahir |
| ------------ | ----------------- | --------------- | ----------- |
| 1            | Christopher Nolan | Inggris         | 1970        |
| 2            | Joko Anwar        | Indonesia       | 1976        |
| 3            | Bong Joon-ho      | Korea Selatan   | 1969        |
| 4            | Steven Spielberg  | Amerika Serikat | 1946        |
| 5            | Greta Gerwig      | Amerika Serikat | 1983        |
| 6            | Denis Villeneuve  | Kanada          | 1967        |
| 7            | Martin Scorsese   | Amerika Serikat | 1942        |

---

## 🎭 Tabel: `genre`

```sql
CREATE TABLE genre (
    id_genre INT PRIMARY KEY AUTO_INCREMENT,
    nama_genre VARCHAR(50),
    keterangan TEXT
);
```

### 📋 Data Genre

| id_genre | nama_genre | keterangan                        |
| -------- | ---------- | --------------------------------- |
| 1        | Action     | Adegan aksi dan ketegangan        |
| 2        | Drama      | Cerita konflik kehidupan          |
| 3        | Thriller   | Misteri dan ketegangan psikologis |
| 4        | Sci-Fi     | Fiksi ilmiah dan teknologi        |
| 5        | Horror     | Cerita menyeramkan                |
| 6        | Comedy     | Unsur humor                       |
| 7        | Adventure  | Petualangan dan eksplorasi        |

---

## 🎞️ Tabel: `film`

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

### 📋 Data Film

| id_film | judul_film     | tahun_rilis | durasi_menit | id_sutradara | id_genre |
| ------- | -------------- | ----------- | ------------ | ------------ | -------- |
| 1       | Inception      | 2010        | 148          | 1            | 4        |
| 2       | Pengabdi Setan | 2017        | 107          | 2            | 5        |
| 3       | Parasite       | 2019        | 132          | 3            | 2        |
| 4       | Jurassic Park  | 1993        | 127          | 4            | 7        |
| 5       | Barbie         | 2023        | 114          | 5            | 6        |
| 6       | Dune           | 2021        | 155          | 6            | 4        |
| 7       | The Irishman   | 2019        | 209          | 7            | 2        |

---

# 🔗 2️⃣ Implementasi JOIN

---

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

### 📊 Hasil INNER JOIN

| judul_film     | nama_sutradara    | nama_genre | tahun_rilis |
| -------------- | ----------------- | ---------- | ----------- |
| Inception      | Christopher Nolan | Sci-Fi     | 2010        |
| Pengabdi Setan | Joko Anwar        | Horror     | 2017        |
| Parasite       | Bong Joon-ho      | Drama      | 2019        |
| Jurassic Park  | Steven Spielberg  | Adventure  | 1993        |
| Barbie         | Greta Gerwig      | Comedy     | 2023        |
| Dune           | Denis Villeneuve  | Sci-Fi     | 2021        |
| The Irishman   | Martin Scorsese   | Drama      | 2019        |

📌 INNER JOIN hanya menampilkan data yang memiliki relasi lengkap.

---

## 🟡 LEFT JOIN

Setelah menambahkan sutradara baru yang belum memiliki film:

```sql
SELECT sutradara.nama_sutradara, film.judul_film
FROM sutradara
LEFT JOIN film
ON sutradara.id_sutradara = film.id_sutradara;
```

### 📊 Hasil LEFT JOIN

| nama_sutradara    | judul_film     |
| ----------------- | -------------- |
| Christopher Nolan | Inception      |
| Joko Anwar        | Pengabdi Setan |
| Bong Joon-ho      | Parasite       |
| Steven Spielberg  | Jurassic Park  |
| Greta Gerwig      | Barbie         |
| Denis Villeneuve  | Dune           |
| Martin Scorsese   | The Irishman   |
| yoyon lagoona     | NULL           |

📌 LEFT JOIN menampilkan semua data dari tabel kiri.

---

## 🔵 RIGHT JOIN

Setelah menambahkan film tanpa genre:

```sql
SELECT genre.nama_genre, film.judul_film
FROM genre
RIGHT JOIN film
ON genre.id_genre = film.id_genre;
```

### 📊 Hasil RIGHT JOIN

| nama_genre | judul_film     |
| ---------- | -------------- |
| Sci-Fi     | Inception      |
| Horror     | Pengabdi Setan |
| Drama      | Parasite       |
| Adventure  | Jurassic Park  |
| Comedy     | Barbie         |
| Sci-Fi     | Dune           |
| Drama      | The Irishman   |
| NULL       | yoyo           |

📌 RIGHT JOIN menampilkan semua data dari tabel kanan.

---

# 📊 Perbandingan JOIN

| JOIN       | Karakteristik                     |
| ---------- | --------------------------------- |
| INNER JOIN | Hanya data yang memiliki pasangan |
| LEFT JOIN  | Semua data kiri tetap tampil      |
| RIGHT JOIN | Semua data kanan tetap tampil     |

---

# 📌 Kesimpulan

Database `db_cinema` berhasil mengimplementasikan konsep:

* Relasi antar tabel
* Foreign key constraint
* Penggabungan data menggunakan JOIN
* Representasi data terstruktur dalam sistem relasional

---
