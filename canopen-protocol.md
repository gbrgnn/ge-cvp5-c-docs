# Protocollo CANopen & Dizionario Oggetti

<span class="badge badge-amber">Standard: CiA DS301 / DS401</span>
<span class="badge badge-blue">CAN ID: 11-bit Standard</span>
<span class="badge badge-green">Default Node-ID: 10 (0x0A)</span>
<span class="badge badge-blue">Default Bitrate: 250 kbit/s</span>

Questa sezione documenta l'interfaccia di comunicazione CANopen implementata sulla centralina CVP5 per consentire l'integrazione con qualsiasi Master CANopen (PLC, controllori industriali, PC o gateway).

---

## Riepilogo Identifier CAN (COB-ID)

Con Node-ID predefinito = **10 (`0x0A`)**:

| Servizio | COB-ID Base | COB-ID Effettivo (ID 10) | Direzione | Descrizione |
| :--- | :--- | :--- | :---: | :--- |
| **NMT Control** | `0x000` | `0x000` | Master &rarr; Slave | Gestione stati NMT della rete |
| **SYNC** | `0x080` | `0x080` | Master &rarr; Slave | Frame di sincronizzazione ciclico |
| **EMCY** | `0x080 + Node-ID` | `0x08A` | Slave &rarr; Master | Notifica di emergenza e guasto |
| **TPDO1** | `0x180 + Node-ID` | `0x18A` | Slave &rarr; Master | Misure correnti (EV1/EV2) e tensioni |
| **RPDO1** | `0x200 + Node-ID` | `0x20A` | Master &rarr; Slave | Comando setpoint corrente bobina |
| **TPDO2** | `0x280 + Node-ID` | `0x28A` | Slave &rarr; Master | Byte stato allarmi e diagnostica |
| **SDO Response** | `0x580 + Node-ID` | `0x58A` | Slave &rarr; Master | Risposta lettura/scrittura dizionario |
| **SDO Request** | `0x600 + Node-ID` | `0x60A` | Master &rarr; Slave | Richiesta lettura/scrittura dizionario |
| **Heartbeat** | `0x700 + Node-ID` | `0x70A` | Slave &rarr; Master | Segnale di presenza del nodo (1000 ms) |
| **LSS Response** | `0x7E4` | `0x7E4` | Slave &rarr; Master | Risposta configurazione Layer Setting Services |
| **LSS Request** | `0x7E5` | `0x7E5` | Master &rarr; Slave | Richiesta configurazione Layer Setting Services |

---

## Interfaccia Real-Time PDO (Process Data Object)

I PDO sono i messaggi periodici ad alta velocita scambiati a macchina attiva senza sovraccarico di protocollo.

### RPDO1 - Comando Setpoint Corrente (`0x200 + Node-ID`)

Inviato dal controllore Master alla valvola per impostare la corrente desiderata:

| Byte | Campo | Tipo | Descrizione |
| :---: | :--- | :---: | :--- |
| **0 - 1** | **CH1_Ref (EV1)** | UINT16 (LE) | Riferimento corrente bobina EV1 scalato |
| **2 - 3** | **CH2_Ref (EV2)** | UINT16 (LE) | Riferimento corrente bobina EV2 (se presente) |
| **4 - 7** | Non Usati | - | Riservati (inviare `0x00`) |

*Nota:* Il valore e in formato Little-Endian scalato rispetto a $I_{max}$.

### TPDO1 - Feedback Analogico Correnti & Tensioni (`0x180 + Node-ID`)

Trasmesso periodicamente dalla centralina al Master (periodo predefinito: **100 ms**):

| Byte | Campo | Tipo | Unita | Descrizione |
| :---: | :--- | :---: | :---: | :--- |
| **0 - 1** | **Iread_EV1** | INT16 (BE) | mA | Corrente misurata effettiva su solenoide EV1 |
| **2 - 3** | **Iread_EV2** | INT16 (BE) | mA | Corrente misurata su EV2 (se presente, altrimenti 0) |
| **4 - 5** | **Vread_EXT** | INT16 (BE) | mV | Tensione analogica ausiliaria (se non presente: -123) |
| **6 - 7** | **Vread_SPP** | INT16 (BE) | mV | Tensione alimentazione di linea ($V_{supply}$) |

*Nota:* I campi di TPDO1 sono trasmessi in formato **Big-Endian**.

### TPDO2 - Allarmi e Diagnostica (`0x280 + Node-ID`)

Trasmesso su variazione di stato o periodicamente per monitorare le anomalie:

| Byte | Campo | Descrizione Bit |
| :---: | :--- | :--- |
| **0** | **ErrorEV1** | **Bit 0:** Cavo aperto (Broken Wire)<br>**Bit 1:** Cortocircuito / Sovracorrente ($I > I_{max}$)<br>**Bit 2:** Sovratensione / Sottotensione linea<br>**Bit 3:** Allarme termico driver |
| **1** | **ErrorEV2** | Stessi flag di allarme applicati al canale EV2 |
| **2** | **ErrorDigital** | Anomalie uscite ausiliarie |
| **3 - 7** | Riservati | Non utilizzati |

---

## Dizionario Oggetti SDO (Service Data Objects)

Il dizionario oggetti memorizza la configurazione persistente, le curve di taratura e i parametri del controllore. Le richieste avvengono su `0x600 + Node-ID` con risposte su `0x580 + Node-ID`.

### Parametri di Comunicazione (0x1000 - 0x1018)

| Indice | Sub-indice | Nome Oggetto | Tipo | Accesso | Valore Default / Descrizione |
| :---: | :---: | :--- | :---: | :---: | :--- |
| `0x1000` | 0 | Device Type | U32 | RO | `0x00000000` (Generic I/O device) |
| `0x1001` | 0 | Error Register | U8 | RO | Registro errori standard CiA DS301 |
| `0x100A` | 0 | Manufacturer Software Version | STR | RO | Versione firmware installata |
| `0x1010` | 1 | Store All Parameters | U32 | RW | Scrivere stringa `"save"` (`0x65766173`) in Flash |
| `0x1011` | 1 | Restore Default Parameters | U32 | RW | Scrivere stringa `"load"` (`0x64616F6C`) per reset di fabbrica |
| `0x1017` | 0 | Producer Heartbeat Time | U16 | RW | Millisecondi (default: `1000` ms) |
| `0x1018` | 1 | Vendor ID | U32 | RO | Identificativo costruttore Galileo Engineering (`22`) |
| `0x1018` | 2 | Product Code | U32 | RO | Codice prodotto CVP-CAN (`1`) |
| `0x1018` | 3 | Revision Number | U32 | RO | Numero revisione hardware/firmware |
| `0x1018` | 4 | Serial Number | U32 | RO | Numero di serie univoco dispositivo |

### Parametri Valvola EV1 (0x2102) & EV2 (0x2103)

| Indice EV1 | Indice EV2 | Sub | Parametro | Default | Limiti | Descrizione |
| :---: | :---: | :---: | :--- | :---: | :---: | :--- |
| `0x2102` | `0x2103` | 1 | **Max Current** | 1000 mA | 100 - 2600 mA | Corrente massima erogabile a fondo scala |
| `0x2102` | `0x2103` | 2 | **Offset Current** | 0 mA | 0 - 500 mA | Corrente minima al primo scatto (gap meccanico) |
| `0x2102` | `0x2103` | 3 | **Ramp Up** | 1000 mA/s | 10 - 10000 mA/s | Velocita massima salita corrente |
| `0x2102` | `0x2103` | 4 | **Ramp Down** | 1000 mA/s | 10 - 10000 mA/s | Velocita massima discesa corrente |
| `0x2102` | `0x2103` | 5 | **Dither Amplitude** | 10 % | 0 - 25 % | Percentuale di oscillazione dither |
| `0x2102` | `0x2103` | 8 | **Control Mode** | 5 | 0 - 7 | Modalita di controllo anello |
| `0x2102` | `0x2103` | 9 | **Zero Offset** | 0 % | 0 - 20 % | Offset calibrazione zero |
| `0x2102` | `0x2103` | 16 | **Mapping Flag** | 0 | 0 / 1 | 1 = Attiva curva di linearizzazione 10 punti |

### Guadagni Regolatore PID (0x2104)

| Indice | Sub | Parametro | Descrizione Funzionale |
| :---: | :---: | :--- | :--- |
| `0x2104` | 1 | `pid_prop_gain_band` | Larghezza banda di transizione proporzionale |
| `0x2104` | 2 | `pid_prop_gain_in` | Guadagno proporzionale all'interno della banda di precisione |
| `0x2104` | 3 | `pid_prop_gain_out` | Guadagno proporzionale esterno (risposta veloce) |
| `0x2104` | 4 | `pid_der_gain` | Guadagno derivativo (smorzamento oscillazioni) |
| `0x2104` | 5 | `pid_int_gain_band_frz` | Soglia di congelamento accumulatore integrale |
| `0x2104` | 6 | `pid_int_gain_band` | Banda di calcolo azione integrale |
| `0x2104` | 7 | `pid_int_gain_band_sat`| Limite di saturazione anti-windup integrale |
| `0x2104` | 8 | `pid_int_gain` | Guadagno integrale $K_i$ principale |

### Configurazione Dither & Rete (0x2105, 0x2106, 0x2107)

- **`0x2105:5` (Dither Frequency):** Frequenza dither in Hertz (range: **50 Hz ... 300 Hz**, default: **124 Hz**).
- **`0x2106:0` (Node-ID):** Modifica del Node-ID CANopen del dispositivo (range: `1 ... 127`). Per renderlo effettivo salvare con `0x1010:1` ed eseguire un reset NMT `0x81`.
- **`0x2107:2` (EV Number):** Numero di valvole collegate (1 = mono-valvola EV1, 2 = bi-valvola EV1+EV2).

---

## Esempi di Frame CAN

### 1. Avvio Nodo in Modalita Operativa (NMT Start)
Per consentire la ricezione dei comandi RPDO1 e la trasmissione dei TPDO:
```
CAN ID: 0x000    DLC: 2    Data: 01 0A
```
*(01 = Start Node, 0A = Node-ID 10)*

### 2. Comando Setpoint Corrente EV1 a 500 mA
Inviare un frame RPDO1 a `0x20A`:
```
CAN ID: 0x20A    DLC: 4    Data: F4 01 00 00
```
*(0x01F4 = 500 decimale in Little-Endian sui byte 0-1)*

### 3. Salvataggio Configurazione in Memoria Non-Volatile Flash (SDO)
Scrivere il valore `0x65766173` ("save") all'indice `0x1010` sub `1`:
```
CAN ID: 0x60A    DLC: 8    Data: 23 10 10 01 73 61 76 65
```
Risposta attesa dalla centralina:
```
CAN ID: 0x58A    DLC: 8    Data: 60 10 10 01 00 00 00 00
```
