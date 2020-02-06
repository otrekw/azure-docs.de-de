---
title: Sicherheit und Authentifizierung – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Sicherheit und Authentifizierung in Event Grid in IoT Edge
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 5dfa17fd702b76e2cfaa7a91066dbc6749c1069e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844512"
---
# <a name="security-and-authentication"></a>Sicherheit und Authentifizierung

Sicherheit und Authentifizierung ist ein erweitertes Konzept und erfordert zunächst Vertrautheit mit den Grundlagen von Event Grid. Beginnen Sie [hier](concepts.md), wenn Sie noch nicht mit Event Grid in IoT Edge vertraut sind. Das Event Grid-Modul baut auf der vorhandenen Sicherheitsinfrastruktur in IOT Edge auf. Ausführliche sowie Einrichtungsinformationen finden Sie in [dieser Dokumentation](../../iot-edge/security.md).

In den folgenden Abschnitten wird ausführlich beschrieben, wie diese Einstellungen geschützt und authentifiziert werden:

* TLS-Konfiguration
* Eingehende Clientauthentifizierung
* Ausgehende Serverauthentifizierung
* Ausgehende Clientauthentifizierung

>[!IMPORTANT]
>Sicherheit und Authentifizierung des Event Grid-Moduls nutzen die vorhandene Infrastruktur, die in IoT Edge verfügbar ist. Die Annahme dabei ist, dass das IOT Edge-Subsystem sicher ist.

>[!IMPORTANT]
>Die Event Grid-Konfiguration ist **standardmäßig sicher**. In den folgenden Unterabschnitten werden alle Optionen und möglichen Werte erläutert, die Sie verwenden können, um Aspekte der Authentifizierung außer Kraft zu setzen. Seien Sie sich der Auswirkungen bewusst, bevor Sie Änderungen vornehmen. Damit Änderungen wirksam werden, muss das Event Grid-Modul erneut bereitgestellt werden.

## <a name="tls-configuration-aka-server-authentication"></a>TLS-Konfiguration (auch als Serverauthentifizierung bekannt)

Das Event Grid-Modul hostet sowohl HTTP- als auch HTTPS-Endpunkte. Jedem IoT Edge-Modul wird vom IoT Edge-Sicherheitsdaemon ein Serverzertifikat zugewiesen. Wir verwenden das Serverzertifikat zum Sichern des Endpunkts. Bei Ablauf wird das Modul automatisch mit einem neuen Zertifikat aus dem IoT Edge-Sicherheitsdaemon aktualisiert.

Standardmäßig ist nur HTTPS-Kommunikation zulässig. Sie können dieses Verhalten mithilfe der Konfiguration **inbound__serverAuth__tlsPolicy** außer Kraft setzen. Die folgende Tabelle enthält die möglichen Werte dieser Eigenschaft.

| Mögliche Werte | Beschreibung |
| ---------------- | ------------ |
| Strict | Standard. Aktiviert nur HTTPS
| Aktiviert | Aktiviert HTTP und HTTPS
| Disabled | Aktivieren nur HTTP

## <a name="inbound-client-authentication"></a>Eingehende Clientauthentifizierung

Clients sind Entitäten, die Verwaltungs- und/oder Laufzeitvorgänge ausführen. Clients können andere IoT Edge-Module oder auch Nicht-IoT-Anwendungen sein.

Das Event Grid-Modul unterstützt zwei Typen der Clientauthentifizierung:

* SAS-Schlüssel-basiert (Shared Access Signature)
* Zertifikatbasiert

Standardmäßig ist das Event Grid-Modul so konfiguriert, dass nur zertifikatbasierte Authentifizierung akzeptiert wird. Beim Starten ruft das Event Grid-Modul ein „TrustBundle“ vom IoT Edge-Sicherheitsdaemon ab und verwendet es, um alle Clientzertifikate zu überprüfen. Clientzertifikate, die sich nicht in diese Kette auflösten lassen, werden mit `UnAuthorized` zurückgewiesen.

### <a name="certificate-based-client-authentication"></a>Zertifikatbasierte Clientauthentifizierung

Zertifikatbasierte Authentifizierung ist standardmäßig aktiviert. Sie können die zertifikatbasierte Authentifizierung über die Eigenschaft **inbound__clientAuth__clientCert__enabled** deaktivieren. Die folgende Tabelle enthält die möglichen Werte.

| Mögliche Werte | Beschreibung |
| ----------------  | ------------ |
| true | Standard. Erfordert von allen in das Event Grid-Modul eingehenden Anforderungen, ein Clientzertifikat vorzuweisen. Außerdem müssen Sie **inbound__clientAuth__clientCert__source** konfigurieren.
| false | Ein Client wird nicht gezwungen, ein Clientzertifikat vorzuweisen.

Die folgende Tabelle enthält die möglichen Werte für **inbound__clientAuth__clientCert__source**.

| Mögliche Werte | Beschreibung |
| ---------------- | ------------ |
| IoT Edge | Standard. Verwendet das IoT Edge-Trustbundle, um alle Clientzertifikate zu überprüfen.

Wenn ein Client ein selbstsigniertes Zertifikat vorweist, weist das Event Grid-Modul solche Anforderungen standardmäßig zurück. Sie können selbstsignierte Clientzertifikate über die Eigenschaft **inbound__clientAuth__clientCert__allowUnknownCA** zulassen. Die folgende Tabelle enthält die möglichen Werte.

| Mögliche Werte | Beschreibung |
| ----------------  | ------------|
| true | Standard. Lässt zu, dass selbstsignierte Zertifikate erfolgreich vorgewiesen werden.
| false | Lässt Anforderungen fehlschlagen, wenn selbstsignierte Zertifikate vorgewiesen werden.

>[!IMPORTANT]
>In Produktionsszenarien kann es sinnvoll sein, **inbound__clientAuth__clientCert__allowUnknownCA** auf **false** festzulegen.

### <a name="sas-key-based-client-authentication"></a>SAS-Schlüssel-basierte Clientauthentifizierung

Zusätzlich zur zertifikatbasierten Authentifizierung kann das Event Grid-Modul auch die SAS-Schlüssel-basierte Authentifizierung durchführen. Ein SAS-Schlüssel entspricht einem im Event Grid-Modul konfigurierten geheimen Schlüssel, der zum Überprüfen aller eingehenden Aufrufe verwendet werden soll. Clients müssen den geheimen Schlüssel im HTTP-Header „aeg-sas-key“ angeben. Die Anforderung wird mit `UnAuthorized` zurückgewiesen, wenn sie nicht übereinstimmt.

Die Konfiguration zur Kontrolle der SAS-Schlüssel-basierten Authentifizierung erfolgt mit **inbound__clientAuth__sasKeys__enabled**.

| Mögliche Werte | Beschreibung  |
| ----------------  | ------------ |
| true | Lässt die SAS-Schlüssel-basierte Authentifizierung zu. Erfordert **inbound__clientAuth__sasKeys__key1** oder **inbound__clientAuth__sasKeys__key2**
| false | Standard. SAS-Schlüssel-basierte Authentifizierung ist deaktiviert.

 **inbound__clientAuth__sasKeys__key1** und **inbound__clientAuth__sasKeys__key2** sind Schlüssel, die Sie für das Event Grid-Modul konfigurieren, um eingehende Anforderungen zu überprüfen. Mindestens einer der Schlüssel muss konfiguriert sein. Der Client, der die Anforderung stellt, muss den Schlüssel als Teil des Anforderungsheaders „**aeg-sas-key**“ vorweisen. Wenn beide Schlüssel konfiguriert sind, kann der Client einen der beiden Schlüssel vorweisen.

> [!NOTE]
>Sie können beide Authentifizierungsmethoden konfigurieren. In einem solchen Fall wird der SAS-Schlüssel zuerst geprüft, und nur wenn dies fehlschlägt, wird die zertifikatbasierte Authentifizierung ausgeführt. Damit eine Anforderung erfolgreich ist, muss nur eine der beiden Authentifizierungsmethoden erfolgreich sein.

## <a name="outbound-client-authentication"></a>Ausgehende Clientauthentifizierung

Client im ausgehenden Kontext verweist auf das Event Grid-Modul. Der Vorgang, der ausgeführt wird, besteht im Übermitteln von Ereignisse an Abonnenten. Abonnierende Module werden als Server betrachtet.

Jedem IoT Edge-Modul wird vom IoT Edge-Sicherheitsdaemon ein Identitätszertifikat zugewiesen. Wir verwenden das Identitätszertifikat für ausgehende Aufrufe. Bei Ablauf wird das Modul automatisch mit einem neuen Zertifikat aus dem IoT Edge-Sicherheitsdaemon aktualisiert.

Die Konfiguration zur Kontrolle der ausgehenden Clientauthentifizierung erfolgt mit **outbound__clientAuth__clientCert__enabled**.

| Mögliche Werte | Beschreibung |
| ----------------  | ------------ |
| true | Standard. Erfordert von allen aus dem Event Grid-Modul ausgehenden Anforderungen, ein Zertifikat vorzuweisen. **outbound__clientAuth__clientCert__source** muss dazu konfiguriert sein.
| false | Fordert vom Event Grid-Modul nicht an, sein Zertifikat vorzuweisen.

Die Konfiguration, die die Quelle für das Zertifikat kontrolliert, lautet **outbound__clientAuth__clientCert__source**.

| Mögliche Werte | Beschreibung |
| ---------------- | ------------ |
| IoT Edge | Standard. Verwendet das von IoT Edge-Sicherheitsdaemon konfigurierte Identitätszertifikat des Moduls.

### <a name="outbound-server-authentication"></a>Ausgehende Serverauthentifizierung

Einer der Zieltypen für einen Event Grid-Abonnenten ist „Webhook“. Standardmäßig werden für solche Abonnenten nur HTTPS-Endpunkte akzeptiert.

Die Konfiguration zur Kontrolle der Webhook-Zielrichtlinie lautet **outbound__webhook__httpsOnly**.

| Mögliche Werte | Beschreibung |
| ----------------  | ------------ |
| true | Standard. Lässt nur Abonnenten mit HTTPS-Endpunkt zu.
| false | Lässt Abonnenten mit HTTP- oder HTTPS-Endpunkt zu.

Standardmäßig überprüft das Event Grid-Modul das Serverzertifikat des Abonnenten. Sie können die Überprüfung überspringen, indem Sie **outbound__webhook__skipServerCertValidation** außer Kraft setzen. Mögliche Werte:

| Mögliche Werte | Beschreibung |
| ----------------  | ------------ |
| true | Das Serverzertifikat des Abonnenten nicht überprüfen.
| false | Standard. Das Serverzertifikat des Abonnenten überprüfen.

Wenn das Zertifikat des Abonnenten selbstsigniert ist, weist das Event Grid-Modul solche Abonnenten standardmäßig ab. Um ein selbstsigniertes Zertifikat zuzulassen, können Sie **outbound__webhook__allowUnknownCA** außer Kraft setzen. Die folgende Tabelle enthält die möglichen Werte.

| Mögliche Werte | Beschreibung |
| ----------------  | ------------ |
| true | Standard. Lässt zu, dass selbstsignierte Zertifikate erfolgreich vorgewiesen werden.
| false | Lässt Anforderungen fehlschlagen, wenn selbstsignierte Zertifikate vorgewiesen werden.

>[!IMPORTANT]
>In Produktionsszenarien sollten Sie **outbound__webhook__allowUnknownCA** auf **false** festlegen.

> [!NOTE]
>Die IoT Edge-Umgebung generiert selbstsignierte Zertifikate. Es wird empfohlen, von autorisierten Zertifizierungsstellen herausgegebene Zertifikate für Produktionsworkloads zu generieren und die Eigenschaft **allowUnknownCA** sowohl für ein- als auch für ausgehend auf **false** festzulegen.

## <a name="summary"></a>Zusammenfassung

Ein Event Grid-Modul ist **standardmäßig sicher**. Es wird empfohlen, diese Standardeinstellungen für Ihre Produktionsbereitstellungen beizubehalten.

Im Folgenden finden Sie die bei der Konfiguration anzuwendenden Leitprinzipien:

* Nur HTTPS-Anforderungen als Eingang in das Modul zulassen.
* Nur zertifikatbasierte Clientauthentifizierung zulassen. Nur die Zertifikate zulassen, die von bekannten Zertifizierungsstellen ausgestellt sind. Selbstsignierte Zertifikate nicht zulassen.
* SASKey-basierte Clientauthentifizierung nicht zulassen.
* Bei ausgehenden Aufrufen immer das Identitätszertifikat des Event Grid-Moduls vorweisen.
* Nur HTTPS-Abonnenten für Webhook-Zieltypen zulassen.
* Das Serverzertifikat des Abonnenten für Webhook-Zieltypen immer überprüfen. Nur von bekannten Zertifizierungsstellen ausgestellte Zertifikate zulassen. Selbstsignierte Zertifikate nicht zulassen.

Standardmäßig wird das Event Grid-Modul mit der folgenden Konfiguration bereitgestellt:

 ```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge",
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true",
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge",
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
```
