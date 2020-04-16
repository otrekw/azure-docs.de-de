---
title: 'Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen'
description: Machen Sie sich mit benutzerdefinierten Benachrichtigungen für den Azure Security Center für IoT-Sicherheitsdienst vertraut, und erfahren Sie, wie Sie diese Benachrichtigungen erstellen und zuweisen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2020
ms.author: mlottner
ms.openlocfilehash: 8ba81b669c25d91efa8ad1f07b1a7835b42c5cf4
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310830"
---
# <a name="quickstart-create-custom-alerts"></a>Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen

Mithilfe benutzerdefinierter Sicherheitsgruppen und Benachrichtigungen können Sie End-to-End-Sicherheitsinformationen und kategorische Geräteinformationen optimal nutzen, um die Sicherheit Ihrer gesamten IoT-Lösung zu verbessern.

## <a name="why-use-custom-alerts"></a>Vorteile benutzerdefinierter Benachrichtigungen

Sie kennen Ihre IoT-Geräte am besten.

Kunden, die umfassend mit dem erwarteten Verhalten ihrer Geräte vertraut sind, können mit Azure Security Center für IoT auf der Grundlage dieses Wissens eine Verhaltensrichtlinie für Geräte erstellen, sodass bei jeder Abweichung vom erwarteten Normalverhalten eine Benachrichtigung ausgelöst wird.

## <a name="security-groups"></a>Sicherheitsgruppen

Mithilfe von Sicherheitsgruppen können Sie logische Gruppen von Geräten definieren und deren Sicherheitsstatus zentral verwalten.

Diese Gruppen können Geräte mit bestimmter Hardware, bereitgestellte Geräte eines bestimmten Standorts oder eine beliebige andere Gruppe darstellen, die Ihren spezifischen Anforderungen entspricht.

Sicherheitsgruppen werden durch eine Tag-Eigenschaft namens **SecurityGroup** für Gerätezwillinge definiert. Standardmäßig besitzt jede IoT-Lösung in IoT Hub eine Sicherheitsgruppe namens **Standard**. Ändern Sie den Wert der Eigenschaft **SecurityGroup**, um die Sicherheitsgruppe eines Geräts zu ändern.

Beispiel:

```
{
  "deviceId": "VM-Contoso12",
  "etag": "AAAAAAAAAAM=",
  "deviceEtag": "ODA1BzA5QjM2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 4,
  "tags": {
    "SecurityGroup": "default"
  },
```

Verwenden Sie Sicherheitsgruppen, um Ihre Geräte in logischen Kategorien zu gruppieren. Die erstellten Gruppen können Sie dann den benutzerdefinierten Benachrichtigungen Ihrer Wahl zuweisen, um eine möglichst effektive End-to-End-IoT-Sicherheitslösung zu erhalten.

## <a name="customize-an-alert"></a>Anpassen einer Benachrichtigung

1. Öffnen Sie Ihre IoT Hub-Instanz.
1. Klicken Sie im Abschnitt **Sicherheit** auf **Benutzerdefinierte Benachrichtigungen**.
1. Wählen Sie eine Sicherheitsgruppe aus, auf die Sie die Anpassung anwenden möchten.
1. Klicken Sie auf **Add a custom alert** (Benutzerdefinierte Benachrichtigung hinzufügen).
1. Wählen Sie in der Dropdownliste eine benutzerdefinierte Benachrichtigung aus.
1. Bearbeiten Sie die erforderlichen Eigenschaften, und klicken Sie anschließend auf **OK**.
1. Klicken Sie auf **SPEICHERN**. Wenn Sie die neue Benachrichtigung nicht speichern, wird sie beim nächsten Schließen von IoT Hub gelöscht.

## <a name="alerts-available-for-customization"></a>Anpassbare Benachrichtigungen

Azure Security Center für IoT bietet eine Vielzahl von Benachrichtigungen, die gemäß Ihren individuellen Anforderungen angepasst werden können. Die [Tabelle zu anpassbaren Benachrichtigungen](concept-customizable-security-alerts.md) enthält den Schweregrad, die Datenquelle und eine Beschreibung sowie die empfohlenen Schritte, die im Falle der jeweiligen Benachrichtigung ausgeführt werden sollten.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie einen Sicherheits-Agent bereitstellen:

> [!div class="nextstepaction"]
> [Select and deploy a security agent on your IoT device](how-to-deploy-agent.md) (Auswählen und Bereitstellen eines Sicherheits-Agents auf Ihrem IoT-Gerät)
