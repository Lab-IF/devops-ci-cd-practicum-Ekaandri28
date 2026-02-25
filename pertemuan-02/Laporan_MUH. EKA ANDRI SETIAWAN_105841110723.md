# 🐳 Laporan Praktikum Pertemuan 02
## Docker Fundamentals

---

## 👤 Identitas Mahasiswa

| Item | Keterangan |
|------|------------|
| **Nama** | ASHABUL KAHFI |
| **NIM** | 105841108523 |
| **Kelas** | 5C |
| **Tanggal** | 2026-02-24 |

---

## 📚 Pemahaman Docker

### Apa itu Docker?

Docker adalah sebuah platform open-source yang memungkinkan developer untuk merancang, menguji, dan menerapkan aplikasi dengan cepat menggunakan container.

- Container adalah lingkungan terisolasi yang mengemas kode aplikasi beserta semua dependensinya (library, runtime, konfigurasi) sehingga aplikasi dapat berjalan dengan konsisten di lingkungan komputasi mana pun (laptop, server pengujian, atau production).

- Images adalah cetak biru (blueprint) bersifat read-only yang berisi instruksi untuk membuat container. Jika image adalah "resep masakan", maka container adalah "kue" yang sudah jadi dan siap dimakan.

- Perbedaannya dengan VM: Virtual Machine (VM) memvirtualisasikan perangkat keras (hardware) dan membutuhkan Sistem Operasi (OS) tamu yang lengkap untuk setiap aplikasinya. Sebaliknya, Docker memvirtualisasikan OS, sehingga beberapa container dapat berbagi kernel OS yang sama dari komputer host. Hal ini membuat Docker jauh lebih ringan, cepat, dan hemat sumber daya dibandingkan VM.

### Komponen Utama Docker

1. Images: Paket executable yang ringan dan mandiri. Image menjadi dasar pembuatan container.

2. Containers: Instansiasi dari image yang sedang berjalan (running environment). Container dapat dibuat, dimulai, dihentikan, dipindahkan, atau dihapus dengan perintah Docker API atau CLI.

3. Registry: Layanan penyimpanan dan distribusi untuk Docker images. Contoh paling populer adalah Docker Hub (seperti GitHub, tetapi khusus untuk menyimpan images).

### Perbedaan Docker vs Virtual Machine

1. Arsitektur: VM memiliki Guest OS (OS Tamu) sendiri di atas Hypervisor. Container berjalan di atas Docker Engine dan berbagi kernel dengan OS Host.

2. Ukuran: VM berukuran sangat besar (Gigabytes) karena mengemas seluruh sistem operasi. Container sangat kecil (Megabytes) karena hanya berisi aplikasi dan dependensinya.

3. Waktu Booting: VM membutuhkan waktu lama (menit) untuk booting seperti menyalakan komputer biasa. Container bisa menyala dalam hitungan detik bahkan milidetik.

---

## 🔧 Praktik Docker Commands

### Output docker ps -a

```
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                                                                                        
                                          NAMES
036d42b9bd8b   pertemuan02-web                       "python app.py"          3 minutes ago    Exited (0) 42 seconds ago                                                                                                
                                          pertemuan02-web-1
a941b22462e7   hello-world                           "/hello"                 54 minutes ago   Exited (0) 53 minutes ago          
```

### Output docker images

```
REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
pertemuan02-web                  latest    b10b234e56d7   4 minutes ago    200MB
flask-app                        latest    6b7e3636b3ac   11 minutes ago   200MB
```

### Docker Run Command

```bash
docker run -d -p 5000:5000 --name web-app flask-app
```

---

## 📄 Dockerfile

### Isi Dockerfile

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

### Penjelasan Dockerfile

FROM python:3.9-slim: Menentukan base image yang digunakan, yaitu Python versi 3.9 versi ringan (slim).

WORKDIR /app: Menetapkan direktori kerja di dalam container menjadi folder /app.

COPY requirements.txt .: Menyalin file daftar dependensi (requirements.txt) dari komputer lokal ke direktori kerja di container.

RUN pip install --no-cache-dir -r requirements.txt: Menjalankan instalasi library Python yang dibutuhkan tanpa menyimpan cache agar ukuran image tetap kecil.

COPY . .: Menyalin seluruh sisa kode sumber aplikasi Flask dari folder lokal ke direktori kerja /app di container.

EXPOSE 5000: Memberi tahu Docker bahwa container akan mendengarkan (listen) pada port 5000 saat berjalan.

CMD ["python", "app.py"]: Perintah utama yang akan dijalankan secara otomatis saat container dihidupkan (menjalankan aplikasi Flask).

---

## 🐙 Docker Compose

### Isi docker-compose.yml

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    environment:
      - FLASK_ENV=development
```

### Penjelasan Docker Compose

version: '3.8': Menentukan versi sintaks Docker Compose yang digunakan.

services:: Mendefinisikan daftar container/layanan yang akan dijalankan. Di sini hanya ada satu layanan bernama web.

build: .: Menginstruksikan Compose untuk membangun image dari Dockerfile yang ada di direktori saat ini (.).

ports: - "5000:5000": Melakukan pemetaan port, yaitu port 5000 di komputer host diarahkan ke port 5000 di dalam container.

volumes: - .:/app: Menyinkronkan folder lokal saat ini (.) dengan folder /app di dalam container. Ini berguna agar perubahan kode bisa langsung terlihat tanpa perlu build ulang image (Live Reloading).

environment:: Mengatur variabel lingkungan di dalam container, dalam hal ini mengatur Flask ke mode development.

### Output Docker Compose Up

```
web-1  |  * Serving Flask app 'app'
web-1  |  * Debug mode: on
web-1  | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
web-1  |  * Running on all addresses (0.0.0.0)
web-1  |  * Running on http://127.0.0.1:5000                                                                                                                                                                            
web-1  |  * Running on http://172.21.0.2:5000       
```

---

## 📸 Screenshots

| No | Screenshot | Keterangan |
|----|------------|------------|
| 1 | ![Container Running](screenshots/01-container-running.png) | Container yang sedang berjalan |
| 2 | ![Docker Images](screenshots/02-docker-images.png) | Daftar Docker images |
| 3 | ![Dockerfile](screenshots/03-dockerfile-content.png) | Isi file Dockerfile |
| 4 | ![Docker Build](screenshots/04-docker-build.png) | Proses docker build |
| 5 | ![App Browser](screenshots/05-app-browser.png) | Aplikasi berjalan di browser |
| 6 | ![Compose Up](screenshots/06-compose-up.png) | Docker Compose up |

---

## 💭 Refleksi & Kesimpulan

### Yang Dipelajari

Saya belajar bagaimana mengemas sebuah aplikasi (beserta seluruh dependensinya) ke dalam sebuah container menggunakan Dockerfile. Saya juga mempelajari cara menjalankan multi-konfigurasi dengan mudah menggunakan docker-compose.yml, sehingga proses setup environment menjadi sangat cepat dan seragam.

### Manfaat Docker

Docker menyelesaikan masalah klasik programmer yaitu "It works on my machine" (Aplikasi berjalan di laptop saya, tapi error di server). Dengan Docker, lingkungan pengembangan, pengujian, dan produksi menjadi identik. Hal ini mempercepat proses deployment dan memudahkan kolaborasi tim.

### Tantangan dan Solusi

Tantangan utama adalah memahami konsep Port Mapping (-p) dan Volumes (-v), di mana saya awalnya bingung membedakan port lokal dan port container. Solusinya, saya membaca dokumentasi resmi Docker, membedah sintaks host_port:container_port, dan bereksperimen mengubah nilai port di docker-compose.yml hingga saya memahami cara kerjanya secara visual di browser.

---

## ✅ Checklist

- [x] Berhasil membuat Dockerfile yang valid
- [x] Berhasil build Docker image
- [x] Container berjalan dan aplikasi bisa diakses
- [x] Docker Compose berhasil dijalankan
- [x] Semua screenshot lengkap dan jelas
- [x] Penjelasan ditulis dengan bahasa sendiri

---

*Laporan ini dibuat pada Selasa, 24 Februari 2026*
