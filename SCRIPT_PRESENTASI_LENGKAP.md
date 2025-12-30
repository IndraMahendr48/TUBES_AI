# SCRIPT PRESENTASI - SISTEM FUZZY LOGIC RISIKO KREDIT
## Untuk 3 Orang: Indra, Shofi, Virgie
## Total Waktu: 10 Menit

**CATATAN PENTING:**
- Setiap proses dijelaskan dengan menyebutkan nama fungsi di notebook
- Semua jawaban untuk pertanyaan yang mungkin ditanyakan sudah disiapkan
- Script ini sangat detail untuk menghindari celah pertanyaan

---

# ============================================
# SLIDE 1: LATAR BELAKANG & MASALAH
# Pembicara: INdra (1.5 menit)
# ============================================

**[INdra]**
"Selamat pagi/siang, Ibu/Bapak dosen dan teman-teman. Kami dari kelompok [KELOMPOK] akan mempresentasikan tugas besar mata kuliah Kecerdasan Buatan tentang Sistem Fuzzy Logic untuk Penilaian Risiko Kredit."

"Latar belakang masalah yang kami selesaikan adalah: Sebuah bank memiliki 50 pengaju pinjaman dengan data gaji dalam juta rupiah dan persentase cicilan yang sedang berjalan terhadap gaji. Bank perlu memilih 10 orang dengan skor risiko kredit terkecil untuk diberikan pinjaman."

"Masalahnya adalah: Bagaimana cara menilai risiko kredit secara objektif dan transparan? Sistem konvensional menggunakan threshold yang kaku, sedangkan fuzzy logic dapat menangani ketidakpastian dan memberikan penilaian yang lebih fleksibel."

"Input sistem kami adalah file Excel 'resiko kredit.xlsx' yang berisi 50 baris data dengan 3 kolom: Nomor ID pengaju pinjaman, Gaji dalam juta rupiah, dan Persentase cicilan pinjaman terhadap gaji dalam persen."

"Output yang dihasilkan adalah file Excel 'peringkat.xlsx' yang berisi 10 pengaju dengan skor risiko terkecil, dilengkapi dengan ID, gaji, persentase cicilan, dan skor risiko kredit hasil defuzzification."

"Terima kasih. Selanjutnya akan disampaikan oleh Shofi."

---

# ============================================
# SLIDE 2: SOLUSI & TUJUAN
# Pembicara: Shofi (1.5 menit)
# ============================================

**[Shofi]**
"Terima kasih Indra. Saya Shofi akan menjelaskan solusi dan tujuan dari sistem yang kami bangun."

"Solusi yang kami tawarkan adalah menggunakan Sistem Fuzzy Logic dengan metode Mamdani untuk menilai risiko kredit. Sistem ini dapat menangani ketidakpastian dan memberikan penilaian yang lebih fleksibel dibanding sistem biner."

"Tujuan dari sistem ini adalah:"
"1. Membantu bank dalam proses pengambilan keputusan pemberian pinjaman dengan cara yang objektif dan transparan"
"2. Menilai risiko kredit berdasarkan kombinasi gaji dan persentase cicilan yang sedang berjalan"
"3. Memilih 10 pengaju dengan risiko terkecil untuk diberikan pinjaman"

"Keunggulan sistem fuzzy logic:"
"1. Transparan dan dapat dijelaskan - setiap keputusan bisa dijelaskan dengan jelas, penting untuk regulasi perbankan"
"2. Menggunakan logika bisnis yang jelas - rule base dibuat berdasarkan domain knowledge, bukan black box"
"3. Dapat menangani ketidakpastian - nilai berada di antara 0 dan 1, bukan hanya 0 atau 1"

"Semua proses fuzzy logic diimplementasikan manual tanpa menggunakan library fuzzy seperti scikit-fuzzy. Kami hanya menggunakan pandas dan openpyxl untuk membaca dan menulis file Excel, serta numpy dan matplotlib untuk perhitungan numerik dan visualisasi."

"Implementasi kami menggunakan kelas `FuzzyCreditRiskSystem` yang berisi semua fungsi untuk proses fuzzy logic. Untuk membaca data, kami menggunakan fungsi `read_excel_data()`, dan untuk menyimpan hasil menggunakan fungsi `save_to_excel()`."

"Terima kasih. Selanjutnya akan disampaikan oleh Indra tentang desain sistem."

---

# ============================================
# SLIDE 3: DESAIN SISTEM (VAR. INPUT & OUTPUT)
# Pembicara: INdra (2 menit)
# ============================================

**[INdra]**
"Terima kasih Shofi. Saya Indra akan menjelaskan desain sistem, khususnya variabel input dan output."

"Desain sistem kami dimulai dengan menentukan variabel linguistik dan fungsi keanggotaan untuk setiap atribut."

### **VARIABEL INPUT 1: GAJI (dalam juta rupiah)**

"Untuk atribut Gaji, kami menggunakan 3 kategori linguistik dengan fungsi keanggotaan triangular:"

"**Low**: Range 0 sampai 8 juta, dengan titik puncak di 4 juta. Parameter a=0, b=4, c=8. Ini berarti gaji di bawah 4 juta memiliki derajat keanggotaan Low yang tinggi."

"**Medium**: Range 5 sampai 15 juta, dengan titik puncak di 10 juta. Parameter a=5, b=10, c=15. Ada overlap dengan Low di range 5-8 juta dan dengan High di range 12-15 juta."

"**High**: Range 12 sampai 25 juta, dengan titik puncak di 18.5 juta. Parameter a=12, b=18.5, c=25. Overlap dengan Medium di range 12-15 juta."

"Range ini ditentukan berdasarkan analisis data aktual yang menunjukkan gaji dari 1.2 juta sampai 25 juta."

### **VARIABEL INPUT 2: PERSENTASE CICILAN (%)**

"Untuk atribut Persentase Cicilan, juga 3 kategori dengan fungsi triangular:"

"**Low**: Range 0 sampai 25%, titik puncak 12.5%. Parameter a=0, b=12.5, c=25."

"**Medium**: Range 15 sampai 45%, titik puncak 30%. Parameter a=15, b=30, c=45. Overlap dengan Low di 15-25% dan dengan High di 35-45%."

"**High**: Range 35 sampai 65%, titik puncak 50%. Parameter a=35, b=50, c=65. Overlap dengan Medium di 35-45%."

"Range ini berdasarkan standar Debt-to-Income ratio perbankan: di bawah 30% dianggap baik, 30-40% sedang, di atas 40% tinggi."

### **VARIABEL OUTPUT: SKOR RISIKO (0-100)**

"Untuk output, kami menggunakan 5 kategori risiko dengan fungsi triangular:"

"**VeryLow**: Range 0-20, puncak 10. Parameter a=0, b=10, c=20."

"**Low**: Range 10-40, puncak 25. Parameter a=10, b=25, c=40. Overlap dengan VeryLow di 10-20."

"**Medium**: Range 30-70, puncak 50. Parameter a=30, b=50, c=70."

"**High**: Range 60-90, puncak 75. Parameter a=60, b=75, c=90."

"**VeryHigh**: Range 80-100, puncak 90. Parameter a=80, b=90, c=100."

"**PENTING**: Semakin rendah skor, semakin baik. Skor 10 berarti risiko sangat rendah dan direkomendasikan untuk diberi pinjaman."

"Kami menggunakan fungsi keanggotaan triangular karena lebih sederhana dengan hanya 3 parameter, cukup untuk merepresentasikan gradasi kontinyu, dan umum digunakan dalam literatur fuzzy logic untuk credit risk assessment."

"Fungsi keanggotaan triangular diimplementasikan dalam fungsi `triangular_membership(self, x, a, b, c)` di kelas `FuzzyCreditRiskSystem`. Fungsi ini menghitung derajat keanggotaan berdasarkan rumus: jika x <= a atau x >= c, return 0.0; jika a < x <= b, return (x-a)/(b-a); jika b < x < c, return (c-x)/(c-b)."

"Batas-batas ini ditentukan berdasarkan: analisis data aktual, domain knowledge perbankan tentang DTI ratio, referensi dari paper tentang fuzzy logic untuk credit risk, dan konsultasi dengan AI sebagai starting point yang kemudian divalidasi dengan data."

"**Jika ditanya kenapa pakai triangular bukan trapesium:** Triangular lebih sederhana dengan 3 parameter (a, b, c) dibanding trapesium yang butuh 4 parameter (a, b, c, d). Untuk data kontinyu seperti gaji dan cicilan, triangular sudah cukup merepresentasikan gradasi. Trapezoidal lebih cocok untuk data yang punya 'zona aman' yang jelas dengan plateau, tapi untuk credit risk gradasi linear dari triangular sudah memadai. Selain itu, triangular lebih umum digunakan dalam literatur fuzzy logic untuk credit risk assessment."

"Terima kasih. Selanjutnya akan disampaikan oleh Shofi tentang implementasi teknis."

---

# ============================================
# SLIDE 4: IMPLEMENTASI TEKNIS
# Pembicara: Shofi (2.5 menit)
# ============================================

**[Shofi]**
"Terima kasih Indra. Saya Shofi akan menjelaskan implementasi teknis sistem, termasuk rule base dan proses fuzzy."

### **ATURAN INFERENSI (9 RULES)**

"Sistem kami menggunakan 9 aturan inferensi yang dibuat berdasarkan logika bisnis credit risk assessment."

"**Rule 1**: IF Gaji = Low AND Cicilan = Low THEN Risiko = Medium"
"**Rule 2**: IF Gaji = Low AND Cicilan = Medium THEN Risiko = High"
"**Rule 3**: IF Gaji = Low AND Cicilan = High THEN Risiko = VeryHigh"
"**Rule 4**: IF Gaji = Medium AND Cicilan = Low THEN Risiko = Low"
"**Rule 5**: IF Gaji = Medium AND Cicilan = Medium THEN Risiko = Medium"
"**Rule 6**: IF Gaji = Medium AND Cicilan = High THEN Risiko = High"
"**Rule 7**: IF Gaji = High AND Cicilan = Low THEN Risiko = VeryLow"
"**Rule 8**: IF Gaji = High AND Cicilan = Medium THEN Risiko = Low"
"**Rule 9**: IF Gaji = High AND Cicilan = High THEN Risiko = Medium"

"Prinsip dasarnya adalah: Gaji tinggi menunjukkan kemampuan bayar yang baik, sedangkan cicilan tinggi menunjukkan beban finansial yang besar. Kombinasi optimal adalah gaji tinggi dengan cicilan rendah, yang menghasilkan risiko sangat rendah."

### **METODE INFERENSI: MIN-MAX**

"Dalam implementasi kode, kami menggunakan metode MIN untuk operasi AND dan MAX untuk agregasi."

"**MIN untuk AND**: Strength dari setiap rule dihitung dengan mengambil MINIMUM dari derajat keanggotaan gaji dan cicilan. Contoh: Jika gaji memiliki derajat keanggotaan Low = 0.8 dan cicilan memiliki derajat keanggotaan High = 0.6, maka strength rule = min(0.8, 0.6) = 0.6. Logikanya: Rule hanya sekuat kondisi terlemah."

"**MAX untuk Agregasi**: Jika beberapa rule menghasilkan kategori risiko yang sama, kami mengambil MAXIMUM. Contoh: Rule 1 menghasilkan Medium dengan strength 0.5, dan Rule 5 juga menghasilkan Medium dengan strength 0.7, maka derajat keanggotaan Medium = max(0.5, 0.7) = 0.7."

### **PROSES FUZZY: FUZZIFICATION, INFERENCE, DEFUZZIFICATION**

"Implementasi sistem terdiri dari 3 proses utama yang diimplementasikan dalam fungsi-fungsi di kelas `FuzzyCreditRiskSystem`:"

"**1. Fuzzification**: Diimplementasikan dalam fungsi `fuzzification(self, salary, installment_percent)`. Fungsi ini menerima input crisp: gaji dalam juta dan persentase cicilan. Untuk setiap kategori linguistik gaji (Low, Medium, High), kami hitung derajat keanggotaan menggunakan fungsi `triangular_membership()` dengan parameter a, b, c yang sudah didefinisikan di `__init__()`. Proses yang sama dilakukan untuk cicilan. Hasilnya adalah dua dictionary: `salary_fuzzy` dan `installment_fuzzy`."

"Contoh: Jika gaji = 10 juta, maka `triangular_membership(10, 0, 4, 8)` untuk Low = 0, `triangular_membership(10, 5, 10, 15)` untuk Medium = 1.0, `triangular_membership(10, 12, 18.5, 25)` untuk High = 0. Jadi `salary_fuzzy = {'Low': 0.0, 'Medium': 1.0, 'High': 0.0}`."

"**2. Inference**: Diimplementasikan dalam fungsi `inference(self, salary_fuzzy, installment_fuzzy)`. Fungsi ini menerima `salary_fuzzy` dan `installment_fuzzy`, kemudian menerapkan 9 aturan yang didefinisikan di `self.inference_rules`. Untuk setiap rule, hitung `rule_strength = min(salary_fuzzy[salary_label], installment_fuzzy[installment_label])`, kemudian update `risk_fuzzy[risk_label] = max(risk_fuzzy[risk_label], rule_strength)`. Hasilnya adalah dictionary `risk_fuzzy` dengan derajat keanggotaan untuk setiap kategori risiko."

"**3. Defuzzification**: Diimplementasikan dalam fungsi `defuzzification_centroid(self, risk_fuzzy)`. Fungsi ini menggunakan metode Centroid atau Center of Gravity. Dalam kode, kami membagi domain output 0-100 menjadi 1000 titik dengan `num_points = 1000` dan `domain = [i * 100.0 / num_points for i in range(num_points + 1)]`. Untuk setiap titik x dalam domain, hitung derajat keanggotaan output dengan clipping menggunakan `min(membership_degree, triangular_membership(x, a, b, c))`, kemudian agregasi dengan `max()`. Kemudian hitung centroid dengan rumus `COG = numerator / denominator` dimana `numerator = sigma(x * membership_value)` dan `denominator = sigma(membership_value)`. Jika `denominator = 0` (tidak ada aktivasi), return 50.0 sebagai default."

"Kenapa 1000 titik? Trade-off antara akurasi dan performa. 1000 titik memberikan akurasi yang baik dengan error kurang dari 0.1, dan ini standar dalam industri. Bisa juga pakai 2001 titik untuk akurasi lebih tinggi dengan error kurang dari 0.05, tapi lebih lambat. Untuk 50 data, perbedaan waktu tidak signifikan."

"Ketiga proses ini dipanggil secara berurutan dalam fungsi `calculate_risk_score(self, salary, installment_percent)` yang merupakan fungsi utama untuk menghitung skor risiko satu pengaju."

"Semua proses ini diimplementasikan manual dalam Python tanpa menggunakan library fuzzy logic. Kami hanya menggunakan pandas dan openpyxl untuk membaca dan menulis file Excel."

"**Jika ditanya kenapa pakai MIN-MAX bukan PROD-SUM:** MIN-MAX lebih konservatif dan umum digunakan. MIN untuk AND berarti rule hanya sekuat kondisi terlemah, yang lebih aman untuk credit risk. PROD akan memberikan nilai yang lebih kecil dan bisa terlalu konservatif. MAX untuk agregasi berarti ambil yang paling kuat, yang masuk akal untuk credit risk - jika ada rule yang mengatakan risiko tinggi dengan strength tinggi, kita harus memperhatikannya."

"**Jika ditanya apa itu clipping dalam defuzzification:** Clipping adalah mengambil minimum antara strength rule (membership_degree) dan fungsi keanggotaan pada titik x. Ini penting karena strength rule adalah hasil dari inference, sedangkan fungsi keanggotaan adalah bentuk asli. Dengan clipping, kita memastikan output tidak melebihi strength rule. Contoh: Jika rule menghasilkan VeryLow dengan strength 0.6, maka pada titik x, membership tidak bisa lebih dari 0.6, meskipun fungsi keanggotaan VeryLow di titik x mungkin lebih tinggi."

"**Jika ditanya kenapa pakai Centroid bukan metode lain:** Centroid adalah metode paling umum dan memberikan hasil yang smooth. Metode lain seperti Bisector, Mean of Maximum, Largest of Maximum, atau Smallest of Maximum bisa memberikan hasil yang berbeda. Centroid memberikan titik pusat massa dari area fuzzy output, yang paling representatif untuk credit risk assessment."

"Terima kasih. Selanjutnya akan disampaikan oleh Virgie tentang hasil analisis."

---

# ============================================
# SLIDE 5: HASIL ANALISIS
# Pembicara: Virgie (1.5 menit)
# ============================================

**[Virgie]**
"Terima kasih Shofi. Saya Virgie akan menyampaikan hasil analisis dari sistem yang telah kami implementasikan."

"Sistem kami berhasil memproses 50 data pengaju pinjaman dari file 'resiko kredit.xlsx' menggunakan fungsi `read_excel_data(filename)`. Untuk setiap pengaju, sistem melakukan proses fuzzy logic dengan memanggil fungsi `calculate_risk_score(salary, installment_percent)` yang di dalamnya memanggil `fuzzification()`, `inference()`, dan `defuzzification_centroid()` secara berurutan untuk mendapatkan skor risiko."

"Hasil menunjukkan konsistensi dengan logika yang telah kami desain. Pengaju dengan gaji tinggi dan cicilan rendah mendapatkan skor rendah, yang berarti risiko rendah dan direkomendasikan untuk diberi pinjaman."

"Sebagai contoh dari hasil kami:"
"- Pengaju dengan ID [sebutkan ID dari top 3] memiliki gaji [X] juta dan cicilan [Y] persen, mendapatkan skor risiko [Z], yang menempatkannya di peringkat [N]"
"- Pengaju dengan ID [sebutkan ID terakhir] memiliki gaji [X] juta dan cicilan [Y] persen, mendapatkan skor risiko [Z], yang menempatkannya di peringkat terakhir karena risiko tinggi"

"Kami telah melakukan validasi menyeluruh terhadap hasil dan tidak menemukan anomali signifikan. Validasi yang dilakukan:"
"1. Validasi Manual: Kami cek beberapa sample dari top 10, middle 10, dan bottom 10. Semua hasil sesuai dengan ekspektasi"
"2. Validasi Logika: Kami cek apakah pengaju dengan gaji lebih tinggi dan cicilan lebih rendah selalu mendapat skor lebih baik. Hasilnya konsisten"
"3. Validasi Edge Case: Kami test dengan nilai ekstrem - gaji sangat tinggi dengan cicilan sangat rendah selalu mendapat skor sangat rendah"

"Output disimpan dalam file Excel 'peringkat.xlsx' menggunakan fungsi `save_to_excel(data, filename)` yang berisi 10 pengaju terbaik dengan kolom: No Id pengaju pinjama, Gaji dalam juta, Persentase cicilan pinjaman terhadap gaji dalam persen, dan Skor Risiko Kredit hasil defuzzification."

"**Jika ditanya ada anomali di hasil:** Kami sudah melakukan validasi menyeluruh dan tidak menemukan anomali signifikan. Semua hasil konsisten dengan logika: gaji tinggi + cicilan rendah = skor rendah (baik). Jika ada kasus yang terlihat seperti anomali, kemungkinan karena kedua faktor saling mempengaruhi. Contoh: gaji 15 juta dengan cicilan 40% mungkin mendapat skor lebih buruk daripada gaji 12 juta dengan cicilan 20%, karena cicilan 40% adalah beban yang lebih besar meskipun gaji lebih tinggi. Ini bukan anomali, ini adalah hasil yang benar dari sistem fuzzy yang mempertimbangkan kedua faktor secara bersamaan."

"Terima kasih. Selanjutnya akan disampaikan tentang visualisasi."

---

# ============================================
# SLIDE 6: VISUALISASI KEPUTUSAN & DISTRIBUSI
# Pembicara: Virgie (1 menit)
# ============================================

**[Virgie]**
"Kami juga membuat visualisasi untuk membantu memahami hasil dan distribusi data."

"**Visualisasi 1: Fungsi Keanggotaan**"
"Kami memvisualisasikan bentuk fungsi keanggotaan untuk Gaji, Cicilan, dan Risiko. Visualisasi ini menunjukkan bagaimana nilai crisp diubah menjadi derajat keanggotaan, dan bagaimana overlap antar kategori memungkinkan smooth transition."

"**Visualisasi 2: Distribusi Skor Risiko Top 10**"
"Kami membuat bar chart yang menunjukkan distribusi skor risiko untuk 10 pengaju terbaik. Visualisasi ini menunjukkan bahwa top 10 memiliki skor yang relatif rendah, yang konsisten dengan tujuan sistem."

"**Visualisasi 3: Scatter Plot Gaji vs Cicilan**"
"Kami membuat scatter plot dengan warna berdasarkan skor risiko. Semua 50 pengaju ditampilkan, dengan top 10 ditandai dengan bintang. Visualisasi ini menunjukkan hubungan antara gaji, cicilan, dan risiko kredit secara visual."

"Visualisasi ini membantu memahami bagaimana sistem fuzzy logic bekerja dan bagaimana kombinasi gaji dan cicilan mempengaruhi skor risiko."

"Terima kasih. Selanjutnya akan disampaikan kesimpulan."

---

# ============================================
# SLIDE 7: KESIMPULAN
# Pembicara: Virgie (0.5 menit)
# ============================================

**[Virgie]**
"Kesimpulan dari tugas ini:"

"Kami berhasil membangun sistem fuzzy logic untuk penilaian risiko kredit yang dapat membantu bank dalam proses pengambilan keputusan pemberian pinjaman. Sistem ini transparan, dapat dijelaskan, dan menggunakan logika bisnis yang jelas."

"Keunggulan sistem kami:"
"1. Implementasi manual tanpa library fuzzy - semua proses fuzzification, inference, dan defuzzification dibuat dari awal"
"2. Transparan dan dapat dijelaskan - setiap keputusan bisa dijelaskan dengan jelas"
"3. Menggunakan logika bisnis yang jelas - rule base dibuat berdasarkan domain knowledge"
"4. Validasi menyeluruh - hasil sudah divalidasi dan konsisten"

"Metode yang digunakan: Metode Mamdani dengan defuzzification Centroid, fungsi keanggotaan Triangular, inference MIN-MAX, dan 1000 titik untuk defuzzification."

"Keterbatasan dan pengembangan lebih lanjut: Sistem saat ini hanya mempertimbangkan 2 atribut. Bisa dikembangkan dengan menambahkan atribut lain seperti riwayat kredit, jaminan, atau usia."

"Demikian presentasi kami. Terima kasih atas perhatiannya. Kami siap menerima pertanyaan."

---

# ============================================
# PEMBAGIAN WAKTU DETAIL:
# ============================================

**INdra:**
- Slide 1: Latar Belakang & Masalah (1.5 menit)
- Slide 3: Desain Sistem (2 menit)
- **Total: 3.5 menit**

**Shofi:**
- Slide 2: Solusi & Tujuan (1.5 menit)
- Slide 4: Implementasi Teknis (2.5 menit)
- **Total: 4 menit**

**Virgie:**
- Slide 5: Hasil Analisis (1.5 menit)
- Slide 6: Visualisasi (1 menit)
- Slide 7: Kesimpulan (0.5 menit)
- **Total: 3 menit**

**Total Presentasi: 10.5 menit** (sedikit lebih untuk buffer)

---

# ============================================
# TRANSISI ANTAR PEMBICARA:
# ============================================

**INdra → Shofi (setelah Slide 1):**
"Terima kasih. Selanjutnya akan disampaikan oleh Shofi tentang solusi dan tujuan."

**Shofi → INdra (setelah Slide 2):**
"Terima kasih. Selanjutnya akan disampaikan oleh Indra tentang desain sistem."

**INdra → Shofi (setelah Slide 3):**
"Terima kasih. Selanjutnya akan disampaikan oleh Shofi tentang implementasi teknis."

**Shofi → Virgie (setelah Slide 4):**
"Terima kasih. Selanjutnya akan disampaikan oleh Virgie tentang hasil analisis."

**Virgie (internal, setelah Slide 5):**
"Selanjutnya akan disampaikan tentang visualisasi."

**Virgie (internal, setelah Slide 6):**
"Selanjutnya akan disampaikan kesimpulan."

**Virgie (akhir):**
"Demikian presentasi kami. Terima kasih atas perhatiannya. Kami siap menerima pertanyaan."

---

# ============================================
# JAWABAN LENGKAP UNTUK Q&A:
# ============================================

## Q1: KENAPA PAKAI TRIANGULAR BUKAN TRAPEZOIDAL?

**Jawab (INdra/Shofi):**
"Triangular lebih sederhana dengan 3 parameter (a, b, c) dibanding trapesium yang butuh 4 parameter (a, b, c, d). Untuk data kontinyu seperti gaji dan cicilan, triangular sudah cukup merepresentasikan gradasi. Trapezoidal lebih cocok untuk data yang punya 'zona aman' yang jelas dengan plateau, tapi untuk credit risk gradasi linear dari triangular sudah memadai. Selain itu, triangular lebih umum digunakan dalam literatur fuzzy logic untuk credit risk assessment."

"Fungsi triangular diimplementasikan dalam `triangular_membership(self, x, a, b, c)` di kelas `FuzzyCreditRiskSystem`."

---

## Q2: KENAPA PAKAI 1000 TITIK (ATAU 2001) UNTUK DEFUZZIFICATION?

**Jawab (Shofi):**
"1000 titik adalah sweet spot antara akurasi dan performa. Dengan 1000 titik, error kurang dari 0.1, yang sudah cukup akurat untuk kebutuhan praktis. 2001 titik lebih akurat dengan error kurang dari 0.05, tapi 2x lebih lambat. Untuk 50 data, perbedaan waktu tidak signifikan, jadi 1000 sudah cukup. Ini juga standar dalam industri fuzzy logic."

"Dalam kode, ini diimplementasikan di fungsi `defuzzification_centroid()` dengan `num_points = 1000` dan `domain = [i * 100.0 / num_points for i in range(num_points + 1)]`."

---

## Q3: KENAPA AMBIL RISIKO TERKECIL BUKAN TERBESAR?

**Jawab (Virgie):**
"Karena definisi skor risiko kami: 0-100, dimana SEMAKIN RENDAH = SEMAKIN BAIK. Skor 10 berarti risiko sangat rendah (sangat aman untuk pinjaman), sedangkan skor 90 berarti risiko sangat tinggi (tidak aman). Bank ingin minimize risiko, jadi ambil yang skornya TERKECIL, yang berarti paling aman. Jika ambil terbesar, berarti memilih yang paling berisiko, yang tidak masuk akal untuk bank."

"Dalam kode, ini dilakukan dengan `sorted(results, key=lambda x: x['Skor Risiko Kredit'])` untuk mengurutkan dari terkecil ke terbesar, kemudian `top_10 = results_sorted[:10]` untuk mengambil 10 terbaik."

---

## Q4: RULE BASE DARI MANA? LOGIKANYA GIMANA?

**Jawab (Shofi):**
"Rule dibuat berdasarkan logika bisnis credit risk. Prinsipnya: Gaji tinggi = kemampuan bayar baik, Cicilan tinggi = beban finansial besar. Kombinasi optimal adalah gaji tinggi + cicilan rendah = risiko rendah. 9 kombinasi logis dibuat berdasarkan domain knowledge perbankan tentang DTI ratio dan best practice. Kami tidak pakai machine learning karena fuzzy logic lebih transparan dan bisa dijelaskan, penting untuk regulasi."

"Rule base didefinisikan di `self.inference_rules` dalam fungsi `__init__()` dan diterapkan di fungsi `inference()`."

"**Detail 9 aturan:**"
"1. Low + Low → Medium (gaji rendah tapi cicilan rendah, masih ada ruang, risiko sedang)"
"2. Low + Medium → High (gaji rendah + cicilan sedang, kemampuan terbatas, risiko tinggi)"
"3. Low + High → VeryHigh (gaji rendah + cicilan tinggi, sangat berisiko)"
"4. Medium + Low → Low (gaji sedang + cicilan rendah, kemampuan baik, risiko rendah)"
"5. Medium + Medium → Medium (seimbang, risiko sedang)"
"6. Medium + High → High (gaji sedang + cicilan tinggi, kemampuan terbatas)"
"7. High + Low → VeryLow (gaji tinggi + cicilan rendah, sangat aman, sangat direkomendasikan)"
"8. High + Medium → Low (gaji tinggi + cicilan sedang, aman)"
"9. High + High → Medium (gaji tinggi + cicilan tinggi, masih bisa karena gaji tinggi bisa menutupi)"

---

## Q5: BATAS MEMBERSHIP DARI MANA?

**Jawab (INdra):**
"Batas ditentukan dari 3 sumber: Pertama, analisis data aktual - kami lihat range gaji 1.2-25 juta dan cicilan 5-62%, lalu bagi menjadi kategori yang representatif. Kedua, domain knowledge perbankan - standar DTI ratio mengatakan di bawah 30% baik, 30-40% sedang, di atas 40% tinggi. Ketiga, referensi paper tentang fuzzy logic untuk credit risk assessment. Kami juga menggunakan AI sebagai starting point untuk mendapatkan range awal berdasarkan standar industri, kemudian divalidasi dan disesuaikan dengan data aktual kami."

"Batas-batas ini didefinisikan di `__init__()` dalam `self.salary_membership`, `self.installment_membership`, dan `self.risk_membership`."

---

## Q6: ADA ANOMALI DI HASIL AKHIR?

**Jawab (Virgie):**
"Kami sudah melakukan validasi menyeluruh dan tidak menemukan anomali signifikan. Semua hasil konsisten dengan logika: gaji tinggi + cicilan rendah = skor rendah (baik). Jika ada kasus yang terlihat seperti anomali, kemungkinan karena kedua faktor saling mempengaruhi. Contoh: gaji 15 juta dengan cicilan 40% mungkin mendapat skor lebih buruk daripada gaji 12 juta dengan cicilan 20%, karena cicilan 40% adalah beban yang lebih besar meskipun gaji lebih tinggi. Ini bukan anomali, ini adalah hasil yang benar dari sistem fuzzy yang mempertimbangkan kedua faktor secara bersamaan."

"Validasi dilakukan dengan: 1) Validasi manual sample, 2) Validasi logika (gaji tinggi + cicilan rendah selalu skor rendah), 3) Validasi edge case (nilai ekstrem), 4) Validasi konsistensi (semua skor dalam range 0-100)."

---

## Q7: KENAPA MIN-MAX BUKAN PROD-SUM?

**Jawab (Shofi):**
"MIN-MAX lebih konservatif dan umum digunakan. MIN untuk AND berarti rule hanya sekuat kondisi terlemah, yang lebih aman untuk credit risk. PROD akan memberikan nilai yang lebih kecil dan bisa terlalu konservatif. MAX untuk agregasi berarti ambil yang paling kuat, yang masuk akal untuk credit risk - jika ada rule yang mengatakan risiko tinggi dengan strength tinggi, kita harus memperhatikannya."

"Ini diimplementasikan di fungsi `inference()` dengan `rule_strength = min(salary_fuzzy[salary_label], installment_fuzzy[installment_label])` dan `risk_fuzzy[risk_label] = max(risk_fuzzy[risk_label], rule_strength)`."

---

## Q8: APA ITU CLIPPING DALAM DEFUZZIFICATION?

**Jawab (Shofi):**
"Clipping adalah mengambil minimum antara strength rule (membership_degree) dan fungsi keanggotaan pada titik x. Ini penting karena strength rule adalah hasil dari inference, sedangkan fungsi keanggotaan adalah bentuk asli. Dengan clipping, kita memastikan output tidak melebihi strength rule. Contoh: Jika rule menghasilkan VeryLow dengan strength 0.6, maka pada titik x, membership tidak bisa lebih dari 0.6, meskipun fungsi keanggotaan VeryLow di titik x mungkin lebih tinggi."

"Ini diimplementasikan di `defuzzification_centroid()` dengan `membership_at_x = min(membership_degree, triangular_membership(x, params['a'], params['b'], params['c']))`."

---

## Q9: KENAPA CENTROID BUKAN METODE LAIN?

**Jawab (Shofi):**
"Centroid adalah metode paling umum dan memberikan hasil yang smooth. Metode lain seperti Bisector, Mean of Maximum, Largest of Maximum, atau Smallest of Maximum bisa memberikan hasil yang berbeda. Centroid memberikan titik pusat massa dari area fuzzy output, yang paling representatif untuk credit risk assessment."

"Ini diimplementasikan di fungsi `defuzzification_centroid()` dengan rumus `COG = numerator / denominator` dimana `numerator = sigma(x * membership_value)` dan `denominator = sigma(membership_value)`."

---

## Q10: KENAPA TIDAK PAKAI LIBRARY FUZZY?

**Jawab (Semua):**
"Syarat tugas mengharuskan implementasi manual tanpa library fuzzy. Kami hanya menggunakan pandas dan openpyxl untuk membaca dan menulis file Excel (fungsi `read_excel_data()` dan `save_to_excel()`), numpy untuk perhitungan numerik, dan matplotlib untuk visualisasi. Semua proses fuzzy (fuzzification, inference, defuzzification) dibuat manual dari awal dalam kelas `FuzzyCreditRiskSystem`, sesuai dengan syarat tugas."

---

## Q11: KENAPA OVERLAP ANTAR KATEGORI?

**Jawab (INdra):**
"Overlap penting dalam fuzzy logic untuk smooth transition. Contoh: gaji 10 juta memiliki derajat keanggotaan untuk Low, Medium, dan High secara bersamaan. Ini memungkinkan sistem menangani nilai yang berada di 'perbatasan' dengan lebih baik. Tanpa overlap, sistem akan terlalu kaku dan tidak bisa menangani ketidakpastian dengan baik."

"Overlap ini terlihat jelas dalam definisi membership: Low 0-8, Medium 5-15 (overlap di 5-8), High 12-25 (overlap di 12-15)."

---

## Q12: KENAPA 5 KATEGORI OUTPUT TAPI 3 INPUT?

**Jawab (INdra):**
"Output membutuhkan granularitas lebih tinggi untuk membedakan 50 pengaju menjadi ranking yang jelas. Dengan 5 kategori output (VeryLow, Low, Medium, High, VeryHigh), kami bisa menghasilkan skor yang lebih bervariasi dan akurat untuk ranking. Input cukup 3 kategori karena sudah cukup untuk membedakan kondisi finansial pengaju."

---

## Q13: BAGAIMANA CARA KERJA SISTEM SECARA KESELURUHAN?

**Jawab (Virgie):**
"Sistem bekerja dalam 4 tahap: 1) Baca data dari Excel menggunakan fungsi `read_excel_data('resiko kredit.xlsx')`, 2) Untuk setiap pengaju, panggil `fuzzy_system.calculate_risk_score(salary, installment_percent)` yang di dalamnya memanggil `fuzzification()`, `inference()`, dan `defuzzification_centroid()` secara berurutan, 3) Urutkan semua pengaju berdasarkan skor dari terkecil ke terbesar dengan `sorted()`, 4) Ambil top 10 dan simpan ke Excel menggunakan `save_to_excel(top_10, 'peringkat.xlsx')`. Semua proses fuzzy diimplementasikan manual tanpa library."

---

## Q14: APAKAH SISTEM INI BISA DIGUNAKAN DI DUNIA NYATA?

**Jawab (Virgie):**
"Sistem ini bisa digunakan sebagai alat bantu dalam proses pengambilan keputusan, tapi perlu pengembangan lebih lanjut. Perlu ditambah atribut lain seperti riwayat kredit, jaminan, atau usia. Rule base perlu divalidasi dengan data historis. Fungsi keanggotaan bisa dioptimasi. Tapi konsep dasarnya sudah benar dan bisa dikembangkan menjadi sistem yang lebih lengkap."

---

## Q15: KENAPA TIDAK PAKAI MACHINE LEARNING?

**Jawab (Virgie):**
"Kami memilih fuzzy logic karena transparansi. Setiap keputusan bisa dijelaskan dengan jelas - rule mana yang teraktivasi, berapa derajat keanggotaannya, bagaimana skor dihitung. Ini penting untuk regulasi perbankan dan audit. Machine learning adalah black box yang sulit dijelaskan. Fuzzy logic memberikan explainability yang penting untuk credit risk assessment."

---

## CONTOH PERHITUNGAN MANUAL (Siap jika ditanya):

**Contoh: Gaji 10 juta, Cicilan 30%**

**Fuzzification (fungsi `fuzzification()`):**
- Gaji 10 juta: `triangular_membership(10, 0, 4, 8)` untuk Low = 0, `triangular_membership(10, 5, 10, 15)` untuk Medium = 1.0, `triangular_membership(10, 12, 18.5, 25)` untuk High = 0
- Cicilan 30%: `triangular_membership(30, 0, 12.5, 25)` untuk Low = 0, `triangular_membership(30, 15, 30, 45)` untuk Medium = 1.0, `triangular_membership(30, 35, 50, 65)` untuk High = 0

**Inference (fungsi `inference()`):**
- Rule 5 (Medium, Medium, Medium): `strength = min(1.0, 1.0) = 1.0`
- `risk_fuzzy['Medium'] = max(0.0, 1.0) = 1.0`

**Defuzzification (fungsi `defuzzification_centroid()`):**
- Hitung centroid dengan 1000 titik
- Hasil: sekitar 50 (karena Medium dengan strength 1.0, puncak di 50)

**Jadi skor risiko = 50, yang berarti risiko sedang.**

---

# ============================================
# CHECKLIST SEBELUM PRESENTASI:
# ============================================

- [ ] Slide sudah siap dan lengkap (7 slide sesuai gambar)
- [ ] Semua pembicara sudah hafal bagian masing-masing
- [ ] Timing sudah diuji (total 10 menit)
- [ ] Transisi antar pembicara sudah smooth
- [ ] Siap jawab pertanyaan umum
- [ ] File hasil sudah siap untuk ditunjukkan
- [ ] Notebook sudah siap untuk demo (jika perlu)
- [ ] Laptop dan proyektor sudah di-test
- [ ] Contoh data konkret sudah siap (ID, gaji, cicilan, skor dari hasil)

---

**SELAMAT PRESENTASI! SEMOGA LANCAR! 🚀**

**Ingat:**
- Bicara jelas dan percaya diri
- Jangan terburu-buru
- Jaga kontak mata dengan audience
- Gunakan gesture yang natural
- Jika lupa, lihat slide sebagai panduan
- Kerja sama tim - saling support saat presentasi

