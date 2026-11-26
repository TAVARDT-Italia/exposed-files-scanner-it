<p align="center">
  <a href="https://ag.tavardt.com/it/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# Scanner di File e Credenziali Esposte

[![Scansione File Esposti](https://img.shields.io/badge/TAVARDT-Superficie%20Attacco-5b3cf5.svg)](https://tools.tavardt.com/it/scansione-file-esposti/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Strumento automatizzato per l'ispezione della superficie di attacco mirato all'individuazione di file di ambiente esposti, dump di database SQL, metadati git e credenziali cloud.

Il motore analizza i perimetri web per identificare configurazioni errate nei server web e dispersioni involontarie di dati confidenziali prima che soggetti malevoli possano sfruttarle. Realizzato senza alcuna dipendenza esterna, funziona nativamente nella console Python e nelle pipeline di sicurezza.

***

## Ambito di Rilevamento della Superficie e Architettura delle Minacce

La disponibilità non protetta di risorse confidenziali all'interno della root pubblica dei server web rappresenta uno dei vettori di Accesso Iniziale più frequenti nell'infrastruttura moderna. Server Nginx, Apache, Caddy o storage S3 non correttamente blindati forniscono regolarmente file nascosti e dump SQL a utenti non autorizzati.

* **Variabili di Ambiente e Segreti**: Verifica sistematica di file `.env`, `.env.local` e `.env.production` che conservano credenziali di database, chiavi di cifratura e token di API.
* **Metadati di Controllo Versione**: Rilevamento di percorsi quali `/.git/HEAD` e `/.git/config`, impedendo la clonazione e l'ispezione non autorizzata del codice sorgente proprietario.
* **Backup di Database**: Riconoscimento di dump SQL sensibili tra cui `backup.sql`, `dump.sql` e `database.sql`, contenenti l'intero schema e dati aziendali.
* **Infrastruttura e Configurazione**: Controllo delle definizioni di container in `docker-compose.yml` e disclosure di diagnostica tramite `phpinfo.php` e `config.json`.
* **Credenziali di Piattaforme Cloud**: Verifica della presenza di file `/.aws/credentials`, scongiurando l'accesso amministrativo abusivo alle infrastrutture in cloud.
* **Conformità Normativa**: Progettato in conformità con i requisiti definiti nelle Linee Guida di Sicurezza ACN, AgID e Garante Privacy.

***

## Matrice di Ispezione della Superficie di Attacco

<table>
  <thead>
    <tr>
      <th align="left">Percorso Esaminato</th>
      <th align="left">Severità</th>
      <th align="left">Vettore di Rischio e Tipologia di Risorsa</th>
      <th align="left">Misura di Protezione e Hardening</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>/.env</code></td>
      <td>CRITICA</td>
      <td>Variabili di ambiente, credenziali di database e chiavi private di accesso API.</td>
      <td>Negare la lettura di file che iniziano con punto o spostare i file fuori dalla root web.</td>
    </tr>
    <tr>
      <td><code>/.env.local</code></td>
      <td>CRITICA</td>
      <td>Configurazioni di sviluppo e segreti di staging inclusi per errore in produzione.</td>
      <td>Impostare regole puntuali nel .gitignore e bloccare le richieste ai dotfile sul reverse proxy.</td>
    </tr>
    <tr>
      <td><code>/.env.production</code></td>
      <td>CRITICA</td>
      <td>Credenziali di produzione operative e token di servizi esterni integrati.</td>
      <td>Sostituire i file statici con soluzioni centralizzate di gestione dei segreti.</td>
    </tr>
    <tr>
      <td><code>/.git/HEAD</code></td>
      <td>CRITICA</td>
      <td>Puntatore del ramo git che rivela l'architettura e lo stato dei branch di codice.</td>
      <td>Interdire la cartella /.git a livello di server web e rimuovere i dati git al deploy.</td>
    </tr>
    <tr>
      <td><code>/.git/config</code></td>
      <td>CRITICA</td>
      <td>Indirizzi di repository remoti, percorsi interni ed eventuali token di accesso.</td>
      <td>Bloccare l'accesso ai metadati di versionamento su ciascun host virtuale attivo.</td>
    </tr>
    <tr>
      <td><code>/backup.sql</code></td>
      <td>CRITICA</td>
      <td>Dump integrale del database comprendente tabelle, hash di password e dati utenti.</td>
      <td>Conservare i backup solo in bucket cloud privati protetti da crittografia a riposo.</td>
    </tr>
    <tr>
      <td><code>/dump.sql</code></td>
      <td>CRITICA</td>
      <td>Esportazione temporanea del database abbandonata dopo migrazioni manuali.</td>
      <td>Eseguire script automatici di pulizia dopo ogni procedura di aggiornamento software.</td>
    </tr>
    <tr>
      <td><code>/database.sql</code></td>
      <td>CRITICA</td>
      <td>Schema delle relazioni del database e dati iniziali di configurazione.</td>
      <td>Vietare la memorizzazione di script SQL nelle directory web consultabili pubblicamente.</td>
    </tr>
    <tr>
      <td><code>/docker-compose.yml</code></td>
      <td>ALTA</td>
      <td>Topologia dei container, identificatori di rete interna e variabili applicative.</td>
      <td>Escludere i manifesti di orchestrazione dai pacchetti web rilasciati in produzione.</td>
    </tr>
    <tr>
      <td><code>/phpinfo.php</code></td>
      <td>MEDIA</td>
      <td>Versione dell'interprete PHP, estensioni attive, percorsi fisici e parametri host.</td>
      <td>Disabilitare phpinfo() nel file php.ini di produzione e rimuovere gli script di test.</td>
    </tr>
    <tr>
      <td><code>/config.json</code></td>
      <td>ALTA</td>
      <td>Configurazione interna del software, percorsi di rete e opzioni operative.</td>
      <td>Restringere l'accesso ai file JSON di configurazione o incorporarvi i valori nel codice.</td>
    </tr>
    <tr>
      <td><code>/.aws/credentials</code></td>
      <td>CRITICA</td>
      <td>Chiavi di accesso e segreti di autenticazione per le infrastrutture AWS.</td>
      <td>Adottare ruoli IAM e profili di istanza evitando il salvataggio di chiavi su disco.</td>
    </tr>
  </tbody>
</table>

***

## Installazione e Guida Rapida all'Uso

Funziona con Python 3.8 o superiore sfruttando esclusivamente la libreria standard (`sys`, `json`, `ssl`, `argparse`, `urllib.request`). Non è richiesta alcuna installazione di pacchetti mediante pip.

Clonare il repository ed eseguire la verifica sul dominio prescelto:

```bash
git clone https://github.com/TAVARDT-Italia/exposed-files-scanner-it.git
cd exposed-files-scanner-it
python scanner.py esempio.it
```

Per ottenere i dati strutturati in formato JSON:

```bash
python scanner.py esempio.it -j
```

Ispezionare i domini indicando espressamente il protocollo web:

```bash
python scanner.py https://esempio.it -j
```

Esempio di payload JSON ottenuto:

```json
{
  "target": "https://esempio.it",
  "status": "Critical (Severe secret or data exposure detected)",
  "inspected_routes": 12,
  "exposed_count": 1,
  "exposed_files": [
    {
      "path": "/.env",
      "url": "https://esempio.it/.env",
      "status_code": 200,
      "severity": "CRITICAL",
      "description": "Environment variables and API secrets"
    }
  ],
  "recommendation": "Immediately decommission exposed files and enforce web server hardening controls."
}
```

***

## Domande Frequenti (FAQ Tecnico)

### In che modo il motore distingue l'effettiva presenza di un file da pagine di errore 404 personalizzate?
Lo scanner esamina la ricezione di un codice HTTP 200 OK sui percorsi auditati. Se il server web restituisce HTTP 200 per pagine inesistenti invece di emettere codici 404 o 403, è necessario configurare correttamente le intestazioni di errore per evitare segnalazioni non pertinenti.

### Quali rischi specifici comporta l'accessibilità pubblica della directory `/.git/`?
La disponibilità libera di rotas come `/.git/HEAD` e `/.git/config` permette a utenti malintenzionati di scaricare gli oggetti del repository tramite utility automatiche. Ciò consente la riproduzione integrale del codice sorgente, l'analisi dei commit passati e l'estrazione di credenziali.

### Perché lo scanner non impone la convalida rigorosa dei certificati SSL durante l'analisi?
Il codice configura un contesto SSL flessibile (`ssl.CERT_NONE`) senza controllo del nome host per consentire l'ispezione di ambienti di staging, certificati autofirmati o domini in fase di rinnovo dei certificati senza bloccare l'esecuzione della scansione.

### Come si integra il formato JSON nei flussi automatizzati di DevSecOps?
Aggiungendo il parametro `-j`, lo script invia una struttura JSON valida direttamente sullo standard output. Le pipeline di CI/CD possono analizzare la chiave `exposed_count` con programmi come jq, interrompendo il rilascio qualora una risorsa critica restituisca un codice HTTP 200.

### Quali misure di hardening immediate devono essere adottate a fronte di queste problematiche?
È opportuno rimuovere senza indugio i file riservati dalla cartella pubblica del server e inserire direttive di blocco in Nginx (`location ~ /\.(?!well-known).* { deny all; }`) o in Apache (`RedirectMatch 404 /\.(git|env)`). I file di backup dei database non devono mai rimanere esposti sul web.

***

## Consulenza in Sicurezza Informatica

Per penetration test, bonifica di vulnerabilità e difesa perimetrale zero trust in Italia, contattare gli esperti di **TAVARDT Italia**.

***

## Link Utili

* **Portale Aziendale:** https://ag.tavardt.com/it/
* **Suite di Diagnostica:** https://tools.tavardt.com/it/
* **Contatto Tecnico:** https://ag.tavardt.com/it/contatto/

***

### Licenza
Rilasciato sotto licenza MIT.

***

© 2026 TAVARDT. Tutti i diritti riservati.
