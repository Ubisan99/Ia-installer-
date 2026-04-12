# 🧪 BortyLab — GLM-4.7-Flash Global Installer Kit

### v1.0 — 12 Aprile 2026

---

## 📢 Annuncio

Questo è il **kit di installazione globale** per **GLM-4.7-Flash** di Z.ai (Zhipu AI), il modello open-weight più forte nella classe 30B per coding e task agentici.

Il kit viene **aggiornato regolarmente** per seguire:

- Nuove versioni di GLM (4.7, 5.0, 5.1 e future release)
- Aggiornamenti di Ollama e compatibilità
- Nuove quantizzazioni e ottimizzazioni per hardware specifico
- Fix di bug e miglioramenti agli script
- Supporto per nuovi sistemi operativi e architetture

---

## 🖥 Piattaforme Supportate

| Sistema | Script | Stato |
|---------|--------|-------|
| macOS (Apple Silicon M1–M5 + Intel) | `install_macos.sh` | ✅ Stabile |
| Linux (Ubuntu, Debian, Fedora, Arch, Manjaro, RHEL) | `install_linux.sh` | ✅ Stabile |
| Windows 10/11 (PowerShell) | `install_windows.ps1` | ✅ Stabile |
| Test & Benchmark (cross-platform) | `test_glm47.py` | ✅ Stabile |

### GPU Supportate

- NVIDIA (CUDA) — tutte le GPU con 6GB+ VRAM
- AMD (ROCm) — supporto Linux nativo, sperimentale su Windows
- Apple Metal — memoria unificata M1/M2/M3/M4/M5
- CPU-only — fallback automatico su qualsiasi sistema

---

## 📋 Cosa fa ogni script

Ogni installer gestisce **automaticamente**:

1. Rilevamento OS, architettura, RAM e GPU
2. Installazione package manager (Homebrew / apt / dnf / pacman / winget)
3. Dipendenze: `curl`, `wget`, `jq`, `python3`, `pip`
4. Librerie Python: `ollama`, `openai`, `requests`
5. Installazione e avvio di Ollama
6. Download del modello GLM-4.7-Flash
7. Scelta automatica della quantizzazione in base alla RAM
8. Configurazione servizio (systemd su Linux)
9. Test di verifica automatico
10. Creazione shortcut `glm-chat` per avvio rapido

---

## 📦 Contenuto del Kit

```
BortyLab_GLM47Flash_InstallKit/
├── README.md              ← Documentazione completa
├── CHANGELOG.md           ← Questo file (changelog e roadmap)
├── install_macos.sh       ← Installer macOS
├── install_linux.sh       ← Installer Linux
├── install_windows.ps1    ← Installer Windows
└── test_glm47.py          ← Test suite cross-platform
```

---

## 🔄 Changelog

### v1.0 — 12 Aprile 2026
- Release iniziale
- Supporto macOS (Apple Silicon + Intel)
- Supporto Linux (Ubuntu, Debian, Fedora, Arch e derivate)
- Supporto Windows 10/11 (PowerShell)
- Rilevamento automatico GPU (NVIDIA CUDA, AMD ROCm, Apple Metal)
- Scelta automatica quantizzazione per RAM disponibile
- Test suite Python con benchmark velocità
- Compatibilità API OpenAI su localhost:11434
- Shortcut `glm-chat` per avvio rapido
- README completo con tabelle hardware e esempi API

---

## 🗺 Roadmap

### v1.1 (prossimo aggiornamento)
- [ ] Supporto Docker (container preconfigurato)
- [ ] Aggiornamento automatico del modello (`--update` flag)
- [ ] Supporto GLM-5 e GLM-5.1 (quant ridotte per hardware consumer)
- [ ] Integrazione con LM Studio e Jan come backend alternativi
- [ ] Script di disinstallazione pulita

### v1.2
- [ ] Supporto WSL2 (Windows Subsystem for Linux)
- [ ] Profili hardware predefiniti (Mac Mini M4 16GB, Steam Deck, etc.)
- [ ] Benchmark comparativo automatico tra quant diverse
- [ ] Integrazione con Claude Code come backend

### v2.0
- [ ] Installer GUI (Electron/Tauri) per utenti non tecnici
- [ ] Multi-modello: installa e switcha tra GLM, Qwen, DeepSeek, Gemma
- [ ] Dashboard web locale per monitoraggio prestazioni
- [ ] Supporto cluster multi-device (EXO)

---

## ⚡ Quick Start

```bash
# macOS
chmod +x install_macos.sh && ./install_macos.sh

# Linux
chmod +x install_linux.sh && sudo ./install_linux.sh

# Windows (PowerShell admin)
Set-ExecutionPolicy Bypass -Scope Process; .\install_windows.ps1

# Test (tutti i sistemi)
python3 test_glm47.py

# Chat
glm-chat
```

---

## 🤝 Contribuire

Questo kit è mantenuto da BortyLab. Per segnalare bug, richiedere feature o contribuire:

- Apri una issue con descrizione del problema e output del terminale
- Includi sempre l'output di `python3 test_glm47.py` per il debug
- Pull request benvenute per nuove piattaforme o fix

---

## 📜 Licenza

GLM-4.7-Flash è rilasciato sotto **licenza MIT** da Z.ai.
Questo kit di installazione è rilasciato sotto **MIT License** — usalo come vuoi.

---

WSL1 è tecnicamente più stabile sul fronte rete, ma c'è un motivo strutturale ben preciso dietro questo comportamento.
Ecco perché accade e quali sono le differenze chiave:
1. Architettura di Rete: "Ospite" vs "Vicino"
La differenza principale risiede nel modo in cui Linux comunica con l'hardware di Windows.
WSL1 (Condivisione dello Stack): WSL1 usa lo stesso stack di rete di Windows. Non ha un suo indirizzo IP separato; è come se fosse un'applicazione che gira direttamente su Windows. Se Windows è connesso, lo è anche WSL1. Zero complicazioni.
WSL2 (Virtualizzazione): WSL2 è una vera macchina virtuale (leggera, ma pur sempre una VM). Ha il suo kernel Linux e, soprattutto, il suo Virtual Ethernet Adapter. Vive in una sottorete privata dietro un NAT (Network Address Translation).
2. Perché WSL2 "fa i capricci"?
Il passaggio alla virtualizzazione ha introdotto tre problemi comuni che in WSL1 semplicemente non esistono:
Il NAT e i DNS: WSL2 deve tradurre le richieste dal suo "mondo" a quello di Windows. Spesso il file /etc/resolv.conf all'interno di Linux non riesce a sincronizzarsi correttamente con i DNS di Windows, specialmente se usi una VPN o cambi spesso rete (es. da Wi-Fi dell'ufficio a casa).
Il Firewall di Windows: Essendo una rete separata, il traffico di WSL2 viene talvolta visto da Windows come "traffico esterno" e bloccato dal firewall se non configurato a dovere.
Cambio di IP al riavvio: Ogni volta che riavvii il PC, l'IP interno di WSL2 cambia. Se hai servizi che devono comunicare tra i due sistemi, questo "balletto" degli indirizzi rompe i collegamenti.

*🧪 BortyLab — Aggiornato al 12 Aprile 2026*
