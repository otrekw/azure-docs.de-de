---
title: Hinzufügen von Daten zu Überwachungsprotokollen mithilfe benutzerdefinierter Header – Azure API for FHIR
description: Dieser Artikel beschreibt das Hinzufügen von Daten zu Überwachungsprotokollen mithilfe benutzerdefinierter HTTP-Header in Azure API for FHIR.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 10/13/2019
ms.openlocfilehash: d274160cc2ed1102dfc8fd11df358b34e40d9923
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "84870299"
---
# <a name="add-data-to-audit-logs-by-using-custom-http-headers"></a>Hinzufügen von Daten zu Überwachungsprotokollen mithilfe benutzerdefinierter HTTP-Header

Möglicherweise möchte ein Benutzer in der Azure FHIR-API (Fast Healthcare Interoperability Resources) zusätzliche Informationen in die Protokolle aufnehmen, die aus dem aufrufenden System stammen.

Wenn der Benutzer der API beispielsweise von einem externen System authentifiziert wird, leitet dieses System den Aufruf an die FHIR-API weiter. Auf Ebene der FHIR-API gehen die Informationen zum ursprünglichen Benutzer verloren, da der Aufruf weitergeleitet wurde. Möglicherweise müssen diese Benutzerinformationen zu Überwachungs- oder Verwaltungszwecken protokolliert und aufbewahrt werden. Das aufrufende System kann die Benutzeridentität, den Standort des Aufrufers oder andere erforderliche Informationen in den HTTP-Headern bereitstellen, die bei der Weiterleitung des Aufrufs übernommen werden.

Dieser Datenfluss ist im folgenden Diagramm dargestellt:

:::image type="content" source="media/custom-headers/custom-headers-diagram.png" alt-text="Diagramm zu benutzerdefinierten Headern":::

Mit benutzerdefinierten Headern können Sie verschiedene Arten von Informationen erfassen. Beispiel:

* Identitäts- und Autorisierungsinformationen
* Ursprung des Aufrufers
* Ursprungsorganisation
* Details zum Clientsystem (elektronische Patientenakte, Patientenportal)

> [!IMPORTANT]
> Beachten Sie, dass die in benutzerdefinierten Headern gesendeten Informationen nach der Bereitstellung in der Azure-Protokollüberwachung 30 Tage lang in einem internen Microsoft-Protokollierungssystem gespeichert werden. Es wird empfohlen, alle Informationen zu verschlüsseln, bevor diese benutzerdefinierten Headern hinzugefügt werden. Sie sollten keine Patienteninformationen über benutzerdefinierte Header übergeben.

Sie müssen die folgende Benennungskonvention für Ihre HTTP-Header verwenden: X-MS-AZUREFHIR-AUDIT-AUDIT-\<Name>.

Diese HTTP-Header sind in einer Eigenschaftensammlung enthalten, die dem Protokoll hinzugefügt wird. Beispiel:

* X-MS-AZUREFHIR-AUDIT-USERID: 1234 
* X-MS-AZUREFHIR-AUDIT-USERLOCATION: XXXX
* X-MS-AZUREFHIR-AUDIT-XYZ: 1234

Diese Informationen werden dann beim Hinzufügen der Eigenschaftenspalte im Protokoll in JSON serialisiert. Beispiel:

```json
{ "X-MS-AZUREFHIR-AUDIT-USERID" : "1234",
"X-MS-AZUREFHIR-AUDIT-USERLOCATION" : "XXXX",
"X-MS-AZUREFHIR-AUDIT-XYZ" : "1234" }
```
 
Wie bei jedem HTTP-Header kann derselbe Headername mit unterschiedlichen Werten wiederholt werden. Beispiel:

* X-MS-AZUREFHIR-AUDIT-USERLOCATION: KrankenhausA
* X-MS-AZUREFHIR-AUDIT-USERLOCATION: Notfall

Beim Hinzufügen zum Protokoll werden die Werte mit einer durch Trennzeichen getrennten Liste kombiniert. Beispiel:

{ "X-MS-AZUREFHIR-AUDIT-USERLOCATION" : "KrankenhausA, Notfall" }
 
Sie können maximal zehn eindeutige Header hinzufügen (Wiederholungen desselben Headers mit unterschiedlichen Werten werden nur als einer gezählt). Die maximale Gesamtlänge des Werts für einen einzigen Header beträgt 2048 Zeichen.

Wenn Sie die Firely C#-Client-API-Bibliothek verwenden, sieht der Code in etwa wie folgt aus:

```C#
FhirClient client;
client = new FhirClient(serverUrl);
client.OnBeforeRequest += (object sender, BeforeRequestEventArgs e) =>
{
    // Add custom headers to be added to the logs
    e.RawRequest.Headers.Add("X-MS-AZUREFHIR-AUDIT-UserLocation", "HospitalA");
};
client.Get("Patient");
```
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie mithilfe von benutzerdefinierten Headern in Azure API for FHIR Daten zu Überwachungsprotokollen hinzufügen können. Im nächsten Artikel lernen Sie weitere Einstellungen kennen, die Sie in Azure API for FHIR konfigurieren können.
 
>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)