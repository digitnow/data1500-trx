# DATA1500
Eksperiment med transaksjon TRANSACTION_READ_UNCOMMITTED.
Balansen blir oppdatert, og ikke rullet tilbake for en spesifikk tidperiode, slik at andre klienter kan avlese en verdi før transaksjonen er avsluttet. 

Dette bør fungere fra hovedmappen med

```
docker-compose up
```

Det er implementert 3 stier i Transactions.java:
- `http://localhost:5005/trx/`
- `http://localhost:5005/trx/balance`

og en sti med POST-metode:
- `http://localhost:5005/trx/update`

For å teste stien med POST-metoden kan du installere curl direkte på din datamaskin (se [Curl hjemmeside](https://curl.se)) og utføre kommandoen:
```
curl -X POST http://localhost:5005/trx/update
```

I POST-metoden er det lagt inn en forsinkelse på 10 sekunder, slik at en annen prosess (tråd) kan aksessere en endret verdi før transaksjon er rullet tilbake (ROLLBACK).

Eksemplet illustrerer problemet når man tillater å lese data hele tiden, uten at det blir satt en lås når en transaksjon starter, slik at det ikke er mulig for andre prosesser å lese data før transaksjon er avsluttet.

# Eventuelle feilmeldinger

En mulig feilmelding er:
```
Error response from daemon: Mounts denied: 
The path .../data1500-trx/data1500-mysql-data is not shared from the host and is not known to Docker.
You can configure shared paths from Docker -> Preferences... -> Resources -> File Sharing.
See https://docs.docker.com/desktop/settings/mac/#file-sharing for more info.
```

Hvis du ikke finner ut hvordan du kan gi Docker tilgang til den konkrete mappen du utfører docker-compose kommandoen i, kan du spørre GPT-verktøy, for eksempel, 'hvordan fikse følgende feilmelding fra docker-compose up ...'. 