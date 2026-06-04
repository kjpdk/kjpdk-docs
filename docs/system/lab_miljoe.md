# ⚙️ KJP-Lab Systemmiljø

Dette er den officielle dokumentation over mit personlige systemmiljø og hardware i **KJP-Lab**. Det kører som et virtuelt Linux-laboratorium direkte på min Windows-maskine.

---

## 🐧 Operativsystem & Kerne

- **OS:** Debian GNU/Linux 13 (Trixie) x86_64
- **Arkitektur:** Windows Subsystem for Linux (WSL2)
- **Kerne (Kernel):** Linux 6.6.87.2-microsoft-standard-WSL2
- **Init System:** systemd (aktiveret til håndtering af baggrundstjenester)

---

## 💻 Hardware Specifikationer

Mit laboratorium drives af en kraftfuld processor, som giver masser af overskud til databehandling og AI-analyser:

- **CPU:** Intel(R) Core(TM) Ultra 7 155H med 22 logiske kerner.
- **GPU:** Intel(R) Arc(TM) Graphics (Integreret).
- **Hukommelse (RAM):** Tildelt op til 15.20 GiB fra værtsmaskinen for maksimal ydeevne under kodning.

---

## 🛠️ Primære Værktøjer

1. **Editor:** Neovim opsat med det fuldvoksne **LazyVim** økosystem.
2. **Pakkehåndtering:** **`uv`** (fra Astral) til lynhurtig styring af Python-miljøer og eksterne biblioteker.
3. **AI-Agent:** **Antigravity CLI** (`agy`) integreret direkte i terminalen til live kodeanalyse.
