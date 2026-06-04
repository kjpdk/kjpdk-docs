# ⚙️ KJP-Lab Systemmiljø

Dette er den officielle dokumentation over mit personlige systemmiljø og hardware i **KJP-Lab**. Det kører som et virtuelt Linux-laboratorium direkte på min Windows-maskine via WSL2.

---

## 🐧 Operativsystem & Miljø

- **OS:** Debian GNU/Linux 13 (Trixie) x86_64
- **Host:** Windows Subsystem for Linux - kjpdk-lab (2.7.7.0)
- **Kerne (Kernel):** Linux 6.18.26.1-microsoft-standard-WSL2
- **Shell:** zsh 5.9 (Med Starship-prompt)
- **Vindueshåndtering:** WSLg 1.0.73.2 (Wayland)
- **Terminal:** WezTerm 20240203-110809-5046fc22

---

## 💻 Hardware Specifikationer

- **CPU:** Intel(R) Core(TM) Ultra 7 155H med 22 logiske kerner @ 3.00 GHz.
- **GPU:** Intel(R) Arc(TM) Graphics (128.00 MiB) [Integrated].
- **Hukommelse (RAM):** 15.20 GiB samlet (Hvoraf ca. 8% udnyttes under normal drift).
- **Diskplads (Linux-rod):** 1006.85 GiB på ext4-filsystemet.

---

## 🛠️ Primære Værktøjer

1. **Editor:** Neovim opsat med det fuldvoksne **LazyVim** økosystem.
2. **Pakkehåndtering:** **`uv`** (fra Astral) til lynhurtig styring af Python 3.13-miljøer.
3. **AI-Agent:** **Antigravity CLI** (`agy`) integreret direkte i terminalen til live kodeanalyse.

---

## 🔗 Relaterede Links

- Se dokumentationen og kildekoden for min [Badminton Manager](../projekter/badminton.md).
- Gå tilbage til [Forsiden](../index.md).
