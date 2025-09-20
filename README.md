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
