---
title: Verbinden von Provance mit dem ITSM-Connector
description: Dieser Artikel enthält Informationen dazu, wie Sie Provance mit dem ITSM-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 38aff6bf0851e97558fb8870a08f33bb0bcb0392
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601807"
---
# <a name="connect-provance-with-it-service-management-connector"></a>Verbinden von Provance mit dem ITSM-Connector

Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrer Provance-Instanz und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten.

> [!NOTE]
> Ab dem 1. Oktober 2020 ist die ITSM-Integration von Provance mit Azure-Benachrichtigungen für neue Kunden nicht mehr aktiviert. Neue ITSM-Verbindungen werden nicht unterstützt.
> Bestehende ITSM-Verbindungen werden unterstützt.

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Provance-Produkts mit dem ITSMC in Azure.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](./itsmc-definition.md#add-it-service-management-connector).
- Die Provance-App muss bei Azure AD registriert sein, und die Client-ID muss verfügbar gemacht werden. Ausführliche Informationen finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../../app-service/configure-authentication-provider-aad.md).

- Benutzerrolle:  Administrator.

## <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Provance-Verbindung zu erstellen:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2. Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-overview/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die Provenance-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **Provance** aus. |
| **Benutzername**   | Geben Sie den Namen des Benutzers ein, der eine Verbindung mit ITSMC herstellen kann.    |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis:** Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert.|
| **Server-URL**   | Geben Sie die URL der Provenance-Instanz ein, die Sie mit ITSMC verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Provance-Instanz generiert haben.  Weitere Informationen zur Client-ID finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../../app-service/configure-authentication-provider-aad.md). |
| **Datensynchronisierungsbereich**   | Wählen Sie die Provance-Arbeitselemente aus, die Sie über den ITSMC mit Azure Log Analytics synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:**   Incidents, Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert.|

![Screenshot mit hervorgehobenen Listen mit Verbindungsnamen und Partnertypen.](media/itsmc-connections-provance/itsm-connections-provance-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der Provance-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Provance-Instanz erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über den ITSM-Connector](itsmc-overview.md)
* [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)