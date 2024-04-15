Per utilizzare l'addon Ngrok con Home Assistant, segui questi passaggi:

Aggiungi il repository GitHub dell'addon a Hass.io: https://github.com/ClauDOC/HomeAssistant
Installa l'addon.

Crea un account Ngrok e copia la tua chiave di autenticazione.

Configura le opzioni nell'addon (vedi descrizioni per ciascuna opzione di seguito).

Aggiungi questo addon alla tua lista di trusted_proxies nel file configuration.yaml di Home Assistant.

yaml
Copy code
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 127.0.0.1
    - 172.30.32.0/24
    - 172.30.33.0/24
Nota: Se hai modificato la rete Supervisor o Docker potresti dover aggiornare gli indirizzi per il tuo sistema. Consulta la documentazione di Home Assistant per ulteriori informazioni.

Avvia l'addon.

Riavvia Home Assistant Core.

Nota: Se non hai specificato un sottodominio o un hostname dovrai aprire l'interfaccia web per ottenere il tuo URL ngrok.io, oppure puoi utilizzare l'API per essere notificato tramite Home Assistant.

Ecco un esempio di configurazione dell'addon:

yaml
Copy code
log_level: info
auth_token: my-auth-token
region: us
tunnels:
  - name: hass
    proto: tls
    addr: 8123
    hostname: home.example.com
  - name: lets-encrypt
    proto: http
    addr: 80
    bind_tls: false
    hostname: home.example.com
Opzioni:

auth_token: Imposta il token di autenticazione di Ngrok. Questa opzione è richiesta se si utilizza un sottodominio o un hostname personalizzato o per tunnel diversi da http.

region: Specifica dove il client Ngrok si collegherà per ospitare i tunnel.

tunnels: Una lista di tunnel. Utilizza le opzioni definite di seguito per creare i tuoi tunnel. È necessario specificare almeno il nome, il proto e l'addr per ciascun tunnel.

Integrazione con Home Assistant:

Puoi sfruttare l'API del client Ngrok per esporre lo stato del tuo tunnel a Home Assistant. Ciò si fa creando un sensore REST API nel file configuration.yaml di Home Assistant.

Ad esempio, per monitorare l'URL pubblico generato da Ngrok, puoi farlo attraverso un sensore RESTful in Home Assistant.

yaml
Copy code
sensor:
  - platform: rest
    resource: http://localhost:4040/api/tunnels/hass
    name: Home Assistant URL
    value_template: '{{ value_json.public_url }}'
Riavvia quindi Home Assistant Core. Ora avrai un sensore chiamato sensor.home_assistant_url che puoi utilizzare per creare un'automazione per avvisarti dell'URL pubblico.

Per ulteriori dettagli, puoi consultare la documentazione di Ngrok.
