# Web Scraping — Data Mining

Proyek scraping data dari dua situs latihan (*quotes.toscrape.com* dan *scrapethissite.com*), dibangun dengan Python. Mencakup pengambilan data terstruktur, penanganan pagination, konsumsi endpoint AJAX/JSON, dan ekspor hasil ke format CSV.

<p align="left">
  <img src="https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white" alt="Python 3.13">
  <img src="https://img.shields.io/badge/BeautifulSoup4-parser-yellow" alt="BeautifulSoup4">
  <img src="https://img.shields.io/badge/pandas-data--processing-150458?logo=pandas&logoColor=white" alt="pandas">
  <img src="https://img.shields.io/badge/status-completed-brightgreen" alt="Status">
</p>

---

## Daftar Isi

- [Ringkasan](#ringkasan)
- [Struktur Proyek](#struktur-proyek)
- [Tech Stack](#tech-stack)
- [Instalasi](#instalasi)
- [Cara Menjalankan](#cara-menjalankan)
- [Detail Implementasi](#detail-implementasi)
- [Hasil Scraping](#hasil-scraping)
- [Etika Scraping](#etika-scraping)
- [Author](#author)

---

## Ringkasan

Repo ini berisi dua modul scraping independen:

| Modul | Target | Tantangan Teknis |
|---|---|---|
| `latihan_mandiri.py` | [quotes.toscrape.com](https://quotes.toscrape.com) | Multi-page crawling, ekstraksi kategori/tag unik |
| `mini_project.py` | [scrapethissite.com/pages](https://www.scrapethissite.com/pages/) | Static HTML parsing, pagination berbasis query parameter, konsumsi endpoint AJAX/JSON |

Setiap modul dijalankan secara mandiri dan menghasilkan output CSV yang siap dianalisis.

## Struktur Proyek

```
WebScrapping/
├── latihan_mandiri.py       # Scraper quotes.toscrape.com
├── mini_project.py          # Scraper scrapethissite.com (3 halaman)
├── output/
│   ├── quotes.csv           # Seluruh quote dari 3 halaman
│   ├── kategori.csv         # Daftar kategori/tag unik
│   ├── countries.csv        # Data negara (nama, ibukota, populasi, luas)
│   ├── hockey_teams.csv     # Data tim hockey lintas 24 halaman
│   └── oscar_films.csv      # Data film pemenang Oscar 2010–2015
└── README.md
```

## Tech Stack

- **Python 3.13**
- [`requests`](https://docs.python-requests.org/) — HTTP client
- [`beautifulsoup4`](https://www.crummy.com/software/BeautifulSoup/) — HTML parsing
- [`pandas`](https://pandas.pydata.org/) — transformasi & ekspor data

## Instalasi

```bash
git clone https://github.com/Ryannfirmansyah/web-scraping-data-mining.git
cd web-scraping-data-mining
pip install requests beautifulsoup4 pandas
```

## Cara Menjalankan

```bash
python latihan_mandiri.py
python mini_project.py
```

Output otomatis tersimpan ke folder `output/` dalam format CSV.

## Detail Implementasi

**Rate limiting** — setiap request diberi jeda 2 detik untuk menghindari pembebanan server target, sekaligus praktik scraping yang bertanggung jawab.

**Pagination** — `mini_project.py` melakukan iterasi melalui parameter `page_num` pada halaman Hockey Teams hingga halaman mengembalikan hasil kosong, alih-alih mengandalkan parsing tombol navigasi yang rentan berubah struktur.

**AJAX/JSON endpoint** — halaman Oscar Winning Films memuat data secara asynchronous. Alih-alih parsing HTML statis, scraper melakukan request langsung ke endpoint data (`?ajax=true&year=<tahun>`) yang mengembalikan JSON, lalu dinormalisasi ke bentuk tabular.

**Ekstraksi kategori** — kategori pada `latihan_mandiri.py` dikumpulkan dari agregasi tag unik di seluruh quote, bukan dari parsing sidebar statis, sehingga hasilnya konsisten dengan data quote yang benar-benar terambil.

## Hasil Scraping

| Dataset | Jumlah Baris | Sumber |
|---|---:|---|
| Quotes | 30 | 3 halaman |
| Kategori unik | 60 | Agregasi tag |
| Countries | 250 | 1 halaman |
| Hockey Teams | 582 | 24 halaman |
| Oscar Films | 87 | 6 tahun (2010–2015) |

## Etika Scraping

Kedua situs target (*quotes.toscrape.com* dan *scrapethissite.com*) merupakan sandbox resmi yang memang disediakan untuk latihan web scraping. Proyek ini menerapkan delay antar-request dan tidak melakukan scraping paralel/agresif, sebagai praktik yang dapat diterapkan pada scraping situs produksi lainnya.

## Author

**Ryan Firmansyah**
NIM H071241082 — Sistem Informasi, Universitas Hasanuddin
Tugas Individu — Mata Kuliah Data Mining
