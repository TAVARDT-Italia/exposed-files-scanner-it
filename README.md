# Scanner di File e Credenziali Esposte

[![Licenza: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Scansione File Esposti](https://img.shields.io/badge/TAVARDT-Superficie%20Attacco-5b3cf5.svg)](https://tools.tavardt.com/it/scansione-file-esposti/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Scanner di superficie di attacco per il rilevamento automatizzato di file di ambiente, backup di database, repository git e credenziali esposte.

Sviluppato e gestito dalla divisione di sicurezza offensiva di **TAVARDT**.

***

## Ambito di Rilevamento della Superficie

* **Variabili di Ambiente e Segreti**: Ispezione dei file `.env`, `.env.local` e `.env.production`.
* **Metadati di Versionamento**: Controllo delle directory `/.git/HEAD` e `/.git/config`.
* **Backup di Database**: Rilevamento di dump SQL come `backup.sql`, `dump.sql` e `database.sql`.
* **Infrastruttura e Configurazione**: Verifica di `docker-compose.yml` ed esposizione di informazioni in `phpinfo.php`.
* **Conformita Normativa**: Allineato alle indicazioni del Linee Guida di Sicurezza ACN, AgID e Garante Privacy.

***

## Installazione e Utilizzo

Richiede esclusivamente Python 3.8 o superiore senza dipendenze terze.

```bash
git clone https://github.com/TAVARDT-Italia/exposed-files-scanner-it.git
cd exposed-files-scanner-it
python scanner.py azienda.it
```

Per ottenere l output strutturato in formato JSON:

```bash
python scanner.py azienda.it -json
```

***

## Diagnosi Online su Web

Per verifiche periodiche e analisi avanzate su interfaccia grafica, consultare lo strumento ufficiale:

* **Scansione File Esposti**
* **[Portale Aziendale TAVARDT](https://ag.tavardt.com/it/)**

***

## Consulenza in Sicurezza Informatica

Per penetration test e hardening del perimetro aziendale in Italia, contattare gli specialisti di **TAVARDT**.

***

## Link Utili

* **Portale Aziendale:** https://ag.tavardt.com/it/
* **Suite di Diagnostica:** https://tools.tavardt.com/it/
* **Contatto Tecnico:** https://ag.tavardt.com/it/contatto/

***

### Licenza
Rilasciato sotto licenza MIT. Per maggiori dettagli, consultare il file [LICENSE](LICENSE).

***

© 2026 TAVARDT. Tutti i diritti riservati.
