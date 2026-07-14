# Segnalazione Problemi Ricevimenti — Barilla / Futura

Web-app mobile per documentare e segnalare i problemi riscontrati in **receiving**.
File unico HTML (`index.html`), senza installazione: si apre dal browser tramite QR affisso in banchina.

---

## Accesso e controllo browser

All'avvio l'app esegue due controlli:

1. **Browser compatibile** — verifica che il browser supporti l'invio dell'allegato (Web Share con file). Se non compatibile (es. DuckDuckGo o browser interni a scanner/social/posta), mostra una schermata che invita a **installare/aprire Chrome** (Android) o **Safari** (iPhone). C'è "Continua comunque" per non bloccare in caso di falso allarme.
2. **Accesso @futurasl.com** — chiede l'email aziendale; procede solo se termina con `@futurasl.com`. L'email diventa l'**operatore** nel report e nel registro; l'invio è bloccato senza accesso valido. L'accesso resta memorizzato (link "cambia" nell'intestazione).

> Nota: il controllo accessi è lato client — utile a evitare invii da account non autorizzati per l'uso normale, ma non a prova di manomissione. Per un blocco reale serve un livello server (Cloudflare Access o login Microsoft 365).

---

## Flusso operativo (5 step)

Il flusso rispecchia la realtà: prima le attività in **banchina**, poi il Delivery Note che si trova in **ufficio**.

1. **Foto etichette SSCC** (banchina) — *obbligatoria*; scanner barcode SSCC (GS1-128) o lettura da foto.
2. **Foto pallet / colli** (banchina).
3. **Descrizione problema** — tipo, gravità, testo o **dettatura vocale** (it-IT).
4. **Foto Delivery Note** (ufficio) — *obbligatoria*; l'OCR legge **N. Delivery Note** e **Vettore** dalla foto; se non riconosciuti, inserimento manuale (entrambi *obbligatori*).
5. **Conferma e invio** — documento Word con foto allegato tramite condivisione nativa.

In qualsiasi momento: **💾 Salva e completa dopo** per sospendere e riprendere in seguito (tab Bozze).

---

## Funzioni principali

- **Numero progressivo** automatico: `RCV-AAAAMMGG-HHMMSS`.
- **Foto obbligatorie**: etichette SSCC (step 1) e Delivery Note (step 4).
- **Scanner barcode SSCC (GS1-128):** restituisce le 18 cifre esatte dall'etichetta.
- **OCR** (Tesseract.js): SSCC dalle etichette; **N. Delivery Note** e **Vettore** dalla bolla — valori sempre verificabili e modificabili.
- **Vettore obbligatorio** (con lettura da bolla).
- **Controllo coerenza etichetta ↔ Delivery Note:** confronta i **riferimenti numerici comuni** (N. DDT / codice articolo / numero pallet) tra etichetta e bolla. Se non c'è alcun riferimento comune, avviso allo step 4 e richiesta di conferma prima dell'invio. Se non verificabile (OCR mancante), non blocca.
- **Allegato Word automatico:** `.docx` con report + foto compresse (max 1280px, JPEG q0.6), preparato in anticipo e allegato via condivisione nativa.
- **Salva bozza / completa dopo:** segnalazioni non inviate salvate sul dispositivo (tab **Bozze**), riprendibili senza perdere dati; rimosse automaticamente dopo l'invio.
- **Registro** locale con **export CSV** (nome file con timestamp).
- **Generatore QR** di avvio postazione (da stampare e affiggere).

---

## Destinatari email

Definiti all'inizio del blocco `<script>` del file:

```js
var TO = ["Maurizio.Giugovaz@barilla.com","caterina.iannuzziello@barilla.com","cristian.quarantelli@barilla.com"];
var CC = ["andrea.bruniera@futurasl.com","matteo.fiori@futurasl.com"];
```

> **ATTENZIONE:** attualmente `TO` è in modalità **TEST** (sostituito da un indirizzo interno; vedi commento nel codice). **Ripristinare i destinatari Barilla prima della produzione.**

---

## Pubblicazione (GitHub Pages)

1. Repository **BARILLA-RECEIVING-FUTURA**, file pubblicato come **`index.html`**.
2. **Settings → Pages → Deploy from a branch**: branch `main`, cartella `/ (root)`.
3. URL pubblico: `https://dalessandro79.github.io/BARILLA-RECEIVING-FUTURA/`.
4. Per aggiornare: caricare il nuovo `index.html` (**Add file → Upload files**, sovrascrivi, **Commit**). La pubblicazione si aggiorna in ~1 minuto; sul telefono può servire un refresh forzato.

**Requisito:** l'app va servita in **HTTPS** (fotocamera, scanner, dettatura e download lo richiedono). SharePoint/OneDrive Business non esegue l'HTML: usarli solo come archivio.

---

## Requisiti d'uso (mobile)

- **Browser: Chrome** su Android, **Safari** su iPhone. Browser interni/DuckDuckGo non supportano l'allegato automatico.
- Al primo utilizzo, **scanner barcode** e **OCR** scaricano le librerie dal web: serve connessione. L'app resta usabile anche se non riescono (inserimento manuale).
- Concedere i permessi **fotocamera** e **microfono**.

---

## Registro e bozze

- **Registro**: ogni segnalazione inviata è salvata localmente; export **CSV** (`registro_segnalazioni MMGGAAAA HHMMSS.csv`).
- **Bozze**: segnalazioni non ancora inviate, con foto e dati; per dispositivo/browser.
- Dati per dispositivo, non condivisi tra utenti. Cancellando i dati del browser si svuotano registro e bozze.

---

## Limiti noti

- **Destinatari nell'invio via condivisione:** l'allegato parte, ma alcune app di posta non precompilano TO/CC (indirizzi comunque riportati nel corpo). Per un invio completamente automatico serve un livello server (Power Automate / Microsoft 365 Graph).
- **OCR:** affidabilità dipendente da qualità foto e layout bolla; verificare sempre i valori.
- **Controllo accessi:** lato client, non a prova di manomissione.
- **localStorage:** bozze con molte foto possono saturare lo spazio del browser.

---

## File

- `index.html` — applicazione completa (interfaccia + logica, file unico).
