# Benchmarks

Kumpulan sumber daya untuk melakukan hardening pada OS dan aplikasi melalui baseline/ benchmark dari beberapa organisasi internasional.

## Perhatian

Pastikan penerapan kontrol **sesuai** dengan kebutuhan bisnis!

Terdapat beberapa level penerapan pada benchmark tersebut, mulai dari yang paling rendah (contoh IG1 pada CIS), gunakan sesuai kebutuhan - tidak perlu berfokus untuk mencapai level penerapan tertinggi, lakukan testing terlebih dahulu sebelum ke lingkungan produksi.


## Microsoft

Microsoft menyediakan baseline pada laman [baseline](https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/windows-security-configuration-framework/windows-security-baselines), dan gunakan [SCT](https://learn.microsoft.com/en-us/windows/security/operating-system-security/device-management/windows-security-configuration-framework/security-compliance-toolkit-10) untuk mengecek implementasi

## CIS

CIS menyediakan benchmark pada laman [benchmark](https://www.cisecurity.org/cis-benchmarks)

Dapat juga menggunakan [CIS CAT lite](https://learn.cisecurity.org/cis-cat-lite), untuk mengecek implementasi.


sebagai contoh kita akan menggunakan [CIS Windows 10](https://github.com/aldosimon/SecuringSME/blob/main/benchmarks/CIS_Microsoft_Windows_10_Stand-alone_Benchmark_v3.0.0.pdf) untuk mengecek implementasi.

## dev-sec.io

dev-sec.io menyediakan baseline pada laman [baseline](https://dev-sec.io/baselines/) mereka. Tersedia pula playbook ansible, chef, puppet terkait.

## STIG

Benchmark tersedia pada [STIG](https://public.cyber.mil/stigs/), atau [GPO terkait](https://public.cyber.mil/stigs/gpo/)


## HardeningKitty

Untuk mempermudah, [HardeningKitty](https://github.com/scipag/HardeningKitty) merupakan sebuah PowerShell module yang bisa mengaudit CIS benchmark dan Microsoft Security Baseline.