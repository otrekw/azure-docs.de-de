---
title: Verbinden von Sophos Cloud Optix-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Sophos Cloud Optix-Connector <PRODUCT NAME>-Protokolle in Azure Sentinel einen Pull ausführen. Zeigen Sie <PRODUCT NAME>-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/26/2021
ms.author: yelevin
ms.openlocfilehash: c66205ffd9bd5a742d645cbf2f9251a44329a16e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743293"
---
# <a name="connect-your-sophos-cloud-optix-to-azure-sentinel"></a>Verbinden von Sophos Cloud Optix mit Azure Sentinel

> [!IMPORTANT]
> Der Sophos Cloud Optix-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Mit dem Connector für Sophos Cloud Optix können Sie alle Protokolle Ihrer Sophos Cloud Optix-Sicherheitslösungen einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern.  Mit dieser Funktion erhalten Sie einen besseren Einblick in den Cloudsicherheits- und Compliancestatus Ihres Unternehmens und verbessern Ihre Funktionen für die Cloudsicherheitsvorgänge. Die Integration von Sophos Cloud Optix und Azure Sentinel stützt sich auf die REST-API.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-sophos-cloud-optix"></a>Konfigurieren und Verbinden von Sophos Cloud Optix

Sophos Cloud Optix kann mit Azure Sentinel integriert werden und Protokolle dann direkt an Azure Sentinel exportieren.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Sophos Cloud Optix (Vorschau)** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Kopieren und speichern Sie die **Arbeitsbereichs-ID** und den **Primärschlüssel** auf der Connectorseite.

1. Befolgen Sie die Anweisungen von Sophos für die [Integration in Microsoft Azure Sentinel](https://docs.sophos.com/pcg/optix/help/en-us/pcg/optix/tasks/IntegrateAzureSentinel.html) (beginnend mit dem dritten Schritt).

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in der Tabelle *SophosCloudOptix_CL* im Abschnitt **CustomLogs** unter **Protokolle** angezeigt.

Um Sophos Cloud Optix-Daten abzufragen, geben Sie `SophosCloudOptix_CL` in das Abfragefenster ein. Auf der Registerkarte **Nächste Schritte** auf der Connectorseite und in der [Sophos-Dokumentation](https://docs.sophos.com/pcg/optix/help/en-us/pcg/optix/concepts/ExampleAzureSentinelQueries.html) finden Sie einige nützliche Beispiele für Abfragen.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Sophos Cloud Optix mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
