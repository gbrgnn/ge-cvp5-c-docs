# Repository & Istruzioni per la Pubblicazione GitHub Pages

<span class="badge badge-blue">Hosting: GitHub Pages</span>
<span class="badge badge-green">CI/CD: GitHub Actions Ready</span>
<span class="badge badge-amber">Motore: Docsify (Zero Build)</span>

In questa pagina sono raccolti i collegamenti a tutti i repository dell'ecosistema Galileo Engineering e le istruzioni dettagliate per pubblicare questo portale di documentazione online su **GitHub Pages**.

---

## 📂 Mappa dei Repository di Progetto

| Componente | Riferimento | Descrizione |
| :--- | :--- | :--- |
| **PyCanGui Desktop App** | [`https://github.com/gbrgnn/GE_PyCAN_GUI`](https://github.com/gbrgnn/GE_PyCAN_GUI) | Repository applicazione desktop PySide6 con releases e installer |
| **Portale Documentazione (Questo)** | `ge-cvp5-c-docs` | Sito statico Docsify per GitHub Pages con manuali, specifiche e download |

---

## 🚀 Come Pubblicare Questo Portale su GitHub Pages

La cartella `ge-cvp5-c-docs` è un repository Git completo e autonomo, dotato di configurazione `.nojekyll` e workflow automatico GitHub Actions `.github/workflows/deploy.yml`.

Seguire questi semplici passaggi per renderlo pubblico su Internet:

### Passo 1: Crea un nuovo repository su GitHub
1. Accedere al proprio account GitHub (es. `https://github.com/gbrgnn` o organizzazione `GalileoEng1`).
2. Creare un nuovo repository pubblico denominato, ad esempio:
   ```
   ge-cvp5-c-docs
   ```
   *(lasciare vuoto: non aggiungere README, .gitignore o licenza iniziale).*

### Passo 2: Collega il repository locale e fai il Push
Aprire PowerShell nella cartella `ge-cvp5-c-docs` ed eseguire:

```powershell
# Entra nella cartella della documentazione
cd "c:\Drive\Lavoro\GalileoEngineering\ELETTROVALVOLA CAN\ge-cvp5-c-docs"

# Collega il remote GitHub (sostituire con il tuo URL se diverso)
git remote add origin https://github.com/gbrgnn/ge-cvp5-c-docs.git

# Rinomina il branch in main (se necessario) e invia i file
git branch -M main
git push -u origin main
```

### Passo 3: Abilita GitHub Pages nelle impostazioni
1. Nel repository GitHub appena creato, andare su **Settings** (in alto a destra).
2. Nel menu laterale sinistro, cliccare su **Pages**.
3. Sotto **Build and deployment**:
   - **Opzione A (Consigliata via Actions):** In *Source*, selezionare **GitHub Actions**. Il workflow `.github/workflows/deploy.yml` incluso eseguirà il deploy in automatico a ogni push!
   - **Opzione B (Deploy diretto da Branch):** In *Source*, selezionare *Deploy from a branch*, impostare Branch: `main` e cartella: `/ (root)`, quindi premere **Save**.
4. Dopo circa 60 secondi, il sito sarà accessibile pubblicamente all'indirizzo:
   ```
   https://<tuo-utente>.github.io/ge-cvp5-c-docs/
   ```

---

## 💻 Come Testare il Portale in Locale (Anteprima)

È possibile visualizzare e navigare il sito sul proprio PC prima di effettuare il push online, senza installare strumenti complessi:

### Metodo 1: Tramite Python (Già disponibile sul sistema)
```powershell
cd "c:\Drive\Lavoro\GalileoEngineering\ELETTROVALVOLA CAN\ge-cvp5-c-docs"
python -m http.server 3000
```
Aprire il browser su: [http://localhost:3000](http://localhost:3000).

### Metodo 2: Tramite Docsify CLI (Opzionale)
```powershell
npx docsify-cli serve .
```
Aprire il browser su: [http://localhost:3000](http://localhost:3000).
