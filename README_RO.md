🌐 **Язык / Limba / Language:** [Русский](README.md) | [Română](README_RO.md) | [English](README_EN.md)

---

# 🕹️ Sega Mega Drive / Genesis Custom Mappers & Bank Switching

Arhitectură personalizată proprie de mapper și sistem de comutare a `Bank Switching` pentru cartușe Sega Mega Drive / Genesis, prezentată în trei versiuni și modulații consecutive (de la schema de bază pe D-flip-flops până la echivalentul complet SSF2).

> **Autor:** Pushkash  
> Toate schemele și soluțiile arhitecturale au fost proiectate personal de autorul proiectului.

---

## 🔍 Particularități de adresare și gestionare a memoriei

* **Adresare directă $A0 \to A1$:** În scheme, adresarea circuitelor de memorie începe de la linia $A0$, în timp ce pe slotul de cartuș al consolei numerotarea magistralei de adrese începe de la $A1$. Prin urmare, linia $A0$ din scheme se conectează la linia $A1$ a slotului.
* **Salvare nevolatilă (32 KB):** În versiunile v.3 și v.4 este implementat suportul pentru salvare bazat pe **FRAM / SRAM de 32 KB** ($256 \text{ Kbit}$) (în cazul instalării unei memorii **SRAM** obișnuite, este necesar un circuit standard de alimentare de la baterie).

---

## 🛠 Analiza schemelor și a arhitecturii

### 📌 v.2 — Mapper de până la 8 MB (64 Mbit) pe D-Flip-Flops
![Schema v2](SSF2_CARD_MAPP_v2.png)

* **Baza de elemente:** Construit pe D-flip-flops fără bloc de salvare.
* **Logica de funcționare:**
  * **Bancurile 0–5:** Controlate direct de procesorul Motorola 68000.
  * **Presetare de pornire (Bancurile 6 și 7):** Realizate prin inversarea liniilor individuale de date pe registre, ceea ce setează hardware stările inițiale: `REG:1 = 6` și `REG:2 = 7`, permițând citirea primilor 4 MB de ROM imediat la pornire.
  * **Comutarea adreselor:** Prin intermediul basculei sunt setați biții superiori de adresă pentru accesarea memoriei de peste 4 MB.

---

### 📌 v.3 — Mapper de până la 32 MB (256 Mbit) pe **74xx174** / **74xx157** cu FRAM / SRAM (32 KB)
![Schema v3](SSF2_CARD_MAPP_v3.png)

* **Baza de elemente:** Registre **74xx174**, multiplexoare **74xx157** și suport pentru memorie **FRAM / SRAM (32 KB)**.
* **Logica de comutare a bancurilor:**
  * **Bancurile 0–5:** Fixate pe procesorul Motorola 68000.
  * **Presetare de pornire (Bancurile 6 și 7):** Realizate prin inversarea liniilor individuale de date pe registre, setând hardware stările inițiale: `REG(inst8) = 6` și `REG(inst9) = 7`, permițând citirea primilor 4 MB de ROM imediat la pornire.
  * **Adresare peste 4 MB:**  
    * Controlat de liniile $A{18}\dots A{20}$ și scrierile în `REG(inst8)` , `REG(inst9)` care generează adresa necesară pe liniile memoriei Flash `ROM_A18 ... ROM_A23`.

---

### 📌 v.4 — Mapper complet SSF2 de până la 32 MB (256 Mbit) pe **74xx174** / **74xx151** cu FRAM / SRAM (32 KB)
![Schema v4](SegaMegaDriveBankSwitching_v4.png)

* **Baza de elemente:** 7 registre **74xx174**, 6 multiplexoare **74xx151** și suport pentru memorie **FRAM / SRAM (32 KB)**.
  * **Bancul 0:** Fixat rigid (vectori de întrerupere și antetul programului).
  * **Presetări hardware la pornire:** Realizate prin inversarea liniilor individuale de date pe registre, setând hardware stările inițiale: `Registrul 1 = 1`, `Registrul 2 = 2` ... `Registrul 7 = 7`, permițând citirea primilor 4 MB de ROM imediat la pornire.
  * **Adresare peste 4 MB:**  
    * Controlat de liniile $A{18}\dots A{20}$ și scrierile în registrele $REG{1}\dots REG{7}$, expunând adresa necesară pe liniile memoriei Flash `ROM_A18 ... ROM_A23`.

---

## 🖼 Fișiere de scheme
* `SSF2_CARD_MAPP_v2.png` — Schema mapper-ului v.2 de până la 8 MB pe D-flip-flops.
* `SSF2_CARD_MAPP_v3.png` — Schema mapper-ului v.3 de până la 32 MB pe **74xx174** / **74xx157** cu FRAM / SRAM 32KB.
* `SegaMegaDriveBankSwitching_v4.png` — Schema mapper-ului v.4 SSF2 (7 registre **74xx174** / 6 multiplexoare **74xx151**) cu FRAM / SRAM 32KB.