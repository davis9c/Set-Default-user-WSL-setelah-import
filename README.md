# 📘 Panduan Lengkap Konfigurasi WSL (Windows Subsystem for Linux)

Dokumen ini merupakan ringkasan dari percakapan dan pengalaman konfigurasi WSL, termasuk pengaturan user default, swap, dan konfigurasi sistem lainnya.

---

## 🔧 1. Menentukan User Default WSL

### 📍 Metode 1: Lewat Registry Editor

1. Buka Registry Editor:
   ```
   HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\{GUID_Distro}
   ```

2. Temukan key bernama `DefaultUid`, klik kanan > **Modify**.

3. Ubah:
   - **Base** ke `Decimal`
   - **Value Data** sesuai UID user (contoh: `1000`)

   Cek UID di WSL dengan:
   ```bash
   id -u <nama_user>
   ```

4. Jalankan:
   ```bash
   wsl --shutdown
   ```

5. Buka kembali WSL. Sekarang default user sudah berubah.

### 📍 Metode 2: Lewat wsl.conf

Di dalam distro WSL (akses root):

```bash
sudo nano /etc/wsl.conf
```

Isi dengan:

```ini
[user]
default=cxzer0
```

Kemudian jalankan:

```bash
wsl --shutdown
```

---

## 💾 2. Lokasi dan Pemindahan Swap File

WSL menyimpan swap secara default di:

```
C:\Users\<user>\AppData\Local\Packages\Canonical...\LocalState\ext4.vhdx
```

Untuk memindahkan swap atau mengatur ulang WSL memory limit, gunakan file konfigurasi:

```ini
# %UserProfile%\.wslconfig
[wsl2]
memory=6GB
processors=4
swap=2GB
swapFile=D:\WSL\swap.vhdx
```
> Lakukan `wsl --shutdown` setelah menyimpan file `.wslconfig`.

---

## 📦 3. Instalasi Paket Wajib

Jalankan setelah setup user:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential git curl wget unzip neofetch htop
```

---

## 🛠️ 4. Tips Tambahan

- Gunakan `lxrunoffline` atau `wsl --export/import` untuk memindahkan distro ke drive lain.
- Hindari penggunaan Registry jika bisa digantikan oleh `wsl.conf`.
- Gunakan PowerShell atau CMD dengan hak administrator saat setting `.wslconfig`.

---

## 📁 Struktur File Penting

| Path | Deskripsi |
|------|-----------|
| `%UserProfile%\.wslconfig` | Konfigurasi global WSL |
| `/etc/wsl.conf` | Konfigurasi per distro |
| `ext4.vhdx` | File image utama distro |
| `swap.vhdx` | File swap jika diset manual |

---

## 🙋‍♂️ Bantuan

Jika WSL gagal startup karena UID tidak valid, akan muncul error seperti:
```
getpwuid(4096) failed
```

Solusinya: pastikan UID valid atau kembali ke root lalu ubah pengaturan ulang.

---

**📝 Disusun dari pengalaman pengguna dengan sistem WSL multi-akun dan konfigurasi swap custom.**
