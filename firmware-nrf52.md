# Funzionalità Centralina & Modalità di Controllo

<span class="badge badge-blue">Controllo: Loop Chiuso di Corrente</span>
<span class="badge badge-green">Frequenza Campionamento: 1 kHz</span>
<span class="badge badge-amber">Dither: Variabile (50-300 Hz)</span>
<span class="badge badge-blue">Protezioni: Sovracorrente & Diagnostica</span>

La centralina elettronica della serie **CVP5** integra un microcontrollore a 32-bit con algoritmo di regolazione digitale ad anello chiuso progettato per il pilotaggio ad alta precisione di solenoidi proporzionali.

---

## 🎯 Regolazione di Corrente ad Anello Chiuso

Il circuito di controllo misura costantemente la corrente reale che attraversa l'avvolgimento della valvola e corregge istantaneamente il duty cycle PWM per mantenere il valore target impostato tramite bus CANopen.

### Vantaggi dell'Anello Chiuso
- **Insensibilità alle variazioni termiche:** Quando la bobina si riscalda durante l'esercizio continuo, la sua resistenza ohmica aumenta. La regolazione in corrente compensa automaticamente questa variazione aumentando la tensione efficace applicata, mantenendo costante la forza idraulica erogata.
- **Insensibilità alle fluttuazioni di alimentazione:** Oscillazioni sulla tensione di linea ($9\text{ V} \dots 32\text{ V}$) non influenzano la portata della valvola.

---

## 🌊 Iniezione Segnale di Dither

Per superare l'attrito meccanico statico (stick-slip) del cursore della valvola proporzionale all'interno del corpo valvola, la centralina sovrappone alla corrente media un segnale di dither a frequenza e ampiezza calibrate:

- **Frequenza di Dither:** Regolabile da **50 Hz a 300 Hz** (valore tipico predefinito: **124 Hz**).
- **Ampiezza di Dither:** Configurabile da **0% a 25%** del fondo scala.
- **Effetto Funzionale:** Riduce drasticamente l'isteresi idraulica (< 1.5%) e aumenta la sensibilità e la ripetibilità di posizionamento della valvola anche con micro-variazioni di comando.

---

## 📈 Linearizzazione della Risposta (Curva a 10 Punti)

La centralina supporta una curva di interpolazione lineare a 10 punti programmabile:
- Permette di eliminare la **zona morta meccanica** iniziale del cassetto valvola (dove la corrente circola ma non c'è ancora passaggio di fluido).
- Corregge la non-linearità della curva portata/corrente, consentendo all'operatore o al PLC di avere una risposta perfettamente proporzionale e intuitiva.

---

## 🛡️ Diagnostica Integrata & Sistemi di Sicurezza

La centralina esegue un controllo continuo dello stato elettrico del carico e dell'alimentazione, segnalando le anomalie sul bus CANopen:

| Condizione | Descrizione | Azione di Sicurezza |
| :--- | :--- | :--- |
| **Sovracorrente ($I > I_{max}$)** | Cortocircuito sul solenoide o guasto cablaggio | Spegnimento immediato dell'uscita e segnalazione errore |
| **Cavo Aperto (Broken Wire)** | Setpoint attivo con assenza di passaggio corrente | Segnalazione stato circuito aperto su TPDO2 |
| **Tensione Linea Anomala** | Tensione inferiore a 9V o superiore a 32V | Blocco pilotaggio per protezione stadi elettronici |
| **Protezione Termica** | Raggiungimento soglia termica interna di allarme | De-rating termico o blocco preventivo |
