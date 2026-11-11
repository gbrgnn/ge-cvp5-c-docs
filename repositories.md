# Repository & Pubblicazione GitHub Pages

<span class="badge badge-blue">Hosting: GitHub Pages</span>
<span class="badge badge-green">CI/CD: GitHub Actions Ready</span>
<span class="badge badge-amber">Motore: Docsify</span>

In questa pagina sono descritti i riferimenti di progetto e le opzioni per la gestione e configurazione del portale su **GitHub Pages**.

---

## Mappa dei Repository

| Componente | Riferimento | Descrizione |
| :--- | :--- | :--- |
| **PyCanGui Desktop App** | [`https://github.com/gbrgnn/GE_PyCAN_GUI`](https://github.com/gbrgnn/GE_PyCAN_GUI) | Repository applicazione desktop PySide6 con releases e installer |
| **Portale Documentazione (Questo)** | `ge-cvp5-c-docs` | Sito statico Docsify per GitHub Pages con manuali, specifiche e download |

---

## Opzioni per Modificare l'Indirizzo Web (URL)

L'indirizzo predefinito attuale assegnato da GitHub e:
`https://gbrgnn.github.io/ge-cvp5-c-docs/`

E possibile modificare l'URL in tre modi diversi:

### 1. Ridenominazione del Repository
Se si desidera un percorso differente sotto il proprio profilo (ad esempio `https://gbrgnn.github.io/cvp5-docs/` oppure `https://gbrgnn.github.io/cvp5/`):
- Andare su GitHub nel repository &rarr; **Settings** &rarr; **General**.
- Modificare il nome del repository nel campo **Repository name** e premere **Rename**.
- L'URL di GitHub Pages si aggiornera automaticamente.

### 2. Trasferimento all'Organizzazione Aziendale
Se si desidera che il sito risieda sotto l'account aziendale **GalileoEng1**:
- Andare su GitHub in **Settings** &rarr; in fondo alla pagina cliccare su **Transfer ownership**.
- Indicare come nuovo proprietario `GalileoEng1`.
- L'indirizzo diventera: `https://GalileoEng1.github.io/ge-cvp5-c-docs/`.

### 3. Utilizzo di un Dominio Aziendale Personalizzato (Custom Domain)
Se Galileo Engineering dispone di un dominio web (ad esempio `galileoengineering.com` o `.it`), e possibile pubblicare il sito sotto un terzo livello aziendale, ad esempio:
`https://cvp5.galileoengineering.it` oppure `https://docs.galileoengineering.it`
- Configurazione: nel repository GitHub &rarr; **Settings** &rarr; **Pages** &rarr; **Custom domain**, inserire il dominio desiderato e configurare il record CNAME sul proprio provider DNS.
