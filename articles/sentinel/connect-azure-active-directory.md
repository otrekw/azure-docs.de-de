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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 37106517c47c86f4a4a562eebd6d120e31e22334
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564525"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Verknüpfen von Daten aus Azure Active Directory (Azure AD)



Mit Azure Sentinel können Sie Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) sammeln und in Azure Sentinel streamen. Sie können wählen, ob Sie [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Auditprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) streamen möchten.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Anmeldedaten aus Azure AD exportieren möchten, müssen Sie über eine P1- oder P2-Lizenz für Azure AD verfügen.

- Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen für den Mandanten, von dem aus Sie die Protokolle streamen möchten.

- Um den Verbindungsstatus sehen zu können, müssen Sie die Berechtigung zum Zugreifen auf Azure AD-Diagnoseprotokolle haben. 


## <a name="connect-to-azure-active-directory"></a>Herstellen einer Verbindung mit Azure Active Directory

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Katalog für Datenconnectors **Azure Active Directory** aus, und klicken Sie anschließend auf die Schaltfläche **Connectorseite öffnen**.

1. Aktivieren Sie die Kontrollkästchen neben den Protokollen, die Sie in Azure Sentinel streamen möchten, und klicken Sie auf **Verbinden**.

1. Sie können auswählen, ob die Warnungen von Azure AD automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

1. Geben Sie `SigninLogs` oder `AuditLogs` in das Abfragefenster ein, um das entsprechende Schema in Log Analytics zum Abfragen von Azure AD-Warnungen zu verwenden.




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Azure Active Directory mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
