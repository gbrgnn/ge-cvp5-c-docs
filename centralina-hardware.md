# Centralina Elettrovalvola CVP5 - Specifiche Hardware

<span class="badge badge-blue">Hardware: CVP5 Series</span>
<span class="badge badge-green">PCB: GE183B / GE184B</span>
<span class="badge badge-amber">Protezione: IP65 / IP67</span>

La serie **CVP5** di Galileo Engineering è una famiglia di driver digitali miniaturizzati ad anello chiuso progettati per essere montati direttamente a bordo di elettrovalvole proporzionali idrauliche e pneumatiche.

---

## 📐 Modelli e Varianti Costruttive

La centralina è disponibile con differenti alloggiamenti e connessioni per adattarsi agli standard industriali e macchine mobili:

| Modello | Connettore Principale | Connessione Valvola | Uscite Valvola | Note |
| :--- | :--- | :--- | :--- | :--- |
| **CVP5-A-H** | DIN EN 175301-803 (Hirschmann) | Attacco diretto a bobina | 1 canale (EV1) | Standard idraulico industriale |
| **CVP5-A-M12** | Connettore circolare M12 (5 poli) | Attacco diretto a bobina | 1 canale (EV1) | Bus CAN + Alimentazione su M12 |
| **CVP5-C-M12** | Connettore circolare M12 (5 poli) | Cavo integrato / Bobina compatta | 1 canale (EV1) | Versione compatta ad alta integrazione |
| **CVP5-H (Doppia)** | Deutsch DT04 / Cablaggio doppio | Connessione a 2 solenoidi | 2 canali (EV1 + EV2) | Per valvole a doppio solenoide (direzionali) |

---

## ⚡ Caratteristiche Elettriche

| Parametro | Min | Tipico | Max | Unità | Note |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Tensione di alimentazione ($V_{supply}$)** | 9.0 | 12.0 / 24.0 | 32.0 | Vdc | Con protezione inversione polarità |
| **Corrente massima pilotata ($I_{max}$)** | - | - | 2.6 | A | Regolazione PWM ad anello chiuso |
| **Risoluzione corrente** | - | 1.0 | - | mA | Campionamento ADC 10/12-bit |
| **Frequenza PWM pilota** | 1.0 | 4.0 | 10.0 | kHz | Modulazione ad alta efficienza |
| **Frequenza di Dither** | 50 | 124 | 300 | Hz | Configurabile via SDO `0x2105:5` |
| **Ampiezza Dither** | 0 | - | 25 | % | Regolabile per annullare l'isteresi meccanica |
| **Resistenza bobina raccomandata** | 4.5 | 10 - 22 | 35 | $\Omega$ | Idonea per bobine 12V e 24V |
| **Consumo a riposo (standby)** | - | 35 | 50 | mA | A 24 Vdc senza eccitazione valvola |
| **Temperatura operativa** | -20 | +25 | +70 | °C | Grado industriale/mobile |

---

## 🔌 Connessioni e Pinout

### Connettore M12 - 5 Poli (Codifica A)

Utilizzato nelle versioni `CVP5-A-M12` e `CVP5-C-M12`:

```
        Pin 2: V+ (12-30 Vdc)
              \  2  /
       Pin 5:  ●   ●  Pin 1: CAN Shield / NC
        NC       ● 5
               ●   ●
       Pin 3: /  4  \ Pin 4: CAN_H
    GND (0V)           Pin 5: CAN_L
```

| Pin | Segnale | Funzione |
| :---: | :--- | :--- |
| **1** | **CAN_SHLD / NC** | Schermatura cavo CAN (o non collegato) |
| **2** | **V+ (Alimentazione)** | Positivo di alimentazione (+12 Vdc ... +30 Vdc) |
| **3** | **GND (Massa)** | Riferimento 0 Vdc comune (alimentazione e bus) |
| **4** | **CAN_H** | Linea differenziale CAN High (ISO 11898) |
| **5** | **CAN_L** | Linea differenziale CAN Low (ISO 11898) |

<div class="callout warning">
  <div class="callout-title">⚠️ Terminazione CAN Bus</div>
  La centralina <strong>non</strong> include un resistore di terminazione da 120&nbsp;&Omega; interno fisso, per consentire l'inserimento in qualsiasi punto della dorsale CAN. Verificare che le due estremità della dorsale CANopen siano correttamente terminate con resistori da 120&nbsp;&Omega; tra CAN_H e CAN_L.
</div>

---

### Documenti e Datasheet Scaricabili

- 📑 [Datasheet CVP5-A-H (PDF)](assets/downloads/datasheets/Data-sheet-CVP5-A-H_rv1_eng.pdf ':target=_blank')
- 📑 [Datasheet CVP5-A-M12 (PDF)](assets/downloads/datasheets/Data-sheet-CVP5-A-M12_rv1_eng.pdf ':target=_blank')
- 📑 [Datasheet CVP5-M12 (PDF)](assets/downloads/datasheets/Data-sheet-CVP5-M12_eng.pdf ':target=_blank')
