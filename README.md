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