## **a. Apa itu AMQP?**

AMQP (Advanced Message Queuing Protocol) adalah protokol terbuka untuk pertukaran pesan di aplikasi. Dengan AMQP, satu sistem (producer) mengirim pesan ke broker (misalnya RabbitMQ), lalu sistem lain (consumer) mengambil dan memproses pesan itu—tanpa perlu tahu detail implementasi masing-masing.

## **b. Penjelasan `guest:guest@localhost:5672`**

Format umumnya:

```
username:password@host:port
```

* **`guest` (pertama)**: nama pengguna (username) untuk masuk ke broker AMQP.
* **`guest` (kedua)**: kata sandi (password) dari pengguna tersebut.
* **`localhost`**: alamat server broker—di sini artinya komputer lokal Anda.
* **`5672`**: nomor port TCP di mana broker mendengarkan koneksi AMQP (standar AMQP).

## Gambar

### 1. Simulasi Slow Response
![Gambar1](<Gambar/Screenshot 2025-05-15 193128.png>)

Di mesin saya saya melihat 2 queues, yaitu:
- user_created – queue utama tempat subscriber menunggu pesan.
- user_created_dlx (dead-letter queue) – otomatis dibuat karena opsi use_dead_letter: true.

### 2. Simulasi Menjalankan 3 Sekaligus
![Gambar2](<Gambar/Screenshot 2025-05-15 194348.png>)
![Gambar3](<Gambar/Screenshot 2025-05-15 194419.png>)

### 3. Refleksi & Perbaikan

1. Load Balancing Consumer
- Dengan 3 subscriber, broker mendistribusikan pesan round-robin ke tiap consumer.
- Terlihat beberapa spike paralel sesuai jumlah instance.

2. Prefetch Count
- Default prefetch = unlimited → consumer dapat mengambil banyak pesan sekaligus.
- Perbaikan: atur basic_qos(prefetch_count) agar tiap subscriber tidak overfetch.

3. Durability & Auto-delete
- Saat ini auto_delete: false dan durable: false.
- Perbaikan: set durable: true agar queue bertahan restart broker, dan auto_delete: true jika ingin queue otomatis hilang saat semua consumer disconnect.

4. Error Handling & Retry
- Kode saat ini hanya unwrap() dan panic on error.
- Perbaikan: tangani error di connect/publish/listen dengan retry/backoff.

5. Concurrency di Subscriber
- Saat ini subscriber memproses satu pesan per thread.
- Perbaikan: spawn worker pool atau gunakan async runtime (tokio::spawn) untuk memproses pesan paralel tanpa blocking utama.