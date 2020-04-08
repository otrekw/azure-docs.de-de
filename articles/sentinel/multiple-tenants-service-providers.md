---
title: Arbeiten mit mehreren Mandanten in Azure Sentinel für MSSP-Dienstanbieter | Microsoft-Dokumentation
description: Informationen zum Arbeiten mit mehreren Mandanten in Azure Sentinel für MSSP-Dienstanbieter.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: fdb58686fcdd18a8e2861aab533717dbc91e8893
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476014"
---
# <a name="work-with-multiple-tenants-in-azure-sentinel"></a>Arbeiten mit mehreren Mandanten in Azure Sentinel 

Wenn Sie ein Dienstanbieter für verwaltete Sicherheit (MSSP) sind und Sie [Azure Lighthouse](../lighthouse/overview.md) zum Verwalten der Security Operations Center (SOC) Ihrer Kunden verwenden, können Sie die Azure Sentinel-Ressourcen Ihrer Kunden verwalten, ohne von Ihrem eigenen Azure-Mandanten aus eine direkte Verbindung mit dem Mandanten des Kunden herzustellen. 

## <a name="prerequisites"></a>Voraussetzungen
- [Onboarding von Azure Lighthouse](../lighthouse/how-to/onboard-customer.md)
- Damit dies ordnungsgemäß funktioniert, muss Ihr Mandant für mindestens ein Abonnement beim Azure Sentinel-Ressourcenanbieter registriert sein. Wenn Sie über einen registrierten Azure Sentinel in Ihrem Mandanten verfügen, sind Sie bereit für die ersten Schritte. Falls nicht, wählen Sie im Azure-Portal **Abonnements** und anschließend **Ressourcenanbieter** aus.  Suchen Sie dann auf dem Bildschirm **SOC – Ressourcenanbieter** nach `Microsoft.OperationalInsights` und `Microsoft.SecurityInsights`, und wählen Sie **Registrieren** aus.
   ![Überprüfen von Ressourcenanbietern](media/multiple-tenants-service-providers/check-resource-provider.png)
## <a name="how-to-access-azure-sentinel-from-other-tenants"></a>Zugreifen auf Azure Sentinel über andere Mandanten
1. Wählen Sie unter **Verzeichnis + Abonnement** die delegierten Verzeichnisse und die Abonnements aus, in denen sich die Azure Sentinel-Arbeitsbereiche Ihres Kunden befinden.

   ![Erstellen von Sicherheitsincidents](media/multiple-tenants-service-providers/directory-subscription.png)

1. Öffnen Sie Azure Sentinel. Es werden alle Arbeitsbereiche in den ausgewählten Abonnements angezeigt, und Sie können problemlos wie in jedem anderen Arbeitsbereich Ihres eigenen Mandanten damit arbeiten.

> [!NOTE]
> Sie können Connectors in Azure Sentinel nicht von einem verwalteten Arbeitsbereich aus bereitstellen. Um einen Connector bereitzustellen, müssen Sie sich direkt bei dem Mandanten anmelden, auf dem Sie einen Connector bereitstellen möchten, und sich dort mit den erforderlichen Berechtigungen authentifizieren.





## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie mehrere Azure Sentinel-Mandanten problemlos verwalten können. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).

