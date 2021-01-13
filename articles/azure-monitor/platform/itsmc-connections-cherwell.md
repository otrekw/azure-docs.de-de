---
title: Verbinden von Cherwell mit dem ITSM-Connector
description: Dieser Artikel enthält Informationen dazu, wie Sie Cherwell mit dem ITSM-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 73fc13cf2a49d7cacd7540d06c6d0afd9cea68e5
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729550"
---
# <a name="connect-cherwell-with-it-service-management-connector"></a>Verbinden von Cherwell mit dem ITSM-Connector

Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrer Cherwell-Instanz und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten.

> [!NOTE]
> Unseren Cherwell- und Provance-Kunden schlagen wir die Verwendung der [Webhookaktion](./action-groups.md#webhook) für Cherwell- und Provance-Endpunkte als weitere Lösung für die Integration vor.

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Cherwell-Produkts mit dem ITSMC in Azure.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](./itsmc-definition.md#add-it-service-management-connector).
- Client-ID wurde generiert. Weitere Informationen: [Generieren der Client-ID für Cherwell](#generate-client-id-for-cherwell).
- Benutzerrolle:  Administrator.

## <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Cherwell-Verbindung zu erstellen:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2. Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die Cherwell-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **Cherwell** aus. |
| **Benutzername**   | Geben Sie den Namen des Cherwell-Benutzers ein, der eine Verbindung mit ITSMC herstellen kann. |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis:** Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert.|
| **Server-URL**   | Geben Sie die URL für die Cherwell-Instanz ein, die Sie mit ITSMC verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Cherwell-Instanz generiert haben.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die Cherwell-Arbeitselemente aus, die Sie über ITSMC synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:**  Incidents, Änderungsanforderungen. |
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

![Cherwell-Verbindung](media/itsmc-connections/itsm-connections-cherwell-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der Cherwell-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Cherwell-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Generieren der Client-ID für Cherwell

Führen Sie folgende Schritte aus, um die Client-ID/den Schlüssel für Cherwell zu generieren:

1. Melden Sie sich als Administrator bei Ihrer Cherwell-Instanz an.
2. Klicken Sie auf **Sicherheit** > **Edit REST API client settings (REST-API-Clienteinstellungen bearbeiten)** .
3. Wählen Sie **Create new client (Neuen Client erstellen)**  > **Geheimer Clientschlüssel**.

    ![Cherwell-Benutzer-ID](media/itsmc-connections/itsmc-cherwell-client-id.png)

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über den ITSM-Connector](itsmc-overview.md)
* [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)