# 🐳 Laporan Praktikum Pertemuan 02
## Docker Fundamentals

---

## 👤 Identitas Mahasiswa

| Item | Keterangan |
|------|------------|
| **Nama** | MUH. EKA ANDRI SETIAWAN |
| **NIM** | 105841110723 |
| **Kelas** | 5C |
| **Tanggal** | 2026-02-25 |

---

## 📚 Pemahaman Docker

### Apa itu Docker?

Docker adalah platform containerization yang digunakan untuk membangun, mengemas, dan menjalankan aplikasi dalam suatu lingkungan terisolasi yang disebut container. Docker memungkinkan aplikasi berjalan secara konsisten di berbagai lingkungan, karena seluruh dependensi dan konfigurasi yang dibutuhkan telah dikemas dalam satu paket.

Container adalah unit eksekusi ringan yang berisi aplikasi beserta seluruh library, dependensi, dan konfigurasi yang diperlukan untuk menjalankannya. Container berjalan di atas sistem operasi host dengan berbagi kernel yang sama, sehingga lebih efisien dan cepat dibandingkan mesin virtual.

Image adalah template atau cetakan dasar yang digunakan untuk membuat container. Image bersifat statis dan tidak berubah, sedangkan container merupakan hasil eksekusi dari image tersebut.

Perbedaan utama antara container dan virtual machine (VM) terletak pada arsitekturnya. Virtual machine menjalankan sistem operasi tersendiri di atas hypervisor, sehingga membutuhkan sumber daya yang lebih besar. Sementara itu, container tidak memerlukan sistem operasi terpisah karena berbagi kernel dengan host, sehingga lebih ringan, cepat dijalankan, dan efisien dalam penggunaan resource.

### Jelaskan Komponen Utama Docker (Images, Containers, Registry)

1.  Image adalah template atau cetakan dasar yang digunakan untuk membuat container. Image berisi aplikasi, dependensi, library, serta konfigurasi yang diperlukan agar aplikasi dapat berjalan. Image bersifat read-only dan tidak berubah. Dari satu image, dapat dibuat banyak container dengan konfigurasi yang sama.

2.  Container adalah instansi yang berjalan dari sebuah image. Container merupakan lingkungan terisolasi yang menjalankan aplikasi beserta seluruh dependensinya. Berbeda dengan image yang bersifat statis, container bersifat dinamis karena dapat dijalankan, dihentikan, dimodifikasi, maupun dihapus sesuai kebutuhan.

3.  Registry adalah tempat penyimpanan dan distribusi image Docker. Registry memungkinkan pengguna untuk mengunggah (push) dan mengunduh (pull) image. Contoh registry publik yang umum digunakan adalah Docker Hub. Registry dapat bersifat publik maupun privat sesuai kebutuhan organisasi.

### Perbedaan Docker vs Virtual Machine

Perbedaan Docker Container dan Virtual Machine (VM) terletak pada arsitektur, penggunaan sumber daya, dan cara menjalankan sistem operasi.

Docker Container adalah lingkungan terisolasi yang berjalan di atas sistem operasi host dan berbagi kernel yang sama. Container hanya mengemas aplikasi beserta dependensinya tanpa membawa sistem operasi lengkap. Karena itu, container bersifat ringan, cepat dijalankan, dan lebih efisien dalam penggunaan CPU serta memori.

Virtual Machine (VM) adalah mesin virtual yang berjalan di atas hypervisor dan memiliki sistem operasi sendiri (guest OS). Setiap VM memerlukan alokasi resource tersendiri seperti CPU, RAM, dan storage, sehingga ukurannya lebih besar dan waktu booting lebih lama dibanding container.

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
| 1 | ![Container Running](screenshots/01-container-running.png) | Container Docker sedang berjalan (hasil perintah docker ps) |
| 2 | ![Nginx Browser](screenshots/02-nginx-browser.png) | Tampilan Nginx default page di browser (localhost) |
| 3 | ![Docker Build](screenshots/03-docker-build.png) | Proses build image menggunakan perintah docker build |
| 4 | ![Custom Image](screenshots/04-custom-image.png) | Image custom berhasil dibuat dan terlihat pada docker images |
| 5 | ![Compose PS](screenshots/05-compose-ps.png) | Daftar service yang berjalan menggunakan docker compose ps |
| 6 | ![Compose Services](screenshots/06-compose-services.png) | Tampilan service Docker Compose yang berjalan dan dapat diakses melalui browser |

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
