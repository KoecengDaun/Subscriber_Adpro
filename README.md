**a. Apa itu AMQP?**
AMQP (Advanced Message Queuing Protocol) adalah protokol terbuka untuk pertukaran pesan di aplikasi. Dengan AMQP, satu sistem (producer) mengirim pesan ke broker (misalnya RabbitMQ), lalu sistem lain (consumer) mengambil dan memproses pesan itu—tanpa perlu tahu detail implementasi masing-masing.

**b. Penjelasan `guest:guest@localhost:5672`**
Format umumnya:

```
username:password@host:port
```

* **`guest` (pertama)**: nama pengguna (username) untuk masuk ke broker AMQP.
* **`guest` (kedua)**: kata sandi (password) dari pengguna tersebut.
* **`localhost`**: alamat server broker—di sini artinya komputer lokal Anda.
* **`5672`**: nomor port TCP di mana broker mendengarkan koneksi AMQP (standar AMQP).

