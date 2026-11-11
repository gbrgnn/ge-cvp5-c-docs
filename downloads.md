# Download Center & Risorse

<span class="badge badge-green">Ultima Release: v1.1.0</span>
<span class="badge badge-blue">Data Rilascio: 09/11/2026</span>
<span class="badge badge-amber">Pacchetto Distributori: CVP5C Pack</span>

In questa sezione e possibile reperire gli installer ufficiali del software di collaudo, i file di configurazione CANopen per integratori PLC, i manuali e i datasheet tecnici.

---

## Installer Windows GE_PyCAN_GUI

L'eseguibile di installazione per sistemi Windows include l'applicazione completa compilata, le icone di sistema, i collegamenti desktop e il pacchetto driver certificato PEAK PCAN-Basic.

<div class="callout success">
  <div class="callout-title">Versione Raccomandata: v1.1.0</div>
  Include la gestione per valvole singole (EV1 only) con indicazione "N.A." su EV2, il box guida CANopen integrato nella schermata dizionario, il filtro categorie e l'ottimizzazione dell'unplug detection.
</div>

| Risorsa | Versione | Dimensione | Piattaforma | Link Download |
| :--- | :---: | :---: | :---: | :--- |
| **PyCanGui Setup Installer** | `v1.1.0` | ~179 MB | Windows 10/11 (64-bit) | [Scarica da GitHub Releases](https://github.com/gbrgnn/GE_PyCAN_GUI/releases/latest ':target=_blank') |
| **PyCanGui Setup Installer (Precedente)** | `v1.0.0` | ~175 MB | Windows 10/11 (64-bit) | [Scarica v1.0.0 (Archivio)](https://github.com/gbrgnn/GE_PyCAN_GUI/releases/tag/v1.0.0 ':target=_blank') |

<div class="callout warning">
  <div class="callout-title">Nota per il Download dell'Eseguibile</div>
  A causa delle dimensioni del pacchetto completo (~179 MB, comprensivo di interprete Python embedded, librerie grafiche PySide6/Qt e driver PEAK), il file binario di installazione viene distribuito tramite la sezione <strong>Releases di GitHub</strong> per garantire velocita di download via CDN globale.
</div>

---

## File Dizionario Oggetti CANopen (EDS / XDD / XML)

File necessari per importare il profilo della centralina CVP5 all'interno dei software di configurazione master CANopen (Codesys, TwinCAT, CANoe, Peak PCAN-Explorer, openCONFIGURATOR).

| File | Formato | Descrizione | Download Diretto |
| :--- | :---: | :--- | :---: |
| `CVP5C_CANopen_ObjectDictionary.eds` | **EDS** (CiA 306) | Electronic Data Sheet standard per PLC industriali | [Scarica .eds](assets/downloads/canopen/CVP5C_CANopen_ObjectDictionary.eds ':target=_blank') |
| `CVP5C_CANopen_ObjectDictionary.xdd` | **XDD** (XML Device Description) | Descrizione dispositivo basata su standard XML | [Scarica .xdd](assets/downloads/canopen/CVP5C_CANopen_ObjectDictionary.xdd ':target=_blank') |
| `CVP5C_CANopen_ObjectDictionary.xml` | **XML** | Progetto sorgente nativo CANopenNode / CANopenEditor | [Scarica .xml](assets/downloads/canopen/CVP5C_CANopen_ObjectDictionary.xml ':target=_blank') |
| `CVP5C_CANopen_Integration_Manual.html` | **HTML** | Manuale di integrazione autonomo per distributori e terze parti | [Apri Manuale](distributor-manual.html ':target=_blank') |

---

## Datasheet Ufficiali Hardware

Specifiche costruttive, dimensioni d'ingombro, curve caratteristiche e schemi di collegamento:

| Documento | Modello | Descrizione | Download |
| :--- | :---: | :--- | :---: |
| **Datasheet CVP5-A-H** | CVP5-A-H | Connettore Hirschmann DIN EN 175301-803, 1 canale EV1 | [Scarica PDF](assets/downloads/datasheets/Data-sheet-CVP5-A-H_rv1_eng.pdf ':target=_blank') |
| **Datasheet CVP5-A-M12** | CVP5-A-M12 | Connettore M12 circolare a 5 poli, 1 canale EV1 | [Scarica PDF](assets/downloads/datasheets/Data-sheet-CVP5-A-M12_rv1_eng.pdf ':target=_blank') |
| **Datasheet CVP5-M12** | CVP5-M12 | Versione connettore M12 standard | [Scarica PDF](assets/downloads/datasheets/Data-sheet-CVP5-M12_eng.pdf ':target=_blank') |
| **Datasheet CVP5-H (Doppia)**| CVP5-H | Versione per comando 2 solenoidi proporzionali (EV1 + EV2) | [Scarica PDF](assets/downloads/datasheets/Data-sheet-CVP5-H_eng.pdf ':target=_blank') |
