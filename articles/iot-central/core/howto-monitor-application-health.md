---
title: Überwachen der Integrität einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Überwachen Sie als Bediener oder Administrator die Gesamtintegrität der mit Ihrer IoT Central-Anwendung verbundenen Geräte.
author: dominicbetts
ms.author: dobett
ms.date: 01/27/2021
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: df89d53e6b5043c1ef3caa1c92f2abaae542d6ec
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107599008"
---
# <a name="monitor-the-overall-health-of-an-iot-central-application"></a>Überwachen der Gesamtintegrität einer IoT Central-Anwendung

> [!NOTE]
> Metriken sind nur für IoT Central-Anwendungen der Version 3 verfügbar. Informationen zum Überprüfen Ihrer Anwendungsversion finden Sie unter [Über Ihre Anwendung](./howto-get-app-info.md).

*Dieser Artikel gilt für Bediener und Administratoren.*

In diesem Artikel erfahren Sie, wie Sie mithilfe der von IoT Central bereitgestellten Metriken die Integrität von mit Ihrer IoT Central-Anwendung verbundenen Geräten sowie die Integrität Ihrer aktiven Datenexporte bewerten.

Metriken sind für Ihre IoT Central-Anwendung standardmäßig aktiviert, und Sie greifen über das [Azure-Portal](https://portal.azure.com/) darauf zu. Die [Azure Monitor-Datenplattform macht diese Metriken verfügbar](../../azure-monitor/essentials/data-platform-metrics.md) und bietet Ihnen mehrere Möglichkeiten zur Interaktion damit. So können Sie beispielsweise Diagramme im Azure-Portal, eine REST-API oder aber Abfragen in PowerShell oder in der Azure CLI verwenden.

### <a name="trial-applications"></a>Testanwendungen

Weil Anwendungen, die den kostenlosen Testplan verwenden, kein Azure-Abonnement zugeordnet wird, unterstützen sie keine Azure Monitor-Metriken. Sie können [eine Anwendung in einen Standard-Tarif konvertieren](./howto-view-bill.md#move-from-free-to-standard-pricing-plan) und Zugriff auf diese Metriken erhalten.

## <a name="view-metrics-in-the-azure-portal"></a>Anzeigen von Metriken im Azure-Portal

Bei den folgenden Schritten wird davon ausgegangen, dass Sie über eine [IoT Central-Anwendung](./quick-deploy-iot-central.md) mit einigen [verbundenen Geräten](./tutorial-connect-device.md) oder über einen aktiven [Datenexport](howto-export-data.md) verfügen.

So zeigen Sie IoT Central-Metriken im Portal an:

1. Navigieren Sie im Portal zu Ihrer IoT Central-Anwendungsressource. IoT Central-Ressourcen befinden sich standardmäßig in der Ressourcengruppe **IOTC**.
1. Wenn Sie ein Diagramm aus den Metriken Ihrer Anwendung erstellen möchten, wählen Sie im Abschnitt **Überwachung** die Option **Metriken** aus.

![Azure-Metriken](media/howto-monitor-application-health/metrics.png)

### <a name="azure-portal-permissions"></a>Azure-Portalberechtigungen

Der Zugriff auf Metriken im Azure-Portal wird von der [rollenbasierten Zugriffssteuerung in Azure](../../role-based-access-control/overview.md) verwaltet. Verwenden Sie das Azure-Portal, um der/dem IoT Central-Anwendung/-Ressourcengruppe/-Abonnement Benutzer hinzuzufügen, damit ihnen Zugriff gewährt wird. Sie müssen einen Benutzer im Portal auch dann hinzufügen, wenn er der IoT Central-Anwendung bereits hinzugefügt wurde. Verwenden Sie [integrierte Azure-Rollen](../../role-based-access-control/built-in-roles.md) für eine präzisere Zugriffssteuerung.

## <a name="iot-central-metrics"></a>IoT Central-Metriken

Eine Liste der aktuell für IoT Central verfügbaren Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](../../azure-monitor/essentials/metrics-supported.md#microsoftiotcentraliotapps).

### <a name="metrics-and-invoices"></a>Metriken und Rechnungen

Metriken können von den Zahlen abweichen, die in Ihrer Azure IoT Central-Rechnung angezeigt werden. Für diese Situation gibt es verschiedene Ursachen, beispielsweise:

- Die [Standard-Tarife](https://azure.microsoft.com/pricing/details/iot-central/) von IoT Central enthalten zwei Geräte und kostenlose variierende Nachrichtenkontingente. Die kostenlosen Elemente sind in der Abrechnung nicht enthalten, werden in den Metriken aber dennoch gezählt.

- IoT Central generiert automatisch eine einzige Testgeräte-ID für jede Gerätevorlage in der Anwendung. Diese Geräte-ID wird auf der Seite **Testgerät verwalten** für eine Gerätevorlage angezeigt. Lösungsentwickler können wahlweise ihre Gerätevorlagen überprüfen, bevor sie sie veröffentlichen. Dazu generieren sie Code, in dem diese Testgeräte-IDs verwendet werden. Diese Geräte sind in der Abrechnung nicht enthalten, werden in den Metriken aber dennoch gezählt.

- Während Metriken möglicherweise eine Teilmenge der Gerät-zu-Cloud-Kommunikation zeigen, wird die gesamte Kommunikation zwischen dem Gerät und der Cloud [als Nachricht zur Abrechnung gezählt](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie Sie Anwendungsvorlagen verwenden. Informieren Sie sich als Nächstes, wie Sie [IoT Central über das Azure-Portal verwalten](howto-manage-iot-central-from-portal.md).