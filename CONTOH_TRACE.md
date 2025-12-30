# 📊 Contoh Trace Algoritma untuk Presentasi

## 🔵 Algoritma Iteratif - Trace Lengkap

### Contoh 1: Array Ganjil
```
Input: arr = [10, 20, 30, 40, 50]
n = len(arr) = 5
```

**Step-by-step:**

| Step | Kode | Nilai | Operasi | Keterangan |
|------|------|-------|---------|------------|
| 1 | `n = len(arr)` | `n = 5` | O(1) | Hitung panjang |
| 2 | `if n == 0:` | `False` | 1 perbandingan | Skip |
| 3 | `if n == 1:` | `False` | 1 perbandingan | Skip |
| 4 | `middle_index = n // 2` | `middle_index = 2` | 1 pembagian | 5 // 2 = 2 |
| 5 | **LOOP: `for i in range(middle_index + 1)`** | **`range(3)` → i = 0, 1, 2** | **3 iterasi** | **Loop dari 0 sampai 2** |
| 5a | **Iterasi 1: i=0** | **`current_val = arr[0] = 10`** | **1 perbandingan, 1 akses** | **Check loop, akses array** |
| 5b | **Iterasi 2: i=1** | **`current_val = arr[1] = 20`** | **1 perbandingan, 1 increment, 1 akses** | **Check loop, increment i, akses array** |
| 5c | **Iterasi 3: i=2** | **`current_val = arr[2] = 30`** | **1 perbandingan, 1 increment, 1 akses** | **Check loop, increment i, akses array** |
| 6 | `if n % 2 == 0:` | `False` (5 % 2 = 1) | 1 modulo | Ganjil, masuk else |
| 7 | `return float(current_val)` | `float(30) = 30.0` | 1 konversi | Return 30.0 |

**Result:** `30.0`

**Total Operasi:** 
- Operasi di luar loop: 6
- Operasi dalam loop: 3 iterasi × 3 ops = 9
- **Total: ~15 operasi dasar**

**Jumlah Iterasi Loop:** 3 = (n//2 + 1) = (5//2 + 1)

---

### Contoh 2: Array Genap
```
Input: arr = [10, 20, 30, 40, 50, 60]
n = len(arr) = 6
```

**Step-by-step:**

| Step | Kode | Nilai | Operasi | Keterangan |
|------|------|-------|---------|------------|
| 1 | `n = len(arr)` | `n = 6` | O(1) | Hitung panjang |
| 2 | `if n == 0:` | `False` | 1 perbandingan | Skip |
| 3 | `if n == 1:` | `False` | 1 perbandingan | Skip |
| 4 | `middle_index = n // 2` | `middle_index = 3` | 1 pembagian | 6 // 2 = 3 |
| 5 | **LOOP: `for i in range(middle_index + 1)`** | **`range(4)` → i = 0, 1, 2, 3** | **4 iterasi** | **Loop dari 0 sampai 3** |
| 5a | **Iterasi 1: i=0** | **`current_val = arr[0] = 10`** | **1 perbandingan, 1 akses** | **Check loop, akses array** |
| 5b | **Iterasi 2: i=1** | **`current_val = arr[1] = 20`** | **1 perbandingan, 1 increment, 1 akses** | **Check loop, increment i, akses array** |
| 5c | **Iterasi 3: i=2** | **`current_val = arr[2] = 30`** | **1 perbandingan, 1 increment, 1 akses** | **Check loop, increment i, akses array** |
| 5d | **Iterasi 4: i=3** | **`current_val = arr[3] = 40`** | **1 perbandingan, 1 increment, 1 akses** | **Check loop, increment i, akses array** |
| 6 | `if n % 2 == 0:` | `True` (6 % 2 = 0) | 1 modulo | Genap, masuk if |
| 7 | `val1 = arr[middle_index - 1]` | `arr[2] = 30` | 1 pengurangan, 1 akses | 3 - 1 = 2 |
| 8 | `val2 = current_val` | `40` | 1 assignment | Menggunakan nilai dari loop |
| 9 | `return (val1 + val2) / 2.0` | `(30 + 40) / 2 = 35` | 1 penjumlahan, 1 pembagian | |

**Result:** `35.0`

**Total Operasi:**
- Operasi di luar loop: 7
- Operasi dalam loop: 4 iterasi × 3 ops = 12
- **Total: ~19 operasi dasar**

**Jumlah Iterasi Loop:** 4 = (n//2 + 1) = (6//2 + 1)

---

## 🟠 Algoritma Rekursif - Trace Lengkap

### Contoh 1: Array Ganjil (n=5, target_index=2)
```
Input: arr = [10, 20, 30, 40, 50]
n = 5
target_index = 5 // 2 = 2
```

**Trace Rekursi `find_element(0, 2)`:**
```
═══════════════════════════════════════════════════════════════
CALL 1: find_element(current_index=0, target_idx=2)
═══════════════════════════════════════════════════════════════
├─ Lokasi: Line 109
├─ Check: if current_index == target_idx
│  └─ if 0 == 2? → FALSE ❌
├─ Operasi: 1 perbandingan
└─ Rekursi: return find_element(0 + 1, 2)
   └─ Call: find_element(1, 2)
   
   ═══════════════════════════════════════════════════════════
   CALL 2: find_element(current_index=1, target_idx=2)
   ═══════════════════════════════════════════════════════════
   ├─ Lokasi: Line 109
   ├─ Check: if current_index == target_idx
   │  └─ if 1 == 2? → FALSE ❌
   ├─ Operasi: 1 perbandingan
   └─ Rekursi: return find_element(1 + 1, 2)
      └─ Call: find_element(2, 2)
      
      ═══════════════════════════════════════════════════════════
      CALL 3: find_element(current_index=2, target_idx=2) ✓ BASE CASE
      ═══════════════════════════════════════════════════════════
      ├─ Lokasi: Line 109
      ├─ Check: if current_index == target_idx
      │  └─ if 2 == 2? → TRUE ✓
      ├─ Operasi: 1 perbandingan, 1 akses array
      └─ Return: arr[2] = 30
         └─ Value: 30
```

**Return Chain:**
```
Call 3 → Return 30
  ↓
Call 2 → Return 30
  ↓
Call 1 → Return 30
  ↓
result = 30
```

**Setelah Rekursi:**
```python
result = 30  # Dari find_element(0, 2)

# Check ganjil/genap
if n % 2 == 0:  # 5 % 2 = 1 (ganjil)
    # Skip
else:
    return float(30)  # Return 30.0
```

**Result:** `30.0`

**Total Operasi:**
- Rekursi: 3 calls × 3 ops = 9 operasi
- Setelah rekursi: ~6 operasi
- **Total: ~15 operasi**

---

### Contoh 2: Array Genap (n=6, target_index=3)
```
Input: arr = [10, 20, 30, 40, 50, 60]
n = 6
target_index = 6 // 2 = 3
```

**Trace Rekursi `find_element(0, 3)`:**
```
═══════════════════════════════════════════════════════════════
CALL 1: find_element(current_index=0, target_idx=3)
═══════════════════════════════════════════════════════════════
├─ Check: 0 == 3? → FALSE ❌
├─ Operasi: 1 perbandingan, 1 penjumlahan, 1 call
└─ Rekursi: find_element(1, 3)
   
   ═══════════════════════════════════════════════════════════
   CALL 2: find_element(current_index=1, target_idx=3)
   ═══════════════════════════════════════════════════════════
   ├─ Check: 1 == 3? → FALSE ❌
   ├─ Operasi: 1 perbandingan, 1 penjumlahan, 1 call
   └─ Rekursi: find_element(2, 3)
      
      ═══════════════════════════════════════════════════════════
      CALL 3: find_element(current_index=2, target_idx=3)
      ═══════════════════════════════════════════════════════════
      ├─ Check: 2 == 3? → FALSE ❌
      ├─ Operasi: 1 perbandingan, 1 penjumlahan, 1 call
      └─ Rekursi: find_element(3, 3)
         
         ═══════════════════════════════════════════════════════════
         CALL 4: find_element(current_index=3, target_idx=3) ✓ BASE CASE
         ═══════════════════════════════════════════════════════════
         ├─ Check: 3 == 3? → TRUE ✓
         ├─ Operasi: 1 perbandingan, 1 akses array
         └─ Return: arr[3] = 40
```

**Return Chain:**
```
Call 4 → Return 40
  ↓
Call 3 → Return 40
  ↓
Call 2 → Return 40
  ↓
Call 1 → Return 40
  ↓
result = 40
```

**Setelah Rekursi:**
```python
result = 40  # Dari find_element(0, 3)

# Check ganjil/genap
if n % 2 == 0:  # 6 % 2 = 0 (genap) ✓
    val1 = arr[target_index - 1]  # arr[2] = 30
    val2 = result  # 40
    return (val1 + val2) / 2.0  # (30 + 40) / 2 = 35.0
```

**Result:** `35.0`

**Total Operasi:**
- Rekursi: 4 calls × 3 ops = 12 operasi
- Setelah rekursi: ~8 operasi
- **Total: ~20 operasi**

---

## 📈 Tabel Perbandingan Operasi

| n | target_index/middle_index | Iteratif (Loop Iterasi) | Iteratif (Ops) | Rekursif (Calls) | Rekursif (Ops) |
|---|---------------------------|-------------------------|----------------|------------------|----------------|
| 1 | 0 | 0 (base case) | ~4 | 0 | ~4 |
| 5 | 2 | 3 | ~15 | 3 | ~15 |
| 10 | 5 | 6 | ~24 | 6 | ~24 |
| 50 | 25 | 26 | ~84 | 26 | ~84 |
| 100 | 50 | 51 | ~159 | 51 | ~159 |
| 500 | 250 | 251 | ~759 | 251 | ~759 |
| 1000 | 500 | 501 | ~1509 | 501 | ~1509 |
| 10000 | 5000 | 5001 | ~15009 | 5001 | ~15009 |

**Catatan:**
- **Iteratif**: Loop berjalan (n//2 + 1) kali
- **Rekursif**: Rekursi dipanggil (n//2 + 1) kali
- **Kedua algoritma**: Sama-sama O(N), tapi iteratif lebih efisien karena tidak ada overhead rekursi

---

## 🎯 Key Points untuk Presentasi

### Untuk Algoritma Iteratif:
- ✅ **Ada loop!** Loop dari 0 sampai middle_index
- ✅ **Operasi linear:** Bergantung pada n, loop berjalan (n//2 + 1) kali
- ✅ **Setiap iterasi:** Akses arr[i] untuk mendapatkan nilai sampai middle_index
- ✅ **O(N) waktu, O(1) ruang**
- ✅ **Bisa dihitung:** T(n) = Σ(i=0 to n//2) 1 = (n//2) + 1 = O(n)

### Untuk Algoritma Rekursif:
- ⚠️ **Ada rekursi:** Memanggil diri sendiri n/2 kali
- ⚠️ **Operasi linear:** Proporsional dengan n
- ⚠️ **Stack frame:** Setiap call memakan memori
- ⚠️ **O(N) waktu dan ruang**

---

## 💡 Contoh Jawaban untuk Dosen

**Q: "Berapa operasi dasar untuk n=100?"**

**A (Iteratif):**
"Untuk iteratif, middle_index = 100//2 = 50, jadi loop berjalan 51 kali (dari i=0 sampai i=50). Setiap iterasi loop ada sekitar 3 operasi (perbandingan kondisi loop, increment i, akses array). Total sekitar 159 operasi. Semakin besar n, semakin banyak iterasi loop, jadi semakin banyak operasinya. Kompleksitas O(N)."

**A (Rekursif):**
"Untuk rekursif, target_index = 100//2 = 50, jadi ada 51 panggilan rekursif. Setiap panggilan sekitar 3 operasi, jadi total sekitar 159 operasi. Semakin besar n, semakin banyak operasinya. Kompleksitas O(N)."

**Perbandingan:**
"Kedua algoritma sama-sama O(N), tapi iteratif biasanya lebih cepat karena tidak ada overhead pemanggilan fungsi rekursif."

**Q: "Variabel i nya berapa?"**

**A (Iteratif):**
"Untuk algoritma iteratif, variabel i dalam loop berubah dari 0 sampai middle_index. Untuk n=5 dengan middle_index=2, i berubah dari 0 → 1 → 2, jadi ada 3 nilai. Loop berjalan 3 kali. Untuk n=100, i berubah dari 0 sampai 50, jadi loop berjalan 51 kali."

**A (Rekursif):**
"Untuk algoritma rekursif, variabel current_index berubah dari 0 sampai target_index. Untuk n=5 dengan target_index=2, current_index berubah dari 0 → 1 → 2. Jadi ada 3 panggilan rekursif. Untuk n=100, i berubah dari 0 sampai 50, jadi ada 51 panggilan."

**Kesimpulan:**
"Rumusnya sama untuk keduanya: i berubah sebanyak (target_index + 1) kali = (n//2 + 1) kali. Bedanya, iteratif menggunakan loop biasa, sedangkan rekursif menggunakan pemanggilan fungsi."

---

**Gunakan trace ini untuk presentasi! 🎤**

