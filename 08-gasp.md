# GRASP Fundamentals

## 1. Apa itu GRASP?

**GRASP** (_General Responsibility Assignment Software Patterns_) adalah kumpulan prinsip untuk menentukan **objek mana yang seharusnya bertanggung jawab** atas suatu perilaku dalam sistem berorientasi objek.

GRASP bukan framework atau aturan yang kaku. Ia membantu pengembang membuat desain yang:
- Mudah dipahami dan dirawat.
- Memiliki ketergantungan antarclass yang rendah.
- Mudah dikembangkan dan diuji.
- Memiliki pembagian tanggung jawab yang jelas.

Contoh pertanyaan yang dijawab GRASP:

> “Class mana yang sebaiknya menghitung total pesanan?”  
> “Objek mana yang membuat objek pembayaran?”  
> “Di mana logika proses checkout seharusnya ditempatkan?”

## 2. Konsep Dasar: _Responsibility_

Dalam desain berorientasi objek, sebuah _responsibility_ adalah kewajiban sebuah objek.

Dua jenis utama responsibility:

1. **Knowing responsibility**  
    - Tanggung jawab untuk mengetahui data atau informasi tertentu.
    - Contoh: `Order` mengetahui daftar `OrderItem` yang dimilikinya.
    
2. **Doing responsibility**  
    - Tanggung jawab untuk melakukan suatu tindakan.
    - Contoh: `Order` menghitung total harga pesanan.
    

---

# 3. Pola-Pola GRASP

## 3.1 Information Expert

**Prinsip:** Berikan tanggung jawab kepada class yang memiliki informasi paling diperlukan untuk menjalankan tugas tersebut.

Contoh: total pesanan seharusnya dihitung oleh `Order`, karena `Order` memiliki daftar item pesanan.

```
class Order {
    private List<OrderItem> items;

    public double calculateTotal() {
        double total = 0;
        for (OrderItem item : items) {
            total += item.getSubtotal();
        }
        return total;
    }
}

class OrderItem {
    private Product product;
    private int quantity;

    public double getSubtotal() {
        return product.getPrice() * quantity;
    }
}
```

`OrderItem` menghitung subtotal karena mengetahui produk dan jumlahnya. `Order` menghitung total karena mengetahui seluruh item.

## 3.2 Creator

**Prinsip:** Class A sebaiknya membuat objek B apabila A:
- Menyimpan atau mengagregasi B.
- Menggunakan B secara dekat.
- Memiliki data yang dibutuhkan untuk membuat B.

Contoh: `Order` membuat `OrderItem` karena menyimpan kumpulan item tersebut.

```
class Order {
    private List<OrderItem> items = new ArrayList<>();

    public void addProduct(Product product, int quantity) {
        OrderItem item = new OrderItem(product, quantity);
        items.add(item);
    }
}
```

## 3.3 Controller

**Prinsip:** Gunakan objek controller untuk menerima dan mengoordinasikan permintaan dari antarmuka pengguna atau sistem eksternal.

Controller tidak seharusnya memuat seluruh logika bisnis; tugasnya adalah meneruskan proses ke objek domain yang tepat.

```
class CheckoutController {
    public void checkout(Order order, Payment payment) {
        order.processPayment(payment);
    }
}
```

Contoh controller dalam aplikasi web: `OrderController`, `LoginController`, atau `CheckoutController`.

## 3.4 Low Coupling

**Prinsip:** Minimalkan ketergantungan antarclass.

Class yang terlalu bergantung pada banyak class lain akan sulit diubah, diuji, dan digunakan kembali.

Kurang baik:

```
class Order {
    void checkout() {
        Database db = new Database();
        EmailService email = new EmailService();
        PaymentGateway gateway = new PaymentGateway();
        // terlalu banyak ketergantungan langsung
    }
}
```

Lebih baik: gunakan abstraksi atau layanan yang disuntikkan sebagai dependensi.

```
class PaymentService {
    private PaymentGateway gateway;

    PaymentService(PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

## 3.5 High Cohesion

**Prinsip:** Pastikan setiap class memiliki tanggung jawab yang fokus dan saling berkaitan.

Class dengan kohesi tinggi lebih mudah dipahami dan dirawat.

Kurang baik:

```
class UserManager {
    void login() {}
    void generateInvoice() {}
    void calculateTax() {}
    void sendEmail() {}
}
```

Lebih baik:

```
class AuthenticationService {
    void login() {}
}

class InvoiceService {
    void generateInvoice() {}
}

class EmailService {
    void sendEmail() {}
}
```

## 3.6 Polymorphism

**Prinsip:** Gunakan polymorphism ketika perilaku berbeda bergantung pada tipe objek.

Contoh: metode pembayaran berbeda memiliki proses pembayaran sendiri.

```
interface Payment {
    void pay(double amount);
}

class CreditCardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Membayar dengan kartu kredit: " + amount);
    }
}

class EWalletPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Membayar dengan e-wallet: " + amount);
    }
}
```

Dengan pendekatan ini, sistem tidak perlu menggunakan banyak `if-else` untuk membedakan tipe pembayaran.

## 3.7 Pure Fabrication

**Prinsip:** Buat class baru yang tidak mewakili konsep domain secara langsung apabila hal itu membantu menjaga low coupling dan high cohesion.

Contoh: `Order` tidak perlu menangani penyimpanan ke database. Sebuah `OrderRepository` dapat dibuat khusus untuk tugas tersebut.

```
class OrderRepository {
    public void save(Order order) {
        // simpan order ke database
    }
}
```

`OrderRepository` adalah _pure fabrication_ karena bukan objek bisnis utama seperti pesanan atau produk, tetapi berguna untuk desain sistem.

## 3.8 Indirection

**Prinsip:** Gunakan objek perantara untuk mengurangi ketergantungan langsung antara dua komponen.

Contoh: aplikasi tidak berkomunikasi langsung dengan layanan pembayaran eksternal, tetapi melalui `PaymentService`.

```
class PaymentService {
    private PaymentGateway gateway;

    public void process(Payment payment, double amount) {
        gateway.send(payment, amount);
    }
}
```

Jika penyedia pembayaran berubah, perubahan lebih banyak terpusat di `PaymentService`.

## 3.9 Protected Variations

**Prinsip:** Lindungi bagian sistem yang berpotensi berubah dengan menggunakan interface atau abstraksi yang stabil.

Contoh: sistem mendukung beberapa pengiriman.

```
interface ShippingStrategy {
    double calculateCost(double weight);
}

class RegularShipping implements ShippingStrategy {
    public double calculateCost(double weight) {
        return weight * 10000;
    }
}

class ExpressShipping implements ShippingStrategy {
    public double calculateCost(double weight) {
        return weight * 25000;
    }
}
```

Jika muncul metode pengiriman baru, cukup tambahkan class baru tanpa mengubah logika utama secara besar-besaran.

---

# 4. Ringkasan Pola GRASP

|Pola|Fokus Utama|Pertanyaan Panduan|
|---|---|---|
|Information Expert|Memberi tugas pada pemilik data|Siapa yang memiliki informasi yang dibutuhkan?|
|Creator|Menentukan pembuat objek|Siapa yang menyimpan atau menggunakan objek baru?|
|Controller|Menangani input sistem|Siapa yang menerima permintaan dari UI?|
|Low Coupling|Mengurangi ketergantungan|Bagaimana class dapat saling tidak terlalu bergantung?|
|High Cohesion|Menjaga fokus class|Apakah class ini memiliki tugas yang terlalu beragam?|
|Polymorphism|Menangani variasi perilaku|Apakah perilaku berubah menurut tipe objek?|
|Pure Fabrication|Membuat layanan pendukung|Perlukah class bantuan agar desain lebih rapi?|
|Indirection|Menambahkan perantara|Perlukah mediator untuk mengurangi ketergantungan?|
|Protected Variations|Mengantisipasi perubahan|Bagian mana yang kemungkinan akan berubah?|

# 5. Studi Kasus Singkat: Sistem Pemesanan Makanan

Misalkan terdapat fitur pemesanan makanan:
1. Pelanggan memilih menu.
2. Sistem menambahkan menu ke pesanan.
3. Sistem menghitung total.
4. Pelanggan memilih pembayaran.
5. Sistem memproses pembayaran.

Penerapan GRASP:
- `Order` menyimpan dan menghitung total item — **Information Expert**.
- `Order` membuat `OrderItem` — **Creator**.
- `OrderController` menerima permintaan checkout — **Controller**.
- `Payment` menggunakan interface untuk kartu, transfer, dan e-wallet — **Polymorphism**.
- `PaymentService` memisahkan integrasi penyedia pembayaran — **Pure Fabrication** dan **Indirection**.
- Interface `Payment` menjaga sistem dari perubahan jenis pembayaran — **Protected Variations**.
- Setiap class memiliki tugas fokus — **High Cohesion**.
- Class domain tidak terhubung langsung ke database atau layanan eksternal — **Low Coupling**.

# 6. Latihan

1. Dalam sistem perpustakaan, class mana yang sebaiknya menghitung denda keterlambatan: `Member`, `Book`, atau `Loan`? Jelaskan dengan prinsip _Information Expert_.
    
2. Sebuah aplikasi memiliki class `ReportManager` yang membuat laporan, mengirim email, menyimpan data, dan mengatur login. Prinsip GRASP apa yang dilanggar?
    
3. Rancang interface untuk tiga jenis notifikasi: email, SMS, dan push notification. Pola GRASP apa yang paling relevan?
    
4. Dalam sistem e-commerce, apakah `Order` sebaiknya langsung berkomunikasi dengan database? Jelaskan menggunakan konsep _Low Coupling_, _Pure Fabrication_, dan _Indirection_.
    

# 7. Kesimpulan

GRASP membantu menentukan pembagian tanggung jawab yang tepat dalam desain berorientasi objek. Prinsip yang paling sering digunakan bersama adalah:
- Berikan tugas kepada pemilik informasi (**Information Expert**).
- Jaga class tetap fokus (**High Cohesion**).
- Kurangi ketergantungan antarclass (**Low Coupling**).
- Gunakan abstraksi untuk variasi perilaku (**Polymorphism** dan **Protected Variations**).

Dengan GRASP, desain perangkat lunak menjadi lebih fleksibel, mudah dipelihara, dan lebih siap menghadapi perubahan.

---
---
