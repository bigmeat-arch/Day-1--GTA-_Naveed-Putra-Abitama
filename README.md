# Day-1--GTA-_Naveed-Putra-Abitama
Repo ini Untuk menjelaskan Materi Workshop pada Hari Sabtu 7/11/2026 oleh Naveed Putra Abitama
# Building with AI, Managing Change with GitHub

Catatan dan materi workshop **"From AI to GitHub"** oleh **Nugi Farhan** (Senior Product Designer). Repository ini merangkum cara menulis prompt AI yang baik dan cara mengelola perubahan kode dengan Git dan GitHub, ditutup dengan praktik membuat aplikasi sederhana: **Student Task Planner**.

## Daftar Isi

- [Tujuan Pembelajaran](#tujuan-pembelajaran)
- [1. Prompt Engineering](#1-prompt-engineering)
- [2. Git dan GitHub](#2-git-dan-github)
- [3. Setup Environment](#3-setup-environment)
- [4. Perintah Git Penting](#4-perintah-git-penting)
- [5. Praktik: Student Task Planner](#5-praktik-student-task-planner)
- [Poin Penting](#poin-penting)
- [Referensi](#referensi)

## Tujuan Pembelajaran

Setelah workshop ini, kamu dapat:

- Menjelaskan bagaimana prompt AI yang lebih spesifik menghasilkan hasil yang lebih baik.
- Membedakan Git dan GitHub.
- Mengidentifikasi repository, commit, branch, dan pull request.
- Melakukan version control.

---

## 1. Prompt Engineering

**Prompt** adalah instruksi atau masukan yang diberikan ke model AI untuk mendapatkan hasil tertentu. Bentuknya bisa berupa pertanyaan sederhana, kata kunci, instruksi rumit, atau potongan kode.

**Prompt engineering** adalah cara merancang instruksi agar AI memberikan jawaban yang tepat, relevan, dan layak digunakan.

### Context is key

| Tanpa konteks | Dengan konteks |
|---|---|
| `Create student task planner` | Prompt yang menjelaskan peran, tujuan, kebutuhan, batasan, dan format output |

### Struktur prompt yang baik

| Elemen | Fungsi |
|---|---|
| **Role** | Peran yang diberikan kepada AI |
| **Task** | Tujuan atau tugas utama |
| **Context** | Kebutuhan (requirements) dan batasan (constraints) |
| **Format** | Bentuk output yang diinginkan |

### Contoh prompt

```markdown
You are helping me build a beginner-friendly student task planner as a simple
web application.

## Goal
Create a task planner that helps first-year university students keep track of
assignments and deadlines.

## Requirements
- Display a list of sample assignments.
- Each assignment must show its title, due date, and completion status.
- Allow users to mark assignments as completed.
- Include a filter for all, pending, and completed assignments.
- Make the interface clean, readable, and responsive.

## Constraints
- Use HTML, CSS, and vanilla JavaScript.
- Keep the implementation simple enough for a beginner to understand.
- Avoid unnecessary libraries and external dependencies.
- Keep the code organized and explain the purpose of each file.

## Output
Provide the complete code for each file, explain how to run the application, and
describe how to test the main interactions.

Do not claim that the application works until you have verified its behavior.
```

### Taktik prompt tambahan

| Taktik | Contoh prompt |
|---|---|
| Gunakan kata kerja aksi | "Write a bulleted list that summarizes the key findings of the attached research paper" |
| Tentukan panjang dan format output | "Compose a 500-word essay discussing the impact of climate change on coastal communities." |
| Tentukan target audiens | "Write a product description for a new line of organic skincare products, targeting young adults concerned with sustainability." |
| Sertakan fakta dan data relevan | "Given that global temperatures have risen by 1 degree Celsius since the pre-industrial era, discuss the potential consequences for sea level rise." |
| Rujuk sumber atau dokumen spesifik | "Based on the attached financial report, analyze the company's profitability over the past five years." |
| Definisikan istilah dan konsep kunci | "Explain the concept of quantum computing in simple terms, suitable for a non-technical audience." |

Sumber: <https://cloud.google.com/discover/what-is-prompt-engineering>

### Keuntungan prompt engineering

- Meningkatkan performa model AI
- Mengurangi bias dan respons yang ofensif
- Meningkatkan kontrol dan prediktabilitas
- Memberikan pengalaman yang lebih baik

---

## 2. Git dan GitHub

### Apa itu Git?

Git adalah *version control system* yang melacak siapa, kapan, dan apa saja yang diubah dalam suatu file. Git diciptakan oleh Linus Torvalds pada 2005 untuk mencatat setiap perubahan pada file.

### Git vs GitHub

| | Git | GitHub |
|---|---|---|
| **Jenis** | Sistem version control terdistribusi (DVCS) | Platform berbasis web |
| **Fungsi** | Melacak dan mengelola perubahan kode secara lokal | Menyimpan repository Git secara online dan mendukung kolaborasi |

### Git workflow

```
Working Directory  ──Stage Fixes──▶  Staging Area  ──Commit──▶  .git directory (Repository)
        ◀─────────────────── Checkout the project ───────────────────
```

- **Working directory / tree**: salinan satu versi proyek yang sedang kamu kerjakan. File di dalamnya diambil dari database di direktori Git lalu ditempatkan di komputer agar bisa digunakan atau dimodifikasi.
- **Staging area (index)**: menyimpan informasi tentang perubahan apa saja yang akan masuk ke commit berikutnya.
- **Git directory (repository)**: tempat Git menyimpan metadata dan object database proyek. Ini komponen terpenting Git, dan bagian inilah yang disalin saat `clone`.

---

## 3. Setup Environment

### Git

1. Install Git (cari "install git" di Google) dan pilih versi sesuai perangkatmu.
2. Buka terminal/cmd, lalu cek instalasi:
   ```bash
   git --version
   # git version 2.xx.xx
   ```
3. Atur identitas Git:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your-email@example.com"
   ```
4. Cek konfigurasi:
   ```bash
   git config --global --list
   ```

### VS Code

1. Install VS Code lalu buka aplikasinya.
2. Tekan `Cmd+Shift+P` / `Ctrl+Shift+P`.
3. Cari **Shell Command: Install 'code' command in PATH**.
4. Di terminal, cek dengan:
   ```bash
   code --version
   ```

### SSH Key untuk GitHub

**1. Buat SSH key baru**

```bash
ssh-keygen -t ed25519 -C "email@domain.com"
```

- Saat diminta lokasi file, tekan **Enter** untuk menerima default.
- Saat diminta passphrase, isi passphrase yang aman (atau tekan Enter dua kali untuk tanpa passphrase).

> **Ed25519** adalah skema tanda tangan kurva eliptik (EdDSA di atas kurva Curve25519, dibuat Daniel J. Bernstein pada 2011) yang dirancang untuk keamanan dan performa tinggi, serta tahan terhadap serangan implementasi kriptografi yang umum.

**2. Tambahkan key ke SSH agent dan macOS Keychain**

```bash
# Jalankan SSH agent
eval "$(ssh-agent -s)"

# Buat atau buka ~/.ssh/config
touch ~/.ssh/config
open -e ~/.ssh/config   # atau: nano ~/.ssh/config
```

Isi file config dengan:

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Lalu tambahkan key ke agent:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**3. Tambahkan public key ke GitHub**

```bash
# Salin public key
pbcopy < ~/.ssh/id_ed25519.pub
```

Buka **GitHub → Settings → SSH and GPG keys**, lalu:

1. Klik **New SSH key**.
2. Isi **Title** dengan label deskriptif (misalnya `MacBook`).
3. Biarkan **Key type** sebagai *Authentication Key*.
4. Tempel key ke kolom **Key**.
5. Klik **Add SSH key**.

**4. Verifikasi koneksi**

```bash
ssh -T git@github.com
# Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Checklist setup

```bash
git --version                  # 1. Cek Git
git config --global user.name  # 2. Cek identitas Git
git config --global user.email
code --version                 # 3. Cek VS Code
ssh -T git@github.com          # 4. Cek koneksi GitHub
```

---

## 4. Perintah Git Penting

### Perintah dasar

```bash
git init                       # Memulai repo baru
git add .                      # Memindahkan file ke staging area
git commit -m 'message'        # Menyimpan perubahan ke repository
```

### Menghubungkan ke GitHub

```bash
git remote add <name> <url>    # Menghubungkan repo lokal ke remote
git push origin main           # Mengirim branch saat ini ke remote tracking branch
git pull origin main           # Mengambil perubahan lalu menggabungkannya ke branch saat ini
```

### Menambahkan repository ke Git

Ada dua cara:

**a. Mulai dari folder lokal**

```bash
git init
```

**b. Clone dari GitHub**

1. Buka GitHub dan buat repository baru.
2. Klik tombol dropdown **Code**.
3. Pada tab **Local**, salin link repository.
4. Jalankan di terminal:
   ```bash
   git clone "url"
   ```

### Working area → staging

| Perintah | Keterangan |
|---|---|
| `git add --all` / `git add -A` | Berperilaku sama, menambahkan semua isi repository ke staging area |
| `git add .` | Menambahkan semua file di repository beserta isinya ke staging area |
| `git add *` | Menambahkan semua file yang dimodifikasi atau baru, tetapi tidak file yang dihapus |

---

## 5. Praktik: Student Task Planner

Proyek sederhana yang dibuat selama workshop.

**Alur kerja:**

1. Install platform berbantuan AI (Claude, Antigravity, ChatGPT).
2. Tulis prompt untuk membuat web/aplikasimu.
3. Tambahkan ke Git dengan `git init`.
4. Commit dan push ke GitHub.
5. Lakukan perubahan.
6. Commit dan push lagi ke GitHub.

```bash
git init
git add .
git commit -m "Initial commit: student task planner"
git remote add origin git@github.com:<username>/<repo>.git
git push origin main
```

---

## Poin Penting

1. **Kualitas output AI bergantung pada kualitas prompt.** Gunakan struktur Role, Task, Context, dan Format.
2. **Minta AI memverifikasi hasilnya** agar tidak mengklaim sesuatu berfungsi sebelum benar-benar diuji.
3. **Git dan GitHub berbeda.** Git melacak perubahan secara lokal, GitHub menyimpan repository secara online untuk kolaborasi.
4. **Perubahan melewati tiga area:** working directory, staging area, lalu repository (`add`, kemudian `commit`).
5. **SSH key (Ed25519)** memberi autentikasi aman ke GitHub tanpa mengetik password berulang kali.
6. **Commit dan push secara rutin** sebagai jaring pengaman.

> *In case of fire: 1. `git commit` 2. `git push` 3. leave building.*

## Referensi

- [What is prompt engineering? (Google Cloud)](https://cloud.google.com/discover/what-is-prompt-engineering)
- Materi slide "From AI to GitHub" oleh Nugi Farhan

## Pemateri

**Nugi Farhan**, Senior Product Designer
- Portfolio: <https://nearfrhn.webflow.io>
- LinkedIn: [in/nearfrhn](https://www.linkedin.com/in/nearfrhn)
- Link Tree: <https://fugi.onepage.me>
