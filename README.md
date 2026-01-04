# LAPORAN TUGAS BESAR
## Simulasi Penggunaan Ruang Parkir di Area Kampus

**Mata Kuliah:** Pemodelan, Simulasi, dan Optimasi  
**Topik:** Simulasi Penggunaan Ruang Parkir di Area Kampus

---

## 1. Pendahuluan

### 1.1 Latar Belakang

Keterbatasan lahan parkir di area kampus sering menimbulkan masalah waktu dan kemacetan internal. Fenomena ini terjadi karena ketidakseimbangan antara jumlah kendaraan yang masuk dengan kapasitas slot parkir yang tersedia. Untuk memahami dan menganalisis dinamika sistem parkir kampus, diperlukan model simulasi yang dapat memodelkan interaksi antara kendaraan (agen) dengan lingkungan parkir.

### 1.2 Tujuan

Tujuan dari penelitian ini adalah:
1. Menganalisis dinamika pencarian parkir di area kampus
2. Menganalisis distribusi penggunaan lahan parkir
3. Mengevaluasi pengaruh berbagai parameter terhadap performa sistem parkir
4. Memberikan rekomendasi untuk optimasi penggunaan ruang parkir

### 1.3 Ruang Lingkup

Simulasi ini menggunakan pendekatan agent-based modeling dimana:
- **Agen** merepresentasikan kendaraan yang masuk, mencari parkir, parkir, dan keluar
- **Patch** merepresentasikan slot parkir dan jalan pada grid 2D
- Sistem mensimulasikan pergerakan kendaraan dalam mencari slot parkir terdekat
- Setiap kendaraan memiliki durasi parkir tertentu sebelum keluar

---

## 2. Desain Model dan Asumsi

### 2.1 Arsitektur Model

Model menggunakan pendekatan **Agent-Based Modeling (ABM)** dengan komponen utama:

#### 2.1.1 Grid Environment
- Grid 2D berukuran 33x33 cells
- Setiap cell dapat berupa:
  - **Grass** (area hijau/rumput): area yang tidak dapat dilalui
  - **Road** (jalan): area yang dapat dilalui kendaraan
  - **Parking** (slot parkir kosong): slot parkir yang tersedia
  - **Occupied** (slot parkir terisi): slot parkir yang sedang digunakan

#### 2.1.2 Agent (Kendaraan)
Setiap kendaraan memiliki atribut:
- **Posisi (x, y)**: koordinat pada grid
- **Status**: `SEARCHING`, `PARKED`, atau `LEAVING`
- **search_timer**: waktu yang dihabiskan untuk mencari parkir
- **park_timer**: waktu yang sudah dihabiskan di tempat parkir
- **target_duration**: durasi parkir yang ditentukan secara random

#### 2.1.3 Layout Parkir
- **Jalan melingkar**: mengelilingi area parkir (ring road)
- **Jalan masuk**: di bagian bawah grid (entrance)
- **Area parkir utama**: di tengah grid dengan pola setiap 3 kolom
- **Area parkir tambahan** (opsional): di sisi kanan grid

### 2.2 Asumsi Model

1. **Pergerakan Kendaraan**:
   - Kendaraan bergerak dalam 4 arah (atas, bawah, kiri, kanan)
   - Kendaraan mencari slot parkir kosong di sekitar posisinya
   - Jika tidak ada slot kosong, kendaraan bergerak secara random mencari parkir

2. **Masuk Kendaraan**:
   - Probabilitas masuk kendaraan baru setiap tick ditentukan oleh parameter `influx_rate` (0-100%)
   - Kendaraan baru masuk dari jalan masuk di bagian bawah grid

3. **Durasi Parkir**:
   - Durasi parkir setiap kendaraan bersifat random
   - Durasi = `base_parking_time` + random(0, `parking_duration_var`)
   - Setelah durasi habis, kendaraan langsung keluar

4. **Pencarian Parkir**:
   - Kendaraan mencari slot parkir kosong di tetangga terdekat (4 arah)
   - Jika ditemukan, kendaraan langsung parkir di slot tersebut
   - Waktu pencarian dihitung dari masuk hingga menemukan slot parkir

5. **Keluar Kendaraan**:
   - Setelah durasi parkir habis, kendaraan mencari jalan terdekat
   - Kendaraan keluar melalui jalan masuk di bagian bawah grid
   - Slot parkir dibebaskan saat kendaraan mulai keluar

### 2.3 Parameter Model

| Parameter | Deskripsi | Nilai Default | Range |
|-----------|-----------|---------------|-------|
| `grid_size` | Ukuran grid | 33 | - |
| `influx_rate` | Probabilitas masuk kendaraan baru (%) | 63 | 0-100 |
| `parking_duration_var` | Variasi durasi parkir | 410 | 0-1000 |
| `base_parking_time` | Durasi parkir minimum | 185 | 10-500 |
| `add_extra_parking` | Tambah area parkir ekstra | False | True/False |

---

## 3. Implementasi Model

### 3.1 Teknologi yang Digunakan

- **Python 3.x**: Bahasa pemrograman utama
- **NumPy**: Untuk manipulasi array dan grid
- **Matplotlib**: Untuk visualisasi dan plotting
- **Random**: Untuk simulasi probabilistik

### 3.2 Struktur Kode

#### 3.2.1 Class `CellType` (Enum)
Mendefinisikan tipe-tipe cell pada grid:
```python
class CellType(Enum):
    GRASS = 0
    ROAD = 1
    PARKING = 2
    OCCUPIED = 3
```

#### 3.2.2 Class `VehicleStatus` (Enum)
Mendefinisikan status kendaraan:
```python
class VehicleStatus(Enum):
    SEARCHING = "searching"
    PARKED = "parked"
    LEAVING = "leaving"
```

#### 3.2.3 Class `Vehicle`
Merepresentasikan kendaraan sebagai agen:
- `update()`: Update status dan posisi kendaraan setiap tick
- `_move_searching()`: Logika pergerakan saat mencari parkir
- `_wait_parking()`: Logika menunggu di tempat parkir
- `_move_leaving()`: Logika pergerakan saat keluar

#### 3.2.4 Class `ParkingSimulation`
Kelas utama untuk simulasi:
- `_setup_grid()`: Inisialisasi grid dengan jalan dan area parkir
- `step()`: Satu langkah simulasi (generate kendaraan, update kendaraan, update metrics)
- `_update_metrics()`: Update metrics seperti waktu pencarian, okupansi, dll
- `get_grid_for_visualization()`: Prepare grid untuk visualisasi

### 3.3 Alur Simulasi

1. **Inisialisasi**:
   - Setup grid dengan jalan dan area parkir
   - Inisialisasi metrics dan history

2. **Loop Simulasi** (setiap tick):
   - Generate kendaraan baru berdasarkan `influx_rate`
   - Update semua kendaraan yang ada:
     - Jika `SEARCHING`: cari slot parkir kosong di sekitar
     - Jika `PARKED`: increment `park_timer`, jika sudah mencapai `target_duration` ubah ke `LEAVING`
     - Jika `LEAVING`: bergerak ke jalan dan keluar
   - Update metrics (waktu pencarian, okupansi, dll)
   - Update history untuk plotting

3. **Visualisasi**:
   - Grid parkir dengan warna berbeda untuk setiap tipe cell
   - Grafik jumlah kendaraan (total dan parkir)
   - Grafik tingkat okupansi
   - Grafik rata-rata waktu pencarian

### 3.4 Metrics yang Diukur

1. **Waktu Pencarian Parkir**: Rata-rata waktu yang dihabiskan kendaraan untuk mencari slot parkir
2. **Tingkat Okupansi**: Persentase slot parkir yang terisi
3. **Total Kendaraan**: Jumlah kendaraan aktif, parkir, dan selesai
4. **Efisiensi**: Rasio kendaraan selesai terhadap waktu pencarian

---

## 4. Hasil Eksperimen dan Analisis

### 4.1 Eksperimen 1: Pengaruh Jumlah Kendaraan (Influx Rate)

**Tujuan**: Menganalisis pengaruh jumlah kendaraan yang masuk terhadap performa sistem parkir.

**Parameter**:
- `influx_rate`: 20%, 40%, 60%, 80%, 100%
- `num_steps`: 1000 ticks
- Parameter lain: default

**Hasil dan Analisis**:

1. **Waktu Pencarian Parkir**:
   - Semakin tinggi `influx_rate`, semakin tinggi waktu pencarian parkir
   - Pada `influx_rate` rendah (20-40%), waktu pencarian relatif stabil
   - Pada `influx_rate` tinggi (80-100%), terjadi peningkatan signifikan waktu pencarian karena kompetisi untuk slot parkir

2. **Tingkat Okupansi**:
   - Okupansi meningkat seiring dengan peningkatan `influx_rate`
   - Pada `influx_rate` tinggi, okupansi mendekati 100% (saturasi)
   - Terjadi keseimbangan dinamis antara kendaraan masuk dan keluar

3. **Total Kendaraan Selesai**:
   - Jumlah kendaraan yang berhasil parkir dan keluar meningkat dengan `influx_rate`
   - Namun, efisiensi (kendaraan selesai per waktu pencarian) menurun pada `influx_rate` tinggi

**Kesimpulan Eksperimen 1**:
- Sistem parkir memiliki kapasitas optimal pada `influx_rate` 60-70%
- Di atas kapasitas optimal, terjadi penurunan efisiensi dan peningkatan waktu pencarian
- Perlu manajemen masuk kendaraan untuk menghindari saturasi

### 4.2 Eksperimen 2: Pengaruh Durasi Parkir

**Tujuan**: Menganalisis pengaruh durasi parkir terhadap performa sistem.

**Parameter**:
- `parking_duration_var`: 200, 300, 400, 500, 600
- `influx_rate`: 63% (default)
- `num_steps`: 1000 ticks

**Hasil dan Analisis**:

1. **Waktu Pencarian Parkir**:
   - Durasi parkir yang lebih lama menyebabkan waktu pencarian meningkat
   - Hal ini karena slot parkir lebih lama terisi, mengurangi ketersediaan slot kosong

2. **Tingkat Okupansi**:
   - Okupansi meningkat dengan durasi parkir yang lebih lama
   - Durasi parkir yang lebih lama menyebabkan akumulasi kendaraan di area parkir

3. **Total Kendaraan Selesai**:
   - Durasi parkir yang lebih lama mengurangi jumlah kendaraan yang dapat dilayani
   - Sistem menjadi kurang efisien dalam melayani kendaraan baru

**Kesimpulan Eksperimen 2**:
- Durasi parkir yang lebih lama mengurangi kapasitas sistem
- Perlu kebijakan untuk membatasi durasi parkir atau meningkatkan kapasitas
- Distribusi durasi parkir yang lebih pendek dapat meningkatkan throughput sistem

### 4.3 Eksperimen 3: Pengaruh Penambahan Area Parkir

**Tujuan**: Menganalisis efektivitas penambahan area parkir tambahan.

**Parameter**:
- `add_extra_parking`: False vs True
- `influx_rate`: 63% (default)
- `num_steps`: 1000 ticks

**Hasil dan Analisis**:

1. **Waktu Pencarian Parkir**:
   - Penambahan area parkir mengurangi waktu pencarian parkir
   - Ketersediaan slot parkir yang lebih banyak memudahkan kendaraan menemukan slot

2. **Tingkat Okupansi**:
   - Dengan area parkir tambahan, okupansi lebih rendah karena lebih banyak slot tersedia
   - Sistem lebih mampu menampung kendaraan tanpa mencapai saturasi

3. **Total Kendaraan Selesai**:
   - Penambahan area parkir meningkatkan jumlah kendaraan yang dapat dilayani
   - Sistem menjadi lebih efisien dalam melayani kendaraan

**Kesimpulan Eksperimen 3**:
- Penambahan area parkir secara signifikan meningkatkan performa sistem
- Investasi dalam penambahan area parkir dapat mengurangi waktu pencarian dan meningkatkan kapasitas
- Perlu analisis cost-benefit untuk menentukan optimalitas penambahan area parkir

### 4.4 Analisis Keseluruhan

Berdasarkan ketiga eksperimen, dapat disimpulkan:

1. **Faktor Kritis**:
   - Jumlah kendaraan yang masuk (`influx_rate`) adalah faktor paling kritis
   - Durasi parkir mempengaruhi kapasitas sistem
   - Kapasitas area parkir menentukan batas maksimal sistem

2. **Optimasi Sistem**:
   - Sistem optimal pada `influx_rate` 60-70% dengan durasi parkir sedang
   - Penambahan area parkir efektif untuk meningkatkan kapasitas
   - Perlu manajemen masuk kendaraan untuk menghindari saturasi

3. **Rekomendasi**:
   - Implementasi sistem reservasi atau pembatasan masuk pada jam sibuk
   - Penambahan area parkir jika okupansi konsisten di atas 80%
   - Kebijakan durasi parkir maksimal untuk meningkatkan turnover

---

## 5. Kesimpulan

### 5.1 Ringkasan

Simulasi agent-based untuk sistem parkir kampus berhasil memodelkan dinamika pencarian parkir dan distribusi penggunaan lahan parkir. Model ini dapat menganalisis pengaruh berbagai parameter terhadap performa sistem dan memberikan insight untuk optimasi.

### 5.2 Temuan Utama

1. **Jumlah Kendaraan**: Faktor paling kritis yang mempengaruhi waktu pencarian dan okupansi
2. **Durasi Parkir**: Mempengaruhi kapasitas dan throughput sistem
3. **Kapasitas Parkir**: Penambahan area parkir secara signifikan meningkatkan performa sistem

### 5.3 Keterbatasan Model

1. **Pergerakan Kendaraan**: Model menggunakan pergerakan random, tidak mempertimbangkan rute optimal
2. **Interaksi Kendaraan**: Tidak memodelkan kemacetan atau antrian di jalan
3. **Faktor Eksternal**: Tidak mempertimbangkan faktor cuaca, event khusus, dll
4. **Distribusi Waktu**: Durasi parkir menggunakan distribusi uniform, tidak mempertimbangkan distribusi real-world

### 5.4 Saran Pengembangan

1. **Algoritma Pencarian**: Implementasi algoritma pencarian yang lebih cerdas (A*, Dijkstra)
2. **Model Kemacetan**: Penambahan model kemacetan dan antrian di jalan
3. **Distribusi Realistik**: Penggunaan distribusi durasi parkir yang lebih realistis (normal, exponential)
4. **Multi-Entrance**: Penambahan multiple entrance/exit points
5. **Sistem Reservasi**: Implementasi sistem reservasi parkir

### 5.5 Kontribusi

Model ini dapat digunakan untuk:
- Analisis kapasitas parkir kampus
- Perencanaan penambahan area parkir
- Evaluasi kebijakan manajemen parkir
- Optimasi layout parkir

---

## 6. Cara Menjalankan Program

### 6.1 Instalasi Dependencies

```bash
pip install numpy matplotlib pillow
```

Atau menggunakan requirements.txt:
```bash
pip install -r requirements.txt
```

### 6.2 Menjalankan Simulasi

#### Mode 1: Simulasi dengan Visualisasi
```bash
python simulasi_parkir_kampus.py 1
```
Menampilkan visualisasi real-time dengan animasi.

#### Mode 2: Semua Eksperimen
```bash
python simulasi_parkir_kampus.py 2
```
Menjalankan semua eksperimen dan menghasilkan grafik analisis.

#### Mode 3-5: Eksperimen Spesifik
```bash
python simulasi_parkir_kampus.py 3  # Eksperimen 1: Jumlah Kendaraan
python simulasi_parkir_kampus.py 4  # Eksperimen 2: Durasi Parkir
python simulasi_parkir_kampus.py 5  # Eksperimen 3: Area Parkir Tambahan
```

### 6.3 Output

Program akan menghasilkan:
- **Visualisasi real-time**: Grid parkir dengan pergerakan kendaraan
- **Grafik metrics**: Jumlah kendaraan, okupansi, waktu pencarian
- **Grafik eksperimen**: Analisis pengaruh parameter (PNG files)

---

## 7. Referensi

1. Wilensky, U. (1999). NetLogo. http://ccl.northwestern.edu/netlogo/
2. Railsback, S. F., & Grimm, V. (2011). Agent-based and individual-based modeling: a practical introduction. Princeton University Press.
3. Helbing, D., & Molnár, P. (1995). Social force model for pedestrian dynamics. Physical review E, 51(5), 4282.

---

## Lampiran

### File Program
- `simulasi_parkir_kampus.py`: File utama program simulasi
- `requirements.txt`: Dependencies yang diperlukan

### Output Eksperimen
- `experiment_influx_rate.png`: Hasil eksperimen jumlah kendaraan
- `experiment_duration.png`: Hasil eksperimen durasi parkir
- `experiment_extra_parking.png`: Hasil eksperimen area parkir tambahan

---

**Disusun oleh:** [Nama Kelompok]  
**Tanggal:** [Tanggal]  
**Versi:** 1.0
