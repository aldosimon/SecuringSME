# Demo

Kumpulan langkah-langkah penerapan kontrol

## Langkah-langkah Mencegah Pengguna Menginstal Ekstensi di Google Chrome:

### Unduh Template Kebijakan Chrome:

Unduh Google Chrome Bundle atau unduh template kebijakan dari tautan [berikut](https://dl.google.com/dl/edgedl/chrome/policy/policy_templates.zip).

### Ekstrak isi bundle atau file zip yang diunduh.
Cari file template kebijakan, bernama chrome.adm. Untuk format ADMX, Anda akan menemukan file chrome.admx dan folder en-US (atau bahasa lain) yang berisi file chrome.adml.

### Buka Group Policy Editor:

Tekan tombol Windows + R untuk membuka kotak dialog Run.
Ketik gpedit.msc dan tekan Enter.

#### Tambahkan Template Kebijakan Chrome:

Di Group Policy Editor, navigasikan ke Local Computer Policy > Computer Configuration > Administrative Templates.

Klik kanan pada Administrative Templates dan pilih Add/Remove Templates....
Jika Anda menggunakan template ADM (chrome.adm):
Klik tombol Add....
Telusuri ke lokasi file chrome.adm yang Anda unduh dan pilih.
Klik Open dan kemudian Close.

### Konfigurasi Policy

Navigasi ke 
```
Classic Administrative Templates / Google / Google Chrome/ Extension/ Configure Extension Installation 
Blocklist 
```

Set ke Enabled

Pada Option tambahkan value ``` * ```

Anda juga dapat melakukan blocking tertentu saja dengan menambahkan extension ID yang diperbolehkan pada
 
```
Classic Administrative Templates / Google / Google Chrome/ Extension/ Configure Extension Installation 
Allowlist 
```




## Langkah-langkah Menggunakan AppLocker dengan Menambahkan Aturan Default (Audit Only)

### Buka Local Security Policy

Tekan tombol `Windows` + `R` untuk membuka kotak dialog Run.

Ketik `secpol.msc` dan tekan `Enter`.

### Navigasi ke AppLocker

Di jendela Local Security Policy, perluas `Security Settings`.

Klik pada `Application Control Policies`.

Klik pada `AppLocker`.

### Pastikan Mode Enforcement adalah "Audit Only"

Klik kanan pada `AppLocker` di panel kiri.

Pilih `Properties`.

Pada tab `Enforcement`, pastikan semua opsi (Executable rules, Windows Installer rules, Script rules) diatur ke **"Audit only"**.

Klik `Apply` dan kemudian `OK`. **Langkah ini krusial untuk menghindari penguncian diri.**

### Tambahkan Aturan Default

Klik pada **`Executable Rules`** di panel kiri.

Klik kanan di area kosong pada panel kanan dan pilih **`Create Default Rules`**. Ini akan menambahkan 

tiga aturan default untuk file executable:
- Mengizinkan semua file di folder `Program Files`.
- Mengizinkan semua file di folder `Windows`.
- Mengizinkan aplikasi yang dijalankan administrator -> bila anda selalu jalan sebagai administrator hapus ini.

Sesuai kebutuhan: Ulangi langkah yang sama untuk **`Windows Installer Rules`** dan **`Script Rules`**. Klik kanan pada setiap kategori dan pilih **`Create Default Rules`**. Ini akan menambahkan aturan default yang sesuai untuk jenis file tersebut.

### Tinjau Aturan Default yang Ditambahkan:

Klik pada setiap kategori aturan (`Executable Rules`, `Windows Installer Rules`, `Script Rules`) di panel kiri.

Periksa aturan default yang baru saja Anda tambahkan di panel kanan. Pastikan Anda memahami izin yang diberikan oleh setiap aturan.

**Catatan:** Aturan default ini bertujuan untuk memberikan izin kepada lokasi dan penerbit tepercaya. Mereka **tidak secara eksplisit memblokir** aplikasi apa pun.

### Verifikasi Layanan Application Identity:

AppLocker bergantung pada layanan **Application Identity** agar dapat berfungsi. Pastikan layanan ini berjalan.

Tekan tombol `Windows` + `R` untuk membuka kotak dialog Run.

Ketik `services.msc` dan tekan `Enter`.

Cari layanan bernama **"Application Identity"**.

Pastikan statusnya adalah **"Running"** 

Jika tidak berjalan, klik kanan padanya dan pilih `Start`. 

Bila anda yakin rule anda sudah tepat, maka  `Startup Type` bisa di-set `Automatic`, klik kanan, pilih `Properties`, ubah `Startup Type`, klik `Apply`, dan kemudian `Start`.

### Periksa Log Peristiwa AppLocker (Setelah Pengujian):

Karena kita masih dalam mode "Audit Only", AppLocker tidak akan memblokir aplikasi apa pun. Ia akan mencatat setiap kali sebuah aplikasi *seharusnya* diblokir jika aturan pemblokiran eksplisit ada dan mode "Enforce rules" diaktifkan. Aturan default yang baru ditambahkan seharusnya *tidak* menghasilkan peristiwa pemblokiran karena sifatnya yang memberikan izin.

Untuk melihat log peristiwa AppLocker:
- Buka Event Viewer (cari "Event Viewer" di Start Menu).
- Navigasi ke `Applications and Services Logs` > `Microsoft` > `Windows` > `AppLocker`.
- Periksa log di bawah `EXE and DLL`, `MSI and Script`.
- Anda mungkin melihat peristiwa informatif (ID 8000) yang mencatat evaluasi aturan. Jika Anda memiliki aturan pemblokiran eksplisit (yang tidak kita buat di sini), Anda akan melihat peristiwa ID 8003.

### Catatan Penting

* **Aturan Default Tidak Memblokir:** Ingatlah bahwa aturan default AppLocker dirancang untuk memberikan izin kepada lokasi dan penerbit yang umumnya dianggap aman. Mereka **tidak akan memblokir aplikasi apa pun secara inheren**.

* **Mode Audit Only Sangat Penting:** Tetap dalam mode "Audit only" memungkinkan Anda untuk melihat bagaimana aturan AppLocker akan mempengaruhi sistem Anda tanpa risiko mengunci diri. Pantau log peristiwa untuk memahami aplikasi mana yang akan diblokir jika Anda membuat aturan pemblokiran di masa mendatang.

* **Untuk Memblokir Aplikasi:** Untuk benar-benar memblokir aplikasi, Anda perlu membuat aturan 

* **"Deny"** secara manual, menargetkan aplikasi spesifik berdasarkan publisher, path, atau hash file.

* **Verifikasi Layanan:** Layanan Application Identity harus berjalan agar AppLocker dapat berfungsi dengan benar.
