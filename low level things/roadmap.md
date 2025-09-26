# 📚 Libri di riferimento

Questi sono considerati i testi fondamentali per capire il kernel Linux (anche se alcune edizioni non sono più nuovissime, i concetti restano validi):

1. **Understanding the Linux Kernel** – Bovet & Cesati
    
    - Copre architetture x86 (fino al kernel 2.6), molto didattico.
        
    - Ottimo per capire concetti come processi, scheduler, VM, filesystem.
        
    - Un po’ datato, ma come _manuale concettuale_ resta eccellente.
        
2. **Linux Kernel Development** – Robert Love
    
    - Più snello, scritto bene, spiega concetti chiave senza annegare nei dettagli.
        
    - Buona introduzione per entrare nel codice.
        
3. **Linux Device Drivers** – Corbet, Rubini, Kroah-Hartman (3ª edizione, gratis su kernel.org)
    
    - Se ti interessa la parte driver e interfaccia hardware.
        
    - Datato ma ottimo per capire la filosofia delle API kernel.
        
4. **Professional Linux Kernel Architecture** – Wolfgang Mauerer
    
    - Più recente e dettagliato, copre anche architetture multiple.
        
    - Un po’ “pesante”, ma è quello più “completo” e professionale.
        

---

# 📖 Documentazione ufficiale

- **Linux Kernel Documentation (kernel.org)**  
    https://www.kernel.org/doc/html/latest/
 
## 📚 Documentazione ufficiale ARM

Queste sono le “bibbia”, indispensabili se vuoi scendere a basso livello:

- **ARM Architecture Reference Manual (ARM ARM)**
    - Per ARMv8-A (64-bit): cerca _ARMv8-A Architecture Reference Manual_.
    - Per ARMv7-A (32-bit): cerca _ARMv7-A and ARMv7-R Architecture Reference Manual_.  
        Sono lunghi e tecnici, ma fondamentali per capire eccezioni, privilegi, MMU.
- **ARM Generic Interrupt Controller (GIC) Architecture Specification**  
    Serve quando implementi gli interrupt.
    
- **ARM System Registers** (specifici per AArch64 o ARMv7): utili quando scrivi il codice assembly.
    

---

## 🔧 Tutorial e blog pratici

- **“Writing a Simple Operating System from Scratch” (Nick Blundell)** → orientato a x86, ma spiega bene i concetti generali (boot, linker, kernel minimalista).
    
- **“Baking Pi – Operating Systems Development” (Cambridge University)** → ARMv6/ARM11, molto accessibile, ottimo per imparare i fondamenti.
    
- **“Operating Systems Development on ARMv8-A” (arm developer blog / community posts)** → esempi passo passo per AArch64.
    
- **mini-arm-os** su GitHub → un kernel educativo in C/ASM per ARM Cortex-M, utile per capire struttura minimale.
    
- **"AArch64 Bare-metal Programming" (Andre Richter, GitHub)** → probabilmente il miglior tutorial pratico per cominciare con QEMU e Raspberry Pi in AArch64.
    

---

## 🛠️ Tooling & Emulatori

- **QEMU** (https://www.qemu.org/