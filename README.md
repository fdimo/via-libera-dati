# Via Libera — dati di zona

Questo archivio contiene i dati che l'app **Via Libera** scarica ogni giorno:
per ogni zona, le stazioni ferroviarie, i passaggi a livello con la loro
posizione lungo la linea, e l'orario dei treni del giorno.

I file stanno in `aree/` (`aree/mantova.json`, …) e vengono rigenerati ogni notte
da un programma automatico. Qui non c'è codice dell'applicazione: solo dati.

## Com'è fatto un file

```jsonc
{
  "slug": "mantova",
  "day": "2026-09-24",        // giorno a cui si riferisce l'orario
  "weekday": 3,               // 0 = lunedì
  "stations": [{ "code": "S02336", "name": "Mantova", "lat": …, "lon": … }],
  "crossings": [{ "id": "pl-viale-oslavia", "name": "Viale Oslavia",
                  "lat": …, "lon": …, "barrier": "full", "covered": true }],
  "segments": { "S02336>S02337": { "m": 7904.6, "x": [[12, 1520.3]] } },
  //            metri di binario fra le due stazioni, e a quanti metri
  //            dalla prima si trova ciascun passaggio a livello
  "trains": [{ "n": 16955, "cat": "REG", "orig": "Mantova", "dest": "Venezia",
               "stops": [{ "s": "S02336", "a": "05:30", "d": "05:34" }] }],
  "model": { "lead_close_s": 150, "lead_open_s": 40, "accel": 0.5, "vmax_ms": 44.44 },
  "fonti": { … }
}
```

`covered: false` vuol dire che il passaggio a livello è noto ma non se ne può
prevedere la chiusura, perché nessuna tratta con orario gli passa sopra. L'app lo
mostra senza stato, invece di inventare un orario.

Il modello di previsione (150 secondi di preavviso alla chiusura, 40 alla
riapertura) usa i valori dichiarati da RFI; `vmax_ms` è un limite di sicurezza
del calcolo, non una velocità reale.

## Fonti e licenze

- **Geometria della rete e passaggi a livello**: [OpenStreetMap](https://www.openstreetmap.org/copyright),
  © i contributori di OpenStreetMap, licenza **ODbL 1.0**. I dati derivati in
  questo archivio sono a loro volta distribuiti sotto **ODbL 1.0**.
- **Orario ferroviario**: [Regione Lombardia — open data](https://www.dati.lombardia.it/),
  dataset "Orario Ferroviario Regionale GTFS" di **Trenord**, licenza **CC-BY**.
- **Treni in circolazione e orari del giorno**: **ViaggiaTreno** (RFI), servizio
  pubblico consultato con richieste limitate e con i risultati messi in cache.

Chi riusa questi file è tenuto alle stesse condizioni: citare OpenStreetMap e
Trenord, e distribuire eventuali derivati sotto ODbL.

## Avvertenza

Le previsioni sono calcolate, non misurate: nessuno dei gestori ferroviari
pubblica lo stato reale delle barriere. Servono a scegliere una strada, non a
decidere se attraversare. Ai passaggi a livello vale sempre e solo quello che
dicono le luci e le barriere.
