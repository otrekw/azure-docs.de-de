---
title: Erstellen einer IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel werden die Optionen zum Erstellen einer IoT Central-Anwendung beschrieben, z. B. über die Azure IoT Central-Website, das Azure-Portal und über eine Befehlszeilenumgebung.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 05/11/2021
ms.topic: how-to
ms.openlocfilehash: 956e84e8eb7281541fbb787a6ce7557a1f5fee03
ms.sourcegitcommit: e832f58baf0b3a69c2e2781bd8e32d4f1ae932c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110586490"
---
# <a name="create-an-iot-central-application"></a>Erstellen einer IoT Central-Anwendung

Sie haben mehrere Möglichkeiten, eine IoT Central-Anwendung zu erstellen. Sie können eine der GUI-basierten Methoden verwenden, wenn Sie einen manuellen Ansatz bevorzugen, oder eine der CLI- oder programmgesteuerten Methoden nutzen, wenn Sie den Prozess automatisieren möchten.

Unabhängig davon, welchen Ansatz Sie wählen, sind die Konfigurationsoptionen identisch, und der Prozess dauert in der Regel weniger als eine Minute.

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

## <a name="options"></a>Optionen

In diesem Abschnitt werden die verfügbaren Optionen beschrieben, wenn Sie eine IoT Central-Anwendung erstellen. Je nach der von Ihnen festgelegten Methode müssen Sie möglicherweise die Optionen in einem Formular oder als Befehlszeilenparameter angeben:

### <a name="pricing-plans"></a>Tarife

Mit dem *kostenlosen* Tarif können Sie eine IoT Central-Anwendung erstellen, die Sie sieben Tage lang testen können. Kostenloser Tarif:

- Erfordert kein Azure-Abonnement.
- Kann nur über die [Azure IoT Central](https://aka.ms/iotcentral)-Website erstellt und verwaltet werden.
- Ermöglicht den Anschluss von bis zu fünf Geräten.
- Wenn Sie die Anwendung behalten möchten, ist ein Upgrade auf einen Standardtarif möglich.

*Standardtarife*:

- Erfordern ein Azure-Abonnement. Sie sollten in Ihrem Azure-Abonnement mindestens über Zugriff als **Mitwirkender** verfügen. Wenn Sie das Abonnement selbst erstellt haben, sind Sie automatisch Administrator mit ausreichendem Zugriff. Weitere Informationen finden Sie in folgendem Artikel: [Was ist die rollenbasierte Zugriffssteuerung in Azure?](../../role-based-access-control/overview.md).
- Ermöglicht die Erstellung und Verwaltung von IoT Central-Anwendungen über eine der verfügbaren Methoden.
- Ermöglicht den Anschluss von beliebig vielen Geräten. Die Abrechnung erfolgt pro Gerät. Weitere Informationen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).
- Kann nicht auf einen kostenlosen Tarif herabgestuft werden, aber ein Upgrade oder Downgrade auf andere Standardtarife ist möglich.

In der folgenden Tabelle werden die Unterschiede zwischen den drei Standardtarifen erläutert:

| Planname | Kostenlose Geräte | Nachrichten/Monat | Anwendungsfall |
| --------- | ------------ | -------------- | -------- |
| S0        | 2            | 400            | Einige Nachrichten pro Tag |
| S1        | 2            | 5\.000          | Einige Nachrichten pro Stunde |
| S2        | 2            | 30.000         | Nachrichten alle paar Minuten |

Weitere Informationen finden Sie unter [Verwalten Ihrer Rechnung in einer IoT Central-Anwendung](howto-view-bill.md).

### <a name="application-name"></a>Anwendungsname

Der von Ihnen gewählte _Anwendungsname_ wird auf jeder Seite in Ihrer IoT Central-Anwendung in der Titelleiste angezeigt. Sie wird auch auf der Kachel Ihrer Anwendung auf der Seite **Meine Apps** auf der [Azure IoT Central](https://aka.ms/iotcentral)-Website angezeigt.

Die von Ihnen ausgewählte _Unterdomäne_ identifiziert Ihre Anwendung eindeutig. Die Unterdomäne ist Teil der URL, die Sie für den Zugriff auf die Anwendung verwenden. Die URL für eine IoT Central-Anwendung sieht wie `https://yoursubdomain.azureiotcentral.com` aus.

### <a name="application-template-id"></a>Anwendungsvorlagen-ID

Die von Ihnen gewählte Anwendungsvorlage bestimmt den ursprünglichen Inhalt Ihrer Anwendung, z. B. Dashboards und Gerätevorlagen. Vorlagen-ID: Für eine benutzerdefinierte Anwendung verwenden Sie `iotc-pnp-preview` als Vorlagen-ID.

Weitere Informationen zu benutzerdefinierten und branchenorientierten Anwendungsvorlagen finden Sie unter [Was sind Anwendungsvorlagen?](concepts-app-templates.md).

### <a name="billing-information"></a>Abrechnungsinformationen

Wenn Sie einen der Standardtarife auswählen, müssen Sie Abrechnungsinformationen angeben:

- Das Azure-Abonnement, das Sie verwenden.
- Das Verzeichnis, das das von Ihnen verwendete Abonnement enthält.
- Der Speicherort zum Hosten Ihrer Anwendung. IoT Central verwendet Azure-Geografien als Speicherorte: USA, Europa, Asien-Pazifik, Australien, Vereinigtes Königreich oder Japan.

## <a name="azure-iot-central-site"></a>Azure IoT Central-Website

Die einfachste Möglichkeit, um mit dem Erstellen von IoT Central-Anwendungen zu beginnen, stellt die [Azure IoT Central](https://aka.ms/iotcentral)-Website dar.

Im [Build](https://apps.azureiotcentral.com/build) können Sie die Anwendungsvorlage auswählen, die Sie verwenden möchten:

:::image type="content" source="media/howto-create-iot-central-application/choose-template.png" alt-text="Screenshot der Buildseite, auf der Sie eine Anwendungsvorlage auswählen können":::

Wenn Sie **App erstellen** auswählen, können Sie die erforderlichen Informationen zum Erstellen einer Anwendung aus der Vorlage bereitstellen:

:::image type="content" source="media/howto-create-iot-central-application/create-application.png" alt-text="Screenshot der Seite „App erstellen“ für IoT Central":::

Auf der Seite **Meine Apps** werden alle IoT Central-Anwendungen aufgeführt, auf die Sie Zugriff haben. Die Liste enthält Anwendungen, die Sie erstellt haben, und Anwendungen, für die Ihnen der Zugriff erteilt wurde.

> [!TIP]
> Alle Anwendungen, die Sie mit einem Standardtarif auf der Azure IoT Central-Website erstellen, verwenden die **IOTC**-Ressourcengruppe in Ihrem Abonnement. Mit den im folgenden Abschnitt beschriebenen Ansätzen können Sie eine zu verwendende Ressourcengruppe auswählen.

## <a name="other-approaches"></a>Andere Ansätze

Sie können auch die folgenden Ansätze verwenden, um eine IoT Central-Anwendung zu erstellen:

- [Erstellen einer IoT Central-Anwendung über das Azure-Portal](howto-manage-iot-central-from-portal.md#create-iot-central-applications)
- [Erstellen einer IoT Central-Anwendung mithilfe der Azure CLI](howto-manage-iot-central-from-cli.md#create-an-application)
- [Erstellen einer IoT Central-Anwendung mithilfe von PowerShell](howto-manage-iot-central-from-powershell.md#create-an-application)
- [Programmgesteuertes Erstellen einer IoT Central-Anwendung](howto-manage-iot-central-programmatically.md)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über die Azure CLI verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)
