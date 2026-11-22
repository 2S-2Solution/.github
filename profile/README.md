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
| [**la-ruota**](https://github.com/2S-2Solution/la-ruota) | [2.15](https://github.com/2S-2Solution/la-ruota/releases/latest) · [2.5.1](https://github.com/2S-2Solution/la-ruota/releases/tag/v2026.11.10) | PyTorch compilato **nativo** per gfx906: **2.15** sul kit ROCm 10.1 con MIOpen 3.6 corretta (richiede Marconi), oppure **2.5.1** autosufficiente | vuoi PyTorch sulla Radeon VII: è la scelta consigliata |
| [**rocm-marconi**](https://github.com/2S-2Solution/rocm-marconi) | [2026.11.14](https://github.com/2S-2Solution/rocm-marconi/releases/latest) | kit ROCm/HIP 5.7: shim con **553 API HIP** (557 export) sulla runtime 5.7 del driver: tutta l'unione ROCm 6.1–7.1 più le famiglie di HIP 7, rocBLAS, hipRTC, clang 21 | vuoi scrivere o usare software HIP, compresi i programmi compilati con HIP 7 e le **wheel ufficiali AMD di PyTorch 2.15** |
| [**miopen-gfx906**](https://github.com/2S-2Solution/miopen-gfx906) | [2026.11.21](https://github.com/2S-2Solution/miopen-gfx906/releases/latest) | MIOpen corretta (installatore 3.5.2 con le cinque patch; patch anche per la 3.6): tre casi di **gradienti sbagliati senza errori** eliminati, anche nel database precompilato di AMD, più due correzioni alle RNN (una corsa fra stream che sbagliava le LSTM lunghe) | usi MIOpen fuori da La Ruota |
| [**zluda-gfx906**](https://github.com/2S-2Solution/zluda-gfx906) | [2026.11.20](https://github.com/2S-2Solution/zluda-gfx906/releases/latest) | ZLUDA, allineato all'originale: programmi CUDA (anche le wheel PyTorch ufficiali) tradotti per gfx906 | devi far girare software CUDA già compilato |
| [**tabpfn-gfx906**](https://github.com/2S-2Solution/tabpfn-gfx906) | [2026.11.15](https://github.com/2S-2Solution/tabpfn-gfx906/releases/latest) | attenzione fp32 veloce per **TabPFN 10** (modello v3.5 circa 3× più veloce) e 8.0.3 · *Built with PriorLabs-TabPFN* | usi TabPFN con La Ruota |

> **Da sapere.** La MIOpen inclusa nelle wheel AMD di PyTorch 2.15 sbaglia ancora in silenzio il
> gradiente della convoluzione 3×3 a 3 canali: con quelle wheel servono
> `MIOPEN_DEBUG_CONV_DIRECT_ASM_WRW3X3=0` e `MIOPEN_DEBUG_CONV_IMPLICIT_GEMM_ASM_WRW_V4R1=0`, oppure la
> MIOpen di miopen-gfx906. Sul driver Windows più stream non danno velocità misurabile, ma il driver può sovrapporre lavoro di stream diversi e l'ordine fra stream non è garantito: le dipendenze vanno sempre dichiarate con gli eventi.

**Dipendenze** (si dichiarano, non si copiano):

```
La Ruota 2.5.1 ...... autosufficiente (driver AMD + Python 3.12)
Marconi ............. base (driver AMD)
  ├─ La Ruota 2.15 .. richiede Marconi ≥ 2026.11.14 (MIOpen 3.6 inclusa)
  └─ MIOpen ......... richiede Marconi
       └─ ZLUDA ..... richiede Marconi e MIOpen
tabpfn-gfx906 ....... richiede La Ruota (2.15 con TabPFN 10, 2.5.1 con TabPFN 8.0.3)
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

**2 Solution** — engineering for special projects: mechanics, automation and artificial intelligence.
Erba (CO), Italy.

🌐 **Website: [www.2solution.it](https://www.2solution.it)** · ✉️ [info@2solution.it](mailto:info@2solution.it) ·
📰 [La Ruota, the story (Italian)](https://www.2solution.it/articolo-la-ruota.html)

#### Local AI on the AMD Radeon VII (gfx906) under Windows

AMD has dropped Vega GPUs from ROCm on Windows: the last HIP runtime that still sees them is the 5.7
runtime shipped with the Adrenalin driver, while modern software expects HIP 6.x/7.x. We publish, as
**separate components**, the work that puts these cards back to use for AI. Each one has its own
repository, its own installer in the Releases (with a published SHA256 and a GPU-vs-CPU check) and
its own documented limits. Take only what you need.

| component | latest | what it does | you need it if… |
|---|---|---|---|
| [**la-ruota**](https://github.com/2S-2Solution/la-ruota) | [2.15](https://github.com/2S-2Solution/la-ruota/releases/latest) · [2.5.1](https://github.com/2S-2Solution/la-ruota/releases/tag/v2026.11.10) | PyTorch built **natively** for gfx906 — no CUDA translation: **2.15** on AMD's ROCm 10.1 kit with a fixed MIOpen 3.6 (needs Marconi), or **2.5.1**, fully self-contained. About 5× faster than PyTorch on ZLUDA | you want PyTorch on a Radeon VII: the recommended choice |
| [**rocm-marconi**](https://github.com/2S-2Solution/rocm-marconi) | [2026.11.14](https://github.com/2S-2Solution/rocm-marconi/releases/latest) | HIP 5.7 extension kit: a shim exposing **553 HIP APIs** (557 exports: the whole ROCm 6.1–7.1 API union plus the HIP 7 families 5.7 lacks) on top of the driver's 5.7 runtime (with ABI translation where the structures changed), rocBLAS with a corrected gfx906 kernel library, hipRTC, clang 21 | you build or run HIP software, including programs compiled with HIP 7 and **AMD's official PyTorch 2.15 wheels** |
| [**miopen-gfx906**](https://github.com/2S-2Solution/miopen-gfx906) | [2026.11.21](https://github.com/2S-2Solution/miopen-gfx906/releases/latest) | fixed MIOpen (installer: 3.5.2 with all five patches; patches also for 3.6): three cases of **silently wrong gradients** removed — also from AMD's precompiled kernel database — plus two RNN multi-stream fixes (a stream race that miscomputed long LSTMs) | you use MIOpen outside La Ruota |
| [**zluda-gfx906**](https://github.com/2S-2Solution/zluda-gfx906) | [2026.11.20](https://github.com/2S-2Solution/zluda-gfx906/releases/latest) | ZLUDA for gfx906, aligned with upstream: runs already-compiled CUDA programs (including the official PyTorch CUDA wheels); cuBLAS → rocBLAS, cuDNN → MIOpen (all 86 cuDNN symbols imported by PyTorch 2.3.1+cu118) | you must run CUDA software you cannot rebuild |
| [**tabpfn-gfx906**](https://github.com/2S-2Solution/tabpfn-gfx906) | [2026.11.15](https://github.com/2S-2Solution/tabpfn-gfx906/releases/latest) | fp32 flash attention for **TabPFN 10** (default v3.5 model about 3× faster) and 8.0.3, plugged in from outside — TabPFN is not modified · *Built with PriorLabs-TabPFN* | you use TabPFN with La Ruota |

**Dependencies** (declared, never bundled across components):

```
La Ruota 2.5.1 ...... self-contained (AMD driver + Python 3.12)
Marconi ............. base (AMD driver)
  ├─ La Ruota 2.15 .. needs Marconi ≥ 2026.11.14 (MIOpen 3.6 included)
  └─ MIOpen ......... needs Marconi
       └─ ZLUDA ..... needs Marconi and MIOpen
tabpfn-gfx906 ....... needs La Ruota (2.15 with TabPFN 10, 2.5.1 with TabPFN 8.0.3)
```

Install order for ZLUDA: **Marconi → MIOpen → ZLUDA**.

> **Good to know.** The MIOpen bundled in AMD's PyTorch 2.15 wheels still silently miscomputes the
> weight gradient of 3×3 convolutions with 3 input channels (the first layer of every RGB network):
> with those wheels set `MIOPEN_DEBUG_CONV_DIRECT_ASM_WRW3X3=0` and
> `MIOPEN_DEBUG_CONV_IMPLICIT_GEMM_ASM_WRW_V4R1=0`, or use miopen-gfx906. On the Windows driver,
> multiple GPU streams give no measurable speed-up, yet the driver can overlap work from different streams
> and ordering across streams is not guaranteed: declare cross-stream dependencies with events.

**What we found along the way** — each with a reproduction script and a fix in the repositories:
- MIOpen: wrong weight gradients with no error in three solvers (one a regression from commit
  `24815807e1`), and AMD's precompiled `gfx906.kdb` shipping 1,468 kernels built without the fix;
- PyTorch on ROCm: fp64 softmax computed with a 32-bit exponential on rows longer than 1024, and
  `lstsq` returning inf/NaN silently on rank-deficient matrices;
- HIP 7 ↔ 5.7: three ABI differences (`hipMemoryType`, `HIP_MEMCPY3D`, external semaphores) that made
  3D copies report success with wrong data until the shim translated them.

Everything is tested on **a single Radeon VII** (Windows 10 Pro 22H2, AMD driver 31.0.21923.11000):
no guarantee on other cards. These are unofficial builds, not by AMD, Meta or NVIDIA.

Thanks to [ZLUDA](https://github.com/vosen/ZLUDA) (Andrzej Janik and contributors),
[PyTorch](https://github.com/pytorch/pytorch), AMD ([ROCm](https://github.com/ROCm), MIOpen, HIP) and
[Prior Labs](https://github.com/PriorLabs/TabPFN) (TabPFN).
