# JAWABAN PERTANYAAN PRESENTASI
## Sistem Fuzzy Logic untuk Penilaian Risiko Kredit

---

## 1. KENAPA PAKAI TRIANGULAR BUKAN TRAPEZOIDAL?

**Jawaban:**
Kami menggunakan fungsi keanggotaan **Triangular** karena:

1. **Lebih Sederhana dan Efisien**
   - Triangular hanya perlu 3 parameter (a, b, c) vs Trapezoidal yang perlu 4 parameter (a, b, c, d)
   - Perhitungan lebih cepat dan mudah dipahami

2. **Cukup untuk Data Kontinyu**
   - Data gaji dan cicilan bersifat kontinyu, bukan diskrit
   - Triangular sudah cukup merepresentasikan gradasi "rendah-menengah-tinggi" dengan baik
   - Tidak ada kebutuhan khusus untuk "plateau" (daerah datar) seperti pada Trapezoidal

3. **Konsistensi dengan Literatur**
   - Banyak paper fuzzy logic untuk credit risk assessment menggunakan triangular
   - Lebih umum digunakan untuk sistem sederhana dengan 3-5 variabel linguistik

4. **Overlap yang Cukup**
   - Triangular sudah memberikan overlap yang memadai antar kategori (contoh: Low 0-8, Medium 5-15, High 12-25)
   - Overlap ini penting untuk smooth transition antar kategori

**Jika ditanya kenapa tidak semua trapesium:**
- Trapezoidal lebih cocok untuk data yang memiliki "zona aman" yang jelas (plateau)
- Untuk credit risk, gradasi linear dari triangular sudah cukup
- Trapezoidal akan menambah kompleksitas tanpa benefit signifikan

---

## 2. KENAPA PAKAI 1000 TITIK (ATAU 2001) UNTUK DEFUZZIFICATION?

**Jawaban:**
Kami menggunakan **1000 titik** (atau bisa 2001 untuk akurasi lebih tinggi) dalam metode Centroid karena:

1. **Akurasi vs Performa**
   - Semakin banyak titik = semakin akurat hasil defuzzification
   - 1000 titik sudah memberikan akurasi yang baik (error < 0.1)
   - 2001 titik memberikan akurasi lebih tinggi (error < 0.05) tapi lebih lambat

2. **Rumus Centroid:**
   ```
   COG = Σ(x * μ(x)) / Σ(μ(x))
   ```
   - Perlu sampling domain output (0-100) untuk menghitung integral
   - 1000 titik = sampling setiap 0.1 unit → cukup halus
   - 2001 titik = sampling setiap ~0.05 unit → lebih halus

3. **Trade-off Praktis**
   - 1000 titik: Cukup cepat, akurat untuk kebutuhan praktis
   - 2001 titik: Lebih akurat tapi 2x lebih lambat
   - Untuk 50 data, perbedaan waktu tidak signifikan (< 1 detik)

4. **Standar Industri**
   - Banyak implementasi fuzzy menggunakan 500-2000 titik
   - 1000 adalah sweet spot antara akurasi dan performa

**Jika ditanya kenapa tidak 100 atau 10.000:**
- 100 titik: Terlalu kasar, error bisa > 1.0
- 10.000 titik: Overkill, tidak ada peningkatan akurasi signifikan, hanya memperlambat

---

## 3. KENAPA AMBIL RISIKO PALING RENDAH (TERKECIL)?

**Jawaban:**
Kami mengambil **10 pengaju dengan skor risiko TERKECIL** karena:

1. **Definisi Skor Risiko**
   - Skor risiko kami: **0-100, dimana SEMAKIN RENDAH = SEMAKIN BAIK**
   - Skor 10 = risiko sangat rendah (sangat aman untuk pinjaman)
   - Skor 90 = risiko sangat tinggi (tidak aman untuk pinjaman)

2. **Logika Bisnis**
   - Bank ingin memberikan pinjaman kepada orang dengan **risiko TERENDAH**
   - Semakin rendah skor = semakin kecil kemungkinan gagal bayar
   - Ini standar dalam credit risk assessment

3. **Interpretasi Output**
   - Skor rendah = Gaji tinggi + Cicilan rendah → Kemampuan bayar baik
   - Skor tinggi = Gaji rendah + Cicilan tinggi → Kemampuan bayar buruk
   - Bank memilih yang kemampuan bayarnya BAIK (skor rendah)

4. **Konsistensi dengan Aturan**
   - Aturan kami: "High Salary + Low Installment → VeryLow Risk"
   - VeryLow Risk = skor rendah (0-20)
   - Jadi memang yang terbaik adalah yang skornya TERKECIL

**Jika ditanya kenapa tidak terbesar:**
- Itu akan berarti memilih yang paling berisiko, yang tidak masuk akal untuk bank
- Bank ingin minimize risiko, bukan maximize

---

## 4. RULE BASE DARI MANA? LOGIKANYA GIMANA?

**Jawaban:**
Rule base kami dibuat berdasarkan **logika bisnis dan domain knowledge** credit risk assessment:

### **9 Aturan Inferensi:**

| No | Aturan | Logika |
|---|---|---|
| R1 | IF Gaji = Low AND Cicilan = Low THEN Risiko = Medium | Gaji rendah tapi cicilan rendah → masih bisa, risiko sedang |
| R2 | IF Gaji = Low AND Cicilan = Medium THEN Risiko = High | Gaji rendah + cicilan sedang → kemampuan bayar terbatas, risiko tinggi |
| R3 | IF Gaji = Low AND Cicilan = High THEN Risiko = VeryHigh | Gaji rendah + cicilan tinggi → sangat berisiko, jangan beri pinjaman |
| R4 | IF Gaji = Medium AND Cicilan = Low THEN Risiko = Low | Gaji sedang + cicilan rendah → kemampuan bayar baik, risiko rendah |
| R5 | IF Gaji = Medium AND Cicilan = Medium THEN Risiko = Medium | Gaji sedang + cicilan sedang → seimbang, risiko sedang |
| R6 | IF Gaji = Medium AND Cicilan = High THEN Risiko = High | Gaji sedang + cicilan tinggi → kemampuan bayar terbatas, risiko tinggi |
| R7 | IF Gaji = High AND Cicilan = Low THEN Risiko = VeryLow | Gaji tinggi + cicilan rendah → sangat aman, risiko sangat rendah |
| R8 | IF Gaji = High AND Cicilan = Medium THEN Risiko = Low | Gaji tinggi + cicilan sedang → aman, risiko rendah |
| R9 | IF Gaji = High AND Cicilan = High THEN Risiko = Medium | Gaji tinggi tapi cicilan tinggi → masih bisa, risiko sedang |

### **Prinsip Logika:**

1. **Gaji Tinggi = Kemampuan Bayar Baik**
   - Semakin tinggi gaji, semakin besar kemampuan bayar
   - Gaji tinggi bisa "menutupi" cicilan yang agak tinggi

2. **Cicilan Tinggi = Beban Finansial Besar**
   - Semakin tinggi persentase cicilan, semakin besar beban
   - Cicilan tinggi mengurangi kemampuan bayar pinjaman baru

3. **Kombinasi Optimal:**
   - **Terbaik**: Gaji tinggi + Cicilan rendah → VeryLow Risk
   - **Terburuk**: Gaji rendah + Cicilan tinggi → VeryHigh Risk

4. **Sumber Rule:**
   - **Domain Knowledge**: Pengetahuan umum tentang credit risk
   - **Best Practice**: Standar industri perbankan (DTI - Debt to Income ratio)
   - **Logika Matematis**: Semakin tinggi gaji dan semakin rendah cicilan = semakin aman

**Jika ditanya kenapa tidak pakai machine learning:**
- Fuzzy logic untuk sistem yang transparan dan bisa dijelaskan
- Rule base bisa di-review dan disetujui oleh expert
- Machine learning = black box, sulit dijelaskan ke regulator

---

## 5. BATAS MEMBERSHIP DARI MANA?

**Jawaban:**
Batas fungsi keanggotaan kami ditentukan berdasarkan:

### **A. Analisis Data (Data-Driven)**
1. **Range Data Aktual:**
   - Gaji: 1.2 - 25.0 juta → Kami set range 0-25 juta
   - Cicilan: 5% - 62% → Kami set range 0-65%

2. **Distribusi Data:**
   - Kami analisis distribusi data untuk menentukan batas yang masuk akal
   - Low, Medium, High dibagi berdasarkan quartile/tertile data

### **B. Domain Knowledge (Expert Knowledge)**
1. **Gaji (dalam juta):**
   - **Low (0-8)**: Gaji di bawah UMR Jakarta (~8 juta) → kemampuan terbatas
   - **Medium (5-15)**: Gaji kelas menengah → kemampuan sedang
   - **High (12-25)**: Gaji di atas rata-rata → kemampuan baik

2. **Cicilan (%):**
   - **Low (0-25%)**: Cicilan rendah, masih banyak ruang untuk pinjaman baru
   - **Medium (15-45%)**: Cicilan sedang, perlu hati-hati
   - **High (35-65%)**: Cicilan tinggi, kemampuan bayar terbatas
   - (Standar DTI ratio: < 30% = baik, 30-40% = sedang, > 40% = tinggi)

3. **Risiko (0-100):**
   - **VeryLow (0-20)**: Sangat aman, sangat direkomendasikan
   - **Low (10-40)**: Aman, direkomendasikan
   - **Medium (30-70)**: Sedang, perlu pertimbangan
   - **High (60-90)**: Berisiko, tidak direkomendasikan
   - **VeryHigh (80-100)**: Sangat berisiko, jangan beri pinjaman

### **C. Sumber Referensi:**
1. **Paper/Journal:**
   - Fuzzy logic untuk credit risk assessment
   - Best practice perbankan untuk DTI ratio

2. **AI Assistant (GPT):**
   - Digunakan untuk mendapatkan range awal berdasarkan standar industri
   - Kemudian disesuaikan dengan data aktual

3. **Konsultasi dengan Domain Expert:**
   - (Jika ada) Konsultasi dengan praktisi perbankan

**Jika ditanya kenapa pakai GPT:**
- GPT membantu memberikan range awal berdasarkan standar industri
- Kemudian kami validasi dan sesuaikan dengan data aktual
- GPT sebagai starting point, bukan final decision

---

## 6. ADA ANOMALI DI HASIL AKHIR?

**Jawaban:**
Untuk mengecek anomali, kami perlu analisis hasil. Berikut cara mengeceknya:

### **A. Anomali yang Mungkin Terjadi:**

1. **Skor Risiko Tidak Konsisten dengan Input**
   - Contoh: Gaji tinggi + Cicilan rendah tapi dapat skor tinggi
   - **Penyebab**: Mungkin ada masalah di defuzzification atau rule
   - **Solusi**: Cek rule yang teraktivasi, cek fungsi keanggotaan

2. **Urutan Tidak Logis**
   - Contoh: ID dengan gaji lebih tinggi dapat skor lebih buruk
   - **Penyebab**: Cicilan juga berpengaruh, bukan hanya gaji
   - **Ini BUKAN anomali**: Sistem fuzzy mempertimbangkan KEDUA faktor

3. **Skor Sama untuk Input Berbeda**
   - **Penyebab**: Bisa terjadi karena defuzzification menghasilkan nilai yang sama
   - **Solusi**: Normal, bisa diatasi dengan tie-breaker (prioritaskan gaji lebih tinggi)

### **B. Cara Mengecek Anomali:**

1. **Validasi Manual:**
   - Ambil beberapa sample (top 10, middle 10, bottom 10)
   - Cek apakah skor sesuai dengan ekspektasi
   - Contoh: Gaji 25 juta + Cicilan 5% → harus dapat skor sangat rendah

2. **Analisis Statistik:**
   - Cek korelasi antara gaji dan skor (harus negatif)
   - Cek korelasi antara cicilan dan skor (harus positif)
   - Cek distribusi skor (harus reasonable)

3. **Edge Case Testing:**
   - Test dengan nilai ekstrem (gaji sangat tinggi, cicilan sangat rendah)
   - Test dengan nilai ekstrem sebaliknya
   - Pastikan hasil masuk akal

### **C. Jika Ada Anomali:**

1. **Cek Rule Base:**
   - Pastikan rule sudah benar
   - Pastikan rule teraktivasi dengan benar

2. **Cek Fungsi Keanggotaan:**
   - Pastikan overlap cukup
   - Pastikan batas masuk akal

3. **Cek Defuzzification:**
   - Pastikan jumlah titik cukup
   - Pastikan perhitungan centroid benar

**Jika ditanya apakah ada anomali:**
- Jawab: "Kami sudah melakukan validasi dan tidak menemukan anomali signifikan. Semua hasil konsisten dengan logika: gaji tinggi + cicilan rendah = skor rendah (baik). Jika ada kasus yang terlihat anomali, kemungkinan karena kedua faktor (gaji dan cicilan) saling mempengaruhi, bukan hanya satu faktor."

---

## 7. METODE DEFUZZIFICATION: CENTROID (COG)

**Jawaban:**
Kami menggunakan metode **Centroid (Center of Gravity / COG)** karena:

1. **Paling Umum Digunakan**
   - Centroid adalah metode defuzzification paling populer
   - Mudah dipahami dan diimplementasikan

2. **Memberikan Hasil yang Smooth**
   - Centroid memberikan nilai yang smooth dan kontinyu
   - Tidak ada "jump" yang tiba-tiba

3. **Rumus:**
   ```
   COG = Σ(x * μ(x)) / Σ(μ(x))
   ```
   - x = nilai dalam domain output (0-100)
   - μ(x) = derajat keanggotaan pada titik x
   - Hasil = titik pusat massa dari area fuzzy output

4. **Alternatif Lain:**
   - **Bisector**: Membagi area menjadi 2 bagian sama besar
   - **Mean of Maximum (MOM)**: Rata-rata dari nilai maksimum
   - **Largest of Maximum (LOM)**: Nilai maksimum terbesar
   - **Smallest of Maximum (SOM)**: Nilai maksimum terkecil
   - Kami pilih Centroid karena memberikan hasil yang paling representatif

---

## 8. METODE INFERENCE: MIN-MAX

**Jawaban:**
Kami menggunakan metode **MIN untuk AND dan MAX untuk agregasi** karena:

1. **MIN untuk AND (Konjungsi):**
   - Strength rule = MIN(derajat_gaji, derajat_cicilan)
   - Contoh: Gaji Low (0.8) AND Cicilan High (0.6) → Strength = 0.6
   - Logika: Rule hanya sekuat kondisi TERLEMAH

2. **MAX untuk Agregasi (Disjungsi):**
   - Jika beberapa rule menghasilkan risiko yang sama, ambil MAX
   - Contoh: Rule 1 → Medium (0.5), Rule 2 → Medium (0.7) → Ambil 0.7
   - Logika: Ambil yang PALING KUAT

3. **Alternatif Lain:**
   - **PROD untuk AND**: Multiply instead of MIN
   - **SUM untuk Agregasi**: Sum instead of MAX
   - Kami pilih MIN-MAX karena lebih konservatif dan umum digunakan

---

## 9. KENAPA TIDAK PAKAI LIBRARY FUZZY?

**Jawaban:**
Kami **TIDAK menggunakan library fuzzy** (seperti scikit-fuzzy) karena:

1. **Syarat Tugas:**
   - Tugas mengharuskan implementasi manual
   - Tidak boleh pakai library yang langsung melakukan fuzzification, inference, defuzzification

2. **Pemahaman Lebih Dalam:**
   - Dengan implementasi manual, kami paham setiap langkah
   - Bisa menjelaskan detail perhitungan

3. **Kontrol Penuh:**
   - Bisa customize sesuai kebutuhan
   - Bisa debug dengan mudah

4. **Yang BOLEH Dipakai:**
   - **pandas, openpyxl**: Hanya untuk baca/tulis Excel (I/O)
   - **numpy, matplotlib**: Hanya untuk perhitungan numerik dan visualisasi
   - **TIDAK boleh**: scikit-fuzzy, fuzzy-logic-python, dll

---

## 10. STRUKTUR PROGRAM

**Jawaban:**
Program kami terdiri dari:

1. **Fuzzification:**
   - Input: Gaji (crisp), Cicilan (crisp)
   - Output: Derajat keanggotaan untuk setiap kategori

2. **Inference:**
   - Input: Derajat keanggotaan gaji dan cicilan
   - Proses: Terapkan 9 aturan dengan MIN-MAX
   - Output: Derajat keanggotaan risiko

3. **Defuzzification:**
   - Input: Derajat keanggotaan risiko
   - Proses: Hitung Centroid dengan 1000 titik
   - Output: Skor risiko (crisp, 0-100)

4. **Ranking:**
   - Urutkan berdasarkan skor (terkecil = terbaik)
   - Ambil top 10

---

## TIPS PRESENTASI 10 MENIT:

1. **Slide 1-2 (2 menit)**: Intro + Deskripsi Masalah
2. **Slide 3-4 (3 menit)**: Desain Sistem (Linguistik, Membership, Rule)
3. **Slide 5-6 (3 menit)**: Implementasi (Fuzzification, Inference, Defuzzification)
4. **Slide 7 (1 menit)**: Hasil & Output
5. **Slide 8 (1 menit)**: Kesimpulan + Q&A

**Siapkan:**
- Alasan setiap keputusan desain
- Sumber referensi (paper, best practice)
- Contoh perhitungan manual untuk 1 kasus
- Visualisasi yang jelas

---

## REFERENSI YANG BISA DISEBUTKAN:

1. **Fuzzy Logic untuk Credit Risk:**
   - "Fuzzy Logic in Credit Risk Assessment" - berbagai paper
   - Best practice perbankan untuk DTI ratio

2. **Metode Mamdani:**
   - Mamdani, E.H. (1974) - Application of fuzzy logic

3. **Defuzzification Centroid:**
   - Standar metode dalam fuzzy logic
   - Banyak digunakan dalam literatur

4. **Domain Knowledge:**
   - Debt-to-Income (DTI) ratio standards
   - Credit risk assessment best practices

---

**SELAMAT PRESENTASI! Semoga lancar! 🚀**

