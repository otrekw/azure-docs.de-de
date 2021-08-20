---
title: Verwenden von Twilio für Sprachanrufe und SMS (Python) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit dem Twilio-API-Dienst in Azure einen Telefonanruf tätigen und eine SMS-Nachricht senden. Die Codebeispiele wurden in Python geschrieben.
services: ''
documentationcenter: python
author: georgewallace
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: gwallace
ms.custom: devx-track-python
ms.openlocfilehash: 7d2539f336da75b2bd2f5151257ecab42d2508f1
ms.sourcegitcommit: 30e3eaaa8852a2fe9c454c0dd1967d824e5d6f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2021
ms.locfileid: "112452636"
---
# <a name="use-twilio-for-voice-and-sms-capabilities-in-python"></a>Verwenden von Twilio für Telefonie- und SMS-Funktionen in Python
In diesem Artikel wird veranschaulicht, wie gängige Programmieraufgaben mit dem Twilio-API-Dienst in Azure ausgeführt werden. Die behandelten Szenarios umfassen das Tätigen eines Telefonanrufs und das Senden einer SMS-Nachricht (Short Message Service). 

Weitere Informationen zu Twilio und zur Verwendung von Telefonie und SMS in Ihren Anweisungen finden Sie im Abschnitt [Nächste Schritte](#NextSteps) .

## <a name="what-is-twilio"></a><a id="WhatIs"></a>Was ist Twilio?
Mit Twilio können Entwickler Sprachfunktionen, die Sprachtelefonie über IP (VoIP) und das Messaging in Anwendungen einbetten. Entwickler virtualisieren die gesamte erforderliche Infrastruktur in einer cloudbasierten, globalen Umgebung und machen diese über die Twilio-API-Plattform verfügbar. Anwendungen können einfach erstellt und skaliert werden.

Zu den Twilio-Komponenten gehören folgende:

- **Twilio Voice:** Hiermit können Ihre Anwendungen Telefonanrufe tätigen und empfangen.
- **Twilio SMS:** Hiermit kann Ihre Anwendung Textnachrichten senden und empfangen.
- **Twilio Client:** Hiermit können Sie VoIP-Anrufe von jedem Telefon, Tablet oder Browser aus tätigen. Dabei wird die WebRTC-Spezifikation für die Echtzeitkommunikation unterstützt.

## <a name="twilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio-Preise und -Sonderangebote
Azure-Kunden erhalten ein [spezielles Angebot][special_offer]: eine Twilio-Gutschrift in Höhe von 10 US-Dollar bei einem Upgrade Ihres Twilio-Kontos. Diese Gutschrift kann für jede beliebige Twilio-Nutzung eingesetzt werden. Eine Gutschrift von 10 US-Dollar reicht für den Versand von 1.000 SMS-Nachrichten oder für bis zu 1.000 eingehende Telefonminuten, abhängig vom Standort der Telefonnummer und dem Nachrichten- oder Anrufziel.

Twilio ist ein Pay-as-you-go-Dienst, also vertragsungebunden. Es fallen keine Einrichtungsgebühren an, und Sie können Ihr Konto jederzeit schließen. Weitere Informationen finden Sie unter [Twilio-Preise][twilio_pricing].

## <a name="concepts"></a><a id="Concepts"></a>Konzepte
Die Twilio-API ist eine RESTful-API, die Telefonie- und SMS-Funktionen für Anwendungen bereitstellt. Clientbibliotheken stehen in mehreren Sprachen zur Verfügung. Eine Liste finden Sie in den [Twilio-API-Bibliotheken][twilio_libraries].

Schlüsselaspekte der Twilio API sind Twilio-Verben und die Twilio Markup Language (TwiML).

### <a name="twilio-verbs"></a><a id="Verbs"></a>Twilio-Verben
Die API verwendet Verben wie die folgenden, um Twilio mitzuteilen, was zu tun ist:

* **&lt;Dial&gt;** : Verbindet den Anrufer mit einem anderen Telefon.
* **&lt;Gather&gt;** : Erfasst Ziffern, die über die Telefontasten eingegeben werden.
* **&lt;Hangup&gt;** : Beendet einen Anruf.
* **&lt;Pause&gt;** : Wartet ohne Ton für eine angegebene Anzahl von Sekunden.
* **&lt;Play&gt;** : Gibt eine Audiodatei wieder.
* **&lt;Queue&gt;** : Hiermit wird das Element zu einer Warteschlange von Aufrufern hinzugefügt.
* **&lt;Record&gt;** : Zeichnet die Stimme des Anrufers auf und gibt eine URL einer Datei mit der Aufzeichnung zurück.
* **&lt;Redirect&gt;** : Überträgt die Steuerung eines Anrufs oder einer SMS an die TwiML an einer anderen URL.
* **&lt;Reject&gt;** : Lehnt einen bei Ihrer Twilio-Nummer eingehenden Anruf ab, ohne dass Ihnen Kosten entstehen.
* **&lt;Say&gt;** : Hiermit wird bei einem Aufruf Text in Sprache konvertiert.
* **&lt;Sms&gt;** : Sendet eine SMS-Nachricht.

Andere Verben und Funktionen werden in der [Dokumentation zur Twilio Markup Language][twiml] erläutert.

### <a name="twiml"></a><a id="TwiML"></a>TwiML
TwiML ist eine Gruppe XML-basierter Anweisungen, die auf Twilio-Verben basieren und Twilio anweisen, wie ein Anruf oder eine SMS-Nachricht verarbeitet werden soll.

Die TwiML-Anweisung im folgenden Beispiel konvertiert den Text **Hello World** in Sprache:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
  <Response>
    <Say>Hello World</Say>
  </Response>
```

Wenn eine Anwendung die [Twilio-API][twilio_api] aufruft, ist einer der API-Parameter die URL, die die TwiML-Antwort zurückgibt. Zu Entwicklungszwecken können Sie von Twilio bereitgestellte URLs verwenden, um die von Ihren Anwendungen genutzten TwiML-Antworten bereitzustellen. Sie können auch Ihre eigenen URLs hosten, um TwiML-Antworten zu generieren. Eine weitere Option besteht in der Verwendung des `TwiMLResponse`-Objekts.

## <a name="create-a-twilio-account"></a><a id="CreateAccount"></a>Erstellen von Twilio-Konten
Wenn Sie ein Twilio-Konto erstellen möchten, melden Sie sich bei [Try Twilio][try_twilio] an. Sie können mit einem kostenlosen Konto beginnen und später ein Upgrade vornehmen.

Wenn Sie sich für ein Twilio-Konto anmelden, erhalten Sie eine Konto-SID (Sicherheits-ID) und ein Authentifizierungstoken. Sie benötigen beides, um Twilio-API-Aufrufe durchführen zu können. Bewahren Sie Ihr Authentifizierungstoken sicher auf, um unbefugten Zugriff auf Ihr Konto zu verhindern. Ihre Konto-SID und das Authentifizierungstoken können Sie in der [Twilio-Konsole][twilio_console] in den Feldern **ACCOUNT SID** bzw. **AUTH TOKEN** einsehen.

## <a name="create-a-python-application"></a><a id="create_app"></a>Erstellen einer Python-Anwendung
Eine Python-Anwendung, die Twilio verwendet und in Azure ausgeführt wird, unterscheidet sich nicht von anderen Python-Anwendungen, die Twilio verwenden. Obwohl Twilio-Dienste REST-basiert sind und in Python auf verschiedene Arten aufgerufen werden können, konzentriert sich dieser Artikel auf die Verwendung von Twilio-Diensten mit der [Twilio-Bibliothek für Python von GitHub][twilio_python]. Weitere Informationen zur Verwendung dieser Bibliothek finden Sie in der [Dokumentation zur Twilio Python Helper Library][twilio_lib_docs].

[Richten Sie zuerst eine neue Azure-Linux-VM ein][azure_vm_setup], die als Host für Ihre neue Python-Webanwendung fungiert. Nachdem der virtuelle Computer ausgeführt wird, müssen Sie Ihre Anwendung an einem öffentlichen Port verfügbar machen.

So fügen Sie eine Eingangsregel hinzu:
  1. Rufen Sie die Seite [Netzwerksicherheitsgruppe][azure_nsg] auf.
  2. Wählen Sie die Ihrem virtuellen Computer entsprechende Netzwerksicherheitsgruppe aus.
  3. Fügen Sie die Informationen für die **Ausgangsregel** für **Port 80** hinzu. Achten Sie darauf, eingehende Aufrufe von allen beliebigen Adressen zuzulassen.

So legen Sie die DNS-Namensbezeichnung fest:
  1. Navigieren Sie zur Seite [Verwalten öffentlicher IP-Adressen][azure_ips].
  2. Wählen Sie die öffentliche IP-Adresse aus, die Ihrem virtuellen Computer entspricht.
  3. Legen Sie die Information **DNS-Namensbezeichnung** im Abschnitt **Konfiguration** fest. In diesem Beispiel sieht dies etwa wie folgt aus: *\<your-domain-label\>.centralus.cloudapp.azure.com*.

Nachdem Sie über SSH eine Verbindung mit dem virtuellen Computer herstellen konnten, können Sie das Webframework Ihrer Wahl installieren. Die beiden bekanntesten Webframeworks in Python sind [Flask](http://flask.pocoo.org/) und [Django](https://www.djangoproject.com). Sie können eine der beiden installieren, indem Sie den Befehl `pip install` ausführen.

Bedenken Sie, dass Sie den virtuellen Computer zum Zulassen von Datenverkehr nur an Port 80 konfiguriert haben. Stellen Sie daher sicher, dass Sie die Anwendung so konfigurieren, dass dieser Port verwendet wird.

## <a name="configure-your-application-to-use-the-twilio-library"></a><a id="configure_app"></a>Konfigurieren einer Anwendung für die Verwendung der Twilio-Bibliothek
Sie können die Anwendung auf zwei verschiedene Arten für die Twilio-Bibliothek für Python konfigurieren:

* Installieren Sie die Twilio-Bibliothek für Python mithilfe des folgenden Befehls als PIP-Paket:
   
  `$ pip install twilio`

* Laden Sie die [Twilio-Bibliothek für Python von GitHub][twilio_python] herunter, und installieren Sie sie wie folgt:

  `$ python setup.py install`

Nachdem Sie die Twilio-Bibliothek für Python installiert haben, können Sie sie in Ihre Python-Dateien importieren:

`import twilio`

Weitere Informationen finden Sie in der [Infodatei für twilio-python](https://github.com/twilio/twilio-python/blob/master/README.md).

## <a name="make-an-outgoing-call"></a><a id="howto_make_call"></a>Tätigen von ausgehenden Anrufen
Das folgende Beispiel veranschaulicht, wie Sie einen ausgehenden Anruf tätigen. Dieser Code verwendet ebenfalls eine von Twilio bereitgestellte Website für die Rückgabe der TwiML-Antwort. Ersetzen Sie die Werte für die Telefonnummern `from_number` und `to_number`. Stellen Sie sicher, dass Sie vor der Ausführung des Codes die Telefonnummer `from_number` für Ihr Twilio-Konto überprüft haben.

```python
from urllib.parse import urlencode

# Import the Twilio Python Client.
from twilio.rest import TwilioRestClient

# Set your account ID and authentication token.
account_sid = "your_twilio_account_sid"
auth_token = "your_twilio_authentication_token"

# The number of the phone initiating the call.
# This should either be a Twilio number or a number that you've verified.
from_number = "NNNNNNNNNNN"

# The number of the phone receiving call.
to_number = "NNNNNNNNNNN"

# Use the Twilio-provided site for the TwiML response.
url = "https://twimlets.com/message?"

# The phone message text.
message = "Hello world."

# Initialize the Twilio client.
client = TwilioRestClient(account_sid, auth_token)

# Make the call.
call = client.calls.create(to=to_number,
                           from_=from_number,
                           url=url + urlencode({'Message': message}))
print(call.sid)
```

> [!IMPORTANT]
> Telefonnummern sollten mit einem Pluszeichen und einem Ländercode formatiert werden. Ein Beispiel hierfür ist `+16175551212` (E.164-Format). Twilio akzeptiert auch unformatierte US-Nummern wie `(415) 555-1212` oder `415-555-1212`.

Dieser Code verwendet eine von Twilio bereitgestellte Website für die Rückgabe der TwiML-Antwort. Sie können stattdessen Ihre eigene Website verwenden, um die TwiML-Antwort bereitzustellen. Weitere Informationen finden Sie unter [Bereitstellen von TwiML-Antworten von der eigenen Website](#howto_provide_twiml_responses).

## <a name="send-an-sms-message"></a><a id="howto_send_sms"></a>Senden einer SMS
Das folgende Beispiel zeigt, wie Sie mithilfe der `TwilioRestClient`-Klasse eine SMS-Nachricht senden. Twilio gibt die Nummer `from_number` für Testkonten zum Senden von SMS-Nachrichten an. Die `to_number` -Nummer muss für Ihr Twilio-Konto verifiziert werden, bevor Sie den Code ausführen.

```python
# Import the Twilio Python Client.
from twilio.rest import TwilioRestClient

# Set your account ID and authentication token.
account_sid = "your_twilio_account_sid"
auth_token = "your_twilio_authentication_token"

from_number = "NNNNNNNNNNN"  # With a trial account, texts can only be sent from your Twilio number.
to_number = "NNNNNNNNNNN"
message = "Hello world."

# Initialize the Twilio client.
client = TwilioRestClient(account_sid, auth_token)

# Send the SMS message.
message = client.messages.create(to=to_number,
                                    from_=from_number,
                                    body=message)
```

## <a name="provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Bereitstellen von TwiML-Antworten von der eigenen Website
Wenn Ihre Anwendung einen Aufruf der Twilio-API startet, sendet Twilio Ihre Anforderung an eine URL, von der die Rückgabe einer TwiML-Antwort erwartet wird. Im vorausgehenden Beispiel wird die von Twilio bereitgestellte URL [https://twimlets.com/message][twimlet_message_url] verwendet. 

> [!NOTE]
> Obwohl TwiML für die Verwendung durch Twilio konzipiert ist, kann TwiML dennoch im Browser angezeigt werden. Wählen Sie beispielsweise [https://twimlets.com/message][twimlet_message_url] aus, um ein leeres `<Response>` -Element anzuzeigen. Wählen Sie als weiteres Beispiel [https://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] aus, um ein `<Response>` -Element anzuzeigen, das ein `<Say>` -Element enthält.

Anstelle der von Twilio bereitgestellten URL können Sie auch Ihre eigene Website für die Rückgabe von HTTP-Antworten erstellen. Sie können die Website mit jeder Sprache erstellen, die XML-Antworten zurückgibt. In diesem Artikel wird davon ausgegangen, dass Sie Python zum Erstellen von TwiML-Antworten verwenden.

Die folgenden Beispiele geben eine TwiML-Antwort aus, die beim Anruf **Hello World** sagt.

Mit Flask:

```python
from flask import Response
@app.route("/")
def hello():
    xml = '<Response><Say>Hello world.</Say></Response>'
    return Response(xml, mimetype='text/xml')
```

Mit Django:

```python
from django.http import HttpResponse
def hello(request):
    xml = '<Response><Say>Hello world.</Say></Response>'
    return HttpResponse(xml, content_type='text/xml')
```

Wie das vorherige Beispiel zeigt, handelt es sich bei der TwiML-Antwort einfach um ein XML-Dokument. Die Twilio-Bibliothek für Python enthält Klassen, die TwiML für Sie generieren. Das folgende Beispiel generiert die gleiche Antwort wie zuvor, jedoch unter Verwendung des `twiml`-Moduls in der Twilio-Bibliothek für Python:

```python
from twilio import twiml

response = twiml.Response()
response.say("Hello world.")
print(str(response))
```

Weitere Informationen zu TwiML finden Sie in der [TwiML-Referenz][twiml_reference].

Nachdem Sie Ihre Python-Anwendung für das Zurückgeben von TwiML-Antworten eingerichtet haben, können Sie die URL der Anwendung an die `client.calls.create`-Methode übergeben. Wenn Sie beispielsweise eine Webanwendung mit dem Namen *MyTwiML* für einen gehosteten Azure-Dienst bereitgestellt haben, können Sie die URL wie im folgenden Beispiel gezeigt als Webhook verwenden:

```python
from twilio.rest import TwilioRestClient

account_sid = "your_twilio_account_sid"
auth_token = "your_twilio_authentication_token"
from_number = "NNNNNNNNNNN"
to_number = "NNNNNNNNNNN"
url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

# Initialize the Twilio Client.
client = TwilioRestClient(account_sid, auth_token)

# Make the call.
call = client.calls.create(to=to_number,
                           from_=from_number,
                           url=url)
print(call.sid)
```

## <a name="use-additional-twilio-services"></a><a id="AdditionalServices"></a>Verwenden zusätzlicher Twilio-Dienste
Zusätzlich zu den hier gezeigten Beispielen bietet Twilio webbasierte APIs, mit denen Sie zusätzliche Twilio-Funktionen in Ihrer Azure-Anwendung verwenden können. Ausführliche Informationen finden Sie in der [Twilio API-Dokumentation][twilio_api].

## <a name="next-steps"></a><a id="NextSteps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen des Twilio-Dienstes vertraut sind, finden Sie unter diesen Links weitere Informationen:

* [Twilio-Sicherheitsrichtlinien][twilio_security_guidelines]
* [Twilio-Anleitungen und Beispielcode][twilio_howtos]
* [Twilio-Schnellstarttutorials][twilio_quickstarts]
* [Twilio auf GitHub][twilio_on_github]
* [Kontakt zum Twilio-Support][twilio_support]

[special_offer]: https://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/python
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/docs/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/all
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
[azure_ips]: ./virtual-network/virtual-network-public-ip-address.md
[azure_vm_setup]: ./virtual-machines/linux/quick-create-portal.md
[azure_nsg]: ./virtual-network/manage-network-security-group.md
