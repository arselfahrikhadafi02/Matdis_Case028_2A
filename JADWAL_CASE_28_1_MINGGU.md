# Jadwal Pengerjaan Case 28 Selama 1 Minggu

## Tujuan Akhir Minggu

Pada akhir minggu, kelompok harus memiliki:

- Analisis masalah Case 28.
- Fishbone 6M dan sumber berita.
- Model graf yang benar.
- Implementasi Kruskal sebagai baseline.
- Implementasi Dynamic Subtree Repair.
- Registry dan runner yang berjalan.
- Semantic logging dan trace JSON.
- Test yang lulus.
- Benchmark baseline versus dynamic MST.
- Draft laporan dan slide presentasi.

Hasil numerik yang wajib sesuai:

```text
MST normal        = 13.0
MST setelah event = 18.0
Edge pengganti    = (0,2)
```

---

## Pembagian Anggota

### Anggota 1 - Analisis Kasus dan Dokumentasi

Fokus pada berita, fishbone, latar belakang, rumusan masalah, tujuan, dan laporan.

### Anggota 2 - Pemodelan Graf dan Baseline

Fokus pada YAML, perhitungan manual, Kruskal, dan validasi MST normal.

### Anggota 3 - Dynamic MST dan Logging

Fokus pada Dynamic Subtree Repair, event perubahan bobot, dan trace log.

### Anggota 4 - Integrasi, Testing, Benchmark, dan Presentasi

Fokus pada registry, runner, test, benchmark, output, grafik, dan integrasi akhir.

---

# Hari 1 - Persiapan dan Penyamaan Pemahaman

## Target Hari 1

Semua anggota memahami konteks kasus dan menyepakati hasil yang harus dicapai.

## Tugas Semua Anggota

1. Baca `README.md`, `TUTORIAL.md`, `GLOSSARY.md`, dan `RENCANA_CASE_28.md`.
2. Baca file `cases/scenarios/case_28_siskamling_portal.yaml`.
3. Pahami fungsi node, edge, bobot, start node, goal node, dan dynamic event.
4. Pastikan semua anggota memahami perbedaan MST dan shortest path.
5. Buat repository branch masing-masing jika menggunakan Git branch.
6. Jalankan test awal:

```bash
py -3 -m pytest -q
```

## Anggota 1

- Kumpulkan data berita smart gate.
- Catat lokasi, waktu, objek, subjek, dan dinamika operasional.
- Susun draft latar belakang 3-5 paragraf.
- Susun rumusan masalah dan tujuan proyek.

## Anggota 2

- Salin node dan edge dari YAML ke tabel dokumentasi.
- Hitung MST normal secara manual dengan Kruskal.
- Tulis urutan edge berdasarkan bobot.
- Tentukan edge yang masuk dan ditolak karena siklus.

## Anggota 3

- Pelajari konsep dynamic MST.
- Buat diagram atau catatan proses subtree repair.
- Jelaskan bagaimana edge `(0,1)` membagi tree menjadi dua subtree.
- Tentukan kandidat edge pengganti.

## Anggota 4

- Periksa cara kerja `AlgorithmRegistry`.
- Periksa alur `SimulationRunner`.
- Catat perintah untuk menjalankan simulation dan benchmark.
- Siapkan format tabel hasil benchmark.

## Output Hari 1

```text
[ ] Semua anggota memahami kasus
[ ] Draft latar belakang tersedia
[ ] Perhitungan MST manual tersedia
[ ] Desain dynamic repair tersedia
[ ] Perintah test dan benchmark disepakati
```

---

# Hari 2 - Dokumentasi dan Pemodelan Graf

## Target Hari 2

Dokumentasi masalah dan model graf selesai serta konsisten dengan YAML.

## Anggota 1

1. Selesaikan analisis berita.
2. Buat atau rapikan fishbone 6M.
3. Hubungkan tiap faktor fishbone dengan perubahan bobot edge.
4. Tulis:
   - Latar belakang.
   - Rumusan masalah.
   - Tujuan.
   - Batasan masalah.
   - Manfaat proyek.

## Anggota 2

1. Validasi lima node pada YAML.
2. Validasi enam edge awal.
3. Buat tabel node dan edge.
4. Buat perhitungan MST normal:

```text
(1,2) = 2
(0,1) = 3
(2,3) = 3
(3,4) = 5
Total = 13
```

5. Buat perhitungan MST setelah event:

```text
(1,2) = 2
(2,3) = 3
(3,4) = 5
(0,2) = 8
Total = 18
```

## Anggota 3

- Buat pseudocode Dynamic Subtree Repair.
- Tentukan kondisi kapan repair lokal dipakai.
- Tentukan kondisi kapan full recalculation digunakan sebagai fallback.
- Tulis penjelasan bahwa pendekatan ini bukan implementasi HLT penuh.

## Anggota 4

- Buat rancangan test case.
- Tentukan expected result untuk setiap test.
- Buat format pencatatan runtime, step count, dan MST cost.

## Output Hari 2

```text
[ ] Fishbone dan latar belakang selesai
[ ] Tabel graf selesai
[ ] Perhitungan manual selesai
[ ] Pseudocode dynamic MST selesai
[ ] Rancangan test selesai
```

---

# Hari 3 - Implementasi Baseline Kruskal

## Target Hari 3

Baseline Kruskal dapat dijalankan untuk kondisi normal dan dynamic event.

## Anggota 1

- Masukkan dokumentasi latar belakang ke draft laporan.
- Tambahkan gambar berita dan fishbone.
- Pastikan istilah node, edge, bobot, MST, dan dynamic event konsisten.

## Anggota 2

Kerjakan file:

```text
algorithms/spanning_tree/static_kruskal.py
```

Implementasikan:

1. Pewarisan `BaseGraphAlgorithm`.
2. `run(start_node, goal_node)`.
3. Struktur Union-Find.
4. Pengurutan edge berdasarkan bobot.
5. Penyimpanan `mst_edges`.
6. Penyimpanan `total_mst_cost`.
7. Rekonstruksi path pada MST.
8. `handle_dynamic_event(event_data)` dengan full recalculation.

Pastikan hasil kondisi normal:

```text
MST cost = 13.0
Path = 0 -> 1 -> 2 -> 3 -> 4
```

## Anggota 3

- Review kontrak `BaseGraphAlgorithm`.
- Review format `log_step()` pada baseline.
- Pastikan event log menyimpan bobot lama dan bobot baru.
- Berikan masukan terhadap kode Anggota 2.

## Anggota 4

- Jalankan test langsung terhadap `StaticKruskal`.
- Cek apakah class dapat dibuat tanpa error abstract method.
- Cek apakah output path dimulai dari node 0 dan berakhir di node 4.
- Catat error dan laporkan ke Anggota 2.

## Output Hari 3

```text
[ ] StaticKruskal dapat di-import
[ ] run(0, 4) berjalan
[ ] MST normal bernilai 13.0
[ ] Path normal valid
[ ] Dynamic event baseline dapat dipanggil
```

---

# Hari 4 - Implementasi Dynamic MST

## Target Hari 4

Dynamic MST dapat mengganti edge MST yang menjadi mahal.

## Anggota 1

- Tulis bagian teori MST dan Kruskal.
- Tulis penjelasan masalah sebelum dan sesudah portal ditutup.
- Siapkan narasi untuk presentasi maksimal 2 menit.

## Anggota 2

- Review hasil baseline.
- Membantu pengecekan struktur edge dan konektivitas.
- Membandingkan hasil dynamic dengan hasil manual.

## Anggota 3

Kerjakan file:

```text
algorithms/spanning_tree/subtree_repair.py
```

Implementasikan:

1. Pewarisan dari `StaticKruskal`.
2. Penyimpanan MST sebelumnya.
3. Deteksi edge yang berubah.
4. Deteksi apakah edge termasuk MST.
5. Pemisahan tree menjadi dua komponen.
6. Pencarian edge lintas-komponen.
7. Pemilihan edge pengganti termurah.
8. Pembaruan total MST cost.
9. Logging `MST_EDGE_REPLACED`.
10. Fallback full recalculation jika repair lokal tidak valid.

Ekspektasi:

```text
Edge lama     = (0,1), weight 95.0
Edge pengganti = (0,2), weight 8.0
Total MST     = 18.0
Path baru     = 0 -> 2 -> 3 -> 4
```

## Anggota 4

- Jalankan dynamic event menggunakan script kecil atau benchmark.
- Periksa apakah registry dapat membuat dynamic solver.
- Periksa apakah delta log bertambah setelah event.

## Output Hari 4

```text
[ ] DynamicSubtreeRepair dapat dibuat
[ ] Event (0,1) -> 95 berjalan
[ ] Edge (0,2) menjadi pengganti
[ ] Total MST menjadi 18.0
[ ] Path baru valid
[ ] Log MST_EDGE_REPLACED tersedia
```

---

# Hari 5 - Registry, Testing, dan Perbaikan

## Target Hari 5

Seluruh implementasi terintegrasi dan test utama lulus.

## Anggota 1

- Review istilah dan narasi laporan.
- Pastikan hasil numerik laporan sama dengan hasil program.
- Tambahkan tabel hasil yang masih kosong.

## Anggota 2

- Review StaticKruskal.
- Cek edge duplikat, siklus, dan konektivitas.
- Periksa apakah graf diperlakukan sebagai graf tak berarah.

## Anggota 3

- Review DynamicSubtreeRepair.
- Cek kasus edge MST naik bobot.
- Cek kasus edge bukan MST yang berubah.
- Cek fallback recalculation.
- Cek konsistensi `mst_edges` dan `total_mst_cost`.

## Anggota 4

Pastikan registry memuat:

```text
static_kruskal
dynamic_subtree_repair
spanning_tree
```

Jalankan:

```bash
py -3 -m pytest -q
```

Lakukan test khusus:

```text
MST normal = 13.0
MST setelah event = 18.0
Edge pengganti = (0,2)
Semua node tetap terhubung
```

## Output Hari 5

```text
[ ] Registry aktif
[ ] Semua test lulus
[ ] Tidak ada error import
[ ] Tidak ada error abstract class
[ ] Trace log konsisten
```

---

# Hari 6 - Simulation, Benchmark, dan Analisis

## Target Hari 6

Output program dan tabel perbandingan selesai.

## Semua Anggota

Sebelum benchmark, instal dependency jika diperlukan:

```bash
py -3 -m pip install -r requirements.txt
```

## Anggota 1

- Menulis analisis dampak penutupan portal.
- Menjelaskan kenaikan cost dari 13 menjadi 18.
- Menjelaskan manfaat menjaga konektivitas seluruh pos.

## Anggota 2

Jalankan baseline benchmark:

```bash
py -3 -m scripts.run_benchmark cases/scenarios/case_28_siskamling_portal.yaml --algorithm static_kruskal
```

Catat:

- Initial runtime.
- Reroute runtime.
- Step count.
- Initial cost.
- Final cost.

## Anggota 3

Jalankan dynamic benchmark:

```bash
py -3 -m scripts.run_benchmark cases/scenarios/case_28_siskamling_portal.yaml --algorithm spanning_tree
```

Periksa apakah log menunjukkan repair lokal.

## Anggota 4

Jalankan simulation:

```bash
py -3 -m scripts.run_simulation cases/scenarios/case_28_siskamling_portal.yaml --algorithm spanning_tree
```

Periksa output di folder:

```text
output/
```

Jika simulation gagal karena `matplotlib`, instal dependency dari `requirements.txt` lalu ulangi.

## Format Tabel Benchmark

| Metrik | Static Kruskal | Dynamic Subtree Repair |
|---|---:|---:|
| Initial MST cost | 13.0 | 13.0 |
| Final MST cost | 18.0 | 18.0 |
| Initial runtime | isi hasil | isi hasil |
| Event runtime | isi hasil | isi hasil |
| Step count | isi hasil | isi hasil |
| Strategi event | full recalculation | subtree repair |

## Output Hari 6

```text
[ ] Benchmark baseline selesai
[ ] Benchmark dynamic selesai
[ ] Simulation selesai
[ ] Trace JSON tersedia
[ ] Tabel perbandingan selesai
[ ] Analisis hasil selesai
```

---

# Hari 7 - Finalisasi Laporan dan Presentasi

## Target Hari 7

Semua dokumen, kode, hasil, dan presentasi siap dikumpulkan.

## Anggota 1

- Finalisasi pendahuluan.
- Finalisasi berita dan fishbone.
- Periksa tata bahasa dan sitasi.

## Anggota 2

- Finalisasi pemodelan graf.
- Finalisasi teori Kruskal.
- Pastikan screenshot atau output MST sesuai hasil program.

## Anggota 3

- Finalisasi teori dynamic MST.
- Finalisasi pseudocode.
- Jelaskan proses edge replacement.
- Pastikan klaim tentang HLT tidak berlebihan.

## Anggota 4

- Finalisasi tabel benchmark.
- Finalisasi hasil testing.
- Gabungkan branch atau perubahan seluruh anggota.
- Jalankan test terakhir.
- Siapkan slide demo program.

## Struktur Laporan Akhir

1. Cover.
2. Identitas kelompok.
3. Latar belakang.
4. Sumber berita.
5. Fishbone 6M.
6. Rumusan masalah.
7. Tujuan.
8. Pemodelan graf.
9. Perhitungan manual.
10. Teori Kruskal.
11. Teori Dynamic MST.
12. Implementasi.
13. Semantic logging.
14. Pengujian.
15. Benchmark.
16. Analisis hasil.
17. Kesimpulan.
18. Daftar pustaka.
19. Lampiran.

## Struktur Slide Presentasi

1. Judul dan anggota kelompok.
2. Latar belakang kasus.
3. Berita smart gate.
4. Fishbone 6M.
5. Model graf.
6. MST kondisi normal.
7. Dynamic event.
8. MST setelah event.
9. Algoritma Kruskal.
10. Dynamic Subtree Repair.
11. Demo atau trace program.
12. Benchmark.
13. Kesimpulan.

---

# Checklist Final

```text
[ ] Berita dan sumber sudah dicantumkan
[ ] Fishbone 6M selesai
[ ] YAML valid
[ ] Model node dan edge benar
[ ] MST normal = 13.0
[ ] Event (0,1) -> 95.0 berjalan
[ ] MST setelah event = 18.0
[ ] Edge (0,2) menjadi pengganti
[ ] Semua node tetap terhubung
[ ] StaticKruskal terdaftar
[ ] DynamicSubtreeRepair terdaftar
[ ] Alias spanning_tree aktif
[ ] Semantic logging tersedia
[ ] Trace JSON berhasil dibuat
[ ] Pytest lulus
[ ] Benchmark selesai
[ ] Simulation berhasil
[ ] Laporan selesai
[ ] Slide selesai
[ ] Semua anggota memahami bagian masing-masing
[ ] Backup kode dan dokumen sudah dibuat
```

## Kriteria Keberhasilan

Proyek dinyatakan selesai apabila kode, hasil perhitungan manual, trace log, benchmark, laporan, dan presentasi menunjukkan hasil yang konsisten:

```text
MST awal       = 13.0
MST setelah    = 18.0
Replacement    = (0,2)
Connectivity   = seluruh node tetap terhubung
```
