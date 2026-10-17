# Embeded systems

>[!WARNING] Obvezna prisotnost lmaoo

>[!IMPORTANT] Ni kolokvijev
> Ni pisnega izpita  project
> IZPIT: Projektna naloga + ustni zagovor
> Prijava na izpit -> **IGNORIRAJ TISTE ROKE / NE SE PRIJAVIT**
> Nalogo zagovoris, kadar jo končas
> roki za ustni vsak teden razen med prb 10.6 - 15.8
> **IZPIT MORA BITI OPRAVLJEN V TEKOČEM ŠTUDIJSKEM LETU**

>[!NOTE]
> Literatura: 
> - Tehnična dokumentacija sistemov, ki bodo rabljeni
> - Učbenik
>   -  opisuje samo koncepte, ne pa **detailov**

>[!NOTE]
> Maili:
> pa3cijo@fri.uni-lj.si
> rok.cesnovar@fri.lj-uni.si

>[!NOTE]
> Vaje se začnejo: 19 OCT

## Jedra
Dve družini: 
1. A $\rightarrow$ aplikacijska jedra
2.  B $\rightarrow$ embeded jedra

# ARM Cortex - M
## Programski model
- 16 splosno namenskih 32 registrov
- splošno namenski = programer lahko spreminja z ukazi
- ortogonalni register = v poljubnem ukazu lahko uprabim katerega koli od ukazov, brez da se spremeni

<table>
<tr>
    <td></td>
    <td>R0</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R1</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R2</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R3</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R4</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>...</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R11</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R12</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>R13</td>
    <td>Stack pointer</td>
</tr>
<tr>
    <td></td>
    <td>R14</td>
    <td>Ret Addr</td>
</tr>
<tr>
    <td></td>
    <td>R15</td>
    <td>PC</td>
</tr>
</table>

**Specialna registra**
- Ne moremo uporabiti kot operatorje v operacijah
- nista dosegljiva iz AL, L/S in jmp ukazov
- statusni register (Program status register)
  - oznaka: x
  - označujeta stanje procesorja po izvršenem ukazu
- kontrolni register (controll register)
  - določa kako procesor izvaja neke ukaze / operacije

<table>
<tr>
    <td>PSR</td>
<tr>
<tr>
    <td>CR</td>
</tr>
</table>

Zastavice:
- N Negative,
- Z Zero
- C Carry
- V Overflow

<table>
<tr>
    <td colspan="4"></td>
    <td>8 alpa 7</td>
    <td>0</td>
</tr>
<tr>
    <td>N</td>
    <td>Z</td>
    <td>C</td>
    <td>V</td>
    <td></td>
    <td>EXCEPTION NUMBER</td>
</tr>
</table>

CPU te flagge nastavi sam


<table>
<tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td colspan="2">Določa trenutni način delovanja cpu</td>
    
</tr>
<tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>na kateri stack se shrani kontekst ob interruptu</td>
    <td>nPRIV [0 -> privileged; 1 -> nepr]</td>
</tr>
</table>

- Načini delovanja
  - privileged
    - koda ki jo izvaja lahko uprablja vse cpe ukaze, ki jih CPU pozna
    - Sme brati in spreminjati vse bite v specialnih registrih
  - ne priviligiran

Kje nam ti nacini pomagajo?
Ločimo kodo na sistemsko in aplikcaijsko

- En stack naj upravlja RTOS
- Em stack naj uprablja apl

Imamo samo 1 sp, kako pole jih imet vec?
ne morem jih ločit na nivoju splosno namenskih registrov, saj lahko user dostopa do njih -> ne smemo reci da naj RTOS rabi R13, apl pa R12

Resitev: 2 dodatna specialna registra

Imamo R13, ki je stack pointer
- Process sp
  - Rabi ne privileged koda
- Main sp
  - Rabi priviligirana koda

Ta dva reg nista vidna -> ju ne moremo rabiti v push in pop ukazih

Vidna sta preko R13

<table>
<tr>
    <td>PSP</td>
</tr>
<tr>
    <td>MSP</td>
</tr>
</table>

Kaj to pomeni?
Obe kodi (priv in ne priv) za dostop do stacka vedno uporabljajo R13, amkak, priv "skozi" SP vidi MSP nepriv pa PSP

SP(R13) ni pravi fizični register -> shadow register
CPU vedno rabi ali PSP ali MSP -> katerega od teh bo rabil doloca nPRIV bit, zato se temu registru rece **shadow register**

### Kako pridem v priviligiran nacin

1. takoj po resetu je CPE v priv nacinu
po resetu: NPriv = 0;

2. Iz nepriv? $\rightarrow$ **Ne gre**

3. Izjeme
Procesor ustavi izvajanje in si sam nastavi PC
in gre izvajat neki druzga
   - Pasti
   - Prekinitve
Ob odzivu na izjemo CP avtomatično nastavi nPriv = 0
Tako se vsi prekinitveno servisni podprogrami izvajajo v priviligiranem načinu

## Izjeme
V CPU se za vse izjeme odzove vedno enako
Izjeme so vektorske -> procesor ob izjemi dobi tudi kodo povzročitelja izjeme (Exception number)

Za pasti je predpisano v naprej
za prekinitve pa določi načrtovalic HW sistema

<table>
<tr>
    <td>Exception Number</td>
    <td>TIP</td>
    <td>Vectorski Naslov</td>
<tr>
<tr>
    <td>1</td>
    <td>RESET</td>
    <td>0x000 0004</td>
<tr>
<tr>
    <td>2</td>
    <td>NMI</td>
    <td></td>
</tr>
<tr>
    <td>15</td>
    <td>SYSTICK</td>
    <td>0x0000 003C</td>
</tr>
<tr>
    <td>16</td>
    <td>ZNANJE PREKINITVE</td>
    <td>0x0000 0040</td>
</tr>
</table>

Ko pride izjema (past / prekinitev)
1. Nemudoma ustavi izvajanje trenutnega programa
2. Shrani kontekst na sklad, ne pa vseh registrov
   - Kontekst je tista vsebina, ki je ne smem izgubit
   - ima pomembne informacije o prekinjenem programu
   - Shranijo se:
     - R0
     - R1
     - R2
     - R3
     - R12
     - LR
     - PC
     - XPSR
    - Stack je full descending
      - Naslovi narascajo navzdol
      - Sklad raste navzgor
3. LR $\leftarrow$ "EXEC RETURN"
4. PC $\leftarrow$ M[vector]
