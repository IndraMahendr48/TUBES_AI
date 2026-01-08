# Simulasi Penggunaan Ruang Parkir di Area Kampus

## 📋 Deskripsi Proyek

Proyek ini merupakan **model simulasi agent-based** untuk menganalisis dinamika pencarian parkir dan distribusi penggunaan lahan parkir di area kampus. Simulasi ini menggunakan pendekatan pemodelan diskrit untuk mempelajari bagaimana kendaraan mencari, menggunakan, dan meninggalkan area parkir.

**Mata Kuliah:** Pemodelan, Simulasi, dan Optimasi (PSO)

---

## 🎯 Tujuan Simulasi

1. **Menganalisis pola penggunaan parkir** di area kampus
2. **Mengukur efisiensi** sistem parkir berdasarkan berbagai parameter
3. **Mengevaluasi pengaruh** jumlah kendaraan, durasi parkir, dan kapasitas parkir
4. **Memberikan rekomendasi** untuk optimasi sistem parkir kampus

---

## 🏗️ Arsitektur Model

### Komponen Utama

#### 1. **Grid (Peta Parkir)**
- **Grid Size:** 33x33 sel (default)
- **Tipe Sel:**
  - 🟢 **GRASS (0)**: Area hijau/rumput
  - ⬜ **ROAD (1)**: Jalan untuk pergerakan kendaraan
  - 🟩 **PARKING (2)**: Slot parkir kosong
  - 🔴 **OCCUPIED (3)**: Slot parkir terisi
  - 🟠 **SEARCHING (4)**: Kendaraan sedang mencari parkir
  - 🔵 **LEAVING (5)**: Kendaraan sedang keluar

#### 2. **Agen (Kendaraan)**
Setiap kendaraan memiliki:
- **Posisi (x, y)** pada grid
- **Status:**
  - `SEARCHING`: Mencari slot parkir
  - `PARKED`: Sudah parkir dan menunggu
  - `LEAVING`: Keluar dari area parkir
- **Timer:**
  - `search_timer`: Waktu yang dihabiskan mencari parkir
  - `park_timer`: Waktu yang sudah parkir
  - `target_duration`: Durasi parkir yang diinginkan

#### 3. **Perilaku Agen**
- **Mencari Parkir:** Kendaraan bergerak mencari slot parkir kosong di sekitar
- **Parkir:** Ketika menemukan slot kosong, kendaraan langsung parkir
- **Menunggu:** Kendaraan menunggu sesuai durasi yang ditentukan
- **Keluar:** Setelah selesai, kendaraan mencari jalan keluar

---

## 📦 Requirements

### Dependencies
```bash
numpy
matplotlib
```

### Instalasi
```bash
pip install numpy matplotlib
```

---

## 🚀 Cara Menjalankan

### Mode 1: Simulasi dengan Visualisasi (Default)
Menampilkan animasi real-time dengan 4 panel:
- Peta parkir (grid)
- Grafik jumlah kendaraan
- Grafik tingkat okupansi
- Grafik rata-rata waktu pencarian

```bash
python simulasi_parkir_kampus.py 1
```

atau

```bash
python simulasi_parkir_kampus.py
# Pilih mode 1
```

### Mode 2: Semua Eksperimen
Menjalankan ketiga eksperimen sekaligus dan menghasilkan grafik perbandingan.

```bash
python simulasi_parkir_kampus.py 2
```

### Mode 3: Eksperimen 1 - Pengaruh Jumlah Kendaraan
Menganalisis bagaimana jumlah kendaraan yang masuk mempengaruhi sistem parkir.

```bash
python simulasi_parkir_kampus.py 3
```

### Mode 4: Eksperimen 2 - Pengaruh Durasi Parkir
Menganalisis bagaimana variasi durasi parkir mempengaruhi sistem.

```bash
python simulasi_parkir_kampus.py 4
```

### Mode 5: Eksperimen 3 - Pengaruh Area Parkir Tambahan
Membandingkan performa dengan dan tanpa area parkir tambahan.

```bash
python simulasi_parkir_kampus.py 5
```

---

## ⚙️ Parameter Simulasi

### Parameter Utama

| Parameter | Default | Deskripsi |
|-----------|---------|-----------|
| `grid_size` | 33 | Ukuran grid (33x33 sel) |
| `influx_rate` | 63 | Probabilitas kendaraan masuk per tick (%) |
| `parking_duration_var` | 410 | Variasi durasi parkir (ticks) |
| `base_parking_time` | 185 | Durasi parkir dasar (ticks) |
| `add_extra_parking` | False | Menambahkan area parkir tambahan |

### Penjelasan Parameter

1. **Influx Rate (63%)**
   - Probabilitas kendaraan baru masuk setiap tick
   - Semakin tinggi, semakin banyak kendaraan yang masuk
   - Range yang diuji: 20%, 40%, 60%, 80%, 100%

2. **Parking Duration**
   - `base_parking_time`: Durasi minimum parkir
   - `parking_duration_var`: Variasi durasi (random 0 sampai var)
   - Durasi aktual = base + random(0, var)
   - Range yang diuji: 200, 300, 400, 500, 600

3. **Grid Size (33x33)**
   - Ukuran area simulasi
   - Terdiri dari:
     - Jalan melingkar (radius 10-14 dari center)
     - Area parkir di tengah (radius 9)
     - Jalan masuk di bagian bawah

---

## ⏱️ Konsep "Tick" dalam Simulasi

### Apa itu Tick?

**Tick** adalah **unit waktu diskrit** dalam simulasi. Setiap kali fungsi `step()` dipanggil, itu berarti **1 tick** telah berlalu.

### Analogi Sederhana

Bayangkan tick seperti **detik** dalam dunia nyata:
- **1 tick** = 1 langkah/iterasi simulasi
- **100 ticks** = 100 langkah simulasi
- Semua aktivitas (kendaraan masuk, mencari parkir, parkir, keluar) terjadi dalam satuan tick

### Contoh dalam Simulasi

1. **Tick 0:** Kendaraan masuk ke area parkir
2. **Tick 1-5:** Kendaraan mencari parkir (search_timer = 5)
3. **Tick 6:** Kendaraan menemukan slot dan parkir
4. **Tick 7-200:** Kendaraan parkir (park_timer = 194)
5. **Tick 201:** Kendaraan mulai keluar

### Mengapa Waktu Pencarian Bisa Sangat Kecil?

Dari grafik yang Anda lihat, nilai waktu pencarian hampir **0 ticks** karena:

1. **Algoritma Pencarian Sederhana:**
   - Kendaraan langsung mengecek **4 tetangga** (atas, bawah, kiri, kanan)
   - Jika ada slot kosong di tetangga → langsung parkir
   - Jadi kendaraan bisa parkir dalam **1-2 ticks** saja

2. **Banyak Slot Kosong:**
   - Jika area parkir masih banyak yang kosong
   - Kemungkinan menemukan slot di sekitar sangat tinggi
   - Hasilnya: waktu pencarian sangat singkat

3. **Cara Perhitungan:**
   ```
   Rata-rata Waktu Pencarian = Total Search Time / Jumlah Kendaraan Selesai
   
   Contoh:
   - Kendaraan 1: search_timer = 1 tick (langsung ketemu)
   - Kendaraan 2: search_timer = 2 ticks
   - Kendaraan 3: search_timer = 1 tick
   - Rata-rata = (1+2+1)/3 = 1.33 ticks
   ```

### Interpretasi Nilai Tick

| Nilai Tick | Arti |
|------------|------|
| **0-5 ticks** | Sangat cepat menemukan parkir (banyak slot kosong) |
| **5-20 ticks** | Cukup cepat (masih ada slot tersedia) |
| **20-50 ticks** | Sedang (perlu mencari lebih lama) |
| **>50 ticks** | Lambat (area parkir hampir penuh) |

### Catatan Penting

- **Tick bukan waktu real** (bukan detik atau menit)
- Tick adalah **unit abstrak** untuk mengukur langkah simulasi
- Untuk konversi ke waktu real, perlu kalibrasi dengan data nyata
- Contoh: Jika 1 tick = 10 detik real, maka 5 ticks = 50 detik

---

## 🔬 Eksperimen

### Eksperimen 1: Pengaruh Jumlah Kendaraan (Influx Rate)

**Tujuan:** Menganalisis bagaimana jumlah kendaraan yang masuk mempengaruhi:
- Waktu pencarian parkir
- Tingkat okupansi
- Total kendaraan yang terlayani

**Variabel yang Diuji:**
- Influx Rate: 20%, 40%, 60%, 80%, 100%
- Durasi simulasi: 1000 ticks

**Metrik yang Diukur:**
- Rata-rata waktu pencarian parkir
- Tingkat okupansi (rata-rata, maksimum)
- Total kendaraan yang selesai
- Efisiensi sistem

**Hasil yang Diharapkan:**
- Semakin tinggi influx rate → waktu pencarian meningkat
- Semakin tinggi influx rate → okupansi meningkat
- Ada titik optimal untuk efisiensi sistem

---

### Eksperimen 2: Pengaruh Durasi Parkir

**Tujuan:** Menganalisis bagaimana variasi durasi parkir mempengaruhi sistem.

**Variabel yang Diuji:**
- Parking Duration Variance: 200, 300, 400, 500, 600
- Influx Rate: 63% (tetap)
- Durasi simulasi: 1000 ticks

**Metrik yang Diukur:**
- Rata-rata waktu pencarian
- Tingkat okupansi
- Total kendaraan yang selesai

**Hasil yang Diharapkan:**
- Durasi parkir lebih lama → okupansi lebih tinggi
- Durasi parkir lebih lama → waktu pencarian meningkat (karena slot lebih lama terisi)

---

### Eksperimen 3: Pengaruh Penambahan Area Parkir

**Tujuan:** Membandingkan performa sistem dengan dan tanpa area parkir tambahan.

**Konfigurasi:**
1. **Tanpa Area Tambahan:** Hanya area parkir utama di tengah
2. **Dengan Area Tambahan:** Area parkir utama + area tambahan di sisi

**Metrik yang Diukur:**
- Rata-rata waktu pencarian
- Tingkat okupansi
- Total kendaraan yang selesai

**Hasil yang Diharapkan:**
- Area tambahan → waktu pencarian menurun
- Area tambahan → lebih banyak kendaraan terlayani
- Area tambahan → okupansi lebih rendah (karena lebih banyak slot)

---

## 📊 Visualisasi

### Panel Visualisasi

1. **Peta Parkir (Grid)**
   - Menampilkan layout area parkir secara real-time
   - Warna menunjukkan status setiap sel
   - Statistik real-time di pojok kiri atas

2. **Grafik Jumlah Kendaraan**
   - Garis biru: Total kendaraan aktif
   - Garis hijau: Kendaraan yang sedang parkir

3. **Grafik Tingkat Okupansi**
   - Persentase slot parkir yang terisi
   - Range: 0-100%

4. **Grafik Rata-rata Waktu Pencarian**
   - Rata-rata waktu yang dibutuhkan untuk menemukan parkir
   - Diukur dalam ticks
   - **Catatan:** Jika nilai sangat kecil (mendekati 0), berarti:
     - Banyak slot parkir kosong
     - Kendaraan langsung menemukan parkir di sekitar
     - Algoritma pencarian sederhana (hanya cek tetangga langsung)

### Output Eksperimen

Setiap eksperimen menghasilkan file PNG:
- `experiment_influx_rate.png`
- `experiment_duration.png`
- `experiment_extra_parking.png`

---

## 📈 Metrik yang Diukur

### 1. Waktu Pencarian (Search Time)
- Waktu yang dihabiskan kendaraan untuk mencari slot parkir
- Diukur dalam ticks
- Semakin rendah semakin baik

### 2. Tingkat Okupansi (Occupancy Rate)
- Persentase slot parkir yang terisi
- Formula: (Slot Terisi / Total Slot) × 100%
- Indikator efisiensi penggunaan ruang

### 3. Total Kendaraan Selesai
- Jumlah kendaraan yang berhasil parkir dan keluar
- Indikator kapasitas sistem

### 4. Efisiensi Sistem
- Formula: Total Kendaraan Selesai / Rata-rata Waktu Pencarian
- Semakin tinggi semakin baik
- Mengukur throughput sistem

---

## 🔍 Interpretasi Hasil

### Skenario Optimal

**Untuk Influx Rate:**
- Influx rate terlalu rendah → kapasitas tidak terpakai optimal
- Influx rate terlalu tinggi → waktu pencarian sangat lama, kemacetan
- **Optimal:** Influx rate yang menghasilkan efisiensi maksimal

**Untuk Durasi Parkir:**
- Durasi terlalu pendek → turnover tinggi, tapi mungkin tidak realistis
- Durasi terlalu panjang → okupansi tinggi, waktu pencarian meningkat
- **Pertimbangan:** Sesuaikan dengan pola penggunaan nyata

**Untuk Area Parkir:**
- Area tambahan → investasi lebih, tapi layanan lebih baik
- **Trade-off:** Biaya vs. Kualitas layanan

---

## 💡 Kesimpulan dan Rekomendasi

### Temuan Utama

1. **Jumlah Kendaraan:**
   - Ada batas optimal untuk influx rate
   - Di atas batas, sistem menjadi tidak efisien

2. **Durasi Parkir:**
   - Durasi parkir mempengaruhi okupansi dan waktu pencarian
   - Perlu keseimbangan antara durasi realistis dan efisiensi

3. **Kapasitas Parkir:**
   - Penambahan area parkir meningkatkan performa
   - Perlu analisis cost-benefit

### Rekomendasi

1. **Monitoring Real-time:** Implementasi sistem monitoring untuk mengukur influx rate aktual
2. **Manajemen Durasi:** Pertimbangkan pembatasan durasi parkir untuk meningkatkan turnover
3. **Ekspansi Strategis:** Analisis kebutuhan sebelum menambah area parkir
4. **Optimasi Layout:** Desain layout yang meminimalkan waktu pencarian

---

## 📝 Struktur Kode

```
simulasi_parkir_kampus.py
├── CellType (Enum)          # Tipe sel pada grid
├── VehicleStatus (Enum)     # Status kendaraan
├── Vehicle (Class)          # Kelas agen kendaraan
│   ├── __init__()          # Inisialisasi kendaraan
│   ├── update()            # Update status dan posisi
│   ├── _move_searching()   # Logika mencari parkir
│   ├── _wait_parking()     # Logika menunggu parkir
│   └── _move_leaving()     # Logika keluar
├── ParkingSimulation (Class) # Kelas utama simulasi
│   ├── __init__()          # Inisialisasi simulasi
│   ├── _setup_grid()       # Setup grid dan layout
│   ├── step()              # Satu langkah simulasi
│   └── _update_metrics()   # Update metrik
├── visualize_simulation()   # Fungsi visualisasi
├── experiment_influx_rate() # Eksperimen 1
├── experiment_parking_duration() # Eksperimen 2
└── experiment_extra_parking()    # Eksperimen 3
```

---

## 🎓 Untuk Presentasi

### Poin-poin Penting

1. **Model Agent-Based:**
   - Setiap kendaraan adalah agen yang independen
   - Perilaku agen menghasilkan pola sistem yang kompleks

2. **Parameter yang Dapat Dikontrol:**
   - Influx rate (jumlah kendaraan)
   - Durasi parkir
   - Kapasitas parkir

3. **Metrik yang Dapat Diukur:**
   - Waktu pencarian
   - Tingkat okupansi
   - Efisiensi sistem

4. **Aplikasi Praktis:**
   - Dapat digunakan untuk perencanaan area parkir
   - Evaluasi kebijakan parkir
   - Optimasi layout parkir

### Tips Presentasi

1. **Mulai dengan Visualisasi:** Tunjukkan animasi simulasi untuk menarik perhatian
2. **Jelaskan Model:** Gunakan diagram untuk menjelaskan arsitektur
3. **Tunjukkan Eksperimen:** Presentasikan hasil ketiga eksperimen dengan grafik
4. **Diskusikan Temuan:** Jelaskan interpretasi hasil dan rekomendasi
5. **Q&A:** Siapkan jawaban untuk pertanyaan tentang:
   - Validasi model
   - Aplikasi ke kasus nyata
   - Keterbatasan model

---

## ⚠️ Keterbatasan Model

1. **Pergerakan Sederhana:** Kendaraan bergerak secara random, tidak menggunakan algoritma pathfinding
2. **Tidak Ada Prioritas:** Semua kendaraan memiliki prioritas yang sama
3. **Grid Diskrit:** Pergerakan terbatas pada grid, tidak kontinyu
4. **Tidak Ada Interaksi Kompleks:** Tidak ada interaksi antara kendaraan (tabrakan, antrian, dll)
5. **Parameter Tetap:** Beberapa parameter tidak berubah selama simulasi

---

## 🔮 Pengembangan Selanjutnya

1. **Pathfinding:** Implementasi algoritma A* atau Dijkstra untuk pergerakan lebih realistis
2. **Prioritas Parkir:** Sistem prioritas berdasarkan waktu kedatangan atau tipe kendaraan
3. **Zona Parkir:** Multiple zona dengan karakteristik berbeda
4. **Event-driven:** Simulasi berdasarkan event (jam masuk/keluar kuliah)
5. **Validasi:** Perbandingan dengan data real dari kampus

---

## 📞 Kontak dan Informasi

**Mata Kuliah:** Pemodelan, Simulasi, dan Optimasi (PSO)  
**Tipe Model:** Agent-Based Simulation  
**Bahasa:** Python 3  
**Library:** NumPy, Matplotlib

---

## 📄 Lisensi

Proyek ini dibuat untuk keperluan akademik.

---

**Selamat Presentasi! 🎉**

