# sistem_cinema
🎬 Dokumentasi Sistem Basis Data
Sistem Informasi Perfilman – db_cinema

Database ini dibuat menggunakan MariaDB 10.4.32 melalui XAMPP.
Tujuan sistem ini adalah untuk mengelola data film, sutradara, dan genre secara terstruktur menggunakan relasi antar tabel.

🏗️ 1. Perancangan Struktur Database
1.1 Pembuatan Database
CREATE DATABASE db_cinema;
USE db_cinema;

Database db_cinema digunakan sebagai pusat penyimpanan seluruh data perfilman.

1.2 Struktur Tabel
🎥 Tabel sutradara

Tabel ini menyimpan informasi pembuat film.

CREATE TABLE sutradara (
    id_sutradara INT PRIMARY KEY AUTO_INCREMENT,
    nama_sutradara VARCHAR(100),
    negara_asal VARCHAR(50),
    tahun_lahir INT
);
🎭 Tabel genre

Tabel ini menyimpan kategori atau jenis film.

CREATE TABLE genre (
    id_genre INT PRIMARY KEY AUTO_INCREMENT,
    nama_genre VARCHAR(50),
    keterangan TEXT
);
🎞️ Tabel film

Tabel utama yang menyimpan data film dan memiliki relasi ke sutradara dan genre.

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
📌 Analisis Relasi

Satu sutradara dapat menyutradarai banyak film (1:M)

Satu genre dapat memiliki banyak film (1:M)

Setiap film memiliki satu sutradara dan satu genre

📥 2. Pengisian Data

Data berhasil dimasukkan ke masing-masing tabel:

Contoh Sutradara:

Christopher Nolan

Joko Anwar

Bong Joon-ho

Steven Spielberg

Greta Gerwig

Denis Villeneuve

Martin Scorsese

Contoh Film:

Inception

Parasite

Barbie

Dune

The Irishman

Jurassic Park

Pengabdi Setan

Semua data saling terhubung melalui foreign key.

🔗 3. Implementasi JOIN
🟢 3.1 INNER JOIN
SELECT film.judul_film,
       sutradara.nama_sutradara,
       genre.nama_genre,
       film.tahun_rilis
FROM film
INNER JOIN sutradara
    ON film.id_sutradara = sutradara.id_sutradara
INNER JOIN genre
    ON film.id_genre = genre.id_genre;
📖 Penjelasan

INNER JOIN menampilkan hanya data yang memiliki pasangan di semua tabel yang dihubungkan.

Hasil:

Setiap film ditampilkan bersama nama sutradara dan genre-nya.

Data yang tidak memiliki relasi tidak akan muncul.

🟢 3.2 LEFT JOIN

Setelah menambahkan satu sutradara baru yang belum memiliki film:

SELECT sutradara.nama_sutradara, film.judul_film
FROM sutradara
LEFT JOIN film
ON sutradara.id_sutradara = film.id_sutradara;
📖 Penjelasan

LEFT JOIN menampilkan seluruh data dari tabel kiri (sutradara), meskipun tidak memiliki pasangan di tabel kanan (film).

Jika tidak memiliki film, maka kolom judul_film akan bernilai NULL.

🟢 3.3 RIGHT JOIN
SELECT genre.nama_genre, film.judul_film
FROM genre
RIGHT JOIN film
ON genre.id_genre = film.id_genre;
📖 Penjelasan

RIGHT JOIN menampilkan seluruh data dari tabel kanan (film), meskipun tidak memiliki pasangan di tabel kiri (genre).

Jika film tidak memiliki genre, maka kolom nama_genre akan bernilai NULL.

📊 4. Ringkasan Perbedaan JOIN
Jenis JOIN	Hasil yang Ditampilkan
INNER JOIN	Data yang cocok di kedua tabel
LEFT JOIN	Semua data kiri + yang cocok di kanan
RIGHT JOIN	Semua data kanan + yang cocok di kiri
🧩 5. Kesimpulan

Database db_cinema berhasil mengimplementasikan:

Struktur tabel relasional

Foreign key sebagai penghubung antar tabel

Operasi INNER JOIN, LEFT JOIN, dan RIGHT JOIN

Pengelolaan data film secara terintegrasi

Melalui implementasi ini, konsep dasar relasi dalam basis data dapat dipahami dengan jelas, terutama dalam penggunaan JOIN untuk menggabungkan informasi dari beberapa tabel.
