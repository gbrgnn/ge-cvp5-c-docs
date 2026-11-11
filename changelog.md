# Changelog & Cronologia Modifiche

<span class="badge badge-green">Versione Corrente: v1.1.0</span>
<span class="badge badge-blue">Ultimo Aggiornamento: Novembre 2026</span>

In questa pagina è documentata l'evoluzione dettagliata del software **PyCanGui**, del **Firmware nRF52** e delle revisioni elettroniche **PCB**.

---

## 🖥️ Software PyCanGui

### Versione 1.1.0 (09 Novembre 2026)
*Aggiornamento incentrato sull'esperienza utente su valvole a solenoide singolo, sull'esplorazione del dizionario e sulla robustezza dei parametri.*

- **Rilevamento Automatico Mono / Bi-Valvola:**
  - Analisi delle capacità del dispositivo alla connessione (`sysconfig_ev_number`).
  - Se il dispositivo collegato ha una sola valvola attiva (es. configurazione CVP5C mono-canale EV1), tutti i controlli, le letture e gli allarmi relativi a EV2 vengono impostati automaticamente sullo stato non disponibile (**`N.A.`**).
  - Il selettore canale valvola si blocca su EV1 prevenendo comandi errati dell'operatore.
- **Grafico di Risposta Adattivo (TrendPlot):**
  - La traccia del canale EV2 viene nascosta automaticamente se la valvola è mono-canale.
  - Il box della legenda riduce le proprie dimensioni grafiche da 62px a 40px per massimizzare l'area visibile del tracciato.
- **Box Guida CANopen Integrato nel Dizionario SDO:**
  - Aggiunto un riquadro di promemoria rapido nella schermata *Dictionary* che riassume al volo i COB-ID base, la mappatura dei byte per PDO analogici, digitali ed errori, e le regole di temporizzazione/heartbeat.
- **Filtro Categorie nel Dizionario Oggetti:**
  - Aggiunto selettore a discesa con raggruppamento per:
    - *All objects* (Vista globale)
    - *Communication* (Oggetti standard 0x1000 - 0x1018)
    - *Manufacturer* (Parametri specifici CVP5 0x2100 - 0x2107)
    - *Device profile* (Parametri profilo DS401)
- **Supporto Node-ID Esteso:**
  - Esteso il range ammesso per l'identificatore CAN di destinazione (`target_can_id`) da `(1, 127)` a `(0, 127)`.
- **Ripristino Impostazioni di Fabbrica Potenziato:**
  - Il ripristino dei parametri di default (`Restore Factory Defaults`) ora preserva esplicitamente il Node-ID corrente della valvola, azzera la mappa di calibrazione non lineare EV1 e reimposta la frequenza di dither standard a 124 Hz.
- **Nuovo Pacchetto Installer Windows:**
  - Compilato l'installer `GE_PyCAN_GUI_Setup_v1.1.0.exe` (179 MB) con driver PEAK e script Inno Setup aggiornato.

---

### Versione 1.0.0 (03 Luglio 2026)
*Release milestone: stabilità del driver PCAN, ottimizzazione del carico CPU e packaging professionale.*

- **Rilevamento Unplug Adattatore PCAN da Thread RX:**
  - Aggiunto il controllo dello stato di disconnessione hardware direttamente dal loop di ricezione a bassa latenza, evitando deadlock o attese indefinite in caso di disconnessione del cavo USB.
- **Eliminazione Polling PCAN dal Thread Grafico (UI):**
  - Rimosso lo scan periodico degli adattatori dal thread principale di Qt, azzerando i micro-blocchi e garantendo una reattività perfetta della finestra anche a bus saturo.
- **Ottimizzazione Pipeline di Ricezione e Refresh Grafico:**
  - Riorganizzato il buffer di decodifica dei messaggi CANopen per ridurre l'allocazione di memoria e rendere più fluida la tracciatura delle correnti.
- **Timeout Adattivo per i Test Dinamici (Rise/Fall Time):**
  - Il tempo massimo per i test a gradino viene calcolato dinamicamente:
    $$\text{Timeout} = \frac{|\Delta I|}{\text{Rampa (mA/s)}} + \text{Margine di Sicurezza}$$
    Elimina i falsi positivi di "Timeout Fallito" quando si impostano rampe lente o gradini di corrente elevati.
- **Protezione Durante i Test Dinamici:**
  - Blocco dell'avvio dei test dinamici in presenza di errori attivi sulla valvola (ad eccezione del superamento della corrente massima).
  - Arresto immediato dell'acquisizione in corso se sopraggiunge un allarme durante il ciclo di misura.
- **Creazione Installer Ufficiale con Inno Setup:**
  - Integrazione dell'installer PEAK PCAN-Basic all'interno del setup di PyCanGui.
  - Configurazione per richiesta automatica di privilegi di amministratore UAC e creazione collegamenti con icone ad alta risoluzione.

---

### Versione 0.1.0 (26 Giugno 2026)
*Prima versione funzionante della GUI desktop in PySide6.*

- Comunicazione di base con adattatore PCAN-USB su `python-can`.
- Scansione nodi e controlli NMT (Start, Stop, Pre-Operational, Reset).
- Visualizzazione real-time delle misure di corrente EV1 ed EV2.
- Lettura e scrittura SDO di base.
- Prima versione dei test di salita e discesa rapida.

---

## ⚡ Aggiornamenti Dispositivo CVP5

### Revisione 2026
- **Dither a Frequenza Variabile:**
  - Implementazione del parametro SDO `0x2105:5` per consentire la regolazione della frequenza del dither tra 50 Hz e 300 Hz a tempo di esecuzione, adattando la risposta al modello specifico di bobina idraulica.
- **Accuratezza Misura Corrente:**
  - Ottimizzazione del campionamento e filtraggio digitale del segnale di corrente per eliminare la deriva termica e garantire massima linearità.
- **Robustezza Elettromagnetica:**
  - Maggiore immunità a disturbi industriali, transienti ISO 7637 e scariche ESD su linea CAN e alimentazione.
- **Dissipazione Termica Migliorata:**
  - Efficienza energetica superiore e migliore smaltimento del calore dello stadio di potenza a correnti elevate (fino a 2.6A continui).
