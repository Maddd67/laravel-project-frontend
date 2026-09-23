
## Struktur komponen

| Komponen | Tanggung jawab |
|---|---|
| App | Pemilik tickets, selectedStatus; menghitung filteredTickets; semua handler event |
| BasePanel | Bingkai judul + slot isi/footer, tidak tahu bentuk data |
| TicketFilter | Menerima modelValue, emit update:modelValue |
| TicketList | Iterasi tiket, meneruskan event advance dari Card ke App |
| TicketCard | Tampilan satu tiket, emit advance(id) |
| TicketStatus | Label status visual, tanpa hak ubah data |
| TicketForm | Draft lokal, emit submit(payload) saat valid |

## Tabel props/emit per komponen

| Komponen | Props diterima | Event di-emit |
|---|---|---|
| TicketFilter | modelValue | update:modelValue |
| TicketList | tickets, categories | advance |
| TicketCard | ticket, categoryName | advance |
| TicketStatus | status | - |
| TicketForm | categories | submit |
| BasePanel | title | - |

## Pemilik state (ownership)
- `tickets` (ref array) — sumber kebenaran, hanya App yang menulis
- `selectedStatus` (ref string) — filter aktif, dimiliki App
- `filteredTickets` (computed) — turunan, dihitung ulang otomatis
- Draft form — lokal di TicketForm, dikirim via emit saat submit

## Perilaku reset/filter/refresh
- Filter mengubah tampilan saja, tidak menghapus data sumber (tickets.length selalu 3 setelah refresh)
- Submit sah mereset draft form (lewat :key="formVersion" yang memaksa remount)
- Tutup/buka form menghapus draft yang belum disubmit
- Refresh browser mengembalikan data ke 3 tiket awal (data hanya di memori, belum terhubung ke backend/API)

## Hasil pengujian (kasus uji)
16 kasus uji (TC-01 s/d TC-16) telah dijalankan. Ringkasan: [isi jumlah lulus/gagal, contoh: "15 dari 16 lulus, TC-10 sebagian karena keterbatasan menguji melebihi maxlength lewat UI biasa"]. Detail lengkap dan bukti tersedia di [nama file laporan Anda].

## Bukti Devtools
Screenshot component tree, state sebelum/sesudah aksi, dan props tersedia di [nama file/folder bukti].

## Batasan
- Validasi hanya di sisi frontend (JavaScript), belum ada validasi backend
- Data tersimpan di memori (ref), hilang saat refresh — belum terhubung ke API/database sungguhan
- ID tiket dibuat lokal (increment), belum sinkron dengan server