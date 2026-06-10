# Product Requirements Document (PRD)
## Glow Suite - Aplikasi Manajemen Keuangan Toko Skincare

---

## 1. Executive Summary

**Nama Produk:** Glow Suite  
**Versi:** 1.0 (MVP)  
**Target Pengguna:** Pemilik toko skincare skala kecil (1-3 karyawan)  
**Platform:** Android (primary), iOS (stretch goal)  
**Model Aplikasi:** Offline-first  
**Target APK Size:** < 10 MB  

### 1.1 Visi Produk
Memberikan solusi pencatatan keuangan yang sederhana, cepat, dan dapat diandalkan untuk pemilik toko skincare skala kecil yang memiliki keterbatasan akses internet dan perangkat mid-low end.

### 1.2 Masalah yang Diselesaikan
- Sulit mengelola pemasukan harian yang beragam (berbagai produk skincare)
- Kesulitan mencatat pengeluaran operasional secara terstruktur
- Tidak ada sistem tracking kasbon karyawan/teman
- Tidak ada laporan ringkasan keuangan harian/mingguan
- Sulit backup data keuangan untuk keperluan akuntansi

---

## 2. User Persona

### 2.1 Primary User: Pemilik Toko Skincare
- **Usia:** 25-45 tahun
- **Pendidikan:** SMA hingga Sarjana
- **Teknis:** Minim - tidak terbiasa dengan aplikasi kompleks
- **Perangkat:** Android mid-low end (RAM 2-3 GB, storage 16-32 GB)
- **Koneksi:** 3G/4G tidak stabil, sering offline
- **Kebutuhan:** Cepat, sederhana, tidak perlu banyak langkah

### 2.2 Secondary User: Karyawan Toko
- **Usia:** 18-35 tahun
- **Teknis:** Menengah
- **Kebutuhan:** Input transaksi cepat, lihat ringkasan shift

---

## 3. Functional Requirements

### 3.1 Fitur Pemasukan (Income)
**Priority:** Must Have

| ID | Requirement | Deskripsi | Acceptance Criteria |
|----|--------------|-----------|---------------------|
| F01 | Input Pemasukan | User dapat mencatat pemasukan harian | Form: tanggal, produk, qty, harga satuan, total |
| F02 | Kategori Pemasukan | Default kategori: Penjualan | Bisa ditambah kategori custom |
| F03 | Lihat Riwayat Pemasukan | List pemasukan per hari | Sort by date DESC, bisa filter by date range |
| F04 | Edit Pemasukan | User bisa edit transaksi yang salah | Edit semua field termasuk jumlah |
| F05 | Hapus Pemasukan | User bisa hapus transaksi | Konfirmasi sebelum hapus |

### 3.2 Fitur Pengeluaran (Expense)
**Priority:** Must Have

| ID | Requirement | Deskripsi | Acceptance Criteria |
|----|--------------|-----------|---------------------|
| F06 | Input Pengeluaran | Catat biaya operasional | Form: tanggal, kategori, jumlah, catatan opsional |
| F07 | Kategori Pengeluaran | Kategori tetap: Bahan Baku, Sewa, Gaji, Listrik, Lainnya | Fixed categories, tidak bisa diubah user |
| F08 | Lihat Riwayat Pengeluaran | List pengeluaran per hari | Sort by date DESC |
| F09 | Edit/Hapus Pengeluaran | CRUD lengkap | Sama seperti pemasukan |

### 3.3 Fitur Kasbon
**Priority:** Must Have

| ID | Requirement | Deskripsi | Acceptance Criteria |
|----|--------------|-----------|---------------------|
| F10 | Input Kasbon | Catat pinjaman karyawan/teman | Form: nama, jumlah, tanggal pinjam, catatan |
| F11 | Status Kasbon | Default: Belum Lunas | Bisa ditandai Lunas |
| F12 | Tandai Lunas | Update status ke Lunas | Input tanggal bayar opsional |
| F13 | Filter Kasbon | Filter: Semua / Belum Lunas / Sudah Lunas | Quick filter di header list |
| F14 | Lihat Total Kasbon | Total kasbon belum lunas | Ditampilkan di summary card |

### 3.4 Fitur Ringkasan (Summary)
**Priority:** Must Have

| ID | Requirement | Deskripsi | Acceptance Criteria |
|----|--------------|-----------|---------------------|
| F15 | Ringkasan Harian | Total pemasukan, pengeluaran, laba rugi | Update real-time saat ada transaksi baru |
| F16 | Ringkasan Bulanan | Agregasi per bulan | Chart atau table sederhana |

### 3.5 Fitur Export
**Priority:** Must Have

| ID | Requirement | Deskripsi | Acceptance Criteria |
|----|--------------|-----------|---------------------|
| F17 | Export CSV | Export data ke file CSV | Format compatible Excel/Google Sheets |
| F18 | Backup Data | Simpan file di device | Bisa di-share via WhatsApp/Email |

---

## 4. Non-Functional Requirements

### 4.1 Performance
| Metric | Target |
|--------|--------|
| App launch time | < 2 detik di device 2GB RAM |
| Input transaksi | < 500ms save ke database |
| List rendering | Smooth 60fps untuk 1000+ items |
| APK size | < 10 MB (compressed) |

### 4.2 Offline Capability
- Semua fitur bekerja 100% offline
- Data disimpan lokal (SQLite)
- Tidak ada dependency pada network untuk operasi dasar

### 4.3 Device Compatibility
- Android 6.0 (API 23) minimum
- Support layar 4.5" hingga 6.5"
- Optimasi untuk RAM 2GB

### 4.4 Usability
- Form input: maksimal 4 field (tanpa catatan opsional)
- Default values untuk field yang sering diisi sama
- Numeric keyboard untuk input jumlah
- Bahasa Indonesia, istilah sederhana

### 4.5 Data Integrity
- Auto-save, tidak ada data loss saat app Force Close
- Validasi input: jumlah > 0, tanggal tidak boleh masa depan
- Soft delete untuk audit trail (opsional v2)

---

## 5. UI/UX Requirements

### 5.1 Navigation Structure
```
Bottom Navigation Bar (3 tabs):
[Pemasukan] [Pengeluaran] [Kasbon]

Floating Action Button (FAB):
[Tambah Baru] - memunculkan pilihan tipe
```

### 5.2 Screen Specifications

#### 5.2.1 Home / Dashboard
- Header: "Glow Suite" + Tanggal hari ini
- Summary Card: Pemasukan hari ini | Pengeluaran hari ini | Laba/Rugi
- Quick action: Lihat Kasbon Belum Lunas
- Recent transactions list (5 item terbaru)

#### 5.2.2 Pemasukan Screen
- List pemasukan dengan date separator
- FAB untuk tambah pemasukan
- Tap item: Show detail | Edit | Hapus
- Empty state: "Belum ada pemasukan hari ini"

#### 5.2.3 Pengeluaran Screen
- Sama layout dengan Pemasukan
- Category chip di setiap item
- FAB untuk tambah pengeluaran

#### 5.2.4 Kasbon Screen
- Filter tabs: Semua | Belum Lunas | Sudah Lunas
- Card layout: Nama | Jumlah | Tanggal | Status badge
- FAB untuk tambah kasbon
- Swipe action: Tandai Lunas

#### 5.2.5 Form Modal (Add/Edit)
```
[Pilih Tipe: Pemasukan/Pengeluaran/Kasbon]

[Tanggal] [Kategori Dropdown]
[Jumlah] [Nama (untuk kasbon)]
[Catatan - opsional]

[Simpan] [Batal]
```

### 5.3 Design System
- **Color Palette:**
  - Primary: #FF6B9D (pink soft - sesuai brand skincare)
  - Secondary: #4ECDC4 (teal soft)
  - Success: #2ECC71 (hijau untuk lunas)
  - Warning: #F39C12 (kuning untuk belum lunas)
  - Background: #F8F9FA (putih keabu-abuan)
  - Text: #2C3E50 (dark grey)

- **Typography:**
  - Headings: Roboto Bold 16-20sp
  - Body: Roboto Regular 14sp
  - Caption: Roboto Regular 12sp

- **Components:**
  - Card: border radius 8dp, elevation 2dp
  - Button: rounded 8dp, min height 48dp
  - Input: outlined style, label atas

---

## 6. Data Model

### 6.1 Transaction Model
```dart
class Transaction {
  int? id;
  String type; // 'pemasukan' | 'pengeluaran'
  String category;
  double amount;
  String? note;
  String date; // YYYY-MM-DD
  DateTime createdAt;
}
```

### 6.2 Kasbon Model
```dart
class Kasbon {
  int? id;
  String personName;
  double amount;
  String status; // 'belum_lunas' | 'lunas'
  String dateBorrowed; // YYYY-MM-DD
  String? datePaid; // YYYY-MM-DD
  String? note;
  DateTime createdAt;
}
```

### 6.3 Database Schema
```sql
-- Table: transactions
CREATE TABLE transactions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT NOT NULL CHECK(type IN ('pemasukan', 'pengeluaran')),
    category TEXT NOT NULL,
    amount REAL NOT NULL,
    note TEXT,
    date TEXT NOT NULL,
    created_at TEXT NOT NULL DEFAULT (datetime('now'))
);

-- Table: kasbon
CREATE TABLE kasbon (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    person_name TEXT NOT NULL,
    amount REAL NOT NULL,
    status TEXT NOT NULL DEFAULT 'belum_lunas' CHECK(status IN ('belum_lunas', 'lunas')),
    date_borrowed TEXT NOT NULL,
    date_paid TEXT,
    note TEXT,
    created_at TEXT NOT NULL DEFAULT (datetime('now'))
);

-- Index untuk performa query
CREATE INDEX idx_transactions_date ON transactions(date);
CREATE INDEX idx_kasbon_status ON kasbon(status);
```

---

## 7. Technical Architecture

### 7.1 Tech Stack
| Layer | Technology | Version | Purpose |
|-------|------------|---------|---------|
| Framework | Flutter | 3.16+ | Cross-platform UI |
| Language | Dart | 3.0+ | Programming language |
| Database | SQLite | via sqflite 2.3+ | Local storage |
| State Mgmt | Provider | 6.1+ | State management |
| Date Format | intl | 0.18+ | Currency & date formatting |
| Export | csv + file_picker | 5.1+ / 6.1+ | CSV generation & file save |
| Icons | cupertino_icons | bundled | UI icons |

### 7.2 Architecture Pattern
```
lib/
├── main.dart                    # App entry point
├── models/                      # Data models
│   ├── transaction.dart
│   └── kasbon.dart
├── screens/                     # UI screens
│   ├── home_screen.dart
│   ├── pemasukan_screen.dart
│   ├── pengeluaran_screen.dart
│   ├── kasbon_screen.dart
│   └── add_edit_screen.dart
├── widgets/                     # Reusable widgets
│   ├── summary_card.dart
│   ├── transaction_card.dart
│   ├── kasbon_card.dart
│   └── empty_state.dart
├── database/                    # Database layer
│   └── db_helper.dart
├── providers/                   # State management
│   ├── transaction_provider.dart
│   └── kasbon_provider.dart
└── utils/                       # Utilities
    ├── formatter.dart
    └── export_helper.dart
```

### 7.3 State Management Flow
```
User Input → Provider → Database → NotifyListeners → UI Update
```

---

## 8. User Flow & Interaction

### 8.1 Flow Tambah Pemasukan
```
1. User buka app → Home Screen
2. Tap FAB (+) → Muncul pilihan: Pemasukan | Pengeluaran | Kasbon
3. Tap "Pemasukan" → Form Modal muncul
4. Input:
   - Tanggal: default hari ini (bisa diubah)
   - Kategori: default "Penjualan" (dropdown)
   - Jumlah: numeric keyboard
   - Catatan: opsional
5. Tap "Simpan" → Validasi → Save ke DB → Close modal → Tampilkan success toast
6. Home Screen update: ringkasan updated, list baru muncul
```

### 8.2 Flow Tandai Kasbon Lunas
```
1. User buka tab Kasbon
2. Lihat list kasbon belum lunas (badge kuning)
3. Tap item kasbon → Detail view
4. Tap "Tandai Lunas" → Muncul dialog konfirmasi
5. Konfirmasi → Update status ke 'lunas', set tanggal bayar
6. List refresh, item pindah ke tab "Sudah Lunas"
```

### 8.3 Flow Export Data
```
1. User tap menu Export (di Home atau Settings)
2. Pilih rentang tanggal (opsional)
3. Pilih format: CSV
4. App generate file, tampilkan path
5. User bisa share via WhatsApp/Email/Simpan ke folder
```

---

## 9. Dependencies (pubspec.yaml)

```yaml
name: glow_suite
description: Aplikasi manajemen keuangan toko skincare

version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  sqflite: ^2.3.0
  path_provider: ^2.1.1
  intl: ^0.18.1
  file_picker: ^6.1.1
  csv: ^5.1.1
  provider: ^6.1.1
  flutter_native_splash: ^2.3.6
  cupertino_icons: ^1.0.6

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^3.0.0

flutter:
  uses-material-design: true
```

---

## 10. Implementation Plan (5 Sprints)

### Sprint 1: Foundation (Week 1)
- [x] Setup Flutter project dengan struktur folder
- [x] Konfigurasi dependencies
- [x] Buat database schema dan DB Helper
- [x] Buat model classes (Transaction, Kasbon)
- [x] Setup Provider untuk state management
- [x] Bottom navigation basic

**Deliverable:** App bisa buka, DB ready, model classes jalan

### Sprint 2: Transaksi (Week 2)
- [ ] Form input Pemasukan & Pengeluaran
- [ ] List transaksi dengan date separator
- [ ] CRUD operations
- [ ] Ringkasan harian (summary card)
- [ ] Validasi input

**Deliverable:** Fitur catat pemasukan & pengeluaran bekerja

### Sprint 3: Kasbon (Week 3)
- [ ] Form input Kasbon
- [ ] List Kasbon dengan status badge
- [ ] Filter tabs (Semua/Belum Lunas/Sudah Lunas)
- [ ] Fitur tandai lunas
- [ ] Total kasbon belum lunas di summary

**Deliverable:** Fitur kasbon lengkap

### Sprint 4: Export & Polish (Week 4)
- [ ] Export CSV untuk transaksi & kasbon
- [ ] Format currency Rupiah
- [ ] Date formatting Indonesia
- [ ] Empty states & error handling
- [ ] Testing di device low-end (2GB RAM)
- [ ] Performance optimization

**Deliverable:** App siap UAT

### Sprint 5: Testing & Launch (Week 5)
- [ ] UAT dengan 3-5 pemilik toko
- [ ] Bug fixing berdasarkan feedback
- [ ] Final testing di berbagai device
- [ ] Build APK release (split per ABI untuk size optimal)
- [ ] Dokumentasi penggunaan singkat

**Deliverable:** APK final siap distribute

---

## 11. Success Metrics

### 11.1 Quantitative
- Time-to-first-transaction: < 30 detik (dari buka app sampai simpan)
- App crash rate: < 0.1%
- APK size: < 10 MB
- Database query time: < 100ms untuk 10.000 records

### 11.2 Qualitative
- User bisa input transaksi tanpa bantuan
- User paham ringkasan yang ditampilkan
- User bisa export dan buka di Excel
- User feel "cukup" dengan fitur yang ada (tidak terlalu banyak)

---

## 12. Out of Scope (v2)

Fitur ini tidak termasuk di v1, bisa jadi roadmap v2:
- Multi-user / Multi-toko
- Cloud sync & backup otomatis
- Notifikasi pengingat kasbon
- Grafik laba/rugi bulanan
- Scan struk (OCR)
- Integrasi dengan marketplace (Shopee/Tokopedia)
- Manajemen stok produk

---

## 13. Risks & Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Device low-end tidak support | High | Test di device RAM 2GB dari day 1 |
| User salah input data | Medium | Konfirmasi dialog untuk hapus, default values |
| Database corruption | High | Implementasi backup export, Future: auto-backup |
| APK size exceeds 10MB | Medium | Proguard, App Bundle, remove unused resources |
| User confuse dengan istilah | Medium | Gunakan bahasa sehari-hari, tooltip singkat |

---

## 14. Appendices

### 14.1 Kategori Default
**Pemasukan:**
- Penjualan Produk
- Paket/Layanan
- Lainnya

**Pengeluaran:**
- Bahan Baku
- Sewa Tempat
- Gaji Karyawan
- Listrik/Water
- Packaging
- Lainnya

### 14.2 Mockup Text Description

**Home Screen:**
```
┌─────────────────────────────┐
│  Glow Suite         📅 10 Jun│
├─────────────────────────────┤
│  Ringkasan Hari Ini         │
│  ┌─────────┬─────────┐     │
│  │ Masuk   │ Keluar  │     │
│  │ Rp 500K │ Rp 200K │     │
│  └─────────┴─────────┘     │
│  Laba: Rp 300K              │
├─────────────────────────────┤
│  Transaksi Terbaru          │
│  ┌─────────────────────┐   │
│  │ 📦 Serum Vit C      │   │
│  │    10x @ Rp 50.000  │   │
│  │    Total: Rp 500.000│   │
│  └─────────────────────┘   │
│  ┌─────────────────────┐   │
│  │ 💸 Bahan Baku       │   │
│  │    Rp 150.000        │   │
│  └─────────────────────┘   │
├─────────────────────────────┤
│ 💰  Kasbon Belum Lunas: 3  │
│    Total: Rp 1.200.000     │
├─────────────────────────────┤
│   🏠   📊   👥   ⚙️        │
└─────────────────────────────┘
```

---

## 15. Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-06-10 | Kilo AI | Initial PRD creation |

---

**Document Status:** DRAFT  
**Next Review:** Setelah Sprint 1 (Setup & Database)
