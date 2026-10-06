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
- statusni register
  - označujeta stanje procesorja po izvršenem ukazu
- kontrolni register
  - določa kako procesor izvaja neke ukaze / operacije

<table>
<tr>
    <td>PSR</td>
<tr>
<tr>
    <td>CR</td>
<tr>
</table>

Zastavice:
- N Negative,
- Z Zero
- C Carry
- V Overflow

<table>
<tr>
    <td colspan="4"></td>
    <td>7</td>
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
