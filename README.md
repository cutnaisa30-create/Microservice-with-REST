# Implementasi dan Analisis Arsitektur Monolith dan Microservices Menggunakan Flask Python

## Deskripsi

Project ini merupakan implementasi praktikum arsitektur Monolith dan Microservices menggunakan Flask Python.

Pada arsitektur Monolith, fitur buku dan pesanan berada dalam satu aplikasi.

Pada arsitektur Microservices, aplikasi dipisahkan menjadi dua service, yaitu:

- Book Service
- Order Service

Book Service bertanggung jawab terhadap data buku, sedangkan Order Service bertanggung jawab terhadap proses pemesanan dan berkomunikasi dengan Book Service melalui HTTP Request.

Project ini juga melakukan pengujian Fault Isolation dengan menghentikan Book Service dan mengamati respons dari Order Service.

## Teknologi yang Digunakan

- Python
- Flask
- Requests
- Visual Studio Code
- Postman
- Git/GitHub

## Struktur Project

```text
flask-microservices/
│
├── monolith_app.py
├── book_service.py
├── order_service.py
├── requirements.txt
├── README.md
└── .gitignore
```

## Persiapan

Pastikan Python sudah terinstall.

Cek versi Python:

```cmd
python --version
```

Masuk ke folder project:

```cmd
cd /d "D:\semester 5\Paradigma Sistem untuk IT\flask-microservices"
```

## Membuat Virtual Environment

Buat virtual environment:

```cmd
python -m venv venv
```

Aktifkan virtual environment:

```cmd
venv\Scripts\activate
```

Jika berhasil, terminal akan menampilkan:

```text
(venv)
```

## Instalasi Library

Install Flask dan Requests:

```cmd
pip install Flask requests
```

Atau jika repository sudah memiliki file `requirements.txt`, gunakan:

```cmd
pip install -r requirements.txt
```

## Menjalankan Monolith

Monolith merupakan aplikasi yang menggabungkan fitur Books dan Orders dalam satu aplikasi.

File yang digunakan:

```text
monolith_app.py
```

Jalankan dengan:

```cmd
python monolith_app.py
```

Aplikasi berjalan pada:

```text
http://127.0.0.1:5000
```

### Pengujian Monolith

Buka Postman.

Untuk melihat daftar buku:

```text
GET http://localhost:5000/books
```

Untuk membuat pesanan:

```text
POST http://localhost:5000/orders
```

Pada Postman pilih:

```text
Body → raw → JSON
```

Masukkan:

```json
{
    "book_id": 1
}
```

Kemudian tekan `Send`.

Jika berhasil, sistem memberikan respons pesanan dengan status:

```json
{
    "book_id": 1,
    "id": 1,
    "status": "berhasil"
}
```

## Menjalankan Book Service

Book Service merupakan service yang bertanggung jawab menangani data buku.

File yang digunakan:

```text
book_service.py
```

Buka terminal baru pada VS Code dan aktifkan virtual environment:

```cmd
venv\Scripts\activate
```

Kemudian jalankan:

```cmd
python book_service.py
```

Book Service berjalan pada:

```text
http://localhost:5001
```

### Pengujian Book Service

Untuk melihat daftar buku:

```text
GET http://localhost:5001/books
```

Untuk melihat detail buku:

```text
GET http://localhost:5001/books/1
```

Jika berhasil, sistem menampilkan informasi buku:

```json
{
    "id": 1,
    "title": "Belajar Flask",
    "stock": 5
}
```

## Menjalankan Order Service

Order Service merupakan service yang bertanggung jawab menangani proses pemesanan.

File yang digunakan:

```text
order_service.py
```

Buka terminal baru pada VS Code.

Aktifkan virtual environment:

```cmd
venv\Scripts\activate
```

Kemudian jalankan:

```cmd
python order_service.py
```

Order Service berjalan pada:

```text
http://localhost:5002
```

### Pengujian Order Service

Pastikan Book Service masih berjalan.

Buka Postman dan gunakan:

```text
POST http://localhost:5002/orders
```

Pilih:

```text
Body → raw → JSON
```

Masukkan:

```json
{
    "book_id": 1
}
```

Kemudian tekan `Send`.

Jika Book Service aktif dan buku tersedia, respons yang diberikan:

```json
{
    "book_id": 1,
    "id": 1,
    "status": "berhasil"
}
```

Pada tahap ini Order Service mengambil informasi buku dari Book Service menggunakan HTTP Request.

## Menjalankan Semua Service

Untuk menjalankan arsitektur Microservices, gunakan dua terminal pada VS Code.

### Terminal 1 - Book Service

```cmd
venv\Scripts\activate
python book_service.py
```

Port:

```text
5001
```

### Terminal 2 - Order Service

```cmd
venv\Scripts\activate
python order_service.py
```

Port:

```text
5002
```

Kedua service harus berjalan secara bersamaan agar Order Service dapat berkomunikasi dengan Book Service.

## Pengujian Fault Isolation

Fault Isolation dilakukan untuk mengetahui apa yang terjadi ketika salah satu service mengalami gangguan.

### Langkah Pengujian

1. Jalankan Book Service pada port 5001.
2. Jalankan Order Service pada port 5002.
3. Pastikan Order Service dapat membuat pesanan melalui Postman.
4. Hentikan Book Service dengan menekan:

```text
Ctrl + C
```

5. Jangan menghentikan Order Service.
6. Buka Postman dan kirim kembali:

```text
POST http://localhost:5002/orders
```

Body:

```json
{
    "book_id": 1
}
```

7. Tekan `Send`.

Hasil yang diperoleh:

```json
{
    "error": "Book Service sedang down!"
}
```

Status HTTP:

```text
500 INTERNAL SERVER ERROR
```

Hasil tersebut menunjukkan bahwa Order Service masih berjalan walaupun Book Service dihentikan, tetapi proses pemesanan yang membutuhkan data dari Book Service tidak dapat dilakukan.

## Perbandingan Monolith dan Microservices

| Aspek | Monolith | Microservices |
|---|---|---|
| Struktur | Semua fitur berada dalam satu aplikasi | Fitur dibagi menjadi beberapa service |
| File | monolith_app.py | book_service.py dan order_service.py |
| Port | 5000 | 5001 dan 5002 |
| Komunikasi | Akses data secara langsung | HTTP Request antar-service |
| Pengujian | Satu aplikasi | Masing-masing service diuji terpisah |
| Gangguan | Dapat berdampak pada aplikasi | Service lain tetap dapat berjalan |
| Kompleksitas | Lebih sederhana | Lebih kompleks |

## Analisis

Pada arsitektur Monolith, fitur buku dan pesanan berada dalam satu aplikasi sehingga komunikasi antarfitur lebih sederhana. Data buku dapat diakses langsung oleh fitur pemesanan.

Pada arsitektur Microservices, aplikasi dipisahkan menjadi Book Service dan Order Service. Book Service berjalan pada port 5001, sedangkan Order Service berjalan pada port 5002.

Order Service tidak mengambil data buku secara langsung. Order Service melakukan HTTP Request kepada Book Service untuk memperoleh informasi mengenai buku.

Hasil pengujian Fault Isolation menunjukkan bahwa ketika Book Service dihentikan, Order Service masih tetap berjalan. Namun, Order Service memberikan pesan:

```text
Book Service sedang down!
```

Hal tersebut menunjukkan adanya pemisahan service pada arsitektur Microservices.

## Kesimpulan

Praktikum ini menunjukkan perbedaan antara arsitektur Monolith dan Microservices menggunakan Flask Python.

Pada Monolith, seluruh fitur berada dalam satu aplikasi sehingga lebih sederhana dalam pembuatan dan komunikasi. Sedangkan pada Microservices, aplikasi dibagi menjadi beberapa service yang dapat berjalan secara terpisah.

Hasil pengujian menunjukkan bahwa Order Service dapat berkomunikasi dengan Book Service melalui HTTP Request. Ketika Book Service dihentikan, Order Service tetap berjalan tetapi tidak dapat memproses pesanan yang membutuhkan data dari Book Service.

Dengan praktikum ini dapat dipahami konsep Monolith, Microservices, komunikasi antar-service, serta Fault Isolation.

## Referensi

1. Politeknik Negeri Lhokseumawe, "Hands-on: Arsitektur Monolith vs Microservices dengan Flask Python", Modul Praktikum Paradigma Sistem untuk IT, 2026.
2. Flask Documentation.
3. Python Documentation.
4. Requests Documentation.