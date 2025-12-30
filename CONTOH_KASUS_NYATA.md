# 💼 Contoh Kasus Nyata - Sistem E-Commerce
## Mencari Harga Median Produk untuk Menampilkan Harga Representatif

---

## 🎯 Skenario: Sistem Penjualan Online

### **Deskripsi Masalah:**

Sebuah platform e-commerce besar (seperti Tokopedia, Shopee, atau Amazon) ingin menampilkan **harga median** dari semua produk dalam satu kategori kepada customer. Harga median digunakan karena:

1. **Lebih representatif** dibanding rata-rata (mean)
2. **Tidak dipengaruhi outlier** (produk super mahal atau super murah)
3. **Memberikan gambaran harga tengah-tengah** yang lebih akurat
4. **Membantu customer menentukan budget** dengan lebih baik

### **Contoh Konkret:**

**Kategori Produk:** Laptop Gaming
**Data Harga (dalam juta rupiah):** `[8, 10, 12, 15, 18, 20, 25, 30, 35, 40, 50, 70]`

**Pertanyaan:** Berapa harga median laptop gaming yang harus ditampilkan kepada customer?

**Jawaban yang Diinginkan:** Harga median = **22.5 juta rupiah**

---

## 📊 Penjelasan Data

### **Array Harga yang Sudah Terurut:**
```
Indeks:   0    1    2    3    4    5    6    7    8    9   10   11
Harga:   [8,  10,  12,  15,  18,  20,  25,  30,  35,  40,  50,  70]
         (dalam juta rupiah)
```

**Karakteristik:**
- Jumlah produk (n) = 12
- Data sudah terurut dari harga termurah (8 juta) ke termahal (70 juta)
- Karena n genap, median adalah rata-rata dua elemen tengah

---

## 🔵 Solusi dengan Algoritma Iteratif

### **Algoritma Iteratif: Menggunakan Loop**

**Pseudocode:**
```
FUNCTION median_iterative(harga_array):
    n = LENGTH(harga_array)
    
    IF n == 0:
        RETURN None
    IF n == 1:
        RETURN harga_array[0]
    
    middle_index = n // 2  // 12 // 2 = 6
    
    // Loop dari 0 sampai middle_index
    current_val = None
    FOR i FROM 0 TO middle_index:
        current_val = harga_array[i]
    
    IF n MOD 2 == 0:  // Genap
        val1 = harga_array[middle_index - 1]  // arr[5] = 20
        val2 = current_val  // arr[6] = 25
        RETURN (val1 + val2) / 2.0  // (20 + 25) / 2 = 22.5
    ELSE:  // Ganjil
        RETURN current_val
```

### **Trace Algoritma Iteratif untuk Data Laptop Gaming:**

**Input:** `harga = [8, 10, 12, 15, 18, 20, 25, 30, 35, 40, 50, 70]`
**n = 12**

**Step-by-Step Execution:**

| Step | Operasi | Nilai | Penjelasan |
|------|---------|-------|------------|
| 1 | `n = len(harga)` | `n = 12` | Hitung jumlah produk |
| 2 | `if n == 0:` | `False` | Skip (ada 12 produk) |
| 3 | `if n == 1:` | `False` | Skip (bukan 1 produk) |
| 4 | `middle_index = 12 // 2` | `middle_index = 6` | Indeks tengah = 6 |
| 5 | **LOOP:** `for i in range(7)` | **i = 0, 1, 2, 3, 4, 5, 6** | **Loop 7 kali** |

**Detail Loop:**

| Iterasi | i | Operasi | `current_val = harga[i]` | Nilai |
|---------|---|---------|--------------------------|-------|
| 1 | 0 | `harga[0]` | `current_val = 8` | 8 juta |
| 2 | 1 | `harga[1]` | `current_val = 10` | 10 juta |
| 3 | 2 | `harga[2]` | `current_val = 12` | 12 juta |
| 4 | 3 | `harga[3]` | `current_val = 15` | 15 juta |
| 5 | 4 | `harga[4]` | `current_val = 18` | 18 juta |
| 6 | 5 | `harga[5]` | `current_val = 20` | 20 juta |
| 7 | 6 | `harga[6]` | `current_val = 25` | 25 juta ✓ |

**Setelah Loop:**
- `current_val = 25` (harga pada indeks 6)

**Karena n genap (12):**
- `val1 = harga[6 - 1] = harga[5] = 20`
- `val2 = current_val = 25`
- `median = (20 + 25) / 2.0 = 22.5`

**Output:** `22.5 juta rupiah`

### **Analisis Kompleksitas Iteratif:**

- **Kompleksitas Waktu:** O(N)
  - Loop berjalan (n//2 + 1) kali = (12//2 + 1) = 7 kali
  - Untuk n=12: 7 iterasi
  - Untuk n=1000: ~501 iterasi
  - Proporsional dengan n → O(N)

- **Kompleksitas Ruang:** O(1)
  - Hanya menggunakan variabel: `n`, `middle_index`, `current_val`, `val1`, `val2`
  - Tidak ada struktur data tambahan
  - Ruang konstan, tidak bergantung pada n

- **Operasi Dasar:**
  - Akses array `harga[i]`: 7 kali (dalam loop)
  - Total operasi: ~10-15 operasi untuk n=12

---

## 🟠 Solusi dengan Algoritma Rekursif

### **Algoritma Rekursif: Menggunakan Rekursi**

**Pseudocode:**
```
FUNCTION median_recursive(harga_array):
    n = LENGTH(harga_array)
    
    IF n == 0:
        RETURN None
    IF n == 1:
        RETURN harga_array[0]
    
    target_index = n // 2  // 12 // 2 = 6
    
    FUNCTION find_element(current_idx, target_idx):
        IF current_idx == target_idx:
            RETURN harga_array[current_idx]
        RETURN find_element(current_idx + 1, target_idx)
    
    result = find_element(0, target_index)  // Mulai dari index 0
    
    IF n MOD 2 == 0:  // Genap
        val1 = harga_array[target_index - 1]  // arr[5] = 20
        val2 = result  // arr[6] = 25
        RETURN (val1 + val2) / 2.0  // (20 + 25) / 2 = 22.5
    ELSE:  // Ganjil
        RETURN result
```

### **Trace Algoritma Rekursif untuk Data Laptop Gaming:**

**Input:** `harga = [8, 10, 12, 15, 18, 20, 25, 30, 35, 40, 50, 70]`
**n = 12, target_index = 6**

**Trace Rekursi `find_element(0, 6)`:**
```
═══════════════════════════════════════════════════════════════
CALL 1: find_element(current_index=0, target_idx=6)
═══════════════════════════════════════════════════════════════
├─ Check: 0 == 6? → FALSE ❌
├─ Operasi: 1 perbandingan
└─ Rekursi: find_element(0 + 1, 6) = find_element(1, 6)
   
   ═══════════════════════════════════════════════════════════
   CALL 2: find_element(current_index=1, target_idx=6)
   ═══════════════════════════════════════════════════════════
   ├─ Check: 1 == 6? → FALSE ❌
   ├─ Operasi: 1 perbandingan
   └─ Rekursi: find_element(1 + 1, 6) = find_element(2, 6)
      
      ═══════════════════════════════════════════════════════════
      CALL 3: find_element(current_index=2, target_idx=6)
      ═══════════════════════════════════════════════════════════
      ├─ Check: 2 == 6? → FALSE ❌
      ├─ Operasi: 1 perbandingan
      └─ Rekursi: find_element(2 + 1, 6) = find_element(3, 6)
         
         ═══════════════════════════════════════════════════════════
         CALL 4: find_element(current_index=3, target_idx=6)
         ═══════════════════════════════════════════════════════════
         ├─ Check: 3 == 6? → FALSE ❌
         ├─ Operasi: 1 perbandingan
         └─ Rekursi: find_element(3 + 1, 6) = find_element(4, 6)
            
            ═══════════════════════════════════════════════════════════
            CALL 5: find_element(current_index=4, target_idx=6)
            ═══════════════════════════════════════════════════════════
            ├─ Check: 4 == 6? → FALSE ❌
            ├─ Operasi: 1 perbandingan
            └─ Rekursi: find_element(4 + 1, 6) = find_element(5, 6)
               
               ═══════════════════════════════════════════════════════════
               CALL 6: find_element(current_index=5, target_idx=6)
               ═══════════════════════════════════════════════════════════
               ├─ Check: 5 == 6? → FALSE ❌
               ├─ Operasi: 1 perbandingan
               └─ Rekursi: find_element(5 + 1, 6) = find_element(6, 6)
                  
                  ═══════════════════════════════════════════════════════════
                  CALL 7: find_element(current_index=6, target_idx=6) ✓ BASE CASE
                  ═══════════════════════════════════════════════════════════
                  ├─ Check: 6 == 6? → TRUE ✓
                  ├─ Operasi: 1 perbandingan, 1 akses array
                  └─ Return: harga[6] = 25
                     └─ Value: 25 juta
```

**Return Chain:**
```
Call 7 → Return 25
  ↓
Call 6 → Return 25
  ↓
Call 5 → Return 25
  ↓
Call 4 → Return 25
  ↓
Call 3 → Return 25
  ↓
Call 2 → Return 25
  ↓
Call 1 → Return 25
  ↓
result = 25
```

**Setelah Rekursi:**
- `result = 25` (harga pada indeks 6)

**Karena n genap (12):**
- `val1 = harga[6 - 1] = harga[5] = 20`
- `val2 = result = 25`
- `median = (20 + 25) / 2.0 = 22.5`

**Output:** `22.5 juta rupiah`

### **Analisis Kompleksitas Rekursif:**

- **Kompleksitas Waktu:** O(N)
  - Rekursi dipanggil (target_index + 1) kali = (6 + 1) = 7 kali
  - Untuk n=12: 7 panggilan rekursif
  - Untuk n=1000: ~501 panggilan rekursif
  - Proporsional dengan n → O(N)

- **Kompleksitas Ruang:** O(N)
  - Setiap panggilan rekursif menambahkan frame ke call stack
  - Untuk n=12: 7 frame di stack
  - Untuk n=1000: ~501 frame di stack
  - Proporsional dengan n → O(N)

- **Operasi Dasar:**
  - Perbandingan `current_index == target_idx`: 7 kali
  - Penjumlahan `current_index + 1`: 6 kali
  - Akses array: 1 kali (call terakhir)
  - Total operasi: ~20-25 operasi untuk n=12 (termasuk overhead rekursi)

---

## 🔄 Perbandingan Kedua Algoritma

### **Untuk Data Laptop Gaming (n=12):**

| Aspek | Iteratif | Rekursif |
|-------|----------|----------|
| **Hasil** | 22.5 juta | 22.5 juta ✓ (sama) |
| **Jumlah Iterasi/Panggilan** | 7 iterasi | 7 panggilan |
| **Kompleksitas Waktu** | O(N) | O(N) |
| **Kompleksitas Ruang** | O(1) | O(N) |
| **Operasi Dasar** | ~15 operasi | ~25 operasi |
| **Stack Frames** | 1 (tidak ada rekursi) | 7 frames |
| **Overhead** | Minimal | Overhead pemanggilan fungsi |

### **Perbandingan untuk Data Lebih Besar:**

**Contoh: 1000 produk laptop (n=1000)**

| Aspek | Iteratif | Rekursif |
|-------|----------|----------|
| **Iterasi/Panggilan** | 501 | 501 |
| **Kompleksitas Waktu** | O(N) | O(N) |
| **Kompleksitas Ruang** | O(1) - konstan | O(N) - 501 frame stack |
| **Risiko** | Tidak ada | Stack overflow mungkin terjadi |
| **Efisiensi** | Lebih efisien | Lebih lambat karena overhead |

---

## 💡 Mengapa Memilih Algoritma Iteratif?

### **Keuntungan Algoritma Iteratif untuk Sistem E-Commerce:**

1. **Hemat Memori:**
   - Kompleksitas ruang O(1) vs O(N) untuk rekursif
   - Penting untuk sistem yang menangani ribuan produk
   - Tidak membebani server dengan stack frames

2. **Lebih Cepat:**
   - Tidak ada overhead pemanggilan fungsi rekursif
   - Operasi lebih sedikit untuk hasil yang sama
   - Penting untuk real-time display di website

3. **Lebih Stabil:**
   - Tidak ada risiko stack overflow
   - Bisa menangani data sangat besar (puluhan ribu produk)
   - Sistem lebih reliable

4. **Lebih Mudah Di-debug:**
   - Kode lebih sederhana dan linear
   - Lebih mudah dirawat oleh tim development
   - Tidak ada kompleksitas rekursi yang sulit ditelusuri

### **Kapan Rekursif Bisa Digunakan?**

- Hanya jika data sangat kecil (n < 50)
- Untuk tujuan pembelajaran
- Jika ada constraint khusus yang mengharuskan rekursif

---

## 💻 Implementasi dalam Sistem E-Commerce

### **Contoh Kode dalam Sistem:**

```python
class ProductCatalog:
    def __init__(self):
        self.products = []
    
    def add_product(self, name, price):
        """Menambahkan produk dan mengurutkan berdasarkan harga"""
        self.products.append({'name': name, 'price': price})
        # Urutkan berdasarkan harga (ascending)
        self.products.sort(key=lambda x: x['price'])
    
    def get_median_price_iterative(self):
        """Menggunakan algoritma iteratif - RECOMMENDED"""
        prices = [p['price'] for p in self.products]
        return median_iterative(prices)
    
    def get_median_price_recursive(self):
        """Menggunakan algoritma rekursif - NOT RECOMMENDED untuk data besar"""
        prices = [p['price'] for p in self.products]
        return median_recursive(prices)
    
    def display_category_info(self):
        """Menampilkan informasi kategori dengan harga median"""
        median_price = self.get_median_price_iterative()
        print(f"Kategori: Laptop Gaming")
        print(f"Jumlah produk: {len(self.products)}")
        print(f"Harga median: Rp {median_price:,.0f}")
        print(f"Rentang harga: Rp {self.products[0]['price']:,.0f} - Rp {self.products[-1]['price']:,.0f}")

# Penggunaan dalam sistem
catalog = ProductCatalog()

# Data produk laptop gaming (sudah terurut berdasarkan harga)
catalog.add_product("Laptop A", 8000000)
catalog.add_product("Laptop B", 10000000)
catalog.add_product("Laptop C", 12000000)
catalog.add_product("Laptop D", 15000000)
catalog.add_product("Laptop E", 18000000)
catalog.add_product("Laptop F", 20000000)
catalog.add_product("Laptop G", 25000000)
catalog.add_product("Laptop H", 30000000)
catalog.add_product("Laptop I", 35000000)
catalog.add_product("Laptop J", 40000000)
catalog.add_product("Laptop K", 50000000)
catalog.add_product("Laptop L", 70000000)

# Tampilkan informasi kategori
catalog.display_category_info()

# Output:
# Kategori: Laptop Gaming
# Jumlah produk: 12
# Harga median: Rp 22,500,000
# Rentang harga: Rp 8,000,000 - Rp 70,000,000
```

---

## 📊 Visualisasi Perbandingan

### **Grafik Waktu Eksekusi:**

```
Waktu Eksekusi vs Ukuran Data:

Iteratif (O(N)):     ████░░░░░░░░ (cepat, stabil)
Rekursif (O(N)):     ████████░░░░ (lambat, overhead)
```

### **Grafik Penggunaan Memori:**

```
Penggunaan Memori vs Ukuran Data:

Iteratif (O(1)):     █░░░░░░░░░░░ (konstan)
Rekursif (O(N)):     ████████████ (meningkat linear)
```

---

## 🎯 Kesimpulan untuk Kasus E-Commerce

### **Rekomendasi:**

✅ **Gunakan Algoritma Iteratif** karena:

1. **Efisiensi:** O(1) ruang vs O(N) untuk rekursif
2. **Kecepatan:** Lebih cepat, tidak ada overhead rekursi
3. **Skalabilitas:** Bisa menangani data besar tanpa masalah
4. **Reliability:** Tidak ada risiko stack overflow
5. **Maintainability:** Kode lebih sederhana dan mudah dirawat

### **Alasan Pemilihan untuk Sistem E-Commerce:**

- Sistem e-commerce menangani ribuan hingga puluhan ribu produk
- Perhitungan median perlu cepat untuk real-time display
- Server perlu efisien dalam penggunaan memori
- Sistem harus reliable dan tidak crash karena stack overflow
- Tim development perlu kode yang mudah dipahami dan dirawat

**Kesimpulan Akhir:** Untuk aplikasi e-commerce yang menangani data besar, algoritma iteratif adalah pilihan yang jelas lebih unggul daripada rekursif.

---

## 🎤 Cara Menjelaskan dalam Presentasi

**"Pak/Bu, kami mengambil contoh kasus sistem e-commerce yang menampilkan harga median produk. Ini adalah aplikasi nyata yang sering digunakan di platform seperti Tokopedia atau Shopee."**

**"Dalam contoh kami, kami memiliki 12 produk laptop gaming dengan harga yang sudah terurut. Sistem perlu menampilkan harga median yaitu 22.5 juta rupiah kepada customer."**

**"Kami mengimplementasikan kedua algoritma - iteratif dan rekursif - dan keduanya memberikan hasil yang sama yaitu 22.5 juta. Namun, dari analisis kompleksitas, algoritma iteratif lebih unggul karena menggunakan ruang O(1) dibanding O(N) untuk rekursif, dan tidak ada risiko stack overflow untuk data besar seperti dalam sistem e-commerce yang menangani ribuan produk."**

**"Oleh karena itu, untuk aplikasi nyata seperti ini, kami merekomendasikan penggunaan algoritma iteratif."**

