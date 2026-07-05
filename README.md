# Bainzuretta Lite

Versione essenziale di [Bainzuretta](https://github.com/AlleRock/bainzuretta), pensata per una consultazione rapida delle attività su iPhone: due sole schermate, nessuno scroll fuori posto, dati sintetici per attività.

## Cosa fa

- **HOME**: login/sincronizzazione con Strava, identica a Bainzuretta.
- **DIARIO**: seleziona l'anno e vedi tutte le attività da gennaio a dicembre, raggruppate per mese, con:
  - km percorsi
  - tempo impiegato
  - dislivello positivo
  - cadenza media
  - battito medio
  - calorie (stima da kilojoules Strava)

Nessuna feature di tracking/obiettivi: quelle restano su Bainzuretta. Lite è solo una vista veloce.

## File

| File | Scopo |
|---|---|
| `index.html` | App vera e propria (single-file, HTML+CSS+JS) |
| `manifest.json` | Manifest PWA (icona, nome, colori) |
| `sw.js` | Service worker per installazione/cache offline |

## Deploy su GitHub Pages

1. Carica i tre file nella root del repo `bainzuretta_lite` (branch `main`).
2. Attiva GitHub Pages su quel branch, root folder.
3. L'app sarà raggiungibile su `https://<tuo-utente>.github.io/bainzuretta_lite/`.

## Configurazione Strava

L'app usa lo stesso **Client ID** di Bainzuretta (`210700`) e — sullo stesso dominio `github.io` — **condivide il token di login** salvato in `localStorage` (`bainzuretta_strava`). Se sei già connesso su Bainzuretta, Lite lo riconosce da solo, senza bisogno di rifare il login.

Se cambi dominio o percorso di pubblicazione, aggiorna in `index.html`:

```js
const STRAVA_REDIRECT_URI = 'https://allerock.github.io/bainzuretta_lite/';
```

e verifica che il dominio sia tra quelli autorizzati nelle impostazioni dell'app Strava (strava.com/settings/api).

## Icona

`icona.png` deve essere presente nella root del repo. Il riferimento include `?v=1` per forzare Safari a non usare una vecchia icona in cache: se la sostituisci in futuro, incrementa il numero di versione in `index.html`, `manifest.json` e `sw.js`.

## Note sui dati

- I dati vengono salvati in `localStorage` sotto la chiave `bainzuretta_lite_raw`, separata da quella di Bainzuretta.
- Cadenza e dislivello sono sempre presenti (già raccolti anche da Bainzuretta). **Battito e calorie** vengono valorizzati solo per le attività sincronizzate da Lite: se un'attività è stata importata solo tramite Bainzuretta, questi due campi mostreranno `-` finché non viene risincronizzata da qui.
- Nessun dato viene inviato a server esterni: tutto resta sul dispositivo, salvo le chiamate dirette alle API di Strava.
