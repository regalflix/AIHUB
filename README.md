# AI HUB — Control Room

Un HUB privato che aggrega 12 AI con tier gratuito dietro **un'unica API aperta**, con catena di
fallback automatica e un "cervello" che testa le chiavi ogni giorno, dismette quelle morte e ti
segnala le nuove AI gratuite da registrare.

## Cosa contiene

- **Dashboard** stile mission control: mappa della rete animata, stato live dei provider, feed attività
- **Playground**: chat personale che usa la catena di fallback
- **Cervello**: scansione giornaliera automatica (Netlify Scheduled Function) + report in dashboard
  con i link di registrazione delle AI non ancora configurate
- **API aperta** compatibile OpenAI: `POST /api/chat` (accetta anche `/api/chat/completions`)
- **Chiave master** che protegge API e pannello

## Deploy su Netlify (gratis, ~5 minuti)

1. Crea un repository su GitHub e carica tutto questo progetto
2. Su [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project** →
   scegli il repo
3. Netlify legge da solo `netlify.toml` (build `npm run build`, publish `dist`, functions in
   `netlify/functions`) — non serve configurare niente. Clicca **Deploy**
4. Apri il sito, vai su **Chiavi** e clicca **GENERA CHIAVE MASTER** → conservala

> Alternativa senza GitHub: `npm i -g netlify-cli` → `netlify deploy --prod` dalla cartella.

## Primo utilizzo

1. Sezione **Cervello** → colonna "DA REGISTRARE": apri il link di un'AI (es. Groq), registrati
   gratis, copia la chiave API
2. Sezione **Chiavi** → incolla la chiave → **TESTA E ATTIVA**: il HUB la verifica e la mette in catena
3. Ripeti per le altre AI che vuoi attivare
4. Nei tuoi software usa: `POST https://<tuosito>.netlify.app/api/chat` con header
   `Authorization: Bearer <chiave master>` e body `{"messages":[{"role":"user","content":"..."}]}`

Le chiavi vivono nei Netlify Blobs (lato server), mai nel codice. Il cervello gira ogni giorno da solo.

## Struttura

```
netlify/functions/   backend serverless (chat, chiavi, cervello, stato)
src/sections/        sezioni della dashboard
src/components/      canvas di rete + componenti UI
netlify.toml         build + scheduled function (@daily)
```
