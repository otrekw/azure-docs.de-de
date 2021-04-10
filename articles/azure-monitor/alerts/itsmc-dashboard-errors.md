---
title: Connectorstatusfehler auf dem Dashboard des ITSM-Connectors
description: Hier finden Sie Informationen zu allgemeinen Fehlern auf dem Dashboard des ITSM-Connectors.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: 727e744c59d0a8d90cf320e1ee2e2a17e10ff847
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103471523"
---
# <a name="connector-status-errors-in-the-itsmc-dashboard"></a>Connectorstatusfehler auf dem Dashboard des ITSM-Connectors

Die auf dem Dashboard des ITSM-Connectors (IT-Service-Management) angezeigten Fehler unterstützen Sie beim Beheben von Connectorproblemen.

In den folgenden Abschnitten finden Sie Informationen zu allgemeinen Fehlern, die im Abschnitt „Connectorstatus“ des Dashboards angezeigt werden, sowie zu deren Behebung.

## <a name="unexpected-response"></a>Unerwartete Antwort

**Fehler:** „Unerwartete Antwort von ServiceNow zusammen mit Erfolgsstatuscode. Antwort: { "import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "result": [ { "transform_map": "OMS Incident", "table": "incident", "status": "error", "error_message": "{Target record not found|Invalid table|Invalid staging table" }“

**Ursache:** Dieser Fehler wird von ServiceNow in folgenden Fällen zurückgegeben:

* Ein in einer ServiceNow-Instanz bereitgestelltes benutzerdefiniertes Skript bewirkt, dass Incidents ignoriert werden.
* Der Code der OMS-Integrator-App wurde von ServiceNow geändert (beispielsweise über das Skript `onBefore`).

**Lösung:** Deaktivieren Sie alle benutzerdefinierten Skripts oder Codeänderungen.

## <a name="exception-update-failure"></a>Fehler bei der Ausnahmenaktualisierung

**Fehler**: „{"error":{"message":"Operation Failed","detail":"ACL Exception Update Failed due to security constraints"}“

**Ursache:** ServiceNow-Berechtigungen sind nicht korrekt konfiguriert.

**Lösung:** Überprüfen Sie, ob alle Rollen ordnungsgemäß wie [hier](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role) angegeben zugewiesen sind.

## <a name="problem-sending-a-request"></a>Problem beim Senden einer Anforderung

**Fehler:** „Fehler beim Senden der Anforderung.“

**Ursache:** Eine ServiceNow-Instanz ist nicht verfügbar.

**Lösung:** Überprüfen Sie Ihre Instanz in ServiceNow. Möglicherweise wurde sie gelöscht oder ist nicht verfügbar.

## <a name="servicenow-rate-problem"></a>Problem mit ServiceNow-Raten

**Fehler:** „ServiceDeskHttpBadRequestException: StatusCode=429“

**Ursache:** Die ServiceNow-Ratenbegrenzungen sind zu hoch oder zu niedrig.

**Lösung:** Erhöhen oder deaktivieren Sie die Ratenbegrenzungen in der ServiceNow-Instanz, wie in der [ServiceNow-Dokumentation](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html) erläutert.

## <a name="invalid-refresh-token"></a>Ungültiges Aktualisierungstoken

**Fehler**: 
  * „AccessToken und RefreshToken sind ungültig. Der Benutzer muss sich erneut authentifizieren.“
  * „Could not sync templates configuration for Event,Alert,Incident. See Exception Message for more details.“ (Die Vorlagenkonfiguration für Ereignis, Warnung, Incident konnte nicht synchronisiert werden. In der Ausnahmemeldung finden Sie weitere Informationen.)

**Ursache:** Ein Aktualisierungstoken ist abgelaufen.

**Lösung:** Synchronisieren Sie den ITSM-Connector, um ein neues Aktualisierungstoken zu generieren, wie unter [Manuelles Beheben von Synchronisierungsproblemen](./itsmc-resync-servicenow.md) erläutert.

## <a name="missing-connector"></a>Fehlender Connector

**Fehler:** „Für die Warnung {alertName} konnte kein Arbeitselement erstellt oder aktualisiert werden. Der ITSM-Connector {connectionIdentifier} ist nicht vorhanden oder wurde gelöscht.“

**Ursache:** Der ITSM-Connector wurde gelöscht.

**Lösung:** Der ITSM-Connector wurde gelöscht, aber die definierte ITSM-Aktionsgruppe (IT-Service-Management) ist ihm immer noch zugeordnet. Dieses Problem kann auf drei Arten gelöst werden:

* Suchen und Deaktivieren oder Löschen dieser Aktionsgruppen
* [Neukonfigurieren der Aktionsgruppen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts), um einen vorhandenen ITSM-Connector zu verwenden
* [Erstellen einer neuen ITSM-Instanz](./itsmc-definition.md#create-an-itsm-connection) und [Neukonfigurieren der Aktionsgruppen, um ihn zu verwenden](itsmc-definition.md#create-itsm-work-items-from-azure-alerts)

## <a name="lack-of-connection-details"></a>Fehlende Verbindungsdetails

**Fehler**: „Leider ist ein Problem aufgetreten. Die Verbindungsdetails konnten nicht abgerufen werden.“ Dieser Fehler wird angezeigt, wenn Sie eine ITSM-Aktionsgruppe definieren.

**Ursache:** Fehler dieser Art treten in einer der folgenden Situationen auf:

* Für eine neu erstellte ITSM-Connectorinstanz muss die Erstsynchronisierung noch abgeschlossen werden.
* Der Connector wurde nicht ordnungsgemäß definiert.

**Lösung**: 

* Wenn eine neue ITSM-Connectorinstanz erstellt wird, beginnt sie mit der Synchronisierung von Informationen aus dem ITSM-System. Zu den synchronisierten Informationen zählen beispielsweise Arbeitselementvorlagen und Arbeitselemente. [Synchronisieren Sie den ITSM-Connector, um ein neues Aktualisierungstoken zu generieren.](./itsmc-resync-servicenow.md)
* [Überprüfen Sie Ihre Verbindungsdetails im ITSM-Connector](./itsmc-connections-servicenow.md#create-a-connection), und vergewissern Sie sich, dass der ITSM-Connector erfolgreich [synchronisiert](./itsmc-resync-servicenow.md) werden kann.


## <a name="ip-restrictions"></a>IP-Einschränkungen
**Fehler:** „Failed to add ITSM Connection named „XXX“ due to Bad Request. Error: Bad request. Invalid parameters provided for connection. Http Exception: Status Code Forbidden.“ (Fehler beim Hinzufügen der ITSM-Verbindung „XXX“ aufgrund einer ungültigen Anforderung. Fehler: Ungültige Anforderung. Für die Verbindung wurden ungültige Parameter angegeben. HTTP-Ausnahme: Statuscode „Unzulässig“.)

**Ursache:** Die IP-Adresse der ITSM-Anwendung lässt keine ITSM-Verbindungen von Partner-ITSM-Tools zu.

**Lösung:** Wenn Sie die ITSM-IP-Adressen auflisten möchten, um ITSM-Verbindungen von Partner-ITSM-Tools zuzulassen, empfiehlt es sich, den gesamten öffentlichen IP-Adressbereich der Azure-Region aufzulisten, zu der der entsprechende Log Analytics-Arbeitsbereich gehört. [Weitere Informationen dazu finden Sie hier.](https://www.microsoft.com/download/details.aspx?id=56519) Für die Regionen „Europa, Süden“, „Europa, Westen“, „Europa, Süden 2“, „USA, Westen 2“ und „USA, Süden-Mitte“ können Kunden nur das Netzwerktag „ActionGroup“ auflisten.
