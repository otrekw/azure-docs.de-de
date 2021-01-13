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
ms.date: 08/11/2020
ms.author: yelevin
ms.openlocfilehash: 279f54c3de964580cc37d1288a6e1b7726348e10
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88208611"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Verknüpfen von Daten aus Azure Active Directory (Azure AD)



Sie können den in Azure Sentinel integrierten Connector verwenden, um Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) zu erfassen und in Azure Sentinel zu streamen. Der Connector ermöglicht es Ihnen, [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Überwachungsprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) zu streamen.

## <a name="prerequisites"></a>Voraussetzungen


- Jede Azure AD-Lizenz (Free/O365/P1/P2) ist für das Erfassen von Anmeldeprotokollen in Azure Sentinel ausreichend. Für Azure Monitor (Log Analytics) und Azure Sentinel können zusätzliche Gebühren auf Gigabytebasis anfallen.

- Ihrem Benutzer muss die Rolle „Azure Sentinel-Mitwirkender“ für den Arbeitsbereich zugewiesen sein.

- Ihrem Benutzer muss die Rolle „Globaler Administrator“ oder „Sicherheitsadministrator“ für den Mandanten zugewiesen sein, von dem aus Sie die Protokolle streamen möchten.

- Ihr Benutzer muss über Lese- und Schreibberechtigungen für die Azure AD-Diagnoseeinstellungen verfügen, um den Verbindungsstatus sehen zu können. 


## <a name="connect-to-azure-active-directory"></a>Herstellen einer Verbindung mit Azure Active Directory

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Katalog für Datenconnectors **Azure Active Directory** aus, und klicken Sie anschließend auf **Open connector page** (Connectorseite öffnen).

1. Aktivieren Sie die Kontrollkästchen neben den Protokollen, die Sie in Azure Sentinel streamen möchten, und klicken Sie auf **Verbinden**.

1. Sie können auswählen, ob die Warnungen von Azure AD automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Create incidents** (Incidents erstellen) die Option **Aktivieren** aus, um die Standardanalyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

1. Geben Sie `SigninLogs` oder `AuditLogs` in das Abfragefenster ein, um das entsprechende Schema in Log Analytics zum Abfragen von Azure AD-Warnungen zu verwenden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Azure Active Directory mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
