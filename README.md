# Percobaan Mengeliminasi Resource Contention RTOS dengan Mutual Exclusion (Mutex)
## Tentang Proyek
Proyek ini mendemonstrasikan penggunaan mutex (mutual exclusion) dalam sistem operasi embedded menggunakan FreeRTOS. Tujuannya adalah untuk menunjukkan bagaimana penggunaan mutex dapat memengaruhi performa sistem ketika beberapa task mengakses sumber daya yang sama. 

## Komponen Task
### Proyek ini menggunakan tiga task yang berbeda:
- GreenLedTask: Task ini menghidupkan dan mematikan LED hijau secara bergantian.
- RedLedTask: Task ini menghidupkan dan mematikan LED merah secara bergantian.
- orangeLedTask: Task ini menghidupkan dan mematikan LED oranye secara bergantian.

## Cara Kerja Program
### Inisialisasi Sistem
1. Melakukan konfigurasi clock sistem
2. Menginisialisasi GPIO untuk LED
3. Nonatktifkan semua LED pada awal program
4. Menginisialisasi kernel RTOS
5. Membuat dan memulai thread-thread

### Logika Program
Program ini dirancang untuk menunjukkan bagaimana sistem operasi dapat mengelola beberapa task secara bersamaan. Setiap task memiliki prioritas yang berbeda-beda, sehingga sistem operasi dapat menentukan task mana yang harus dijalankan terlebih dahulu.
Task GreenLedTask dan RedLedTask memiliki prioritas yang lebih rendah daripada task orangeLedTask. Oleh karena itu, ketika task orangeLedTask dijalankan, task GreenLedTask dan RedLedTask akan dihentikan sementara.
Program ini menggunakan sumber daya yang sama, yaitu LED hijau, merah, dan oranye. Oleh karena itu, program ini harus menghindari konflik sumber daya agar tidak terjadi kesalahan.
Untuk menghindari konflik sumber daya, program ini menggunakan fungsi taskENTER_CRITICAL() dan taskEXIT_CRITICAL() untuk mengunci dan membuka sumber daya. Fungsi ini memastikan bahwa hanya satu task yang dapat mengakses sumber daya pada satu waktu.

### Diagram Alur Kerja Program
![image](https://github.com/user-attachments/assets/ba465353-f51c-4200-a4d0-6c9c2c450dd0)

## Konfigurasi Hardware
### Program menggunakan konfigurasi GPIO berikut:
 - GPIOE: LED Oranye dan Biru
 - GPIOB: LED Hijau dan Merah

## Catatan Pengembangan
- Program menggunakan HAL (Hardware Abstraction Layer) STM32

## Percobaan
### 7.1: Jalankan program dengan hanya satu task yang aktif pada satu waktu.
![image](https://github.com/user-attachments/assets/0cfcded6-6448-4e34-8fb0-31975732123f)
![merah](https://github.com/user-attachments/assets/d53b3664-5843-4dd8-8914-2ab1267761d3)
![oren](https://github.com/user-attachments/assets/640927f6-4094-4ba8-9d11-977d57296657)
![ijo](https://github.com/user-attachments/assets/4d3cbbdf-4db4-46ed-95f3-1ab21abee303)


Pada percobaan ini, hanya satu tugas yang aktif pada satu waktu, dan sistem berperilaku seperti pada timing diagram untuk tugas tersebut. Misalnya, jika hanya FlashGreenLedTask yang aktif, maka LED hijau akan berkedip setiap 200ms.

### 7.2: Eksekusi sistem dengan semua tugas aktif
![7_2](https://github.com/user-attachments/assets/fc96bdba-ef01-4af7-b827-7419480d479b)
Pada percobaan ini, semua tugas aktif secara konkuren, dan sistem menunjukkan perilaku konkuren. LED hijau, merah, dan oranye akan berkedip pada kecepatan yang berbeda dari ketentuan delay yang sebelumnya sudah ditetapkan. LED biru akan dinyalakan dan dimatikan secara acak karena terjadi akses konkuren ke sumber daya bersama (startFlag). Sistem menunjukkan perilaku yang tidak dapat diprediksi, dan LED tidak berkedip seperti yang diharapkan karena akses konkuren ke sumber daya bersama. Fungsi AccessSharedData tidak akan menyediakan eksklusi mutual, dan tugas-tugas akan saling mengganggu saat mengakses sumber daya bersama.

### 7.3: Ulangi 7.2, tetapi gunakan eksklusi mutual
![7_3](https://github.com/user-attachments/assets/3c745955-e43b-40f7-86cc-5d86b358f6fd)
Pada percobaan ini, semua tugas aktif secara konkuren, tetapi fungsi AccessSharedData sekarang menggunakan mutual exclusion untuk melindungi sumber daya bersama (startFlag). Sistem akan menunjukkan perilaku yang dapat diprediksi, dan LED akan berkedip seperti yang diharapkan. LED biru tidak akan hidup karena tidak terjadi resource contention, dan tugas-tugas tidak akan saling mengganggu saat mengakses sumber daya bersama. Penggunaan mutual exclusion akan mencegah tugas-tugas mengakses sumber daya bersama secara simultan, sehingga sistem berperilaku seperti yang diharapkan.
