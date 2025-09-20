# Gh0st-Coin

> **Meme coin dengan identitas unik** — bagian dari ekosistem **T4n OS** dan **pr0gshell**.

---

## 📖 Ringkasan

**Gh0st-Coin** adalah proyek *cryptocurrency* berbasis komunitas yang terinspirasi dari Dogecoin, namun dibangun dengan filosofi berbeda(Progress):  
menjadi **mata uang komunitas** yang **terintegrasi langsung dengan sistem operasi (T4n OS)** dan **custom shell (pr0gshell)**.

Dengan pendekatan ini, Gh0st-Coin bukan sekadar *meme coin* untuk spekulasi, melainkan sebuah **alat transaksi ringan, donasi, dan tipping** dalam ekosistem T4n.

---

## ✨ Fitur Utama

- 🎭 **Meme Coin + Identitas Unik** → karakter Gh0sT4n sebagai simbol kebebasan & komunitas.  
- 🖥️ **Integrasi OS (T4n OS-Progress)** → GUI wallet bawaan, donasi developer langsung dari desktop.  
- 💻 **Integrasi Shell (pr0gshell-Progress)** → wallet CLI sederhana dengan command bawaan.  
- 🔒 **Transparan** → kode open-source, audit-friendly, GPL-3.0.  
- 🌐 **Ekosistem Nyata** → terhubung dengan T4n-Manager & project lain di T4n.  

---

## 🔗 Hubungan dengan Ekosistem T4n(Progress)

### T4n OS  
- Wallet GUI tersedia default di desktop.  
- Bisa dipakai untuk:
  - Donasi developer lewat aplikasi bawaan.  
  - Transaksi ringan antar user T4n OS.  
  - Opsi pembayaran *package premium* di T4n-Manager.  

### pr0gshell  
- CLI wallet built-in:  
  ```sh
  # cek saldo
  pr0gshell gh0st balance

  # kirim koin
  pr0gshell gh0st send <alamat> <jumlah>

  # lihat alamat wallet
  pr0gshell gh0st address
  ```

- Memudahkan developer & power-user untuk integrasi scripting.  

---

## 📊 Tokenomics (Draft) (Plan)

- **Total Supply**: 1.000.000.000 Gh0st  
- **Distribusi**:  
  - 40% Komunitas & reward early adopter  
  - 30% Development & ekosistem  
  - 20% Likuiditas & listing exchange  
  - 10% Tim inti & maintenance  

> ⚠️ Catatan: Model distribusi masih bisa berubah lewat voting komunitas.

---

## 🛠 Roadmap (Tahap Pengembangan)

- [x] Ideasi & branding Gh0st-Coin  
- [ ] Wallet CLI di **pr0gshell**  
- [ ] Wallet GUI di **T4n OS**  
- [ ] Listing di DEX pertama  
- [ ] Program komunitas (airdrop, tipping, staking)  
- [ ] Integrasi penuh dengan **T4n-Manager**  
- [ ] Voting komunitas untuk tokenomics final  

---

## 💡 Contoh Kasus Penggunaan

- Donasi developer langsung dari T4n OS.  
- Pembayaran opsional di T4n-Manager.  
- Media tipping antar user komunitas.  
- Branding unik untuk komunitas T4n.  

---

## 🏗️ Teknologi

Gh0st-Coin ditulis dengan **Zig 0.15.0**, terinspirasi dari arsitektur **Rheia blockchain**, namun diimplementasikan ulang dari nol (*clean-room implementation*).  

Komponen inti:  
- **Concurrency**: thread-per-core, async I/O (`io_uring`).  
- **Consensus**: probabilistic finality dengan sampling.  
- **Database**: robin-hood hash table + sparse merkle trie.  
- **Mempool**: hash table untuk transaksi pending.  
- **Gossip Protocol**: push/pull efisien untuk distribusi transaksi.  

---

#Gh0st-Blockchain

Blockchain yang terinspirasi dari projek (rheia)[https://github.com/lithdew/rheia]
Gh0st-Coin adalah proyek cryptocurrency berbasis komunitas yang terinspirasi dari Dogecoin

## Design

### Concurrency
- arsitektur thread-per-core (kumpulan thread untuk pekerjaan yang terikat CPU)
- I/O disk dan jaringan asinkron menggunakan io_uring (loop peristiwa single-thread)
- Antrean bebas kunci (s/m)psc untuk komunikasi lintas-thread
- eventfd untuk notifikasi lintas-thread

### Consensus
- finalitas probabilistik blok dan transaksi
  - batching transaksi ke dalam blok meningkatkan throughput transaksi
- konsensus tanpa pemimpin berbasis sampel
  - memungkinkan konsensus berbasis voting, konsensus berbasis proof-of-work dengan menyediakan fungsi bobot sampel khusus

### Database
- sstables untuk format penyimpanan on-disk
- memtable untuk melacak status blockchain (tabel hash Robin Hood?)
- tabel hash Robin Hood untuk melacak transaksi yang tertunda
- status rantai direpresentasikan sebagai trie merkle sparse dari pasangan kunci-nilai (mirip dengan Ethereum)
  - apa cara paling efisien untuk mempertahankan trie merkle sparse atas daftar pasangan kunci-nilai yang diurutkan?
- setelah finalitas blok, hapus semua perubahan status dan transaksi yang telah difinalisasi ke sstable

- protokol push/pull (transaksi push out, transaksi pull in)
- mampu menyetel latensi vs. throughput per node dengan lebih baik
- transaksi pull lebih sering daripada push, karena push merupakan ancaman bagi serangan DOS

### Transaction Gossip
- protokol pengambilan sampel berbasis pull (menarik blok yang telah difinalisasi dan blok yang diusulkan).
  - Mampu menyetel latensi vs. throughput per node dengan lebih baik.
  - Tarik transaksi lebih sering daripada dorong, karena dorong merupakan masalah bagi serangan DOS

### kontrak pintar
- ebpf? webassembly?

## memulai
Jalankan node pada port 9000:

```
$ zig run main.zig --name rheia -lc -O ReleaseFast
```

Jalankan alat benchmark untuk membuat, menandatangani, dan mengirim transaksi ke 127.0.0.1:9000:

```
$ zig run bench.zig --name rheia_bench -lc -O ReleaseFast
```

## Research

### LRU Cache

Proyek Rheia banyak memanfaatkan cache LRU untuk melacak kumpulan data tak terbatas yang dapat dengan mudah diregenerasi kapan saja dengan cara yang lossy, misalnya kumpulan semua transaksi yang telah digosipkan ke alamat tujuan tertentu.

[Cache LRU Rheia](lru.zig) merupakan gabungan dari Tabel Hash Robin Hood dan Deque Tertaut Ganda. Ide untuk menggabungkan tabel hash dan deque tertaut ganda untuk membangun cache LRU terinspirasi oleh [postingan blog ini](https://medium.com/@udaysagar.2177/fastest-lru-cache-in-java-c22262de42ad).

Implementasi cache LRU alternatif juga diujicobakan, di mana entri deque dan entri tabel hash dialokasikan secara terpisah. Implementasi semacam itu hanya menghasilkan throughput keseluruhan yang lebih baik dibandingkan dengan implementasi cache LRU Rheia yang sudah ada, namun ketika kapasitas cache kecil dan faktor beban maksimum 50%.

Di laptop saya, menggunakan cache LRU Rheia dengan faktor beban maksimum 50%, hasilnya kurang lebih:

1. 19,81 juta entri dapat di-upsi per detik.
2. 20,19 juta entri dapat di-query per detik.
3. 9,97 juta entri dapat di-query dan dihapus per detik.

Kode benchmark tersedia [di sini](benchmarks/lru/main.zig). Contoh pengujiannya tersedia di bawah ini.
```
$ zig run benchmarks/lru/main.zig -lc -O ReleaseFast --main-pkg-path .
info(linked-list lru w/ load factor 50% (4096 elements)): insert: 61.92us
info(linked-list lru w/ load factor 50% (4096 elements)): search: 64.429us
info(linked-list lru w/ load factor 50% (4096 elements)): delete: 100.595us
info(linked-list lru w/ load factor 50% (4096 elements)): put: 2010, get: 2010, del: 2010
info(intrusive lru w/ load factor 50% (4096 elements)): insert: 129.446us
info(intrusive lru w/ load factor 50% (4096 elements)): search: 79.754us
info(intrusive lru w/ load factor 50% (4096 elements)): delete: 169.099us
info(intrusive lru w/ load factor 50% (4096 elements)): put: 2010, get: 2010, del: 2010
info(linked-list lru w/ load factor 100% (4096 elements)): insert: 178.883us
info(linked-list lru w/ load factor 100% (4096 elements)): search: 37.786us
info(linked-list lru w/ load factor 100% (4096 elements)): delete: 37.522us
info(linked-list lru w/ load factor 100% (4096 elements)): put: 3798, get: 1905, del: 2827
info(intrusive lru w/ load factor 100% (4096 elements)): insert: 154.161us
info(intrusive lru w/ load factor 100% (4096 elements)): search: 21.533us
info(intrusive lru w/ load factor 100% (4096 elements)): delete: 61.936us
info(intrusive lru w/ load factor 100% (4096 elements)): put: 3798, get: 934, del: 2827
info(linked-list lru w/ load factor 50% (1 million elements)): insert: 79.469ms
info(linked-list lru w/ load factor 50% (1 million elements)): search: 48.164ms
info(linked-list lru w/ load factor 50% (1 million elements)): delete: 101.94ms
info(linked-list lru w/ load factor 50% (1 million elements)): put: 453964, get: 453964, del: 453964
info(intrusive lru w/ load factor 50% (1 million elements)): insert: 65.143ms
info(intrusive lru w/ load factor 50% (1 million elements)): search: 38.909ms
info(intrusive lru w/ load factor 50% (1 million elements)): delete: 95.001ms
info(intrusive lru w/ load factor 50% (1 million elements)): put: 453964, get: 453964, del: 453964
info(linked-list lru w/ load factor 100% (1 million elements)): insert: 123.995ms
info(linked-list lru w/ load factor 100% (1 million elements)): search: 29.77ms
info(linked-list lru w/ load factor 100% (1 million elements)): delete: 48.993ms
info(linked-list lru w/ load factor 100% (1 million elements)): put: 974504, get: 487369, del: 736132
info(intrusive lru w/ load factor 100% (1 million elements)): insert: 104.109ms
info(intrusive lru w/ load factor 100% (1 million elements)): search: 19.557ms
info(intrusive lru w/ load factor 100% (1 million elements)): delete: 47.728ms
info(intrusive lru w/ load factor 100% (1 million elements)): put: 974504, get: 249260, del: 736132
```

### Mempool

Salah satu struktur data terpenting yang dibutuhkan oleh Projek Rheia adalah indeks memori utama yang dirancang untuk melacak semua transaksi yang belum diselesaikan berdasarkan protokol konsensus Rheia (atau dengan kata lain, mempool).

Mempool secara umum memetakan transaksi berdasarkan ID-nya ke isinya. ID transaksi adalah checksum dari isinya. Dalam kasus Rheia, checksum atau ID transaksi dihitung menggunakan fungsi hash BLAKE3 dengan ukuran keluaran 256 bit.

Ada dua hal penting yang perlu diperhatikan dalam menentukan struktur data yang tepat untuk mempool Rheia mengingat pilihan protokol konsensus Rheia.

1. Iterasi semua transaksi berdasarkan ID-nya secara leksikografis seharusnya murah.
2. Penyisipan/penghapusan harus cepat dengan asumsi bahwa mungkin ada sekitar 300 ribu hingga 1 juta transaksi yang diindeks setiap saat.

Banyak struktur data berbeda yang diuji, dan struktur data akhir yang saya putuskan untuk digunakan sebagai mempool Rheia adalah tabel hash Robin Hood.

Untuk membuat keputusan, struktur data berikut diuji:

1. Robin Hood Hash Table ([lithdew/rheia](benchmarks/mempool/hash_map.zig))
1. B-Tree ([tidwall/btree.c](https://github.com/tidwall/btree.c))
2. Adaptive Radix Tree ([armon/libart](https://github.com/armon/libart))
3. Skiplist ([MauriceGit/skiplist](https://github.com/MauriceGit/skiplist) - ported to [zig](benchmarks/mempool/skiplist.zig))
4. Red-black Tree ([ziglang/std-lib-orphanage](https://github.com/ziglang/std-lib-orphanage/blob/master/std/rb.zig))
5. Radix Tree ([antirez/rax](https://github.com/antirez/rax) - ported to [zig](benchmarks/mempool/rax.zig))
6. Binary Heap ([ziglang/zig](https://github.com/ziglang/zig/blob/master/lib/std/priority_queue.zig))
7. Adaptive Radix Tree ([armon/libart](https://github.com/armon/libart) - ported to [zig](benchmarks/mempool/art.zig))
8. Adaptive Radix Tree ([travisstaloch/art.zig](https://github.com/travisstaloch/art.zig))

Tabel hash Robin Hood menunjukkan throughput keseluruhan rata-rata tertinggi pada pengujian berikut:

1. Masukkan 1 juta hash berbeda ke dalam struktur data.
2. Periksa apakah terdapat 1 juta hash berbeda dalam struktur data.
3. Hapus 1 juta hash berbeda dari struktur data.

Menggunakan tabel hash Robin Hood dengan faktor beban maksimum 50%, kira-kira:

1. 19,58 juta transaksi dapat diindeks per detik.
2. 25,07 juta transaksi dapat dicari berdasarkan ID-nya per detik.
3. 20,91 juta transaksi dapat dicari dan dihapus indeksnya berdasarkan ID-nya per detik.

Kode benchmark tersedia [di sini](benchmarks/mempool/main.zig). Contoh pengujiannya disediakan di bawah ini.

```
$ zig run benchmarks/mempool/main.zig benchmarks/mempool/*.c -I benchmarks/mempool -lc -fno-sanitize-c --name mempool --main-pkg-path . -O ReleaseFast

info(hash_map): insert: 45.657ms
info(hash_map): search: 33.438ms
info(hash_map): delete: 42.524ms
info(hash_map): put: 456520, get: 456520, del: 456520
info(btree): insert: 434.437ms
info(btree): search: 399.978ms
info(btree): delete: 423.974ms
info(red_black_tree): insert: 617.099ms
info(red_black_tree): search: 582.107ms
info(red_black_tree): skipping delete...
info(binary_heap): insert: 42.173ms
info(binary_heap): skipping search/delete...
info(skiplist): insert: 1.325s
info(skiplist): skipping search/delete...
info(libart): insert: 162.963ms
info(libart): search: 93.652ms
info(libart): delete: 253.205ms
info(art_travis): insert: 209.661ms
info(art_travis): search: 90.561ms
info(art_travis): delete: 217.101ms
info(art): insert: 165.919ms
info(art): search: 205.135ms
info(art): delete: 235.739ms
info(rax): insert: 566.947ms
info(rax): search: 380.085ms
info(rax): delete: 597.44ms
```

---

## 🤝 Kontribusi

Siapapun bisa ikut membangun Gh0st-Coin!  

1. Fork repo  
2. Buat branch: `git checkout -b feat/my-feature`  
3. Commit perubahan dengan jelas  
4. Pull Request ke repo utama  

---

## 📜 Lisensi

Gh0st-Coin dilisensikan di bawah **GPL-3.0**.  

```
This project is licensed under the GNU General Public License v3.0 (GPL-3.0).
You are free to use, modify, and distribute under the same license terms.
```

---

✨ **Gh0st-Coin = Meme Coin yang Fun + Berguna di Ekosistem T4n**  

