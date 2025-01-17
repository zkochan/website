---
id: ssl
title: "Configurare i Certificati SSL"
---

Seguire queste istruzioni per configurare un certificato SSL che serva al registro NPM su HTTPS.

* Aggiornare la proprietà listen in `~/.config/verdaccio/config.yaml`:

    listen: 'https://your.domain.com/'
    

Una volta aggiornata la proprietà listen e provato ad avviare verdaccio di nuovo, chiederà i certificati.

* Generare i certificati

     $ openssl genrsa -out /Users/user/.config/verdaccio/verdaccio-key.pem 2048
     $ openssl req -new -sha256 -key /Users/user/.config/verdaccio/verdaccio-key.pem -out /Users/user/.config/verdaccio/verdaccio-csr.pem
     $ openssl x509 -req -in /Users/user/.config/verdaccio/verdaccio-csr.pem -signkey /Users/user/.config/verdaccio/verdaccio-key.pem -out /Users/user/.config/verdaccio/verdaccio-cert.pem
     ````
    
    * Modificare il file di configurazione `/Users/user/.config/verdaccio/config.yaml` e aggiungere la parte seguente:
    
    

https: key: /Users/user/.config/verdaccio/verdaccio-key.pem cert: /Users/user/.config/verdaccio/verdaccio-cert.pem ca: /Users/user/.config/verdaccio/verdaccio-csr.pem

    <br />In alternativa, se si possiede un certificato con il formato `server.pfx`, si può aggiungere la parte seguente: (La passphrase è facoltativa e solo necessaria se il certificato è criptato.)
    
    

https: pfx: /Users/user/.config/verdaccio/server.pfx passphrase: 'secret' ````

Ulteriori informazioni sugli argomenti `key`, `cert`, `ca`, `pfx` e `passphrase` nella [documentazione Node ](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options)

* Eseguire `verdaccio` nella linea di comando.

* Aprire il browser e visitare `https://your.domain.com:port/`

Queste istruzioni sono ampiamente valide per OSX e Linux; per Windows i percorsi varieranno, ma i passaggi sono gli stessi.

## Docker

If you are using the Docker image, you have to set the `VERDACCIO_PROTOCOL` environment variable to `https`, as the `listen` argument is provided in the [Dockerfile](https://github.com/verdaccio/verdaccio/blob/master/Dockerfile#L43) and thus ignored from your config file.

You can also set the `VERDACCIO_PORT` environment variable if you are using a port other than `4873`.