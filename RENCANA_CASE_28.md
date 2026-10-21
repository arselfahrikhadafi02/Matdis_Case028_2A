# Rencana Pengerjaan Case 28: Siskamling Portal - MST

## 1. Tujuan Proyek

Menganalisis jaringan komunikasi siskamling yang mengalami perubahan ketika portal malam ditutup, kemudian membandingkan:

1. **Baseline:** algoritma Kruskal statis.
2. **Dynamic MST:** pendekatan Dynamic Subtree Repair yang terinspirasi konsep Dynamic MST Holm, de Lichtenberg, dan Thorup.

Hasil utama yang harus dibuktikan:

```text
MST normal        = 13.0
MST setelah event = 18.0
Edge pengganti    = (0,2)
```

---

## 2. Pemahaman Kasus

### 2.1 Identitas

```text
Kasus     : 28
Judul     : Siskamling Portal
Algoritma : Minimum Spanning Tree
Kelas     : 2A
```

### 2.2 Masalah Utama

Penutupan portal pada pukul 00.00-05.30 menyebabkan koneksi `(0,1)` mengalami kenaikan bobot:

```text
3.0 -> 95.0
```

Perubahan tersebut membuat koneksi langsung dari Pos Utama ke Portal Utara tidak lagi ekonomis. Sistem harus mencari topologi jaringan minimum baru tanpa memutus konektivitas seluruh pos.

### 2.3 Sumber Data

Gunakan:

- Berita detikNews tentang smart gate Gandaria Utara.
- Slide dosen.
- Diagram fishbone 6M.
- File `cases/scenarios/case_28_siskamling_portal.yaml`.
- Referensi Kruskal, Prim, dan Holm-de Lichtenberg-Thorup.

---

## 3. Dokumentasi Latar Belakang

Jelaskan hal-hal berikut:

- Terdapat empat titik smart gate.
- Portal ditutup pada malam hari.
- Penutupan mengganggu komunikasi antarpos.
- Personel ronda malam terbatas.
- Gang sempit dan bangunan menghambat transmisi.
- Jalur komunikasi tertentu mengalami penalti bobot.

### Fishbone 6M

1. **Machine:** smart gate, HT, router atau repeater.
2. **Methods:** SOP penutupan portal dan topologi statis.
3. **Man:** keterbatasan petugas ronda dan koordinasi manual.
4. **Material:** portal besi dan kabel komunikasi.
5. **Measurement:** kenaikan bobot `(0,1)` dari `3.0` menjadi `95.0`.
6. **Mother Nature:** gang sempit, malam hari, dan hambatan bangunan.

Output dokumentasi:

- Narasi latar belakang.
- Sumber berita.
- Diagram fishbone.
- Hubungan masalah nyata dengan graf.

---

## 4. Pemodelan Graf

Gunakan graf tak berarah berbobot:

$$
G = (V, E, W)
$$

### 4.1 Node

| Node | Keterangan |
|---|---|
| 0 | Pos Ronda Utama |
| 1 | Portal Sektor Utara |
| 2 | Portal Sektor Selatan |
| 3 | Pos Lapangan Tengah |
| 4 | Balai Desa |

### 4.2 Edge Awal

| Edge | Bobot |
|---|---:|
| `(0,1)` | 3.0 |
| `(0,2)` | 8.0 |
| `(1,2)` | 2.0 |
| `(1,3)` | 6.0 |
| `(2,3)` | 3.0 |
| `(3,4)` | 5.0 |

### 4.3 Dynamic Event

```yaml
type: DYNAMIC_EDGE_WEIGHT
target_edge: [0, 1]
new_weight: 95.0
```

### 4.4 Perhitungan Manual

MST kondisi normal:

```text
(1,2) = 2
(0,1) = 3
(2,3) = 3
(3,4) = 5

Total = 13
```

MST setelah event:

```text
(1,2) = 2
(2,3) = 3
(3,4) = 5
(0,2) = 8

Total = 18
```

Perhitungan manual menjadi acuan validasi kode.

---

## 5. Persiapan Project

Pastikan dependency tersedia:

```bash
py -3 -m pip install -r requirements.txt
```

Periksa struktur utama:

```text
algorithms/
cases/
config/
core/
scripts/
tests/
output/
```

Jalankan test awal sebelum perubahan:

```bash
py -3 -m pytest -q
```

Catat hasil awal sebagai pembanding.

---

## 6. Implementasi Baseline Kruskal

File utama:

```text
algorithms/spanning_tree/static_kruskal.py
```

### Persyaratan

Kelas `StaticKruskal` harus:

- Mewarisi `BaseGraphAlgorithm`.
- Memiliki `run(start_node, goal_node)`.
- Memiliki `handle_dynamic_event(event_data)`.
- Menggunakan Union-Find.
- Menghasilkan MST.
- Menyimpan `mst_edges` dan `total_mst_cost`.
- Menghasilkan path dari start ke goal melalui MST.
- Mencatat proses menggunakan `self.log_step()`.

### Perilaku `run()`

1. Membaca seluruh node dan edge.
2. Mengubah graf menjadi graf tak berarah.
3. Mengurutkan edge berdasarkan bobot.
4. Menambahkan edge yang tidak membentuk siklus.
5. Menyimpan MST.
6. Mencari path dari node `0` ke node `4`.
7. Mencatat total MST cost.

### Perilaku Dynamic Event

Ketika `(0,1)` berubah menjadi `95.0`:

1. Ubah bobot edge.
2. Catat event.
3. Jalankan ulang Kruskal dari awal.
4. Kembalikan MST dan path baru.

---

## 7. Implementasi Dynamic MST

File utama:

```text
algorithms/spanning_tree/subtree_repair.py
```

### Konsep Dynamic Subtree Repair

Jika edge MST mengalami kenaikan bobot:

1. Hapus edge dari MST.
2. Tree terpecah menjadi dua komponen.
3. Cari edge yang menghubungkan kedua komponen.
4. Pilih edge pengganti dengan bobot terkecil.
5. Masukkan edge pengganti.
6. Hitung ulang total cost.

Untuk kasus ini:

```text
Edge yang dihapus  : (0,1)
Edge pengganti     : (0,2)
```

### Fallback

Jika edge bukan bagian dari MST, tidak ada kandidat pengganti, atau repair lokal tidak valid, algoritma dapat melakukan full recalculation.

### Catatan Akademik

Implementasi ini harus disebut sebagai:

> Dynamic Subtree Repair sebagai pendekatan praktis yang terinspirasi konsep Dynamic MST Holm-de Lichtenberg-Thorup.

Jangan menyebutnya sebagai implementasi penuh struktur data HLT.

---

## 8. Registry dan Integrasi

Daftarkan algoritma berikut:

```text
static_kruskal
dynamic_subtree_repair
spanning_tree
```

Alias:

```text
spanning_tree -> dynamic_subtree_repair
```

Runner tidak boleh melakukan hardcode terhadap kelas algoritma.

### Simulasi

```bash
py -3 -m scripts.run_simulation cases/scenarios/case_28_siskamling_portal.yaml --algorithm spanning_tree
```

### Benchmark Baseline

```bash
py -3 -m scripts.run_benchmark cases/scenarios/case_28_siskamling_portal.yaml --algorithm static_kruskal
```

### Benchmark Dynamic

```bash
py -3 -m scripts.run_benchmark cases/scenarios/case_28_siskamling_portal.yaml --algorithm spanning_tree
```

---

## 9. Semantic Logging

Minimal event log yang harus tersedia:

```text
INITIALIZATION
MST_PATH_FOUND
UPDATE_WEIGHT_EVENT
MST_EDGE_REPLACED
```

Data yang dicatat:

```text
target_edge
old_weight
new_weight
removed_edge
replacement_edge
replacement_weight
active_path
path_cost
total_mst_cost
```

Validasi logging:

```python
solver.step_count > 0
len(solver.delta_logs) == solver.step_count
```

Trace JSON disimpan di folder `output/`.

---

## 10. Testing

### Test MST Normal

```text
Jumlah edge MST = 4
Total MST cost = 13.0
```

### Test Event

```text
Total MST cost = 18.0
Edge (0,1) tidak lagi berada di MST
Edge (0,2) digunakan sebagai pengganti
```

### Test Path

```text
Sebelum event : 0 -> 1 -> 2 -> 3 -> 4
Setelah event : 0 -> 2 -> 3 -> 4
```

### Test Konektivitas

Semua node harus tetap terhubung setelah event.

### Jalankan Test

```bash
py -3 -m pytest -q
```

Target minimal: seluruh test berhasil.

---

## 11. Benchmark dan Analisis

Buat tabel berikut:

| Metrik | Kruskal | Dynamic Subtree Repair |
|---|---:|---:|
| Initial MST cost | 13.0 | 13.0 |
| Final MST cost | 18.0 | 18.0 |
| Initial runtime | hasil program | hasil program |
| Event runtime | hasil program | hasil program |
| Step count | hasil program | hasil program |
| Edge replacement | recalculation | local repair |

Analisis yang perlu ditulis:

- Kruskal lebih sederhana dan mudah dipahami.
- Kruskal melakukan perhitungan ulang penuh.
- Dynamic repair memanfaatkan MST sebelumnya.
- Dynamic repair lebih sesuai untuk perubahan graf berulang.
- Pada graf kecil, perbedaan runtime dapat tidak terlalu besar.
- Keuntungan dynamic algorithm lebih terlihat pada graf besar dan event yang sering.

---

## 12. Struktur Laporan

1. Cover.
2. Identitas kelompok.
3. Latar belakang kasus.
4. Sumber berita.
5. Fishbone 6M.
6. Pemodelan graf.
7. Perhitungan MST manual.
8. Teori Kruskal.
9. Teori Dynamic MST.
10. Desain sistem.
11. Implementasi program.
12. Semantic logging.
13. Skenario dynamic event.
14. Pengujian.
15. Benchmark.
16. Analisis hasil.
17. Kelebihan dan kekurangan.
18. Kesimpulan.
19. Daftar pustaka.
20. Lampiran kode dan output.

---

## 13. Pembagian Tugas Empat Orang

### Anggota 1: Analisis Kasus dan Dokumentasi

Tanggung jawab:

- Menganalisis berita.
- Menulis latar belakang.
- Menyusun fishbone 6M.
- Menjelaskan masalah utama.
- Menulis hubungan masalah nyata dengan graf.
- Menyiapkan bagian pendahuluan laporan.

Output:

```text
Latar belakang
Fishbone
Rumusan masalah
Tujuan
Sumber berita
```

### Anggota 2: Pemodelan Graf dan Baseline

Tanggung jawab:

- Memvalidasi node dan edge.
- Menghitung MST manual.
- Mengimplementasikan atau memeriksa `StaticKruskal`.
- Membuat perhitungan baseline.
- Menjelaskan kompleksitas Kruskal.

Output:

```text
Model graf
Perhitungan MST normal
Kode StaticKruskal
Analisis kompleksitas
```

### Anggota 3: Dynamic MST dan Semantic Logging

Tanggung jawab:

- Mendesain Dynamic Subtree Repair.
- Mengimplementasikan `subtree_repair.py`.
- Menangani event `(0,1) -> 95.0`.
- Memastikan edge `(0,2)` menjadi pengganti.
- Memeriksa logging dan trace JSON.
- Menjelaskan hubungan dengan HLT.

Output:

```text
Kode DynamicSubtreeRepair
MST setelah event
Trace log
Penjelasan algoritma dynamic
```

### Anggota 4: Integrasi, Testing, Benchmark, dan Presentasi

Tanggung jawab:

- Memeriksa registry.
- Menjalankan simulasi.
- Menjalankan pytest.
- Menjalankan benchmark.
- Membuat tabel runtime dan step count.
- Membuat grafik perbandingan jika diperlukan.
- Menggabungkan laporan dan menyiapkan presentasi.

Output:

```text
Hasil test
Hasil benchmark
Tabel perbandingan
Output JSON
Slide presentasi
```

---

## 14. Urutan Kerja Kelompok

### Sesi 1: Persiapan

- Menyamakan pemahaman kasus.
- Memastikan data graf.
- Menentukan format output.
- Membagi tugas dan branch.

### Sesi 2: Pengerjaan Inti

- Anggota 1 menyelesaikan analisis dan fishbone.
- Anggota 2 menyelesaikan model graf dan Kruskal.
- Anggota 3 menyelesaikan desain dynamic MST.
- Anggota 4 menyiapkan test dan format benchmark.

### Sesi 3: Integrasi

- Menggabungkan seluruh kode.
- Mendaftarkan algoritma pada registry.
- Menjalankan test.
- Memperbaiki error.

### Sesi 4: Validasi

- Menjalankan simulasi dan benchmark.
- Memvalidasi hasil dengan perhitungan manual.
- Membuat tabel dan grafik.
- Menulis analisis.

### Sesi 5: Finalisasi

- Finalisasi laporan.
- Finalisasi slide.
- Latihan presentasi.
- Memastikan semua anggota memahami kode dan hasil.

---

## 15. Checklist Selesai

```text
[ ] YAML Case 28 valid
[ ] MST normal menghasilkan cost 13.0
[ ] Event bobot (0,1) -> 95.0 berjalan
[ ] MST baru menghasilkan cost 18.0
[ ] Edge (0,2) menjadi pengganti
[ ] Semua node tetap terhubung
[ ] Baseline dan dynamic algorithm terdaftar di registry
[ ] Semantic logging tersedia
[ ] Semua test lulus
[ ] Benchmark berhasil
[ ] Trace JSON berhasil dibuat
[ ] Laporan selesai
[ ] Slide presentasi selesai
[ ] Semua anggota memahami alur program
```

## 16. Kesimpulan yang Diharapkan

Penutupan portal menyebabkan bobot koneksi `(0,1)` meningkat dari `3.0` menjadi `95.0`. Dynamic MST mengganti koneksi tersebut dengan edge `(0,2)`, sehingga seluruh pos tetap terhubung.

Total cost jaringan meningkat dari `13.0` menjadi `18.0`, tetapi jaringan tetap memiliki struktur minimum spanning tree dan dapat digunakan untuk komunikasi siskamling.
