# 📦 Contoh Kasus Nyata - Pencarian Median

## 🎯 Use Case: Sistem Penjualan Online

### **Studi Kasus: Mencari Harga Median Produk**

**Skenario:**
Sebuah toko online ingin menampilkan **harga median** dari semua produk dalam satu kategori untuk memberikan gambaran harga rata-rata kepada pelanggan.

**Contoh:** Kategori "Laptop"
- Produk sudah diurutkan berdasarkan harga (dari termurah ke termahal)
- Kebutuhan: Tampilkan harga median untuk membantu customer menentukan budget

**Implementasi:**
```python
# Daftar harga laptop yang sudah terurut (dalam juta rupiah)
harga_laptop = [5, 7, 8, 10, 12, 15, 18, 20, 25, 30]

# Cari harga median menggunakan algoritma iteratif
harga_median = median_iterative(harga_laptop)
print(f"Harga median laptop: Rp {harga_median} juta")
# Output: Harga median laptop: Rp 13.5 juta
```

---

## 🎯 Use Case: Sistem Rating Produk

### **Studi Kasus: Menentukan Rating Median**

**Skenario:**
Platform e-commerce ingin menampilkan **rating median** dari semua review produk untuk memberikan representasi yang lebih adil dibanding rata-rata.

**Contoh:** Produk "Smartphone X"
- Semua rating diurutkan dari terendah ke tertinggi
- Rating median lebih representatif karena tidak dipengaruhi outlier (rating ekstrem)

**Implementasi:**
```python
# Rating produk yang sudah diurutkan (1-5 bintang)
ratings = [1, 2, 3, 3, 4, 4, 4, 5, 5, 5]

# Cari rating median
rating_median = median_iterative(ratings)
print(f"Rating median: {rating_median} bintang")
# Output: Rating median: 4.0 bintang
```

---

## 🎯 Use Case: Analisis Data Penjualan

### **Studi Kasus: Mencari Median Penjualan Harian**

**Skenario:**
Manager toko ingin mengetahui **median penjualan harian** dalam sebulan untuk:
- Menentukan target penjualan yang realistis
- Membandingkan performa hari tertentu dengan median
- Perencanaan stok barang

**Contoh:** Penjualan bulan Desember (30 hari)
- Data penjualan harian sudah diurutkan
- Median menunjukkan nilai tengah yang tidak dipengaruhi hari promosi ekstrem

**Implementasi:**
```python
# Penjualan harian yang sudah diurutkan (dalam juta rupiah)
penjualan_harian = [5, 8, 10, 12, 15, 18, 20, 25, 30, 35, 40, 50, ...]

# Cari median penjualan
median_penjualan = median_iterative(penjualan_harian)
print(f"Median penjualan harian: Rp {median_penjualan} juta")
```

---

## 🎯 Use Case: Sistem Rekomendasi Harga

### **Studi Kasus: Menentukan Harga Jual Optimal**

**Skenario:**
Marketplace ingin membantu seller menentukan harga jual dengan menampilkan **median harga kompetitor** untuk produk sejenis.

**Contoh:** Seller ingin menjual "Earphone Bluetooth"
- Sistem mengumpulkan semua harga produk sejenis
- Data diurutkan dan ditampilkan median untuk referensi

**Implementasi:**
```python
# Harga kompetitor yang sudah diurutkan
harga_kompetitor = [150000, 200000, 250000, 300000, 350000, 400000]

# Cari harga median untuk rekomendasi
harga_rekomendasi = median_iterative(harga_kompetitor)
print(f"Harga median pasar: Rp {harga_rekomendasi:,.0f}")
# Output: Harga median pasar: Rp 275,000
```

---

## 🎯 Use Case: Analisis Waktu Pengiriman

### **Studi Kasus: Estimasi Waktu Pengiriman**

**Skenario:**
Service pengiriman ingin memberikan estimasi waktu yang lebih akurat dengan menggunakan **median waktu pengiriman** dari data historis.

**Contoh:** Pengiriman Jakarta-Bandung
- Data waktu pengiriman (dalam jam) sudah diurutkan
- Median lebih representatif dibanding rata-rata

**Implementasi:**
```python
# Waktu pengiriman historis yang sudah diurutkan (jam)
waktu_pengiriman = [6, 8, 10, 12, 14, 16, 18, 20, 24, 30]

# Cari median waktu pengiriman
median_waktu = median_iterative(waktu_pengiriman)
print(f"Estimasi waktu pengiriman: {median_waktu} jam")
# Output: Estimasi waktu pengiriman: 15.0 jam
```

---

## 🎯 Use Case: Sistem Pencarian Barang

### **Studi Kasus: Menemukan Produk di Tengah Rentang Harga**

**Skenario:**
Customer ingin mencari produk dengan filter "harga tengah-tengah" dari kategori tertentu.

**Contoh:** Customer mencari "Kamera DSLR"
- Sistem menampilkan produk dengan harga yang berada di tengah rentang harga semua produk
- Produk sudah terurut berdasarkan harga

**Implementasi:**
```python
# Harga semua kamera yang sudah diurutkan
harga_kamera = [3000000, 4000000, 5000000, 6000000, 7000000, 8000000, 10000000]

# Cari harga median
harga_median = median_iterative(harga_kamera)
print(f"Produk dengan harga median: Rp {harga_median:,.0f}")
# Output: Produk dengan harga median: Rp 6,000,000

# Tampilkan produk dengan harga mendekati median
produk_median = [p for p in products if abs(p.harga - harga_median) <= 500000]
```

---

## 📊 Mengapa Menggunakan Median?

### Kelebihan Median dibanding Rata-rata:

1. **Tidak dipengaruhi outlier:**
   - Jika ada 1 produk super mahal, rata-rata akan tinggi
   - Median tetap stabil

2. **Lebih representatif:**
   - Menunjukkan "nilai tengah" yang sebenarnya
   - Lebih sesuai untuk data yang tidak normal

3. **Mudah dipahami:**
   - Customer lebih mudah memahami "harga tengah-tengah"
   - Tidak perlu penjelasan statistik rumit

---

## 💻 Contoh Implementasi dalam Sistem

```python
class ProductCatalog:
    def __init__(self):
        self.products = []
    
    def add_product(self, name, price):
        self.products.append({'name': name, 'price': price})
        # Urutkan berdasarkan harga
        self.products.sort(key=lambda x: x['price'])
    
    def get_median_price(self):
        """Mengembalikan harga median dari semua produk"""
        prices = [p['price'] for p in self.products]
        return median_iterative(prices)
    
    def get_products_near_median(self, tolerance=100000):
        """Mengembalikan produk dengan harga mendekati median"""
        median_price = self.get_median_price()
        return [
            p for p in self.products 
            if abs(p['price'] - median_price) <= tolerance
        ]

# Penggunaan
catalog = ProductCatalog()
catalog.add_product("Laptop A", 8000000)
catalog.add_product("Laptop B", 12000000)
catalog.add_product("Laptop C", 10000000)
catalog.add_product("Laptop D", 15000000)
catalog.add_product("Laptop E", 9000000)

print(f"Harga median: Rp {catalog.get_median_price():,.0f}")
# Output: Harga median: Rp 10,000,000

print("Produk dengan harga mendekati median:")
for product in catalog.get_products_near_median():
    print(f"- {product['name']}: Rp {product['price']:,.0f}")
```

---

## 🎤 Cara Menjelaskan dalam Presentasi

**"Pak/Bu, algoritma mencari median ini sangat berguna dalam berbagai aplikasi praktis, misalnya:"**

1. **Sistem e-commerce:** Menampilkan harga median produk untuk membantu customer menentukan budget
2. **Analisis penjualan:** Menentukan median penjualan untuk perencanaan bisnis
3. **Sistem rating:** Menampilkan rating median yang lebih representatif
4. **Rekomendasi harga:** Membantu seller menentukan harga jual optimal

**"Kami memilih kasus mencari median pada array terurut karena:"**
- Sering digunakan dalam aplikasi nyata
- Perlu efisiensi karena data bisa sangat besar
- Penting untuk memilih algoritma yang tepat (iteratif vs rekursif)

**"Dari analisis kami, algoritma iteratif lebih efisien untuk kasus ini karena kompleksitas ruang O(1) dan tidak ada risiko stack overflow untuk data besar seperti dalam sistem e-commerce."**

---

## 📝 Kesimpulan

Algoritma mencari median sangat relevan untuk:
- ✅ Sistem e-commerce dan marketplace
- ✅ Analisis data bisnis
- ✅ Sistem rekomendasi
- ✅ Aplikasi yang membutuhkan nilai tengah representatif

Dengan menggunakan algoritma yang efisien (iteratif), sistem dapat memproses data dalam jumlah besar dengan cepat dan hemat memori.

