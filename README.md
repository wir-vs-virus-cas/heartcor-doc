# heartcor-doc
Dieses Repository enthält die Dokumentation zum Beitrag [<3cor](https://heartcor.org) im [Wir vs. Virus Hackathon](https://wirvsvirushackathon.devpost.com/).

## Softwarearchitektur
Die bisherige Implementierung von <3cor basiert auf zwei funktionalen Komponenten:
1. TONI - Telefonische ONline Integration: unser Telefon zu Alexa Gateway
2. KALLI - Call-management: unser Alexa Skill zum senden und empfangen von Nachrichten

![Softwarearchitektur](architecture.png)

### TONI
Toni ist als Java Dienst umgesetzt, welcher als virtuelles Amazon Alexa Gerät agiert und den Ton von Anrufen an den [Alexa Voice Service](https://developer.amazon.com/de-DE/alexa/alexa-voice-service) weiterleitet.
Um den Dienst zu betreiben sind ein Konto auf Amazon Seite sowie ein SIP Zugang erforderlich, für unsere Tests haben wir hier auf [Sipgate](https://www.sipgatebasic.de/) gesetzt.
Ist der Dienst bereitgestellt, so ist dieser dann als normaler Festnetzanschluss erreichbar.
Die Implementierung des Dienstes findet ihr auf Github als [wir-vs-virus-cas/heartcor-sip-proxy](https://github.com/wir-vs-virus-cas/heartcor-sip-proxy).

### KALLI
Das Call-management ist technisch in mehrere Subkomponenten aufgeteilt. Das Nutzerinterface ist momentan als Alexa Skill implementiert. Der Quellcode ist in unserem Repository [wir-vs-virus-cas/heartcor-alexa-skill](https://github.com/wir-vs-virus-cas/heartcor-alexa-skill) zu finden.
Dieser beinhaltet neben der Skilldefinition die Quellen für den Dienst, welcher zusätzlich bereitgestellt werden muss.
Für unser Hosting setzen wir auf [Tomcat](https://tomcat.apache.org/) mit [nginx](https://nginx.org/) als Reverse Proxy für SSL.
Die notwendigen Zertifikate kommen von [Let's Encrypt](https://letsencrypt.org/).

Dieser Skill greift auf unseren Backend-Dienst zu, der entsprechende Quellcode ist auf Github unter [wir-vs-virus-cas/heartcor-server](https://github.com/wir-vs-virus-cas/heartcor-server) zu finden.
Technisch gliedert der Dienst sich in eine API und einen funktionalen Kern.
Für die Datenhaltung nutzen wir hier auf [MongoDB](https://www.mongodb.com/download-center/community) und [RabbitMQ](https://www.rabbitmq.com/).
Weitere Dienste wie die konzeptionell geplante grafische Oberfläche können ebenfalls auf dieses Backend zugreifen.