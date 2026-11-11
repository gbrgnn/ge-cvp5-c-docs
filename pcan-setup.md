# Configurazione Interfaccia PEAK PCAN-USB

<span class="badge badge-amber">Hardware: PEAK System PCAN-USB</span>
<span class="badge badge-blue">Driver: PCAN-Basic API</span>
<span class="badge badge-green">Default Bitrate: 250 kbit/s</span>

Questa guida illustra la corretta installazione e verifica dell'adattatore **PEAK PCAN-USB** per l'interfacciamento tra il PC Windows e la rete CANopen della centralina CVP5.

---

## Installazione Driver

L'installer ufficiale di **PyCanGui** include gia al proprio interno il pacchetto driver certificato PEAK System.
Qualora sia necessaria un'installazione manuale:

1. Scaricare il pacchetto driver dal sito ufficiale [PEAK-System](https://www.peak-system.com/) o dall'archivio fornito.
2. Eseguire il file di setup con privilegi di amministratore.
3. Selezionare l'installazione del driver di dispositivo e delle librerie **PCAN-Basic API** (DLL 32 e 64 bit).
4. Al termine, collegare l'adattatore PCAN-USB al PC: il LED di stato dell'adattatore lampeggera lentamente.

---

## Verifica con PCAN-View

**PCAN-View** e l'utility diagnostica fornita da PEAK per verificare il traffico CAN a basso livello.

1. Avviare **PCAN-View**.
2. Nella finestra di connessione:
   - Selezionare l'interfaccia: `PCAN-USB (Channel 1)`
   - Impostare il **Baud rate:** `250 kBit/s`
   - Cliccare su **OK**.
3. Alimentare la centralina CVP5 a 24 Vdc.
4. Dovrebbe comparire immediatamente il messaggio di **Boot-up/Heartbeat** della valvola:
   ```
   CAN-ID: 70Ah    DLC: 1    Data: 00  (o 7F in pre-operativo)
   ```

---

## Risoluzione Problemi di Comunicazione

### Errore "Bus-Heavy" o "Bus-Off" (LED Rosso Fisso)
- **Causa:** Il baud rate impostato sull'adattatore e differente da quello programmato nella centralina, oppure mancano i resistori di terminazione.
- **Soluzione:** Verificare che entrambi i dispositivi siano a 250 kbit/s e che tra CAN_H e CAN_L sia presente una resistenza di circa 60&nbsp;&Omega; (data da due terminatori da 120&nbsp;&Omega; in parallelo a bus spento).

### Nessun messaggio ricevuto all'accensione della valvola
- Verificare che il pin di massa (GND) dell'alimentatore sia in comune o collegato al riferimento dell'impianto.
- Verificare l'inversione delle linee CAN_H e CAN_L sui pin 4 e 5 del connettore M12.
- Verificare la corretta alimentazione a monte (tensione compresa tra 12 e 30 Vdc).
