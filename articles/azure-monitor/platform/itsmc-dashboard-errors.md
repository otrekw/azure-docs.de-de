---
title: Häufige Fehler
description: Dieses Dokument enthält Informationen zu häufigen Fehlern, die Sie im Dashboard finden können.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: 7240c1b0f19dc49ab4130c5ee2516dcfefb2e2c2
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98602185"
---
# <a name="errors-in-the-connector-status"></a>Fehler im Connectorstatus

Die Connectorstatusliste kann Fehler enthalten, die Ihnen beim Beheben von Problemen in Ihrem ITSM-Connector behilflich sein können.

## <a name="status-common-errors"></a>Allgemeine Fehler im Status

Dieser Abschnitt enthält die allgemeinen Fehler, die im Abschnitt mit dem Connectorstatus angegeben werden, sowie Informationen zu deren Behebung:

* **Fehler:** „Unerwartete Antwort von ServiceNow zusammen mit Erfolgsstatuscode. Antwort: { "import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "result": [ { "transform_map": "OMS Incident", "table": "incident", "status": "error", "error_message": "{Target record not found|Invalid table|Invalid staging table" }“

    **Ursache:** Dieser Fehler wird von ServiceNow in folgenden Fällen zurückgegeben:
  * Ein in der ServiceNow-Instanz bereitgestelltes benutzerdefiniertes Skript bewirkt, dass Incidents ignoriert werden.
  * Der „OMS Integrator App"-Code selbst wurde auf ServiceNow-Seite geändert, z. B. das onBefore-Skript.

    **Lösung:** Deaktivieren Sie alle benutzerdefinierten Skripts oder Codeänderungen des Datenimportpfads.

* **Fehler**: „{"error":{"message":"Operation Failed","detail":"ACL Exception Update Failed due to security constraints"}“

    **Ursache:** Fehlerhafte Konfiguration von ServiceNow-Berechtigungen

    **Lösung:** Überprüfen Sie, ob alle Rollen ordnungsgemäß wie [angegeben](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role) zugewiesen wurden.

* **Fehler:** „Fehler beim Senden der Anforderung.“

    **Ursache:** „ServiceNow-Instanz nicht verfügbar“

    **Lösung:** Überprüfen Sie Ihre Instanz in ServiceNow. Diese ist möglicherweise gelöscht oder nicht verfügbar.

* **Fehler:** „ServiceDeskHttpBadRequestException: StatusCode=429“

    **Ursache:** Die ServiceNow-Ratenbegrenzungen sind zu hoch oder zu niedrig.

    **Lösung:** Erhöhen Sie die Ratenbegrenzungen in der ServiceNow-Instanz oder heben Sie die Begrenzungen auf wie [hier](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html) erläutert.

* **Fehler:** „AccessToken und RefreshToken sind ungültig. Der Benutzer muss sich erneut authentifizieren.“

    **Ursache:** Das Aktualisierungstoken ist abgelaufen.

    **Lösung:** Synchronisieren Sie den ITSM-Connector, um ein neues Aktualisierungstoken zu generieren wie [hier](./itsmc-resync-servicenow.md) erläutert.

* **Fehler:** „Für die Warnung {alertName} konnte kein Arbeitselement erstellt oder aktualisiert werden. Der ITSM-Connector {connectionIdentifier} ist nicht vorhanden oder wurde gelöscht.“

    **Ursache:** Der ITSM-Connector wurde gelöscht.

    **Lösung:** Der ITSM-Connector wurde gelöscht, es sind aber noch zugeordnete ITSM-Aktionsgruppen definiert. Es gibt zwei Möglichkeiten zur Behebung dieses Problems:
  * Suchen und Deaktivieren oder Löschen dieser Aktion
  * [Konfigurieren Sie die Aktionsgruppe neu,](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts) um einen vorhandenen ITSM-Connector zu verwenden.
  * [Erstellen Sie einen neuen ITSM-Connector](./itsmc-definition.md#create-an-itsm-connection), und [konfigurieren Sie die Aktionsgruppe neu, damit sie diesen verwendet](itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="ui-common-errors"></a>Allgemeine Fehler auf der Benutzeroberfläche

* **Fehler**: „Leider ist ein Problem aufgetreten. Die Verbindungsdetails konnten nicht abgerufen werden.“ Dieser Fehler wird angezeigt, wenn der Kunde die ITSM-Aktionsgruppe definiert.

    **Ursache:** Für einen neu erstellten ITSM-Connector muss die anfängliche Synchronisierung noch abgeschlossen werden.

    **Lösung:** Wenn ein neuer ITSM-Connector erstellt wird, beginnt der ITSM-Connector mit der Synchronisierung von Informationen aus dem ITSM-System, z. B. Arbeitselementvorlagen und Arbeitselemente. Synchronisieren Sie den ITSM-Connector, um ein neues Aktualisierungstoken zu generieren wie [hier](./itsmc-resync-servicenow.md) erläutert.
