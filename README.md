# Vexmon Launcher, release (`vexmon-launcher-releases`)

Repo pubblica con le release del launcher Vexmon e il file `update.json` che il launcher legge all'avvio.

## Cosa fa

Il codice del launcher sta nella repo privata `vexmon-launcher`. Questa repo pubblica contiene solo i file che i giocatori scaricano. Le GitHub Release contengono i jar (o gli installer) del launcher. `update.json` dice al launcher quale versione è l'ultima e dove scaricarla.

All'avvio il launcher legge `https://raw.githubusercontent.com/Vexmon/vexmon-launcher-releases/main/update.json`. Se la versione è più nuova, si aggiorna. Poi legge il manifest del modpack dalla repo `vexmon-pack`.

## Stato

| | |
|---|---|
| Ultima release | `v1.0.11`, "Fix optional mod selections after updates" |
| Asset | `vexmon-launcher.jar` |
| Tipo | `jar` (aggiornamento automatico) |

La repo deve restare pubblica. Con la repo privata il launcher riceve un 404 su `raw.githubusercontent.com`.

## Origine

Interna. Le release vengono da [`vexmon-launcher`](https://github.com/Vexmon/vexmon-launcher).

## Stack

- Nessun codice: solo `update.json` e asset delle GitHub Release.
- Il launcher è un'applicazione Java. Il jar si costruisce con `./gradlew shadowJar`, l'installer Windows con `./gradlew createInstaller`.

## Formato di `update.json`

```json
{
  "version": "1.0.11",
  "type": "jar",
  "url": "https://github.com/Vexmon/vexmon-launcher-releases/releases/download/v1.0.11/vexmon-launcher.jar",
  "downloads": {
    "jar": "https://github.com/Vexmon/vexmon-launcher-releases/releases/download/v1.0.11/vexmon-launcher.jar"
  },
  "changelog": "Fix optional mod selections after updates"
}
```

| Chiave | Cosa contiene |
|---|---|
| `version` | Ultima versione del launcher. Deve combaciare con `Config.LAUNCHER_VERSION` della release |
| `type` | `jar`: il launcher si aggiorna da solo e si riavvia. `installer`: il launcher mostra una schermata bloccante e chiede di scaricare il nuovo installer |
| `url` | Link all'asset della release |
| `downloads` | Link per piattaforma o tipo di asset |
| `changelog` | Testo mostrato all'utente |

Usa `installer` solo per cambi di versione Java, nuove dipendenze native o ristrutturazioni. Per bugfix, funzioni e UI usa `jar`.

## Pubblicare una release

Usa lo script `release.sh` nella repo `vexmon-launcher`. Lo script:

1. chiede il tipo di release (`jar` o `installer`);
2. aggiorna `LAUNCHER_VERSION` in `Config.java` e, per l'installer, `MyAppVersion` in `installer/setup.iss`;
3. costruisce il jar o l'installer;
4. crea la release `v<versione>` qui con `gh release create` e allega l'asset;
5. aggiorna `update.json` in questa repo e fa il push.

Non modificare `update.json` a mano senza creare prima la release. Un URL senza asset rompe l'aggiornamento dei giocatori. La guida completa è `RELEASE-GUIDE.md` in `vexmon-launcher`.

## Branch

| Branch | Uso |
|---|---|
| `main` | `update.json` letto dal launcher |

## Maintainer

ytnoos
