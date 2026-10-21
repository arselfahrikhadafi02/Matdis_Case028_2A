# Case 28 Implementation

Implementasi ulang dari awal untuk Case 28: Siskamling Portal menggunakan Minimum Spanning Tree.

## Struktur

```text
case_28_implementation/
├── data/       # YAML skenario graf dan dynamic event
├── docs/       # laporan, fishbone, dan catatan teori
├── output/     # trace JSON dan hasil benchmark
├── src/        # model graf, baseline Kruskal, dan dynamic MST
├── tests/      # unit test dan integration test
├── README.md
└── requirements.txt
```

## Target hasil

```text
MST normal        = 13.0
MST setelah event = 18.0
Edge pengganti    = (0,2)
```

## Urutan pengerjaan

1. Validasi data pada `data/case_28_siskamling_portal.yaml`.
2. Buat model graf.
3. Implementasikan Kruskal baseline.
4. Implementasikan Dynamic Subtree Repair.
5. Tambahkan semantic logging.
6. Tambahkan registry dan runner.
7. Buat test.
8. Jalankan benchmark dan dokumentasikan hasil.

## Instalasi

```bash
py -3 -m pip install -r requirements.txt
```

## Validasi

```bash
py -3 -m pytest -q
```
