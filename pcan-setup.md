# Guida alla Configurazione Adattatore PEAK PCAN-USB

<span class="badge badge-amber">Hardware: PEAK System PCAN-USB</span>
<span class="badge badge-blue">Driver: PCAN-Basic API</span>
<span class="badge badge-green">Default Bitrate: 250 kbit/s</span>

Questa guida illustra la corretta installazione e verifica dell'adattatore **PEAK PCAN-USB** per l'interfacciamento tra il PC Windows e la rete CANopen della centralina CVP5.

---

## 🛠️ Installazione Driver

L'installer ufficiale di **PyCanGui** include già al proprio interno il pacchetto driver certificato PEAK System.
Qualora sia necessaria un'installazione manuale o su un PC dedicato:

1. Scaricare il pacchetto driver dal sito ufficiale [PEAK-System](https://www.peak-system.com/) oppure utilizzare l'archivio presente nella cartella di progetto `PEAK-System_Driver-Setup/`.
2. Eseguire il file di setup con privilegi di amministratore.
3. Selezionare l'installazione del driver di dispositivo e delle librerie **PCAN-Basic API** (DLL a 32 e 64 bit).
4. Al termine dell'installazione, collegare l'adattatore PCAN-USB alla porta USB del computer: il LED di stato dell'adattatore lampeggerà lentamente in rosso o verde.

---

## 🔍 Verifica con PCAN-View

**PCAN-View** è l'utility diagnostica fornita da PEAK per verificare il traffico CAN a basso livello.

1. Avviare **PCAN-View**.
2. Nella finestra di connessione:
   - Selezionare l'interfaccia: `PCAN-USB (Channel 1)`
   - Impostare il **Baud rate:** `250 kBit/s` (oppure `250.000 bit/s`)
   - Cliccare su **OK**.
3. Alimentare la centralina CVP5 a 24 Vdc.
4. Dovrebbe comparire immediatamente il messaggio di **Boot-up/Heartbeat** della valvola:
   ```
   CAN-ID: 70Ah    DLC: 1    Data: 00  (o 7F se in pre-operativo)
   ```

### File di Test Preconfigurato (`GE_Valve_PCAN_ViewTest.xmt`)
Nel progetto è presente un file di configurazione predefinito con i comandi NMT e RPDO preimpostati per collaudo rapido. È possibile caricarlo direttamente da *Transmit Messages* in PCAN-View.

---

## ⚠️ Risoluzione Problemi Comuni di Comunicazione

### 1. Errore "Bus-Heavy" o "Bus-Off" (LED Rosso Fisso)
- **Causa:** Il baud rate impostato sull'adattatore è diverso da quello programmato nella centralina, oppure mancano i resistori di terminazione.
- **Soluzione:** Verificare che entrambi i dispositivi siano a 250 kbit/s e che tra CAN_H e CAN_L sia presente una resistenza di circa 60&nbsp;&Omega; (data da due terminatori da 120&nbsp;&Omega; in parallelo a bus spento).

### 2. Nessun messaggio ricevuto all'accensione della valvola
- Verificare che il pin di massa (GND) dell'alimentatore sia in comune o collegato al riferimento dell'impianto.
- Verificare l'inversione delle linee CAN_H e CAN_L sui pin 4 e 5 del connettore M12.
- Verificare la corretta alimentazione a monte (tensione compresa tra 12 e 30 Vdc).
