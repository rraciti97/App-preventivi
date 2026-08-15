# Preventivi Gasbeton

Web app installabile per calcolare preventivi di blocchi in calcestruzzo aerato autoclavato (AAC) in cantiere, e generare il PDF da consegnare al cliente.

Nasce da un problema concreto: un venditore di materiali edili faceva i preventivi su un foglio di calcolo, dal telefono, davanti al cliente. Funzionava, ma zoomare su celle da 4 mm non è un modo di lavorare, e il foglio non produce niente di presentabile da lasciare a chi compra. L'app conserva lo stesso motore di calcolo — verificato riga per riga contro il foglio originale — e ne cambia solo l'involucro.

## Funzioni

- Selezione dello spessore da una fila di blocchi con larghezza proporzionale a quella reale
- Quantità in bancali, metri quadri o pezzi, con conversione automatica
- Due linee di prodotto con sconto, IVA e regimi di trasporto indipendenti
- Calcolo del collante: chilogrammi, sacchi e bancali sulle quantità del preventivo
- Export PDF con logo, scomposizione del prezzo unitario, totali con IVA e bancali fuori totale
- Funziona offline; i dati restano sul dispositivo, con export e import JSON

## Modello di calcolo

```
netto        = listino × (1 − sconto)
trasporto/mq = (costo camion ÷ bancali per camion) ÷ mq per bancale
€/mq         = netto + trasporto
€/pezzo      = €/mq × mq per bancale ÷ pezzi per bancale
```

I bancali restano fuori dal prezzo al metro quadro e si conteggiano a parte. Il collante segue una catena parallela guidata dai metri quadri, con consumo al mq che dipende dallo spessore.

## Scelte tecniche

**PWA anziché app nativa.** SwiftUI avrebbe richiesto un Mac per compilare e, senza App Store, una reinstallazione ogni sette giorni. Per un form con calcoli e un export PDF, una web app installabile dà lo stesso risultato senza scadenze e senza intermediari.

**Un solo file, nessun build step.** Niente da compilare, nessuna dipendenza da aggiornare.

**Nessun backend.** I dati vivono in `localStorage`: listini e margini non attraversano la rete. Un service worker network-first con fallback su cache tiene l'app utilizzabile dove il segnale manca.

Stack: HTML, CSS e JavaScript vanilla. [jsPDF](https://github.com/parallax/jsPDF) da CDN per il PDF.

## Installazione

Qualsiasi hosting statico. Con GitHub Pages: `Settings → Pages → Deploy from a branch → main / (root)`. Poi da Safari: `Condividi → Aggiungi alla schermata Home`.

Dopo ogni modifica, incrementa la costante `CACHE` in `sw.js`, altrimenti il service worker continua a servire la versione in cache.

## Configurazione

I prezzi di listino inclusi sono quelli pubblicati dal produttore. **Sconti e costi di trasporto sono a zero di proposito**: sono condizioni commerciali che variano da rivenditore a rivenditore. Si impostano una volta dall'app in `Listini`, oppure si importa un backup JSON già compilato da `Azienda → Backup`.

Da verificare col proprio fornitore: sacchi per bancale di collante e prezzo al sacco.

## Licenza

[Apache License 2.0](LICENSE).

---

Strumento non ufficiale, sviluppato in modo indipendente e non affiliato ad alcun produttore. "Gasbeton" è un marchio registrato dei rispettivi titolari, usato a fini descrittivi. I prezzi inclusi hanno valore di esempio: verificare sempre i listini ufficiali in vigore.
