# Orderflow BTC Live

Dashboard orderflow in un singolo file HTML, senza dipendenze né build:
apri `index.html` nel browser e si collega alle API pubbliche di Binance
(REST + WebSocket, nessuna chiave richiesta).

## Funzioni

- **Candele** con volume profile laterale (split buy/sell), POC / VAH / VAL,
  value area, VWAP giornaliero, PDH / PDL / PDC e open giornaliero.
- **Footprint** stile orderflow: celle sell×buy per livello di prezzo,
  POC per barra evidenziato, imbalance (≥3×) in risalto, raggruppamento
  tick configurabile.
- **Riga statistiche** per barra: Volume, Delta, Delta Max, Delta Min e
  CVD cumulativo, con celle heat-map (volumi in valuta quote, es. USDT).
- **Live**: trade aggregati via WebSocket aggiornano footprint, delta e
  CVD in tempo reale; il footprint storico è ricostruito da kline a
  granularità fine (1s/1m/5m a seconda del timeframe).
- Timeframe 1m–1D, crosshair con OHLC/Δ/CVD, zoom (rotella/pinch),
  pan (trascina), doppio clic per reset, pulsante «⇥ Live» per riagganciare
  l'ultimo prezzo.
- **Open Interest futures** (Binance USD-M): storico per barra + polling
  live ogni 30 s, riga **ΔOI** nel footer e valore assoluto nell'infobar.
  La riga compare solo se la coppia ha un mercato futures raggiungibile.
- **Alert** (campanella 🔔 nella toolbar, da attivare con un tocco):
  - **Delta**: scatta quando il |delta| della barra live supera 2×/3×/5×
    la media delle ultime 50 barre;
  - **Imbalance**: scatta su ≥3 (o ≥4) livelli consecutivi con un lato
    ≥3× l'altro nella barra live.
  Ogni alert mostra un toast, suona un beep (tono alto = buy, basso =
  sell), vibra dove supportato e invia una notifica browser se autorizzata.
  Un alert per tipo per barra.

## Parametri URL

| Parametro | Esempio | Note |
|---|---|---|
| `symbol` | `?symbol=ETHUSDT` | qualunque coppia spot Binance |
| `tf` | `?tf=1h` | `1m 5m 15m 1h 4h 1d` |
| `mode` | `?mode=candles` | default `footprint` |
| `demo` | `?demo=1` | dati sintetici, nessuna rete |

Se Binance non è raggiungibile la pagina passa da sola in modalità demo
e lo segnala nella barra in basso.

## Limiti

- Il footprint storico è sintetizzato da kline fini (un campione per
  sotto-candela a prezzo hlc3): fedele al raggruppamento tick tipico
  (≥ $10 su BTC), non è un footprint tick-by-tick. La barra live invece
  è costruita dai trade reali (aggTrade).
- Delta e volumi storici derivano dal taker buy volume delle kline Binance.
- Nessun consiglio finanziario: è uno strumento di visualizzazione.

## Apri la dashboard

- **GitHub Pages**: https://valentino500x.github.io/orderflow-dashboard/
- Mirror immediato: https://raw.githack.com/valentino500x/orderflow-dashboard/main/index.html
