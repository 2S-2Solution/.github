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

| componente | cosa fa | ti serve se… |
|---|---|---|
| [**la-ruota**](https://github.com/2S-2Solution/la-ruota) | PyTorch 2.5.1 compilato **nativo** per ROCm, autosufficiente | vuoi PyTorch sulla Radeon VII: è la scelta consigliata |
| [**rocm-marconi**](https://github.com/2S-2Solution/rocm-marconi) | kit ROCm/HIP 5.7: shim delle API HIP 6.x/7.x, rocBLAS, hipRTC, clang 21 | vuoi scrivere o usare software HIP |
| [**miopen-gfx906**](https://github.com/2S-2Solution/miopen-gfx906) | MIOpen 3.5.2 corretta: tre casi di **gradienti sbagliati senza errori** eliminati | usi MIOpen fuori da La Ruota |
| [**zluda-gfx906**](https://github.com/2S-2Solution/zluda-gfx906) | ZLUDA: programmi CUDA (anche le wheel PyTorch ufficiali) tradotti per gfx906 | devi far girare software CUDA già compilato |
| [**tabpfn-gfx906**](https://github.com/2S-2Solution/tabpfn-gfx906) | attenzione fp32 veloce per TabPFN 8.0.3 · *Built with PriorLabs-TabPFN* | usi TabPFN con La Ruota |

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
extension kit), [miopen-gfx906](https://github.com/2S-2Solution/miopen-gfx906) (MIOpen with three
silent wrong-gradient bugs fixed), [zluda-gfx906](https://github.com/2S-2Solution/zluda-gfx906)
(ZLUDA for gfx906) and [tabpfn-gfx906](https://github.com/2S-2Solution/tabpfn-gfx906) (fp32 attention
for TabPFN). Install order for ZLUDA: Marconi → MIOpen → ZLUDA. Tested on a single Radeon VII.
