# 2S · 2Solution

**2 Solution** — ingegneria per progetti speciali: meccanica, automazione e intelligenza artificiale.
Erba (CO), Italia.

🌐 **Sito: [www.2solution.it](https://www.2solution.it)** · ✉️ [info@2solution.it](mailto:info@2solution.it) ·
📰 [La Ruota, la storia](https://www.2solution.it/articolo-la-ruota.html)

---

## Intelligenza artificiale in locale su AMD Radeon VII (gfx906) sotto Windows

AMD ha tolto le GPU Vega dal supporto ROCm per Windows. Qui pubblichiamo, in **componenti separati**,
il lavoro che le rimette in uso per l'IA: ognuno ha il suo repository, il suo installatore nelle
Releases e i suoi limiti dichiarati. Si prende solo ciò che serve.

| componente | ultima versione | cosa fa | ti serve se… |
|---|---|---|---|
| [**la-ruota**](https://github.com/2S-2Solution/la-ruota) | [2026.11.10](https://github.com/2S-2Solution/la-ruota/releases/latest) | PyTorch 2.5.1 compilato **nativo** per ROCm, autosufficiente | vuoi PyTorch sulla Radeon VII: è la scelta consigliata |
| [**rocm-marconi**](https://github.com/2S-2Solution/rocm-marconi) | [2026.11.14](https://github.com/2S-2Solution/rocm-marconi/releases/latest) | kit ROCm/HIP 5.7: shim con **557 API HIP 6.x/7.x** sulla runtime 5.7 del driver, rocBLAS, hipRTC, clang 21 | vuoi scrivere o usare software HIP, compresi i programmi compilati con HIP 7 e le **wheel ufficiali AMD di PyTorch 2.15** |
| [**miopen-gfx906**](https://github.com/2S-2Solution/miopen-gfx906) | [2026.11.11](https://github.com/2S-2Solution/miopen-gfx906/releases/latest) | MIOpen 3.5.2 corretta: tre casi di **gradienti sbagliati senza errori** eliminati, anche nel database precompilato di AMD | usi MIOpen fuori da La Ruota |
| [**zluda-gfx906**](https://github.com/2S-2Solution/zluda-gfx906) | [2026.11.14](https://github.com/2S-2Solution/zluda-gfx906/releases/latest) | ZLUDA, allineato all'originale: programmi CUDA (anche le wheel PyTorch ufficiali) tradotti per gfx906 | devi far girare software CUDA già compilato |
| [**tabpfn-gfx906**](https://github.com/2S-2Solution/tabpfn-gfx906) | [2026.11.11](https://github.com/2S-2Solution/tabpfn-gfx906/releases/latest) | attenzione fp32 veloce per TabPFN 8.0.3 · *Built with PriorLabs-TabPFN* | usi TabPFN con La Ruota |

> **Da sapere.** La MIOpen inclusa nelle wheel AMD di PyTorch 2.15 sbaglia ancora in silenzio il
> gradiente della convoluzione 3×3 a 3 canali: con quelle wheel servono
> `MIOPEN_DEBUG_CONV_DIRECT_ASM_WRW3X3=0` e `MIOPEN_DEBUG_CONV_IMPLICIT_GEMM_ASM_WRW_V4R1=0`, oppure la
> MIOpen di miopen-gfx906. Sul driver Windows gli stream della GPU non vanno mai in parallelo.

**Dipendenze** (si dichiarano, non si copiano):

```
La Ruota ............ autosufficiente (driver AMD + Python 3.12)
  └─ tabpfn-gfx906 .. richiede La Ruota
Marconi ............. base (driver AMD)
  └─ MIOpen ......... richiede Marconi
       └─ ZLUDA ..... richiede Marconi e MIOpen
```

Ordine d'installazione per ZLUDA: **Marconi → MIOpen → ZLUDA**.

Tutto è provato su **una sola Radeon VII** (Windows 10 Pro 22H2, driver AMD 31.0.21923.11000):
nessuna garanzia su altre schede. Ogni installatore ha l'impronta SHA256 pubblicata accanto e una
verifica che confronta la GPU con la CPU.

Ringraziamenti: [ZLUDA](https://github.com/vosen/ZLUDA) (Andrzej Janik e contributori),
[PyTorch](https://github.com/pytorch/pytorch), AMD ([ROCm](https://github.com/ROCm), MIOpen, HIP),
[Prior Labs](https://github.com/PriorLabs/TabPFN) (TabPFN).

---

### English

**2 Solution** — engineering for special projects (mechanics, automation, AI), Erba (CO), Italy ·
**[www.2solution.it](https://www.2solution.it)**.

Open-source, separately installable components that bring local AI back to the **AMD Radeon VII
(gfx906) on Windows**: [la-ruota](https://github.com/2S-2Solution/la-ruota) (native ROCm PyTorch 2.5.1,
self-contained, recommended), [rocm-marconi](https://github.com/2S-2Solution/rocm-marconi) (HIP 5.7
extension kit: 557 HIP 6.x/7.x APIs on the driver's 5.7 runtime, runs HIP 7 programs and AMD's
official PyTorch 2.15 wheels), [miopen-gfx906](https://github.com/2S-2Solution/miopen-gfx906) (MIOpen
with three silent wrong-gradient bugs fixed, including in AMD's precompiled kernel database),
[zluda-gfx906](https://github.com/2S-2Solution/zluda-gfx906) (ZLUDA for gfx906, aligned with upstream)
and [tabpfn-gfx906](https://github.com/2S-2Solution/tabpfn-gfx906) (fp32 attention for TabPFN).
Install order for ZLUDA: Marconi → MIOpen → ZLUDA. Note: the MIOpen inside AMD's PyTorch 2.15 wheels
still miscomputes the 3-channel 3×3 weight gradient — set the two `MIOPEN_DEBUG_*=0` variables or use
miopen-gfx906. Tested on a single Radeon VII.
