# Segnalazione Problemi Ricevimenti — Barilla / Futura

Web-app mobile per documentare e segnalare i problemi riscontrati in **receiving**.
File unico HTML, senza installazione: si apre da browser tramite QR code affisso in banchina.

---

## Funzioni

- Avvio tramite **QR code** posto in postazione (la scansione apre l'app).
- Flusso guidato a 5 step: foto etichette → foto colli → descrizione → preparazione → conferma e invio.
- **Dettatura vocale** della descrizione (italiano, dove supportata dal browser).
- **Numero progressivo** automatico per ogni segnalazione: `RCV-AAAAMMGG-HHMMSS`.
- **Email precompilata** verso i destinatari Barilla (TO) e Futura (CC).
- **Registro** locale delle segnalazioni sul dispositivo, con **export CSV**.
- Generatore integrato del **QR di avvio** (da stampare e affiggere).

---

## Flusso operativo

1. L'operatore scansiona il QR in banchina → si apre l'app (già assegnato un numero progressivo).
2. **Step 1** — Foto delle etichette (SSCC, lotto, articolo, DDT).
3. **Step 2** — Foto del collo o dei colli e del difetto.
4. **Step 3** — Tipo di problema, gravità, descrizione (testo o vocale).
5. **Step 4** — Dati opzionali (fornitore, DDT/ordine, riferimenti, operatore) e riepilogo.
6. **Step 5** — Conferma: si apre l'email precompilata; le foto si scaricano e si allegano nel client di posta prima dell'invio.

---

## Pubblicazione (GitHub Pages)

1. Creare un repository (es. `receiving`).
2. Caricare il file rinominato in `index.html`.
3. **Settings → Pages → Deploy from a branch**: branch `main`, cartella `/ (root)`, **Save**.
4. Attendere ~1 minuto; l'URL sarà del tipo `https://UTENTE.github.io/receiving/`.
5. Aprire l'URL dal telefono (HTTPS) e concedere l'accesso a fotocamera e microfono.

**Requisito:** l'app va servita in **HTTPS** perché fotocamera, dettatura e download funzionino.
La cartella SharePoint/OneDrive Business non esegue l'HTML (lo scarica o ne mostra l'anteprima): usarla solo come archivio, non come host di avvio.

---

## Generazione del QR di avvio

Nell'app: scheda **QR avvio** → inserire l'URL di pubblicazione → **Genera QR** → **Stampa**.
Affiggere il QR in postazione di ricevimento.

---

## Configurazione destinatari

Gli indirizzi sono definiti all'inizio del blocco `<script>` del file:

```js
var TO = ["Maurizio.Giugovaz@barilla.com","caterina.iannuzziello@barilla.com","cristian.quarantelli@barilla.com"];
var CC = ["andrea.bruniera@futurasl.com"];
```

Modificare gli array per cambiare i destinatari. È anche possibile adeguare l'elenco `PROBLEM_TYPES` (tipologie di problema).

---

## Registro e CSV

- Ogni segnalazione confermata viene salvata **localmente** sul dispositivo (browser).
- Export dal tab **Registro → Esporta CSV**; il nome file include il timestamp: `registro_segnalazioni MMGGAAAA HHMMSS.csv`.
- Il registro è per dispositivo/browser: non è condiviso tra utenti.

---

## Note e limiti

- **Allegati:** il protocollo `mailto` non allega le foto in automatico. Usare **“Scarica foto”** e allegarle manualmente prima dell'invio.
- **Repository pubblico:** in un repo pubblico gli indirizzi email nel codice sorgente sono leggibili pubblicamente. Se non desiderato, usare un host con accesso protetto o un nome repo non intuitivo.
- **Compatibilità:** la dettatura vocale richiede un browser che supporti la Web Speech API (es. Chrome su Android). Dove non supportata, resta disponibile l'inserimento testuale.
- **Dati locali:** cancellando i dati del browser si svuota il registro sul dispositivo.

---

## File

- `index.html` — applicazione completa (interfaccia + logica, file unico).
