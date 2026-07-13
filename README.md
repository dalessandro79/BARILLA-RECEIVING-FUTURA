# Segnalazione Problemi Ricevimenti — Barilla / Futura

Web-app mobile per documentare e segnalare i problemi riscontrati in **receiving**.
File unico HTML (`index.html`), senza installazione: si apre dal browser tramite QR affisso in banchina.

---

## Accesso

L'app è riservata agli utenti **@futurasl.com**. All'avvio viene chiesto l'indirizzo email aziendale: si procede solo se termina con `@futurasl.com`. L'email viene registrata come **operatore** nel report e nel registro, e l'invio è bloccato senza accesso valido. L'accesso resta memorizzato sul dispositivo (link "cambia" nell'intestazione per cambiarlo).

> Nota: è un controllo lato client, utile a evitare invii da account non autorizzati per l'uso normale, ma non è un blocco a prova di manomissione. Per un blocco reale serve un livello server (Cloudflare Access o login Microsoft 365).

---

## Flusso operativo (5 step)

1. **Delivery Note + etichette** — foto del **Delivery Note (DDT)** *obbligatoria*; in più le foto delle etichette dei colli (SSCC, lotto, articolo).
2. **Foto collo / colli** — foto dei colli interessati e del difetto.
3. **Descrizione problema** — tipo, gravità, descrizione (testo o **dettatura vocale** in italiano).
4. **Preparazione** — DDT e SSCC letti automaticamente dalle foto; **scanner barcode SSCC**; **Vettore** *obbligatorio* (con lettura da bolla via OCR); fornitore, operatore; riepilogo.
5. **Conferma e invio** — il documento Word con foto e dati viene preparato in anticipo; "Invia" apre la condivisione con l'allegato già inserito.

---

## Funzioni principali

- **Numero progressivo** automatico per ogni segnalazione: `RCV-AAAAMMGG-HHMMSS`.
- **Foto Delivery Note obbligatoria** (blocca l'avanzamento se mancante).
- **Scanner barcode SSCC (GS1-128):** legge il codice a barre dell'etichetta e restituisce le 18 cifre esatte, senza errori di OCR.
- **OCR automatico** su Delivery Note ed etichette per precompilare **DDT**, **SSCC** e **Vettore** (valori sempre verificabili e modificabili).
- **Vettore obbligatorio:** l'invio è bloccato se il campo non è compilato.
- **Dettatura vocale** della descrizione (dove supportata dal browser).
- **Allegato Word automatico:** genera un `.docx` con report + foto (compresse) e lo allega all'email tramite la condivisione nativa del telefono.
- **Registro** locale delle segnalazioni con **export CSV**.
- **Generatore QR** di avvio postazione (da stampare e affiggere).

---

## Destinatari email

Definiti all'inizio del blocco `<script>` del file:

```js
var TO = ["Maurizio.Giugovaz@barilla.com","caterina.iannuzziello@barilla.com","cristian.quarantelli@barilla.com"];
var CC = ["andrea.bruniera@futurasl.com","matteo.fiori@futurasl.com"];
```

> In fase di test la lista `TO` è temporaneamente sostituita da un indirizzo interno (vedi commento nel codice): **ripristinare i destinatari Barilla prima della produzione**.

Per cambiare i destinatari modificare gli array `TO` e `CC`. È anche possibile adeguare l'elenco `PROBLEM_TYPES` (tipologie di problema) e la lista delle parole chiave usate per riconoscere il Vettore in bolla.

---

## Pubblicazione (GitHub Pages)

1. Repository: **BARILLA-RECEIVING-FUTURA**, file pubblicato come **`index.html`**.
2. **Settings → Pages → Deploy from a branch**: branch `main`, cartella `/ (root)`.
3. URL pubblico: `https://dalessandro79.github.io/BARILLA-RECEIVING-FUTURA/`.
4. Per aggiornare l'app: caricare il nuovo `index.html` (**Add file → Upload files**, sovrascrivi, **Commit**). La pubblicazione si aggiorna in ~1 minuto; sul telefono può servire un refresh forzato.

**Requisito:** l'app va servita in **HTTPS** (fotocamera, scanner, dettatura e download lo richiedono). SharePoint/OneDrive Business non esegue l'HTML: usarli solo come archivio, non come host di avvio.

---

## Requisiti d'uso (mobile)

- **Browser consigliato: Chrome** su Android. DuckDuckGo e alcuni browser interni (scanner QR, Outlook, SharePoint) **non supportano l'allegato automatico** (condivisione file): in quel caso l'app scarica il Word e chiede di allegarlo a mano.
- Al primo utilizzo, **scanner barcode** e **OCR** scaricano le librerie dal web: serve connessione. L'app resta usabile anche se non riescono (inserimento manuale).
- Concedere i permessi **fotocamera** (foto/scanner) e **microfono** (dettatura).

---

## Generazione del QR di avvio

Nell'app: scheda **QR avvio** → inserire l'URL di pubblicazione → **Genera QR** → **Stampa**. Affiggere il QR in postazione di ricevimento.

---

## Registro e CSV

- Ogni segnalazione confermata viene salvata **localmente** sul dispositivo (browser).
- Export dal tab **Registro → Esporta CSV**; il nome file include il timestamp: `registro_segnalazioni MMGGAAAA HHMMSS.csv`.
- Il registro è per dispositivo/browser: non è condiviso tra utenti. Cancellando i dati del browser si svuota il registro.

---

## Limiti noti

- **Destinatari nell'invio via condivisione:** l'allegato parte automaticamente, ma alcune app di posta non precompilano TO/CC dalla condivisione nativa (gli indirizzi sono comunque riportati nel corpo). Per un invio completamente automatico (destinatari + allegato + invio) serve un livello server (es. Power Automate / Microsoft 365).
- **OCR del Vettore:** dipende da come è scritta la bolla; verificare sempre il valore.
- **Controllo accessi:** lato client, non a prova di manomissione (vedi sezione Accesso).

---

## File

- `index.html` — applicazione completa (interfaccia + logica, file unico).
