# Changelog & Note di Rilascio

<span class="badge badge-green">Versione Corrente: v1.1.0</span>
<span class="badge badge-blue">Ultimo Aggiornamento: Novembre 2026</span>

Cronologia dettagliata dell'evoluzione del software **PyCanGui** e degli aggiornamenti della centralina **CVP5**.

---

## Software PyCanGui

### Versione 1.1.0 (09 Novembre 2026)
*Aggiornamento per valvole a singolo solenoide, navigazione dizionario e robustezza parametri.*

- **Rilevamento Automatico Mono / Bi-Valvola:**
  - Analisi delle capacita del dispositivo alla connessione (`sysconfig_ev_number`).
  - Se il dispositivo collegato ha una sola valvola attiva (es. configurazione CVP5C mono-canale EV1), tutti i controlli, le letture e gli allarmi relativi a EV2 vengono impostati automaticamente sullo stato non disponibile (**`N.A.`**).
  - Il selettore canale valvola si blocca su EV1 prevenendo comandi errati dell'operatore.
- **Grafico di Risposta Adattivo (TrendPlot):**
  - La traccia del canale EV2 viene nascosta automaticamente se la valvola e mono-canale.
  - Il box della legenda riduce le proprie dimensioni grafiche da 62px a 40px per massimizzare l'area visibile del tracciato.
- **Box Guida CANopen Integrato nel Dizionario SDO:**
  - Aggiunto un riquadro di promemoria rapido nella schermata *Dictionary* che riassume i COB-ID base, la mappatura dei byte per PDO analogici, digitali ed errori, e le regole di temporizzazione/heartbeat.
- **Filtro Categorie nel Dizionario Oggetti:**
  - Aggiunto selettore a discesa con raggruppamento per:
    - *All objects* (Vista globale)
    - *Communication* (Oggetti standard 0x1000 - 0x1018)
    - *Manufacturer* (Parametri specifici CVP5 0x2100 - 0x2107)
    - *Device profile* (Parametri profilo DS401)
- **Supporto Node-ID Esteso:**
  - Esteso il range ammesso per l'identificatore CAN di destinazione (`target_can_id`) da `(1, 127)` a `(0, 127)`.
- **Ripristino Impostazioni di Fabbrica Potenziato:**
  - Il ripristino dei parametri di default preserva esplicitamente il Node-ID corrente della valvola, azzera la mappa di calibrazione non lineare EV1 e reimposta la frequenza di dither standard a 124 Hz.
- **Nuovo Pacchetto Installer Windows:**
  - Compilato l'installer `GE_PyCAN_GUI_Setup_v1.1.0.exe` (179 MB) con driver PEAK e script Inno Setup aggiornato.

---

### Versione 1.0.0 (03 Luglio 2026)
*Release milestone: stabilita del driver PCAN, ottimizzazione del carico CPU e packaging professionale.*

- **Rilevamento Unplug Adattatore PCAN da Thread RX:**
  - Controllo dello stato di disconnessione hardware direttamente dal loop di ricezione a bassa latenza, evitando attese indefinite in caso di disconnessione del cavo USB.
- **Eliminazione Polling PCAN dal Thread Grafico (UI):**
  - Rimosso lo scan periodico degli adattatori dal thread principale di Qt, azzerando i micro-blocchi e garantendo fluidita dell'interfaccia.
- **Ottimizzazione Pipeline di Ricezione e Refresh Grafico:**
  - Riorganizzato il buffer di decodifica dei messaggi CANopen per ridurre l'allocazione di memoria e rendere fluido il tracciamento delle correnti.
- **Timeout Adattivo per i Test Dinamici (Rise/Fall Time):**
  - Il tempo massimo per i test a gradino viene calcolato dinamicamente in funzione del delta di corrente e della velocita di rampa (mA/s).
- **Protezione Durante i Test Dinamici:**
  - Blocco dell'avvio dei test dinamici in presenza di errori attivi sulla valvola.
  - Arresto immediato dell'acquisizione in corso se sopraggiunge un allarme durante il ciclo di misura.
- **Creazione Installer Ufficiale con Inno Setup:**
  - Integrazione dell'installer PEAK PCAN-Basic all'interno del setup di PyCanGui con elevazione amministrativa UAC.

---

### Versione 0.1.0 (26 Giugno 2026)
*Prima versione della GUI desktop in PySide6.*

- Comunicazione di base con adattatore PCAN-USB su `python-can`.
- Scansione nodi e comandi NMT (Start, Stop, Pre-Operational, Reset).
- Visualizzazione real-time delle misure di corrente EV1 ed EV2.
- Lettura e scrittura SDO di base.

---

## Aggiornamenti Dispositivo CVP5

### Revisione 2026
- **Dither a Frequenza Variabile:**
  - Parametro SDO `0x2105:5` per la regolazione della frequenza del dither tra 50 Hz e 300 Hz a tempo di esecuzione, adattando la risposta alla bobina oleodinamica.
- **Accuratezza Misura Corrente:**
  - Ottimizzazione del campionamento e filtraggio digitale del segnale di corrente per eliminare la deriva termica e garantire massima linearita.
- **Robustezza Elettromagnetica:**
  - Maggiore immunita a disturbi industriali, transienti ISO 7637 e scariche ESD su linea CAN e alimentazione.
- **Dissipazione Termica Migliorata:**
  - Efficienza energetica superiore e migliore smaltimento del calore dello stadio di potenza a correnti elevate (fino a 2.6A continui).
