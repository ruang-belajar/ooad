# Sequence Diagram

Sequence Diagram merupakan bagian dari UML 2.x dan termasuk kelompok Interaction Diagram.

Interaction Diagram digunakan untuk memodelkan komunikasi antarpartisipan dalam suatu sistem.

Diagram interaksi yang umum dalam UML meliputi:

| Diagram                      | Fungsi                                            |
| ---------------------------- | ------------------------------------------------- |
| Sequence Diagram             | Menggambarkan urutan pesan berdasarkan waktu      |
| Communication Diagram        | Menggambarkan hubungan komunikasi antarobjek      |
| Interaction Overview Diagram | Menggambarkan alur interaksi tingkat tinggi       |
| Timing Diagram               | Menggambarkan perubahan kondisi berdasarkan waktu |
|                              |                                                   |

Fokus utama Sequence Diagram adalah urutan interaksi, bukan struktur kelas.

## 1. Elemen Sequence Diagram
### 1.1. Actor

Actor adalah pihak eksternal yang berinteraksi dengan sistem.

Actor dapat berupa:
- Manusia atau pengguna.
- Sistem eksternal.
- Perangkat atau entitas eksternal lainnya.

Contoh:
- Mahasiswa
- Admin
- Customer
- Payment Gateway
- Sistem Email

Actor biasanya digambarkan menggunakan simbol stick figure.

![](files/06-seq-1.png)

Actor bukan selalu manusia. Sistem eksternal yang berkomunikasi dengan sistem juga dapat menjadi actor.

### 1.2. Lifeline

Lifeline menggambarkan keberadaan suatu partisipan dalam interaksi.

Partisipan dapat berupa:
- Actor.
- Object.
- Instance dari class.
- Subsystem.
- Komponen eksternal.

Lifeline digambarkan sebagai kotak nama partisipan di bagian atas dan garis vertikal putus-putus ke bawah.

![](files/06-sec-3.png)


### 1.3. Message

Message adalah komunikasi yang dikirim dari satu partisipan ke partisipan lainnya.

Message dapat merepresentasikan:
- Pemanggilan operasi atau method.
- Pengiriman sinyal.
- Permintaan layanan.
- Pengembalian hasil operasi.

Message digambarkan menggunakan garis horizontal dengan panah.

![](files/06-sec-2.png)

### 1.4. Activation / Execution Specification

Activation atau Execution Specification menunjukkan periode ketika suatu partisipan sedang menjalankan aktivitas atau operasi.

Secara visual, activation digambarkan sebagai persegi panjang tipis pada lifeline.

![](files/06-sec-3.png)


Activation membantu memperlihatkan:
- Kapan operasi mulai dieksekusi.
- Berapa lama suatu operasi berlangsung secara relatif.
- Operasi yang memanggil operasi lain.
- Aktivitas bersarang (nested execution).

Activation bukan representasi waktu nyata atau durasi pasti kecuali diagram dilengkapi informasi waktu.

### 1.5. Message

![](files/06-sd-1.webp)

#### Synchronous Message

Synchronous Message adalah pesan yang mengindikasikan pemanggilan operasi secara sinkron.

Pengirim biasanya menunggu operasi yang dipanggil selesai sebelum melanjutkan proses berikutnya.

Karakteristik:
- Umumnya digunakan untuk pemanggilan method.
- Pengirim menunggu penyelesaian operasi.
- Sering ditampilkan dengan panah garis penuh dan kepala panah tertutup.

#### Asynchronous Message

Asynchronous Message adalah pesan yang dikirim tanpa mengharuskan pengirim menunggu penyelesaian penerima.

Contoh:

```
OrderService → NotificationService : sendNotification()
```

Karakteristik:
- Pengirim dapat melanjutkan proses.
- Cocok untuk komunikasi event atau pemrosesan latar belakang.
- Umumnya menggunakan panah dengan kepala terbuka.

Contoh implementasi:

```
notificationService.sendNotification(order);
```

Pemanggilan di atas dapat berupa asynchronous jika implementasinya memang tidak memblokir pengirim.

Catatan: Jenis message dalam diagram menjelaskan semantik interaksi, bukan sekadar penggunaan thread atau teknologi tertentu.

#### Return Message / Result

Return Message adalah pesan yang menunjukkan hasil atau pengembalian dari suatu pemanggilan operasi.

Biasanya digambarkan dengan garis putus-putus dan panah terbuka. Dalam UML, return message bersifat opsional apabila hasil operasi sudah jelas dari interaksi.

#### Self Message

Self Message adalah pesan yang dikirim oleh suatu partisipan kepada dirinya sendiri.

Digunakan ketika suatu objek memanggil operasi internal atau melakukan pemrosesan sendiri.

![](files/06-sd-2.png)
#### Create & Destroy Message

Create Message menunjukkan proses pembuatan objek baru. 

Delete Message menunjukkan penghapusan atau penghentian keberadaan objek dalam interaksi.

Penghapusan lifeline tidak otomatis berarti objek dihapus dari memori oleh garbage collector. Diagram menunjukkan semantik penghentian objek dalam model.

```mermaid

sequenceDiagram
    autonumber
    actor Customer as Pelanggan
    participant UI as OrderUI
    participant Controller as OrderController
    participant Order as Order

    Customer->>UI: checkout()
    UI->>Controller: createOrder(orderData)

    create participant NewOrder as NewOrder : Order
    Controller->>NewOrder: <<create>>(orderData)

    NewOrder->>NewOrder: calculateTotal()
    NewOrder-->>Controller: orderCreated

    Controller-->>UI: orderSuccess
    UI-->>Customer: tampilkanKonfirmasi()

    Customer->>UI: cancelOrder()
    UI->>Controller: cancelOrder(orderId)

    Controller->>NewOrder: destroy()

    destroy NewOrder
    Controller-xNewOrder: << destroy >>

    Controller-->>UI: cancellationSuccess
    UI-->>Customer: tampilkanPembatalan()

```

---

## 2. Struktur Visual Sequence Diagram

Sequence Diagram memiliki dua dimensi utama.
* Dimensi horizontal: Menunjukkan partisipan atau objek yang terlibat dalam interaksi.
- Dimensi vertikal: Menunjukkan urutan kejadian dari atas ke bawah.

Prinsip penting:
1. Partisipan diletakkan di bagian atas diagram.
2. Lifeline ditarik secara vertikal.
3. Pesan ditampilkan secara horizontal.
4. Pesan yang lebih awal ditempatkan lebih tinggi.
5. Interaksi dibaca dari atas ke bawah.
6. Jarak vertikal tidak harus menunjukkan durasi waktu sebenarnya.

---
## 3. Combined Fragments

Combined Fragment digunakan untuk menggambarkan kondisi, pengulangan, dan pola interaksi khusus.

Combined Fragment ditampilkan menggunakan frame persegi panjang dengan operator di bagian kiri atas.


### 3.1. Operator `alt`

`alt` digunakan untuk memodelkan alternatif interaksi berdasarkan kondisi.

Contoh UML:
```mermaid

sequenceDiagram
    actor User
    participant ATM
    participant Bank

    User->>ATM: withdraw(amount)
    ATM->>Bank: checkBalance()

    alt saldo cukup
        Bank-->>ATM: approved
        ATM-->>User: dispenseCash()
    else saldo tidak cukup
        Bank-->>ATM: rejected
        ATM-->>User: insufficientBalance()
    end

```
### 3.2. Operator `opt`

`opt` digunakan untuk interaksi yang hanya dijalankan jika kondisi tertentu terpenuhi.

Contoh:

```
Jika pengguna memilih "Remember Me":
    sistem menyimpan preferensi login
```

`opt` dapat dipahami sebagai alternatif dengan satu operand dan kondisi tertentu.

Contoh:


```mermaid

sequenceDiagram
    actor User
    participant UI as LoginUI
    participant Service as AuthService

    User->>UI: login()
    UI->>Service: authenticate()

    opt Remember Me dipilih
        UI->>Service: savePreference()
    end

```
---
### 3.3. Operator `loop`

`loop` digunakan untuk menggambarkan pengulangan interaksi.

Contoh:

```
Selama jumlah item > 0:
    sistem memproses item
```

Contoh:

```mermaid

sequenceDiagram
    actor User
    participant Cart
    participant Order

    User->>Cart: checkout()

    loop untuk setiap item
        Cart->>Order: addItem(item)
    end

    Order-->>User: orderSummary

```

Pengulangan dapat memiliki kondisi atau batas iterasi.

Contoh kondisi:

```
loop [jumlah percobaan < 3]
```
---

### 3.4. Operator `par`

`par` digunakan untuk menggambarkan interaksi yang berlangsung secara paralel.

Contoh, setelah transaksi berhasil, sistem:
- Mengirim email konfirmasi.
- Memperbarui notifikasi.
- Memperbarui data analitik.

```mermaid

sequenceDiagram
    participant Order
    participant Email
    participant Notification
    participant Analytics

    Order->>Order: completeOrder()

    par Kirim email
        Order->>Email: sendConfirmation()
    and Kirim notifikasi
        Order->>Notification: notifyCustomer()
    and Update analitik
        Order->>Analytics: recordSale()
    end

```

`par` menunjukkan kemungkinan eksekusi paralel, bukan jaminan bahwa seluruh proses benar-benar berjalan bersamaan pada implementasi.


![](files/Pasted%20image%2020261109214156.png)

---
### 3.5. Operator `break`

`break` digunakan untuk menunjukkan interaksi yang menghentikan bagian interaksi yang sedang berlangsung ketika kondisi tertentu terpenuhi.

Contoh:

```
Jika stok tidak tersedia:
    proses checkout dihentikan
```

```mermaid

sequenceDiagram
    actor User
    participant Cart
    participant Inventory

    User->>Cart: checkout()
    Cart->>Inventory: checkStock()

    break stok tidak tersedia
        Inventory-->>Cart: outOfStock
        Cart-->>User: checkoutFailed
    end

```

---
## 4. Study Kasus

### 4.1. Contoh Sequence Diagram: Sistem Login

##### Studi Kasus

- Sebuah aplikasi memiliki fitur login.
- Pengguna memasukkan username dan password. Sistem memeriksa data pengguna melalui database.
- Jika data valid, pengguna diarahkan ke dashboard.
- Jika data tidak valid, sistem menampilkan pesan kesalahan.

##### Skenario Use Case

| No. | Aktor / Sistem | Aktivitas                        |
| --- | -------------- | -------------------------------- |
| 1   | User           | Membuka halaman login            |
| 2   | User           | Memasukkan username dan password |
| 3   | LoginUI        | Mengirim data login              |
| 4   | AuthService    | Memvalidasi kredensial           |
| 5   | UserRepository | Mengambil data pengguna          |
| 6   | AuthService    | Mengembalikan hasil autentikasi  |
| 7   | LoginUI        | Menampilkan hasil login          |

![](files/Pasted%20image%2020261109213116.png)
##### Penjelasan diagram
- `User` merupakan actor.
- `LoginUI` bertanggung jawab menerima input.
- `AuthService` melakukan proses autentikasi.
- `UserRepository` bertanggung jawab mengambil data pengguna.
- `Database` menyimpan data pengguna.
- `alt` digunakan untuk menunjukkan dua kondisi hasil autentikasi.

Diagram ini menunjukkan bahwa proses login bukan hanya aktivitas pengguna, tetapi juga komunikasi antarobjek internal sistem.

---
### 4.2. Studi Kasus: Sistem Pemesanan Barang Online

- Sebuah aplikasi e-commerce menyediakan fitur pemesanan barang.
- Proses dimulai ketika pelanggan memilih produk dan melakukan checkout.
- Sistem memeriksa ketersediaan stok. Jika stok tersedia, sistem membuat pesanan dan memproses pembayaran.
- Jika pembayaran berhasil, sistem mengonfirmasi pesanan.
- Jika pembayaran gagal, sistem menampilkan informasi kegagalan.

| Partisipan       | Tanggung Jawab                        |
| ---------------- | ------------------------------------- |
| Customer         | Memilih produk dan melakukan checkout |
| OrderUI          | Menangani interaksi pengguna          |
| OrderService     | Memproses pesanan                     |
| InventoryService | Memeriksa ketersediaan stok           |
| PaymentService   | Memproses pembayaran                  |
| Database         | Menyimpan data pesanan                |

##### Sequence Diagram E-Commerce

```mermaid

sequenceDiagram
    autonumber

    actor Customer as Pelanggan
    participant UI as ECommerceUI
    participant Order as OrderService
    participant Inventory as InventoryService
    participant Payment as PaymentService
    participant DB as Database
    participant Notify as NotificationService

    Customer->>UI: pilihProduk()
    Customer->>UI: checkout(cart)

    UI->>Order: createOrder(cart)

    opt Pengguna belum login
        Order-->>UI: requestLogin()
        UI-->>Customer: tampilkanFormLogin()
        Customer->>UI: login(username, password)
        UI->>Order: authenticate(username, password)
        Order-->>UI: authenticationResult
    end

    Order->>Inventory: checkStock(cart)

    alt Stok tersedia
        Inventory-->>Order: stockAvailable

        Order->>DB: saveOrder(order)
        DB-->>Order: orderId

        Order->>Payment: processPayment(orderId, total)

        alt Pembayaran berhasil
            Payment-->>Order: paymentSuccess

            Order->>DB: updateStatus(PAID)
            DB-->>Order: statusUpdated

            loop Untuk setiap item
                Order->>Inventory: updateStock(item, qty)
                Inventory-->>Order: stockUpdated
            end

            par Proses notifikasi
                Order->>Notify: sendEmailConfirmation(orderId)
            and
                Order->>Notify: sendOrderNotification(orderId)
            end

            Order-->>UI: orderConfirmed(orderId)
            UI-->>Customer: tampilkanKonfirmasiPesanan()

        else Pembayaran gagal
            Payment-->>Order: paymentFailed

            Order->>DB: updateStatus(PAYMENT_FAILED)
            DB-->>Order: statusUpdated

            Order-->>UI: paymentError()
            UI-->>Customer: tampilkanPesanGagal()
        end

    else Stok tidak tersedia
        Inventory-->>Order: stockUnavailable

        Order-->>UI: stockError()
        UI-->>Customer: tampilkanStokTidakTersedia()
    end

```

Diagram tersebut memperlihatkan beberapa prinsip desain berorientasi objek.

#### Separation of Concerns

Setiap objek memiliki tanggung jawab yang berbeda.
- `OrderUI`: interaksi dengan pengguna.
- `OrderService`: logika pemrosesan pesanan.
- `InventoryService`: pemeriksaan stok.
- `PaymentService`: pembayaran.

Pemisahan ini membantu mengurangi ketergantungan antarbagian sistem.

#### Encapsulation
- Customer tidak berinteraksi langsung dengan database.
- Customer mengirimkan permintaan ke UI, kemudian UI berkomunikasi dengan service.
- Database diakses melalui objek yang bertanggung jawab terhadap proses tersebut.

#### Object Collaboration
- Proses checkout melibatkan kolaborasi beberapa objek.
- Satu objek tidak harus menangani seluruh proses.

#### Conditional Interaction
Penggunaan `alt` memperlihatkan alternatif alur berdasarkan kondisi:
- Stok tersedia atau tidak.
- Pembayaran berhasil atau gagal.


---

## 6. Langkah-Langkah Membuat Sequence Diagram

Berikut tahapan sistematis yang dapat digunakan mahasiswa.

1. Identifikasi Use Case
    - Tentukan proses yang akan dimodelkan, misalnya login, checkout, registrasi, atau peminjaman buku.
    
2. Tentukan Skenario
    - Tuliskan alur utama dan alur alternatif berdasarkan kebutuhan sistem.
    
3. Identifikasi Actor dan Partisipan
    - Tentukan siapa yang memulai interaksi dan objek apa saja yang terlibat.
    
4. Tentukan Tanggung Jawab Objek
    - Tentukan objek yang menerima input, menjalankan logika bisnis, mengakses data, dan berkomunikasi dengan sistem eksternal.
    
5. Susun Lifeline
    - Tempatkan actor dan objek di bagian atas diagram.
    
6. Gambarkan Message
    - Susun pesan dari atas ke bawah sesuai urutan interaksi.
    
7. Tambahkan Combined Fragment
    - Gunakan `alt`, `opt`, `loop`, atau `par` jika diperlukan.
    
8. Validasi Diagram
    - Pastikan alur sesuai dengan skenario use case, tanggung jawab objek, dan kebutuhan sistem.
    
---
## 7. Kesalahan Umum dalam Membuat Sequence Diagram

| Kesalahan                                     | Penjelasan                                  |
| --------------------------------------------- | ------------------------------------------- |
| Lifeline terlalu banyak                       | Diagram menjadi sulit dibaca                |
| Pesan tidak berurutan                         | Tidak sesuai dengan skenario use case       |
| Actor langsung mengakses database             | Mengabaikan tanggung jawab objek            |
| Semua proses dilakukan satu objek             | Menunjukkan desain yang terlalu terpusat    |
| Tidak ada alur alternatif                     | Kondisi gagal tidak dimodelkan              |
| Return message berlebihan                     | Diagram menjadi penuh dan sulit dipahami    |
| Tidak menggunakan activation                  | Struktur eksekusi sulit dianalisis          |
| Pesan tidak sesuai operasi class              | Diagram tidak konsisten dengan desain objek |
| Menganggap garis vertikal sebagai waktu nyata | Salah memahami dimensi temporal diagram     |

> 💡 **Prinsip desain:** Sequence Diagram harus cukup detail untuk menjelaskan interaksi, tetapi tidak terlalu detail sampai setiap instruksi program digambarkan. Fokuslah pada komunikasi antarpartisipan dan tanggung jawab objek.

---
## 8. 🎓Diskusi & Tugas

### Latihan 1 — Sistem Login
Buat Sequence Diagram untuk proses login dengan ketentuan:
- Pengguna memasukkan username dan password.
- Sistem memeriksa data pengguna.
- Jika valid, sistem menampilkan dashboard.
- Jika tidak valid, sistem menampilkan pesan kesalahan.
- Pengguna dapat mencoba login kembali.

Pertanyaan:
1. Siapa actor dalam sistem?
2. Objek apa saja yang terlibat?
3. Message apa yang digunakan?
4. Bagaimana menggambarkan kondisi login berhasil dan gagal?
5. Bagaimana menggambarkan pengulangan login?

---
### Latihan 2 — Sistem Perpustakaan

Buat Sequence Diagram untuk proses peminjaman buku.

Skenario:
1. Mahasiswa mencari buku.
2. Sistem menampilkan daftar buku.
3. Mahasiswa memilih buku.
4. Sistem memeriksa ketersediaan buku.
5. Jika tersedia, sistem mencatat peminjaman.
6. Jika tidak tersedia, sistem menampilkan pesan.
7. Sistem mengirim konfirmasi peminjaman.

Partisipan yang dapat digunakan:
- Mahasiswa
- BookUI
- BookService
- LoanService
- BookRepository
- Database

Pertanyaan analisis:
- Apakah BookService perlu berinteraksi langsung dengan database?
- Apa tanggung jawab LoanService?
- Bagaimana menggambarkan buku yang tidak tersedia?
- Apakah notifikasi dapat dilakukan secara asynchronous?
---
### Latihan 3 — Sistem Pembayaran

Buat Sequence Diagram untuk sistem pembayaran dengan ketentuan:
- Customer melakukan pembayaran.
- Sistem membuat transaksi.
- Payment Gateway memproses pembayaran.
- Jika pembayaran berhasil, sistem mengubah status pesanan.
- Jika gagal, sistem menampilkan informasi kegagalan.
- Sistem mengirim notifikasi setelah pembayaran berhasil.

Gunakan:
- `alt`
- `par`
- Synchronous Message
- Asynchronous Message

---

# Communication Diagram

Dalam contoh notasi untuk communication diagram, objek (aktor dalam kasus penggunaan/use case) direpresentasikan oleh persegi panjang. Dalam contoh tersebut (communication diagram generik):

- Objek-objek yang ada adalah Object1, Object2, Object..., ObjectN-1, ..., dan ObjectN.
- Pesan yang dikirimkan antarobjek direpresentasikan oleh panah berlabel yang bermula dari objek (aktor) pengirim dan berakhir pada objek penerima.
- Contoh pesan yang dikirimkan antarobjek diberi label 1: message1, 2: message2, 3: message3, dan seterusnya, di mana awalan numerik pada nama pesan menunjukkan urutannya dalam rangkaian tersebut.
- Object1 pertama-tama mengirimkan pesan message1 kepada Object2, kemudian Object2 mengirimkan pesan message2 kepada ObjectN-1, dan seterusnya.
- Pesan yang dikirimkan oleh objek kepada dirinya sendiri ditunjukkan sebagai perulangan/loop (contohnya, message5).

![](files/06-com-1.png)



## Communication Example - Hotel Reservation

![](files/06-com-2.png)



- Setiap pesan dalam communication diagram memiliki nomor urut.
- Pesan tingkat atas diberi nomor 1.
- Pesan-pesan yang dikirim selama pemanggilan yang sama memiliki awalan desimal yang sama, namun dengan akhiran 1, 2, dan seterusnya, bergantung pada urutan kemunculannya.

## Communication Diagram vs Sequence Diagram

communication diagram dan diagram urutan (sequence diagram) memiliki kemiripan. Keduanya secara semantik setara—artinya, keduanya menyajikan informasi yang sama—dan Anda dapat mengubah communication diagram menjadi diagram urutan, begitu pula sebaliknya. Perbedaan utama di antara keduanya adalah bahwa communication diagram menyusun elemen-elemen berdasarkan ruang, sedangkan diagram urutan menyusunnya berdasarkan waktu.

Di antara kedua jenis diagram interaksi tersebut, diagram urutan tampaknya jauh lebih sering digunakan dibandingkan communication diagram. Lantas, mengapa Anda perlu menggunakan communication diagram? Pertama, diagram ini sangat berguna untuk memvisualisasikan hubungan antarobjek yang berkolaborasi dalam melaksanakan tugas tertentu—hal yang sulit ditentukan melalui diagram urutan. Selain itu, communication diagram juga dapat membantu Anda memastikan akurasi model statis Anda (yaitu, class diagram).

## Example - Sequence diagram vs Communication (Library Item Overdue)

![](files/06-com-3.png)

communication diagram seperti yang diperlihatkan pada gambar di bawah ini:

![](files/06-com-4.png)

**Catatan**: Jika Anda membandingkan kedua diagram tersebut, Anda akan melihat bahwa keduanya sama-sama memuat objek dan pesan. Selain itu, terlihat jelas bahwa menentukan urutan waktu pesan jauh lebih mudah dengan melihat diagram urutan (*sequence diagram*), sementara untuk melihat hubungan antarobjek, lebih mudah menggunakan communication diagram (*communication diagram*).

## Communication Diagram Elements

**Objek** yang berpartisipasi dalam suatu kolaborasi terbagi menjadi dua jenis: penyedia (*supplier*) dan klien (*client*).

- **Objek penyedia (*supplier objects*)** adalah objek yang menyediakan metode yang dipanggil, sehingga objek ini menerima pesan.
- **Objek klien (*client objects*)** adalah objek yang memanggil metode pada objek penyedia, sehingga objek ini mengirimkan pesan.

**Tautan (*Links*)**

- Garis penghubung yang digambarkan di antara objek-objek dalam communication diagram disebut sebagai tautan (*links*).
- Tautan inilah yang membedakan communication diagram dengan diagram urutan (*sequence diagrams*). Tautan memungkinkan Anda untuk melihat hubungan antarobjek.
- Setiap tautan merepresentasikan hubungan antarobjek dan melambangkan kemampuan objek-objek tersebut untuk saling mengirim pesan.
- Jika suatu objek mengirim pesan kepada dirinya sendiri, tautan yang membawa pesan tersebut digambarkan sebagai ikon perulangan (*loop*). Perulangan ini dapat dilihat baik pada objek antarmuka pengguna (UI) maupun objek transaksi.

**Pesan (*Messages*)** dalam communication diagram ditunjukkan sebagai panah yang mengarah dari objek klien ke objek penyedia. Biasanya, pesan merepresentasikan tindakan klien yang memanggil suatu operasi pada objek penyedia. Pesan dapat dimodelkan bersama dengan objek-objek terkait sebagai berikut:

- Ikon pesan dapat memuat satu atau lebih pesan.
- Pesan terdiri dari teks pesan yang diawali dengan nomor urut (*sequence number*).
- Nomor urut ini menunjukkan urutan waktu dari pesan tersebut.

Sebagai contoh, dalam communication diagram pada gambar di bawah ini, Anda dapat mengikuti nomor urut untuk menentukan urutan pesan di antara objek-objek yang ada:

![](files/06-com-5.png)



## Memahami Penomoran Pesan dalam communication diagram

- Pesan pertama dalam communication diagram selalu diberi nomor 1, pesan kedua diberi nomor 2, dan seterusnya.
- Anda dapat menunjukkan bahwa suatu pesan bersarang (nested) di bawah pesan induk dengan menambahkan titik desimal serta angka tambahan pada nomor urut pesan induk tersebut.

Sebagai contoh:

Berdasarkan contoh di atas, pesan "CalAmtCanBorrow" merupakan pesan bersarang pertama di bawah "EnquireBorrower" dan diberi nomor urut 1.1. Pesan bersarang kedua di bawah "EnquireBorrower" adalah "DisplayInvalidMsg", sehingga pesan ini diberi nomor urut 1.2.

## Contoh - Dari Diagram Urutan (Sequence Diagram) ke communication diagram (Communication Diagram)

![](files/06-com-6.png)

**Perhatikan bahwa**:

- **Fokus kendali (focus of control)**: disebut juga sebagai **kejadian eksekusi/aktivasi (execution occurrence/activation)**. Elemen ini ditampilkan sebagai persegi panjang yang tinggi dan ramping pada sebuah garis kehidupan (*lifeline*), yang merepresentasikan **periode** di mana suatu elemen sedang melakukan suatu operasi.
- Bagian atas dan bawah persegi panjang tersebut masing-masing sejajar dengan waktu dimulainya dan waktu selesainya operasi.
- Dalam communication diagram (*communication diagram*), fokus kendali bersifat eksplisit sehingga dapat direpresentasikan melalui penomoran bertingkat (*nest numbering*) pada pesan.

📚 Sumber: https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-communication-diagram/