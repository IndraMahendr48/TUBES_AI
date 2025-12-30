# 📚 Panduan Presentasi - Analisis Kompleksitas Algoritma
## Pencarian Median pada Array Terurut

**Kelompok:**
- Indra (Presenter & Demo)
- Tubagus (Penjelasan Algoritma)
- Vandin (Analisis Kompleksitas & Hasil)

---

## 📋 Daftar Isi

1. [Struktur Program & Nama Fungsi](#struktur-program--nama-fungsi)
2. [Penjelasan Algoritma Iteratif (Detail)](#penjelasan-algoritma-iteratif-detail)
3. [Penjelasan Algoritma Rekursif (Detail)](#penjelasan-algoritma-rekursif-detail)
4. [Analisis Operasi Dasar & Hitungan](#analisis-operasi-dasar--hitungan)
5. [Jawaban untuk Pertanyaan Dosen](#jawaban-untuk-pertanyaan-dosen)
6. [Script Presentasi](#script-presentasi)

---

## 🏗️ Struktur Program & Nama Fungsi

### Class: `MedianAnalyzer`

Program ini menggunakan satu class utama yaitu `MedianAnalyzer` yang berisi:

#### 1. `__init__(self)`
- **Fungsi**: Inisialisasi object, membuat dictionary untuk menyimpan hasil
- **Parameter**: `self`
- **Return**: Tidak ada (constructor)
- **Isi**: Membuat dictionary `results` dengan keys: sizes, iterative_times, recursive_times, iterative_results, recursive_results

#### 2. `generate_sorted_array(self, n)`
- **Fungsi**: Generate array terurut untuk pengujian
- **Parameter**: 
  - `n` (int): Ukuran array yang ingin dibuat
- **Return**: `arr` (list): Array terurut berisi angka random 1-10000
- **Penjelasan**: 
  - Generate n buah angka random antara 1-10000
  - Array diurutkan menggunakan `sorted()`
  - Contoh: n=5 → [123, 456, 789, 1234, 5678]

#### 3. `median_iterative(self, arr)`
- **Fungsi**: Menghitung median dengan pendekatan iteratif menggunakan loop
- **Parameter**: 
  - `arr` (list): Array terurut
- **Return**: `float` atau `None`: Nilai median
- **Kompleksitas**: O(N) waktu, O(1) ruang
- **Ini adalah ALGORITMA ITERATIF yang akan dijelaskan detail**

#### 4. `median_recursive(self, arr)`
- **Fungsi**: Menghitung median dengan pendekatan rekursif
- **Parameter**: 
  - `arr` (list): Array terurut
- **Return**: `float` atau `None`: Nilai median
- **Kompleksitas**: O(N) waktu, O(N) ruang
- **Mengandung helper function**: `find_element(current_index, target_idx)`
- **Ini adalah ALGORITMA REKURSIF yang akan dijelaskan detail**

#### 5. `run_benchmark(self, sizes, iterations)`
- **Fungsi**: Menjalankan pengujian benchmark untuk berbagai ukuran input
- **Parameter**: 
  - `sizes` (list): List ukuran input yang akan diuji
  - `iterations` (int): Jumlah iterasi per ukuran (default: 10)
- **Return**: Tidak ada (menyimpan hasil ke `self.results`)
- **Penjelasan**: 
  - Untuk setiap ukuran n, generate array dan test kedua algoritma
  - Setiap test diulang beberapa kali untuk akurasi
  - Simpan rata-rata waktu eksekusi

#### 6. `plot_graph(self, save_path)`
- **Fungsi**: Membuat grafik perbandingan waktu eksekusi
- **Parameter**: 
  - `save_path` (str): Path untuk menyimpan file grafik
- **Return**: Tidak ada (menyimpan file PNG)

#### 7. `export_results_to_file(self, filename)`
- **Fungsi**: Mengekspor hasil ke file teks
- **Parameter**: 
  - `filename` (str): Nama file output
- **Return**: Tidak ada (menulis file)

---

## 🔵 Penjelasan Algoritma Iteratif (Detail)

### Kode Lengkap:
```python
def median_iterative(self, arr):
    n = len(arr)                    # Operasi 1: Hitung panjang array
    
    # Base case: array kosong
    if n == 0:                      # Operasi 2: Perbandingan
        return None
    
    # Base case: array dengan 1 elemen
    if n == 1:                      # Operasi 3: Perbandingan
        return arr[0]               # Operasi 4: Akses array
    
    # Iteratif: menggunakan loop untuk iterate sampai ke elemen tengah
    middle_index = n // 2           # Operasi 5: Pembagian integer
    
    # Loop dari 0 sampai middle_index (inclusive)
    current_val = None
    for i in range(middle_index + 1):  # Loop: dari 0 sampai middle_index
        current_val = arr[i]        # Operasi dalam loop: Akses array
    
    # Jika jumlah elemen genap, median adalah rata-rata dua elemen tengah
    if n % 2 == 0:                  # Operasi: Modulo (mengecek ganjil/genap)
        val1 = arr[middle_index - 1]  # Operasi: Pengurangan, akses array
        val2 = current_val            # Operasi: Assignment
        return (val1 + val2) / 2.0    # Operasi: Penjumlahan, pembagian
    else:
        # Jika ganjil, median adalah elemen tengah
        return float(current_val)     # Operasi: Konversi
```

### 🔍 Penjelasan Step-by-Step dengan Contoh:

#### **Contoh 1: Array Ganjil (n = 5)**
Array: `[10, 20, 30, 40, 50]` (indeks: 0, 1, 2, 3, 4)

**Step 1:** `n = len(arr)`
- `n = 5`
- Operasi: 1 kali perhitungan panjang array

**Step 2:** Check base case
- `if n == 0:` → False, lanjut
- `if n == 1:` → False, lanjut
- Operasi: 2 kali perbandingan

**Step 3:** Hitung indeks tengah
- `middle_index = n // 2`
- `middle_index = 5 // 2 = 2`
- Operasi: 1 kali pembagian integer

**Step 4:** Loop sampai middle_index
- `for i in range(middle_index + 1):` → `for i in range(3):` → i = 0, 1, 2
- **Iterasi 1:** i=0, `current_val = arr[0]` = 10
- **Iterasi 2:** i=1, `current_val = arr[1]` = 20
- **Iterasi 3:** i=2, `current_val = arr[2]` = 30
- Operasi dalam loop: 3 kali akses array
- Operasi loop: 3 kali perbandingan (check kondisi loop), 3 kali increment

**Step 5:** Check ganjil/genap
- `if n % 2 == 0:` → `if 5 % 2 == 0:` → False (ganjil)
- Operasi: 1 kali modulo

**Step 6:** Return elemen tengah
- `return float(current_val)`
- `return float(30)`
- `return 30.0`
- Operasi: 1 kali konversi float

**Total Operasi:**
- Perbandingan: 2 (base case) + 3 (loop condition) = 5
- Aritmatika: 1 (pembagian), 1 (modulo), 3 (increment i)
- Akses array: 3 (dalam loop)
- Konversi: 1
- **Total: ~13 operasi dasar**
- **Jumlah iterasi loop: 3 = (n//2 + 1) = (5//2 + 1)**
- **Kompleksitas: O(N) - linear, bergantung pada n**

---

#### **Contoh 2: Array Genap (n = 6)**
Array: `[10, 20, 30, 40, 50, 60]` (indeks: 0, 1, 2, 3, 4, 5)

**Step 1:** `n = len(arr)`
- `n = 6`
- Operasi: 1

**Step 2:** Check base case
- `if n == 0:` → False
- `if n == 1:` → False
- Operasi: 2 perbandingan

**Step 3:** Hitung indeks tengah
- `middle_index = 6 // 2 = 3`
- Operasi: 1 pembagian

**Step 4:** Loop sampai middle_index
- `for i in range(middle_index + 1):` → `for i in range(4):` → i = 0, 1, 2, 3
- **Iterasi 1:** i=0, `current_val = arr[0]` = 10
- **Iterasi 2:** i=1, `current_val = arr[1]` = 20
- **Iterasi 3:** i=2, `current_val = arr[2]` = 30
- **Iterasi 4:** i=3, `current_val = arr[3]` = 40
- Operasi dalam loop: 4 kali akses array
- Operasi loop: 4 kali perbandingan (check kondisi loop), 4 kali increment

**Step 5:** Check ganjil/genap
- `if n % 2 == 0:` → `if 6 % 2 == 0:` → True (genap)
- Operasi: 1 modulo

**Step 6:** Ambil dua elemen tengah dan rata-rata
- `val1 = arr[middle_index - 1]` → `arr[2]` = 30
- `val2 = current_val` → 40
- Operasi: 1 pengurangan, 1 akses array, 1 assignment
- `return (val1 + val2) / 2.0` → `(30 + 40) / 2.0` → `35.0`
- Operasi: 1 penjumlahan, 1 pembagian

**Total Operasi:**
- Perbandingan: 2 (base case) + 4 (loop condition) = 6
- Aritmatika: 1 pembagian, 1 modulo, 1 pengurangan, 1 penjumlahan, 1 pembagian, 4 increment
- Akses array: 4 (dalam loop) + 1 (val1) = 5
- Assignment: 1
- **Total: ~17 operasi dasar**
- **Jumlah iterasi loop: 4 = (n//2 + 1) = (6//2 + 1)**
- **Kompleksitas: O(N) - linear, bergantung pada n**

---

### 📊 Analisis Kompleksitas Iteratif:

**Kompleksitas Waktu: O(N)**
- **Best Case:** O(N) - loop berjalan (n//2 + 1) kali
- **Average Case:** O(N) - loop berjalan (n//2 + 1) kali
- **Worst Case:** O(N) - loop berjalan (n//2 + 1) kali
- **Ada loop!** Loop berjalan dari 0 sampai middle_index
- Jumlah operasi **BERGANTUNG** pada ukuran array n
- Sum of operations: `T(n) = Σ(i=0 to n//2) 1 = n/2 + 1 = O(n)`

**Kompleksitas Ruang: O(1)**
- Hanya menggunakan variabel lokal: `n`, `middle_index`, `current_val`, `val1`, `val2`
- Ruang yang digunakan konstan, tidak bergantung pada n
- Tidak ada struktur data tambahan yang bergantung pada n

**Mengapa O(N)?**
- Ada loop `for i in range(middle_index + 1)`
- Loop berjalan sebanyak (n//2 + 1) kali ≈ n/2 kali
- Dalam setiap iterasi ada operasi akses array
- Total operasi proporsional dengan n: O(n/2) = O(N)
- Bisa dihitung dengan sum of operations

---

## 🟠 Penjelasan Algoritma Rekursif (Detail)

### Kode Lengkap:
```python
def median_recursive(self, arr):
    n = len(arr)                    # Operasi 1
    
    # Base case: array kosong
    if n == 0:                      # Operasi 2
        return None
    
    # Base case: array dengan 1 elemen
    if n == 1:                      # Operasi 3
        return float(arr[0])        # Operasi 4
    
    target_index = n // 2           # Operasi 5
    
    # Helper function rekursif
    def find_element(current_index, target_idx):
        # Base case: sudah mencapai indeks target
        if current_index == target_idx:  # Operasi: perbandingan setiap rekursi
            return arr[current_index]     # Operasi: akses array
        # Recursive case: panggil diri sendiri dengan indeks berikutnya
        return find_element(current_index + 1, target_idx)  # Rekursi!
    
    # Panggil fungsi rekursif
    result = find_element(0, target_index)  # Mulai dari index 0
    
    # Jika jumlah elemen genap, hitung rata-rata dua elemen tengah
    if n % 2 == 0:                  # Operasi
        val1 = arr[target_index - 1]  # Operasi
        val2 = result                 # Operasi
        return (val1 + val2) / 2.0    # Operasi
    else:
        return float(result)          # Operasi
```

### 🔍 Penjelasan Step-by-Step dengan Contoh:

#### **Contoh 1: Array Ganjil (n = 5)**
Array: `[10, 20, 30, 40, 50]` (indeks: 0, 1, 2, 3, 4)
Target: `target_index = 5 // 2 = 2`

**Trace Rekursi `find_element(0, 2)`:**
```
Call 1: find_element(current_index=0, target_idx=2)
  ├─ Check: 0 == 2? → False
  └─ Rekursi: find_element(0 + 1, 2) = find_element(1, 2)
  
Call 2: find_element(current_index=1, target_idx=2)
  ├─ Check: 1 == 2? → False
  └─ Rekursi: find_element(1 + 1, 2) = find_element(2, 2)
  
Call 3: find_element(current_index=2, target_idx=2)
  ├─ Check: 2 == 2? → True ✓ (BASE CASE)
  └─ Return: arr[2] = 30
```

**Detail Operasi per Call:**

**Call 1:**
- Operasi: 1 perbandingan (`0 == 2`)
- Operasi: 1 penjumlahan (`0 + 1`)
- Operasi: 1 pemanggilan fungsi (overhead)

**Call 2:**
- Operasi: 1 perbandingan (`1 == 2`)
- Operasi: 1 penjumlahan (`1 + 1`)
- Operasi: 1 pemanggilan fungsi (overhead)

**Call 3:**
- Operasi: 1 perbandingan (`2 == 2`)
- Operasi: 1 akses array (`arr[2]`)
- Return: 30

**Total untuk find_element:**
- Jumlah panggilan: 3 (untuk n=5, target_index=2)
- Operasi per call: ~3 operasi
- **Total operasi: 3 × 3 = 9 operasi**
- **Stack frame: 3 frame** (memakan memori)

**Kembali ke median_recursive:**
- Check: `if n % 2 == 0` → False (ganjil)
- Return: `float(30)` = 30.0

**Total Operasi Rekursif:**
- `len(arr)`: 1
- Perbandingan base case: 2
- `target_index`: 1
- Rekursi find_element: 9 operasi
- Perbandingan ganjil/genap: 1
- Return: 1
- **Total: ~15 operasi** (lebih banyak dari iteratif!)

---

#### **Contoh 2: Array Genap (n = 6)**
Array: `[10, 20, 30, 40, 50, 60]` (indeks: 0, 1, 2, 3, 4, 5)
Target: `target_index = 6 // 2 = 3`

**Trace Rekursi `find_element(0, 3)`:**
```
Call 1: find_element(0, 3)
  ├─ 0 == 3? → False
  └─ Rekursi: find_element(1, 3)

Call 2: find_element(1, 3)
  ├─ 1 == 3? → False
  └─ Rekursi: find_element(2, 3)

Call 3: find_element(2, 3)
  ├─ 2 == 3? → False
  └─ Rekursi: find_element(3, 3)

Call 4: find_element(3, 3)
  ├─ 3 == 3? → True ✓ (BASE CASE)
  └─ Return: arr[3] = 40
```

**Total:**
- Jumlah panggilan: 4 (untuk n=6, target_index=3)
- Operasi rekursi: 4 × 3 = 12 operasi
- **Stack frame: 4 frame**

**Kembali ke median_recursive:**
- Check: `if n % 2 == 0` → True (genap)
- `val1 = arr[3 - 1]` = `arr[2]` = 30
- `val2 = 40` (dari result)
- Return: `(30 + 40) / 2.0` = 35.0

---

### 📊 Analisis Kompleksitas Rekursif:

**Kompleksitas Waktu: O(N)**

**Mengapa O(N)?**
- Fungsi `find_element` dipanggil sebanyak `target_index` kali
- `target_index = n // 2`
- Untuk n besar, `n // 2 ≈ n/2 ≈ O(n)`
- Setiap panggilan melakukan operasi konstan O(1)
- Total: O(n) × O(1) = **O(N)**

**Contoh hitungan:**
- n = 10 → target_index = 5 → 5 panggilan
- n = 100 → target_index = 50 → 50 panggilan
- n = 1000 → target_index = 500 → 500 panggilan
- **Proporsional dengan n!**

**Kompleksitas Ruang: O(N)**

**Mengapa O(N)?**
- Setiap panggilan rekursif menambahkan frame ke call stack
- Jumlah frame = jumlah panggilan = target_index ≈ n/2
- Setiap frame menyimpan:
  - Parameter: `current_index`, `target_idx`
  - Return address
  - Local variables
- Total ruang: O(n/2) = **O(N)**

**Stack Overflow:**
- Python default recursion limit: ~1000
- Untuk n > 2000, bisa terjadi RecursionError
- Oleh karena itu kita set `sys.setrecursionlimit(25000)`

---

## 🔢 Analisis Operasi Dasar & Hitungan

### Operasi Dasar dalam Pemrograman:

1. **Perbandingan** (`==`, `!=`, `<`, `>`, `<=`, `>=`)
2. **Aritmatika** (`+`, `-`, `*`, `/`, `//`, `%`)
3. **Akses Array** (`arr[index]`)
4. **Assignment** (`=`)
5. **Pemanggilan Fungsi** (overhead)
6. **Return** (overhead)

### Perbandingan Operasi Dasar:

#### **Algoritma Iteratif (n = 100):**

| Operasi | Jumlah | Keterangan |
|---------|--------|------------|
| `len(arr)` | 1 | O(1) |
| Perbandingan (`==`, `%`) | 3 + (n//2+1) | 2 base case + 1 ganjil/genap + loop condition |
| Pembagian (`//`) | 1 | Hitung middle_index |
| **Loop: for i in range(middle_index + 1)** | **(n//2 + 1) iterasi** | **n//2 + 1 = 51 kali** |
| - Akses Array dalam loop | n//2 + 1 | 51 kali akses arr[i] |
| - Increment i | n//2 + 1 | 51 kali increment |
| Akses Array (val1 jika genap) | 0-1 | Tergantung ganjil/genap |
| Aritmatika (`+`, `-`, `/`) | 0-2 | Tergantung ganjil/genap |
| **Total** | **~55-60** | **BERGANTUNG pada n** |

**Untuk n = 10:** ~7-8 operasi dalam loop = ~15 operasi total  
**Untuk n = 100:** ~51 operasi dalam loop = ~60 operasi total  
**Untuk n = 10000:** ~5001 operasi dalam loop = ~5010 operasi total  
**Kesimpulan: O(N) - Linear!**

**Sum of Operations:**
```
T(n) = Σ(i=0 to n//2) 1 = (n//2) + 1 = O(n)
```

---

#### **Algoritma Rekursif (n = 100):**

**Untuk n = 100:**
- `target_index = 100 // 2 = 50`
- Jumlah panggilan `find_element` = 50

| Operasi | Jumlah per Call | Total (50 calls) |
|---------|-----------------|------------------|
| Perbandingan (`==`) | 1 | 50 |
| Penjumlahan (`+1`) | 1 | 50 |
| Pemanggilan Fungsi | 1 | 50 (overhead) |
| Akses Array | 0-1 | 1 (hanya call terakhir) |
| **Total Operasi dalam Rekursi** | **~3** | **~150** |

**Plus operasi di median_recursive:**
- `len(arr)`: 1
- Perbandingan base case: 2
- `target_index`: 1
- Perbandingan ganjil/genap: 1
- Return/konversi: 1-2

**Total: ~156 operasi untuk n=100**

**Untuk n = 10:** ~16 operasi  
**Untuk n = 100:** ~156 operasi  
**Untuk n = 1000:** ~1506 operasi  
**Kesimpulan: O(N) - Linear!**

---

### Perbandingan Visual:

```
n = 10:
  Iteratif:  ~15 operasi  ███
  Rekursif:  ~16 operasi  ███

n = 100:
  Iteratif:  ~60 operasi  ████████████████
  Rekursif:  ~156 operasi █████████████████████████████████

n = 1000:
  Iteratif:  ~510 operasi ████████████████████████████████████████████████████
  Rekursif:  ~1506 operasi ███████████████████████████████████████████████████████████████████████████████████████████████████████████████████
```

**Kesimpulan:** Kedua algoritma memiliki kompleksitas waktu O(N), namun:
- **Iteratif:** Operasi lebih sedikit karena tidak ada overhead rekursi
- **Rekursif:** Operasi lebih banyak karena overhead pemanggilan fungsi

---

## ❓ Jawaban untuk Pertanyaan Dosen

### Pertanyaan 1: "Jelasin logika algoritma nya sama hitungannya"

#### **Jawaban untuk Algoritma Iteratif:**

**Logika:**
"Baik Pak, algoritma iteratif bekerja dengan cara menggunakan loop untuk iterate dari index 0 sampai middle_index (n//2). Loop ini akan mengakses setiap elemen array dari awal sampai elemen tengah. Setelah loop selesai, kita sudah mendapatkan nilai di middle_index. Jika genap, kita ambil juga elemen sebelumnya dan rata-rata."

**Hitungan (Contoh n=5):**
1. Hitung panjang: `n = 5`
2. Hitung indeks tengah: `middle_index = 5 // 2 = 2`
3. Loop: `for i in range(3)` → i = 0, 1, 2
   - Iterasi 1: i=0, akses arr[0]
   - Iterasi 2: i=1, akses arr[1]
   - Iterasi 3: i=2, akses arr[2]
4. Karena ganjil, return arr[2] = 30
5. **Total: ~13 operasi dasar (termasuk 3 operasi dalam loop), kompleksitas O(N)**

**Hitungan (Contoh n=6):**
1. `n = 6`, `middle_index = 6 // 2 = 3`
2. Loop: `for i in range(4)` → i = 0, 1, 2, 3 (4 iterasi)
   - Setiap iterasi: akses array
3. Karena genap, ambil arr[2] dan arr[3] (dari current_val)
4. Rata-rata: `(30 + 40) / 2 = 35`
5. **Total: ~17 operasi dasar (termasuk 4 operasi dalam loop), tetap O(N)**

**Sum of Operations:**
`T(n) = Σ(i=0 to n//2) 1 = (n//2) + 1 = O(n)`

---

#### **Jawaban untuk Algoritma Rekursif:**

**Logika:**
"Algoritma rekursif menggunakan fungsi helper `find_element` yang memanggil dirinya sendiri secara rekursif. Fungsi ini mulai dari index 0, kemudian setiap panggilan menaikkan index sampai mencapai target_index (yang adalah n//2). Ini seperti 'berjalan' melalui array secara rekursif sampai menemukan elemen tengah."

**Hitungan (Contoh n=5, target_index=2):**
1. `find_element(0, 2)`: Check 0==2? No → rekursi
2. `find_element(1, 2)`: Check 1==2? No → rekursi  
3. `find_element(2, 2)`: Check 2==2? Yes → return arr[2]=30
4. **Total: 3 panggilan, ~9 operasi, kompleksitas O(N)**

**Untuk n=100:**
- target_index = 50
- Jumlah panggilan = 50
- **Total: ~150 operasi, proporsional dengan n**

---

### Pertanyaan 2: "Ditanya kaya operasi dasar mana berapa dimana"

#### **Jawaban Lengkap:**

**Algoritma Iteratif - Operasi Dasar:**

| No | Operasi | Lokasi | Jumlah | Contoh (n=5, middle_index=2) |
|----|---------|--------|--------|------------------------------|
| 1 | `len(arr)` | Line 51 | 1 | `n = len([10,20,30,40,50])` = 5 |
| 2 | Perbandingan `==` | Line 54 | 1 | `if 5 == 0` → False |
| 3 | Perbandingan `==` | Line 58 | 1 | `if 5 == 1` → False |
| 4 | Pembagian `//` | Line 62 | 1 | `5 // 2 = 2` |
| 5 | **Loop: for i in range(3)** | Line (loop) | **3 iterasi** | **i = 0, 1, 2** |
| 5a | Perbandingan loop condition | Dalam loop | 3 | Check i < 3 |
| 5b | Increment i | Dalam loop | 3 | i++ |
| 5c | Akses Array `arr[i]` | Dalam loop | 3 | arr[0], arr[1], arr[2] |
| 6 | Modulo `%` | Line 65 | 1 | `5 % 2 = 1` |
| 7 | Konversi `float()` | Line 71 | 1 | `float(30) = 30.0` |

**Total: 1 + 2 + 1 + (3×3) + 1 + 1 = ~15 operasi (untuk n=5)**
**Untuk n=100: ~60 operasi, untuk n=1000: ~510 operasi**

**Algoritma Rekursif - Operasi Dasar:**

| No | Operasi | Lokasi | Jumlah | Contoh (n=5, target=2) |
|----|---------|--------|--------|------------------------|
| 1 | `len(arr)` | Line 90 | 1 | `n = 5` |
| 2-3 | Perbandingan base case | Line 93, 97 | 2 | `if 5 == 0`, `if 5 == 1` |
| 4 | Pembagian `//` | Line 100 | 1 | `5 // 2 = 2` |
| 5-7 | **Rekursi Call 1** | Line 112 | 3 | `find_element(0, 2)`: perbandingan, penjumlahan, call |
| 8-10 | **Rekursi Call 2** | Line 112 | 3 | `find_element(1, 2)`: perbandingan, penjumlahan, call |
| 11-13 | **Rekursi Call 3** | Line 112 | 3 | `find_element(2, 2)`: perbandingan, akses array, return |
| 14 | Perbandingan `%` | Line 118 | 1 | `if 5 % 2 == 0` |
| 15 | Return | Line 123 | 1 | `return float(30)` |

**Total: ~15 operasi untuk n=5, ~150 untuk n=100**

---

### Pertanyaan 3: "i nya berapa" (Variabel current_index dalam rekursif)

#### **Jawaban:**

**Variabel `current_index` (biasanya disebut `i`):**

"Pak, `current_index` adalah parameter yang menunjukkan posisi saat ini dalam array. Nilainya berubah setiap panggilan rekursif."

**Trace untuk n=5 (target_index=2):**

| Panggilan | current_index (i) | target_idx | Kondisi | Aksi |
|-----------|-------------------|------------|---------|------|
| Call 1 | `i = 0` | 2 | `0 != 2` | Rekursi dengan `i+1` |
| Call 2 | `i = 1` | 2 | `1 != 2` | Rekursi dengan `i+1` |
| Call 3 | `i = 2` | 2 | `2 == 2` ✓ | Return `arr[2]` |

**Kode:**
```python
def find_element(current_index, target_idx):  # i = current_index
    if current_index == target_idx:           # Check: i == target?
        return arr[current_index]             # Return arr[i]
    return find_element(current_index + 1, target_idx)  # Rekursi dengan i+1
```

**Jumlah nilai i:**
- Untuk n=5: i berubah dari 0 → 1 → 2 (3 nilai)
- Untuk n=10: i berubah dari 0 → 1 → ... → 5 (6 nilai)
- Untuk n=100: i berubah dari 0 → 1 → ... → 50 (51 nilai)
- **Rumus: i berubah sebanyak target_index + 1 kali = (n//2) + 1 kali**

---

### Pertanyaan 4: "Satu iteratif 2 rekursif"

#### **Jawaban:**

"Maksudnya Pak, untuk menjelaskan dua algoritma ini:"

**Bagian 1: Algoritma Iteratif (1 penjelasan)**
- Penjelasan logika algoritma iteratif dengan loop
- Contoh step-by-step dengan trace loop
- Analisis kompleksitas O(N) dengan sum of operations

**Bagian 2: Algoritma Rekursif (2 penjelasan)**
- **2.1:** Penjelasan logika algoritma rekursif secara umum
- **2.2:** Penjelasan detail fungsi `find_element` dan trace rekursinya

**Atau bisa berarti:**
- **1 iteratif:** Satu cara pendekatan (menggunakan loop)
- **2 rekursif:** Dua bagian dalam rekursif:
  1. Fungsi utama `median_recursive`
  2. Helper function `find_element` yang rekursif

---

## 📝 Script Presentasi

### **Pembukaan (Indra):**

"Selamat pagi/siang Pak/Bu. Kami dari kelompok [nama], akan mempresentasikan tugas besar Analisis Kompleksitas Algoritma dengan judul 'Analisis Kompleksitas Algoritma Iteratif dan Rekursif dalam Mencari Nilai Median Pada Array Terurut'."

"Kelompok kami terdiri dari:
- Saya (Indra) akan melakukan demo program
- Tubagus akan menjelaskan logika algoritma
- Vandin akan menjelaskan analisis kompleksitas dan hasil pengujian"

---

### **Penjelasan Struktur Program (Indra):**

"Program kami menggunakan Python dengan class `MedianAnalyzer` yang memiliki beberapa fungsi utama:
1. `generate_sorted_array` untuk generate data uji
2. `median_iterative` untuk algoritma iteratif
3. `median_recursive` untuk algoritma rekursif
4. `run_benchmark` untuk menjalankan pengujian
5. `plot_graph` untuk visualisasi hasil"

---

### **Penjelasan Algoritma Iteratif (Tubagus):**

"Baik Pak, saya akan jelaskan algoritma iteratif. Algoritma ini bekerja dengan cara menggunakan loop untuk iterate dari index 0 sampai middle_index. Loop ini akan mengakses setiap elemen array sampai mencapai elemen tengah."

"Langkah-langkahnya:
1. Hitung panjang array: `n = len(arr)`
2. Hitung indeks tengah: `middle_index = n // 2`
3. Loop dari i=0 sampai i=middle_index (inclusive)
   - Di setiap iterasi, akses arr[i]
4. Setelah loop, kita punya nilai di middle_index
5. Jika ganjil, return nilai tersebut
6. Jika genap, ambil juga elemen sebelumnya dan rata-rata"

**Contoh:**
"Misalnya array `[10, 20, 30, 40, 50]` dengan n=5:
- middle_index = 5 // 2 = 2
- Loop: for i in range(3) → i = 0, 1, 2
  - Iterasi 1: i=0, akses arr[0]=10
  - Iterasi 2: i=1, akses arr[1]=20
  - Iterasi 3: i=2, akses arr[2]=30
- Karena ganjil, return 30"

"Operasi dasar yang dilakukan:
- 1 kali len(arr)
- 2 kali perbandingan base case
- 1 kali pembagian (//)
- Loop: (n//2 + 1) kali iterasi
  - Setiap iterasi: 1 perbandingan, 1 increment, 1 akses array
- Total: ~13 operasi untuk n=5, ~60 operasi untuk n=100
- Kompleksitas: O(N) - linear, bergantung pada n
- Sum of operations: T(n) = Σ(i=0 to n//2) 1 = (n//2) + 1 = O(n)"

---

### **Penjelasan Algoritma Rekursif (Tubagus):**

"Sekarang algoritma rekursif. Algoritma ini menggunakan fungsi helper `find_element` yang rekursif. Fungsi ini 'berjalan' melalui array secara rekursif dari index 0 sampai mencapai target_index (n//2)."

**Trace Rekursi:**
"Untuk array `[10, 20, 30, 40, 50]` dengan target_index=2:

```
Call 1: find_element(0, 2)
  → Check: 0 == 2? No
  → Rekursi: find_element(1, 2)

Call 2: find_element(1, 2)
  → Check: 1 == 2? No  
  → Rekursi: find_element(2, 2)

Call 3: find_element(2, 2)
  → Check: 2 == 2? Yes ✓
  → Return: arr[2] = 30
```

"Jadi untuk n=5, ada 3 panggilan rekursif. Untuk n=100, akan ada sekitar 50 panggilan."

"Variabel `current_index` (i) berubah setiap panggilan: 0 → 1 → 2"

"Operasi dasar per panggilan:
- 1 perbandingan (i == target?)
- 1 penjumlahan (i+1)
- 1 pemanggilan fungsi (overhead)
- Total untuk n=5: ~15 operasi, kompleksitas O(N)!"

---

### **Analisis Kompleksitas (Vandin):**

"Berdasarkan analisis operasi dasar:

**Iteratif:**
- Kompleksitas waktu: O(N) - linear (loop dari 0 sampai n//2)
- Kompleksitas ruang: O(1) - konstan
- Operasi: Loop berjalan (n//2 + 1) kali, bergantung pada n
- Sum of operations: T(n) = Σ(i=0 to n//2) 1 = (n//2) + 1 = O(n)

**Rekursif:**
- Kompleksitas waktu: O(N) - linear
- Kompleksitas ruang: O(N) - karena stack rekursi
- Operasi: ~3n/2 operasi, PROPORSIONAL dengan n"

**Perbandingan:**
- n=10: Iteratif ~15 ops vs Rekursif ~16 ops
- n=100: Iteratif ~60 ops vs Rekursif ~156 ops
- n=1000: Iteratif ~510 ops vs Rekursif ~1506 ops

"Kedua algoritma memiliki kompleksitas O(N), tapi iteratif lebih efisien karena tidak ada overhead rekursi!"

---

### **Hasil Pengujian (Vandin):**

"Dari hasil benchmark dengan berbagai ukuran input:
- Algoritma iteratif: waktu eksekusi meningkat linear dengan n (sesuai O(N))
- Algoritma rekursif: waktu eksekusi meningkat linear dengan n (sesuai O(N))
- Ratio: untuk n=10000, rekursif sekitar 2-3x lebih lambat dari iteratif karena overhead rekursi!"

"Grafik menunjukkan:
- Garis biru (iteratif): meningkat linear - O(N), tapi lebih cepat
- Garis merah (rekursif): meningkat linear - O(N), tapi lebih lambat karena overhead"

---

### **Kesimpulan (Semua):**

"Kesimpulan:
1. Kedua algoritma memiliki kompleksitas waktu O(N) yang sama
2. Algoritma iteratif lebih efisien karena tidak ada overhead rekursi
3. Algoritma iteratif lebih hemat memori: O(1) vs O(N) untuk rekursif
4. Algoritma iteratif direkomendasikan untuk kasus ini
5. Rekursif memiliki overhead pemanggilan fungsi dan risiko stack overflow
6. Iteratif bisa dihitung dengan sum of operations secara jelas"

"Terima kasih, kami siap menerima pertanyaan."

---

## 🎯 Tips Presentasi

1. **Siapkan contoh konkret** dengan array kecil (n=5 atau n=6)
2. **Tulis trace rekursi di papan/PPT** saat menjelaskan
3. **Highlight operasi dasar** dengan warna atau penekanan
4. **Siapkan jawaban** untuk pertanyaan tentang variabel i (current_index)
5. **Demonstrasi program** di akhir untuk menunjukkan hasil real

**Selamat presentasi! 🎉**

