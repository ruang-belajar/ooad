# Kurikulum Mata Kuliah Object-Oriented Analysis and Design (OOAD)


## **Identitas & Deskripsi Mata Kuliah**

- **Nama Mata Kuliah**: Analisis dan Perancangan Berbasis Objek (_Object-Oriented Analysis and Design_)
- **Jenjang / Bobot**: S1 (Rumpun Ilmu Komputer / Rekayasa Perangkat Lunak) / 3 SKS
- **Prasyarat**: Pemrograman Berorientasi Objek (_Object-Oriented Programming_)
- **Deskripsi Singkat**: Mata kuliah ini membahas metode, teknik, dan formalisme visual dalam menganalisis kebutuhan sistem dan merancang arsitektur perangkat lunak berorientasi objek. Pokok bahasan mencakup evolusi metodologi OOAD (Booch, OMT, OOSE hingga UML 2.x), pemodal visual UML (statis dan dinamis), prinsip alokasi tanggung jawab kelas (GRASP), prinsip desain berpenataan tinggi (SOLID), pola desain (_Design Patterns_), serta konvergensi OOAD modern dalam _Domain-Driven Design_ (DDD) dan _Microservices_.

---

**Capaian Pembelajaran Mata Kuliah (CPMK)**

1. **CPMK-1**: Mampu menganalisis dan membandingkan landasan teoritis paradigma objek serta sejarah evolusi metodologi OOAD.
2. **CPMK-2**: Mampu memodelkan kebutuhan fungsional dan perilaku sistem menggunakan diagram perilaku UML (_Behavioral Diagrams_).
3. **CPMK-3**: Mampu merancang struktur entitas dan domain sistem menggunakan diagram struktural UML (_Structural Diagrams_).
4. **CPMK-4**: Mampu menerapkan prinsip _Responsibility-Driven Design_ (GRASP) dan prinsip kualitas desain (SOLID) dalam mengalokasikan tanggung jawab kelas.
5. **CPMK-5**: Mampu mengimplementasikan pola desain GoF (_Gang of Four_) dan memetakan rancangan objek ke dalam arsitektur fisik serta arsitektur modern (DDD/Microservices).

---

## **Rincian Materi Perkuliahan (14 Pertemuan)**


### **Pertemuan 1: Pengantar Paradigma Objek & SDLC Tradisional vs. OOAD**

- **Pokok Bahasan**: Fondasi paradigma berorientasi objek dan evolusi siklus hidup pengembangan perangkat lunak (SDLC).
- **Sub-Pokok Bahasan**:
    - Krisis perangkat lunak dan keterbatasan pendekatan prosedural terstruktur.
    - Empat pilar utama paradigma objek: **Abstraksi**, **Enkapsulasi**, **Modularitas**, dan **Hirarki** (Inheritansi & Komposisi).
    - Konsep _Object Identity_, _State_, _Behavior_, serta _Static vs. Dynamic Binding_.
    - Perbandingan _Big Design Up Front_ (BDUF) vs. OOAD iteratif-inkremental.
- **Indikator Capaian**: Mahasiswa mampu membedakan pemikiran prosedural dan berorientasi objek serta menjelaskan fungsi pilar-pilar paradigma objek.

---

### **Pertemuan 2: Evolusi Metodologi OOAD & Standardisasi UML**

- **Pokok Bahasan**: Sejarah metodologi pra-UML, _The Three Amigos_, dan kerangka _Rational Unified Process_ (RUP).
- **Sub-Pokok Bahasan**:
    - Metodologi **Booch** (proses Makro/Mikro & notasi awan).
    - Metodologi **Rumbaugh OMT** (Model Objek, Dinamis, dan Fungsional/DFD).
    - Metodologi **Jacobson OOSE** (Pendekatan _Use Case Driven_ & _Objectory_).
    - Unifikasi notasi oleh OMG menjadi **UML 2.x**.
    - Empat fase RUP: _Inception_, _Elaboration_, _Construction_, dan _Transition_.
- **Indikator Capaian**: Mahasiswa memahami asal-usul notasi UML dan tahapan analisis-desain dalam proses RUP.

---

### **Pertemuan 3: Elicitation Kebutuhan Sistem & Pemodelan Use Case**

- **Pokok Bahasan**: Penangkapan kebutuhan fungsional sistem dengan _Use Case Diagram_ dan spesifikasi skenario.
- **Sub-Pokok Bahasan**:
    - Identifikasi _Actor_, _System Boundary_, dan _Use Case_.
    - Relasi antar _Use Case_: `<<include>>`, `<<extend>>`, dan _Generalization_.
    - Penulisan teks spesifikasi _Use Case_ (skenario utama dan alternatif).
- **Sintaks PlantUML**:

```
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Mahasiswa" as student
actor "Dosen Wali" as lecturer

rectangle "Sistem Informasi Akademik" {
  usecase "Pengisian KRS" as UC_KRS
  usecase "Validasi Prasyarat" as UC_Valid
  usecase "Persetujuan KRS" as UC_Approve
  usecase "Pembayaran UKT" as UC_Pay
}

student --> UC_KRS
UC_KRS .> UC_Valid : <<include>>
UC_KRS ..> UC_Pay : <<extend>>
lecturer --> UC_Approve
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu menyusun diagram _Use Case_ dan dokumen spesifikasinya berdasarkan kasus nyata.

---

### **Pertemuan 4: Pemodelan Alur Kerja Bisnis: Activity Diagram**

- **Pokok Bahasan**: Pemodelan alur kerja (_workflow_) tingkat bisnis dan eksplorasi skenario _Use Case_.
- **Sub-Pokok Bahasan**:
    - Elemen _Activity Diagram_: _Action State_, _Control Nodes_ (_Decision_, _Merge_, _Fork_, _Join_).
    - Pengelompokan tanggung jawab eksekusi menggunakan _Swimlanes_ (_Partition_).
    - Pemodelan eksekusi paralel (_Concurrency_) dan aliran objek (_Object Flow_).
- **Sintaks PlantUML**:

```
@startuml
|Mahasiswa|
start
:Mengisi Formulir KRS;
:Submit KRS;

|Sistem Akademik|
if (Cek Kuota & Prasyarat?) then (Lolos)
  :Ubah Status KRS ke 'Pending';
  |Dosen Wali|
  :Review Draf KRS;
  if (Setuju?) then (Ya)
    :Approve KRS;
    |Sistem Akademik|
    :Cetak KSM;
  else (Tidak)
    :Beri Catatan Revisi;
  endif
else (Gagal)
  |Sistem Akademik|
  :Tampilkan Pesan Error;
endif
stop
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu memodelkan proses bisnis kompleks yang melibatkan banyak peran secara sinkron/asinkron.

---

### **Pertemuan 5: Analisis Ranah Domain: Class Diagram Lapis Analisis & Konsep Relasi**

- **Pokok Bahasan**: Pemodelan struktur konseptual/domain dan spesifikasi hubungan antar kelas.
- **Sub-Pokok Bahasan**:
    - Identifikasi _Conceptual Classes_ dari domain permasalahan.
    - Anotasi kelas: _Attributes_, _Operations_, dan _Visibility_ (`+`, `-`, `#`).
    - Jenis relasi: **Association** (dan kardinalitas/kederajatan), **Aggregation** (_shared part-whole_), **Composition** (_strict part-whole lifecycle_), dan **Generalization**.
    - Penggunaan _Association Class_.
- **Sintaks PlantUML**:

```
@startuml
class Mahasiswa {
  - String nim
  - String nama
  + mendaftarKrs(): void
}

class KRS {
  - String semester
  - Date tanggalPengisian
  + hitungTotalSKS(): int
}

class MataKuliah {
  - String kodeMK
  - String namaMK
  - int sks
}

Mahasiswa "1" -- "1" KRS : memiliki >
KRS "1" *-- "1..*" DetailKRS : terdiri dari >
DetailKRS "*" -- "1" MataKuliah : mengacu pada >
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu merancang diagram kelas konseptual dengan hubungan agregasi, komposisi, dan kardinalitas yang tepat.

---

### **Pertemuan 6: Pemodelan Interaksi Dinamis: Sequence & Communication Diagram**

- **Pokok Bahasan**: Visualisasi pertukaran pesan antar objek (_message passing_) secara kronologis.
- **Sub-Pokok Bahasan**:
    - Elemen _Sequence Diagram_: _Lifeline_, _Activation Bar_, _Synchronous/Asynchronous Messages_, _Return Value_, dan _Self-call_.
    - Penggunaan _Combined Fragments_ (`alt`, `loop`, `opt`, `par`).
    - _Communication Diagram_ (fokus pada kolaborasi struktural) vs. _Sequence Diagram_ (fokus pada urutan waktu).
    - Pemetaan _Use Case Realization_ ke interaksi objek.
- **Sintaks PlantUML**:

```
@startuml
actor Mahasiswa
participant "UI: FormKRS" as UI
participant "Controller: KRSController" as Ctrl
participant "Domain: KRS" as KrsObj

Mahasiswa -> UI : clickSubmit()
activate UI
UI -> Ctrl : simpanKRS(nim, listMK)
activate Ctrl
Ctrl -> KrsObj : create(nim)
activate KrsObj
loop tiap MataKuliah
  Ctrl -> KrsObj : addMataKuliah(mk)
end
KrsObj --> Ctrl : statusValid
deactivate KrsObj
Ctrl --> UI : krsBerhasilDisimpan()
deactivate Ctrl
UI --> Mahasiswa : tampilkanKonfirmasi()
deactivate UI
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu membuat diagram sekuens untuk merealisasikan skenario kasus penggunaan.

---

### **Pertemuan 7: Pemodelan Siklus Hidup & Status Objek: State Machine Diagram**

- **Pokok Bahasan**: Pemodelan perubahan status dinamis suatu objek reaktif sepanjang siklus hidupnya.
- **Sub-Pokok Bahasan**:
    - Konsep _State_, _Event_, _Transition_, _Guard Condition_, dan _Action_ (`entry`, `do`, `exit`).
    - _Composite States_ / _Superstates_ dan _Concurrent Substates_.
    - Identifikasi kelas-kelas yang memiliki perilaku dinamis kompleks (_state-dependent behavior_).
- **Sintaks PlantUML**:

```
@startuml
[*] -> Draft : buatKRS()

state Draft {
  Draft : entry / setTanggalBuat()
}

Draft -> Diajukan : submitKRS() [SKS <= 24]
Diajukan -> Disetujui : approve(DosenWali)
Diajukan -> Ditolak : reject(DosenWali)

Ditolak -> Draft : revisiKRS()
Disetujui -> Terkunci : kunciKRS()

Terkunci -> [*]
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu menganalisis dan menggambarkan perubahan _state_ entitas kompleks dalam sistem.

---

### **Pertemuan 8: Evaluasi Tengah Semester (UTS) / Review Sintesis Analisis Sistem**

- **Focus**: Ujian Evaluasi Capaian Pemodelan Analisis (Pertemuan 1–7) & Transisi dari OOA (_Analysis_) ke OOD (_Design_).
- **Pembahasan**:
    - Evaluasi integrasi diagram UML (_Use Case_, _Activity_, _Class_, _Sequence_, _Statechart_).
    - Arsitektur Berlapis (_Layered Architecture_): Pemisahan _Presentation Layer_ (UI), _Domain Layer_ (Business Logic), dan _Technical Services/Access Layer_ (DB/Infrastructure).

---

**Pertemuan 9: Prinsip Alokasi Tanggung Jawab I: GRASP Fundamentals**

- **Pokok Bahasan**: Penerapan prinsip dasar _General Responsibility Assignment Software Patterns_ (GRASP) karangan Craig Larman.
- **Sub-Pokok Bahasan**:
    - Konsep _Responsibility-Driven Design_ (RDD): _Doing_ vs. _Knowing Responsibilities_.
    - **Information Expert**: Mengalokasikan tanggung jawab pada kelas yang memiliki informasi.
    - **Creator**: Aturan menentukan kelas yang bertanggung jawab membuat instansiasi objek.
    - **Controller**: Mengisolasi lapisan UI dari logika domain menggunakan non-UI _facade_ / _use-case controller_.
    - Evaluatif **Low Coupling** & **High Cohesion**: Menjaga ketergantungan minimal dan fokus tugas kelas.
- **Indikator Capaian**: Mahasiswa mampu menetapkan tanggung jawab metode pada kelas yang tepat berdasarkan 5 prinsip dasar GRASP.

---

### **Pertemuan 10: Prinsip Alokasi Tanggung Jawab II: Advanced GRASP & Arsitektur Mikro**

- **Pokok Bahasan**: Penggunaan pola GRASP tingkat lanjut untuk menangani variasi dan kompleksitas struktur.
- **Sub-Pokok Bahasan**:
    - **Polymorphism**: Mengubah kondisi percabangan `if-else`/`switch` menjadi struktur polimorfi.
    - **Pure Fabrication**: Membuat kelas buatan/layanan (non-domain) demi mempertahankan _High Cohesion_ dan _Low Coupling_ (misal: _Repository_, _Service_).
    - **Indirection**: Memasang objek perantara untuk memutus ketergantungan langsung antar komponen.
    - **Protected Variations**: Membungkus titik variasi/instabilitas di balik antarmuka (_interface_) yang stabil.
- **Indikator Capaian**: Mahasiswa mampu menyelesaikan persoalan keterikatan kelas (_coupling_) tinggi menggunakan pola GRASP tingkat lanjut.

---

### **Pertemuan 11: Desain Berkelanjutan & Maintainability: Prinsip SOLID**

- **Pokok Bahasan**: Pendalaman 5 prinsip _micro-architecture_ SOLID dan korelasi/saling melengkapi dengan pola GRASP.
- **Sub-Pokok Bahasan**:
    - **Single Responsibility Principle (SRP)** vs. GRASP _High Cohesion_.
    - **Open/Closed Principle (OCP)** vs. GRASP _Protected Variations_.
    - **Liskov Substitution Principle (LSP)** & _Subtype Contract Constraints_.
    - **Interface Segregation Principle (ISP)** & Antarmuka Spesifik Klien.
    - **Dependency Inversion Principle (DIP)**: Modul tingkat tinggi bergantung pada abstraksi, bukan implementasi konkrit.
- **Indikator Capaian**: Mahasiswa mampu menganalisis _code smell_ dan mengevaluasi rancangan kelas agar memenuhi tolok ukur SOLID.

---

### **Pertemuan 12: Pattern-Oriented Software Design: Gang of Four (GoF) Design Patterns**

- **Pokok Bahasan**: Penerapan pola desain standar GoF (_Gang of Four_) sebagai implementasi konkrit prinsip GRASP & SOLID.
- **Sub-Pokok Bahasan**:
    - **Creational Patterns**: _Factory Method_, _Abstract Factory_, dan _Singleton_.
    - **Structural Patterns**: _Adapter_ (implementasi _Indirection_), _Facade_ (implementasi _Controller_), dan _Decorator_.
    - **Behavioral Patterns**: _Strategy_ (implementasi _Polymorphism/Protected Variations_), _Observer_, dan _Command_.
    - Sintesis hubungan antara GoF, GRASP, dan _Frameworks_.
- **Indikator Capaian**: Mahasiswa mampu memilih dan menerapkan pola GoF yang sesuai untuk menangani masalah perancangan berulang.

---

### **Pertemuan 13: Pemodelan Arsitektur Fisik & Komponen: Component & Deployment Diagram**

- **Pokok Bahasan**: Pemodelan arsitektur fisik perangkat lunak, komponen eksekusi, dan topologi penyebaran (_deployment_).
- **Sub-Pokok Bahasan**:
    - **Package Diagram**: Pengelompokan modul/subsistem terorganisir.
    - **Component Diagram**: Visualisasi komponen modul eksekusi, pustaka (_DLL/JAR_), dan _provided/required interfaces_.
    - **Deployment Diagram**: Visualisasi _hardware nodes_, jaringan, dan pembagian runtime.
- **Sintaks PlantUML**:

```
@startuml
node "Client Device" {
  [Web Browser] as browser
}

node "Application Server" {
  package "Web App Context" {
    [UI Component] as ui
    [Academic Service] as service
  }
}

node "Database Server" {
  database "PostgreSQL" as db
}

browser -- ui : HTTPS
ui ..> service : uses
service -- db : JDBC / SQL
@enduml
```

- **Indikator Capaian**: Mahasiswa mampu memodelkan pemetaan perangkat lunak logika ke dalam infrastruktur fisik dan arsitektur runtime.

---

### **Pertemuan 14: Konvergensi OOAD Modern: Domain-Driven Design (DDD), Microservices & Refactoring**

- **Pokok Bahasan**: Tren modern arsitektur berorientasi objek dalam ekosistem _Cloud-Native_, _Microservices_, dan _Test-Driven Development_ (TDD).
- **Sub-Pokok Bahasan**:
    - Konsep **Domain-Driven Design (DDD)**: _Strategic Design_ (_Bounded Context_, _Ubiquitous Language_) dan _Tactical Design_ (_Entities_, _Value Objects_, _Aggregates_, _Repositories_).
    - Penerapan paradigma objek pada **Microservice Architecture** vs. Monolitik.
    - Integrasi OOAD dengan **Test-Driven Development (TDD)** dan _Refactoring_ berkelanjutan untuk mencegah pembusukan arsitektur (_design decay_).
- **Indikator Capaian**: Mahasiswa memahami relevansi OOAD klasik dalam praktik arsitektur lunak modern berbasis skrip, microservices, dan DDD.

---

**Metode Penilaian & Evaluasi Pembelajaran**

|Komponen Penilaian|Bobot|Deskripsi|
|---|---|---|
|**Tugas Mandiri & Kuis**|15%|Kuis pemahaman konsep (GRASP/SOLID/UML) dan latihan membuat sintaks PlantUML.|
|**Tugas Besar (Project Kelompok)**|25%|Proyek analisis & perancangan sistem nyata dari _Inception_ hingga _Design Class Diagram_ dan komponen.|
|**Ujian Tengah Semester (UTS)**|30%|Evaluasi pemahaman OOA, _Use Case_, _Class Diagram_, dan pemodelan dinamis (Pertemuan 1–7).|
|**Ujian Akhir Semester (UAS)**|30%|Evaluasi OOD, GRASP, SOLID, GoF Patterns, dan Arsitektur Fisik/Modern (Pertemuan 9–14).|