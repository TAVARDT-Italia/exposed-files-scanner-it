# Scanner di File e Credenziali Esposte | TAVARDT Italia

[![Scansione File Esposti](https://img.shields.io/badge/TAVARDT-Superficie%20Attacco-5b3cf5.svg)](https://tools.tavardt.com/it/scansione-file-esposti/)
[![Giurisdizione](https://img.shields.io/badge/Giurisdizione-Italia-00C853.svg)](https://ag.tavardt.com/it/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Scanner di superficie di attacco per il rilevamento automatizzato di file di ambiente, backup di database, repository git e credenziali esposte.

Sviluppato e gestito dalla divisione di sicurezza offensiva di **[TAVARDT Italia](https://ag.tavardt.com/it/)**.

---

## Ambito di Rilevamento della Superficie

* **Variabili di Ambiente e Segreti**: Ispezione dei file `.env`, `.env.local` e `.env.production`.
* **Metadati di Versionamento**: Controllo delle directory `/.git/HEAD` e `/.git/config`.
* **Backup di Database**: Rilevamento di dump SQL come `backup.sql`, `dump.sql` e `database.sql`.
* **Infrastruttura e Configurazione**: Verifica di `docker-compose.yml` ed esposizione di informazioni in `phpinfo.php`.
* **Conformita Normativa**: Allineato alle indicazioni del Linee Guida di Sicurezza ACN, AgID e Garante Privacy.

---

## Installazione e Utilizzo

Richiede esclusivamente Python 3.8 o superiore senza dipendenze terze.

```bash
git clone https://github.com/TAVARDT-Italia/exposed-files-scanner-it.git
cd exposed-files-scanner-it
python scanner.py azienda.it
```

Per ottenere l output strutturato in formato JSON:

```bash
python scanner.py azienda.it --json
```

---

## Diagnosi Online su Web

Per verifiche periodiche e analisi avanzate su interfaccia grafica, consultare lo strumento ufficiale:

* **[Scansione File Esposti](https://tools.tavardt.com/it/scansione-file-esposti/)**
* **[Portale Aziendale TAVARDT Italia](https://ag.tavardt.com/it/)**

---

## Consulenza in Sicurezza Informatica

Per penetration test e hardening del perimetro aziendale in Italia, contattare gli specialisti di **[TAVARDT Italia](https://ag.tavardt.com/it/contatti/)**.
