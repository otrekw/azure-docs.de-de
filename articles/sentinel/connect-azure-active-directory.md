---
title: Verknüpfen von Azure Active Directory-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten aus Azure Active Directory sammeln und Azure AD-Anmeldeprotokolle und -Überwachungsprotokolle in Azure Sentinel streamen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/20/2021
ms.author: yelevin
ms.openlocfilehash: eb89d2a4e719e34ad5ea31656dc9e3c02472b07d
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98802251"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Verknüpfen von Daten aus Azure Active Directory (Azure AD)

Sie können den in Azure Sentinel integrierten Connector verwenden, um Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) zu erfassen und in Azure Sentinel zu streamen. Der Connector ermöglicht es Ihnen, [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Überwachungsprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) zu streamen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/)-Abonnement, um Anmeldeprotokolle in Azure Sentinel erfassen zu können. Für Azure Monitor (Log Analytics) und Azure Sentinel können zusätzliche Gebühren auf Gigabytebasis anfallen.

- Ihrem Benutzer muss die Rolle „Azure Sentinel-Mitwirkender“ für den Arbeitsbereich zugewiesen sein.

- Ihrem Benutzer muss die Rolle „Globaler Administrator“ oder „Sicherheitsadministrator“ für den Mandanten zugewiesen sein, von dem aus Sie die Protokolle streamen möchten.

- Ihr Benutzer muss über Lese- und Schreibberechtigungen für die Azure AD-Diagnoseeinstellungen verfügen, um den Verbindungsstatus sehen zu können. 

## <a name="connect-to-azure-active-directory"></a>Herstellen einer Verbindung mit Azure Active Directory

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Katalog für Datenconnectors **Azure Active Directory** aus, und klicken Sie anschließend auf **Open connector page** (Connectorseite öffnen).

1. Aktivieren Sie die Kontrollkästchen neben den Protokolltypen, die Sie in Azure Sentinel streamen möchten, und klicken Sie auf **Verbinden**. Zwischen diesen Protokolltypen können Sie wählen:

    - **Anmeldeprotokolle**: Informationen zur Nutzung von verwalteten Anwendungen und Benutzeranmeldungsaktivitäten.
    - **Überwachungsprotokolle**: Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung, zu verwalteten Anwendungen und Verzeichnisaktivitäten.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter **LogManagement** in den folgenden Tabellen angezeigt:

- `SigninLogs`
- `AuditLogs`

Geben Sie zum Abfragen der Azure AD-Protokolle im oberen Bereich des Abfragefensters den Tabellennamen ein.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Azure Active Directory mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
