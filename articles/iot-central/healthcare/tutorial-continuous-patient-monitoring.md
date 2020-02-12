---
title: Erstellen einer App für die ständige Überwachung von Patienten mit Azure IoT Central | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit Azure IoT Central-Anwendungsvorlagen eine Anwendung für die ständige Überwachung von Patienten erstellen.
author: philmea
ms.author: philmea
ms.date: 09/24/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 35ac39109bfcb4dc63b738c947d2ad8caf8ac0a6
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77021286"
---
# <a name="tutorial-deploy-and-walkthrough-a-continuous-patient-monitoring-app-template"></a>Tutorial: Bereitstellen einer App für die ständige Überwachung von Patienten und exemplarische Vorgehensweise für die zugehörige Vorlage



In diesem Tutorial wird für Sie als Lösungsentwickler veranschaulicht, wie Sie in dieses Thema einsteigen können, indem Sie eine IoT Central-Vorlage für eine Anwendung für die ständige Überwachung von Patienten bereitstellen. Sie erfahren, wie Sie die Vorlage bereitstellen, was im Lieferumfang enthalten ist und welche nächsten Schritte Sie ausführen können.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Anwendungsvorlage
> * Exemplarische Vorgehensweise für die Anwendungsvorlage

## <a name="create-an-application-template"></a>Erstellen einer Anwendungsvorlage

Navigieren Sie zur [Anwendungs-Manager-Website von Azure IoT Central](https://apps.azureiotcentral.com/). Wählen Sie in der linken Navigationsleiste **Erstellen** aus, und klicken Sie dann auf die Registerkarte **Gesundheitswesen**. 

>[!div class="mx-imgBorder"] 
>![Anwendungs-Manager: Gesundheitswesen](media/app-manager-health.png)

Klicken Sie auf die Schaltfläche **App erstellen**, um mit der Erstellung Ihrer Anwendung zu beginnen, und melden Sie sich dann mit einem persönlichen Microsoft-Konto oder einem Geschäfts-, Schul- oder Unikonto an. Sie gelangen zur Seite **Neue Anwendung**.

![Erstellen einer Anwendung: Gesundheitswesen](media/app-manager-health-create.png)

![Erstellen einer Anwendung: Abrechnungsinformationen im Bereich Gesundheitswesen](media/app-manager-health-create-billinginfo.png)

Erstellung Sie Ihre Anwendung wie folgt:

1. Azure IoT Central schlägt entsprechend der von Ihnen ausgewählten Vorlage automatisch einen Anwendungsnamen vor. Sie können diesen Namen übernehmen oder einen eigenen aussagekräftigen Anwendungsnamen eingeben, z. B. **Ständige Überwachung von Patienten**. Azure IoT Central generiert für Sie außerdem entsprechend dem Anwendungsnamen ein Präfix mit einer eindeutigen URL. Sie können dieses URL-Präfix in einen einprägsameren Wert ändern.

2. Sie können auswählen, ob Sie die Anwendung unter dem Tarif *Free* oder unter einem der Tarife vom Typ *Standard* erstellen möchten. Anwendungen, die Sie unter dem Free-Tarif erstellen, sind sieben Tage lang kostenlos, bevor sie ablaufen, und es sind bis zu fünf kostenlose Geräte zulässig. Sie können eine Anwendung vor dem Ablauf jederzeit vom Free-Tarif auf einen Standard-Tarif umstellen. Bei Wahl des Free-Tarifs müssen Sie Ihre Kontaktdaten eingeben und auswählen, ob Sie Informationen und Tipps von Microsoft erhalten möchten. Anwendungen, die Sie unter einem Standard-Tarif erstellen, unterstützen bis zu zwei kostenlose Geräte, und Sie müssen Ihre Informationen zum Azure-Abonnement für die Abrechnung eingeben.

3. Wählen Sie unten auf der Seite die Option **Erstellen** aus, um Ihre Anwendung bereitzustellen.

## <a name="walk-through-the-application-template"></a>Exemplarische Vorgehensweise für die Anwendungsvorlage

### <a name="dashboards"></a>Dashboards

Nach der Bereitstellung der App-Vorlage gelangen Sie zunächst zum **Lamna-Dashboard für die Überwachung interner Patienten**. Lamna Healthcare ist ein fiktives Krankenhaus mit zwei Einrichtungen: Woodgrove Hospital und Burkville Hospital. Dieses Bedienerdashboard für Woodgrove Hospital enthält Informationen und Telemetriedaten zu den Geräten dieser Vorlage sowie die verfügbaren Befehle, Aufträge und Aktionen. Über das Dashboard können Sie Folgendes durchführen:

* Zeigen Sie Gerätetelemetriedaten und -eigenschaften an, z. B. den **Akkustand** oder den **Konnektivitätsstatus** Ihres Geräts.

* Zeigen Sie den **Raumplan** und den Standort des Smart Vitals Patch-Geräts an.

* Führen Sie die **erneute Bereitstellung** des Smart Vitals Patch-Geräts für einen neuen Patienten durch.

* Zeigen Sie ein Beispiel für ein **Anbieterdashboard** an, das von einem Pflegeteam des Krankenhauses ggf. zum Nachverfolgen der Patienten genutzt wird.

* Ändern Sie den **Patientenstatus** Ihres Geräts, um anzugeben, ob das Gerät für einen internen oder externen Patienten verwendet wird.

>[!div class="mx-imgBorder"] 
>![Lamna: Interne Patienten](media/lamna-in-patient.png)

Sie können auch auf **Go to remote patient dashboard** (Zu Dashboard für externe Patienten wechseln) klicken, um das zweite Bedienerdashboard anzuzeigen, das für das Burkville Hospital genutzt wird. Dieses Dashboard enthält ähnliche Aktionen, Telemetriedaten und Informationen. Darüber hinaus sehen Sie, dass mehrere Geräte verwendet werden und dass Sie jeweils die **Firmware aktualisieren** können.

>[!div class="mx-imgBorder"] 
>![Lamna: Externe Patienten](media/lamna-remote.png)

In beiden Dashboards können Sie jeweils auf diese Dokumentation zugreifen.

### <a name="device-templates"></a>Gerätevorlagen

Wenn Sie auf die Registerkarte **Gerätevorlagen** klicken, werden für die Vorlage zwei unterschiedliche Gerätetypen angezeigt:

* **Smart Vitals Patch**: Bei diesem Gerät handelt es sich um ein Patchgerät, mit dem verschiedene lebenswichtige Werte gemessen werden. Es kann für die Überwachung von Patienten innerhalb und außerhalb des Krankenhauses verwendet werden. Wenn Sie auf die Vorlage klicken, sehen Sie Folgendes: Zusätzlich zum Senden von Gerätedaten, z. B. Akkustand und Gerätetemperatur, sendet das Patchgerät auch Daten zum Gesundheitszustand des Patienten, z. B. Atemfrequenz und Blutdruck.

* **Smart Knee Brace**: Bei diesem Gerät handelt es sich um eine Stützmanschette für das Knie, die für Patienten mit einem neuen Kniegelenk genutzt werden kann. Wenn Sie auf diese Vorlage klicken, werden zusätzlich zu den Gerätedaten Funktionen wie Bewegungsbereich und Beschleunigung angezeigt.

>[!div class="mx-imgBorder"] 
>![Gerätevorlage „Smart Vitals Patch“](media/smart-vitals-device-template.png)

Wenn Sie auf die Registerkarte **Gerätegruppen** klicken, sehen Sie auch, dass für diese Gerätevorlagen automatisch Gerätegruppen erstellt werden.

### <a name="rules"></a>Regeln

Wenn Sie zur Registerkarte „Regeln“ wechseln, werden in der Anwendungsvorlage drei Regeln angezeigt:

* **Brace temperature high** (Hohe Manschettentemperatur): Diese Regel wird ausgelöst, wenn die Gerätetemperatur des Smart Knee Brace-Geräts fünf Minuten lang über 95 &deg;F (35 °C) liegt. Sie können diese Regel verwenden, um den Patienten und das Pflegeteam zu informieren und das Gerät per Remotesteuerung herunterzukühlen.

* **Fall detected** (Sturz erkannt): Diese Regel wird ausgelöst, wenn ein Sturz des Patienten erkannt wird. Sie können diese Regel verwenden, um eine Aktion zum Alarmieren eines Teams zu konfigurieren, das dem gestürzten Patienten zur Hilfe eilt.

* **Patch battery low** (Niedriger Akkustand des Patchgeräts): Diese Regel wird ausgelöst, wenn der Akkustand des Gerät unter einen Wert von 10 % sinkt. Sie können diese Regel verwenden, um für den Patienten eine Benachrichtigung mit dem Hinweis auszulösen, dass er sein Gerät aufladen sollte.

>[!div class="mx-imgBorder"] 
>![Regel für hohe Manschettentemperatur](media/brace-temp-rule.png)

### <a name="devices"></a>Geräte

Klicken Sie auf die Registerkarte **Geräte**, und wählen Sie anschließend eine Instanz von **Smart Knee Brace** aus. Es sind drei Ansichten verfügbar, in denen Sie Informationen zum jeweils ausgewählten Gerät untersuchen können. Diese Ansichten werden erstellt und veröffentlicht, wenn Sie die Gerätevorlage für Ihr Gerät erstellen. Dies bedeutet, dass sie auf allen Geräten, die Sie verbinden oder simulieren, einheitlich sind.

Die Ansicht **Dashboard** enthält eine Übersicht über die Telemetriedaten und Eigenschaften vom Gerät, die für den Bediener bestimmt sind.

Auf der Registerkarte **Properties** (Eigenschaften) können Sie Cloudeigenschaften bearbeiten und Geräteeigenschaften lesen/schreiben.

Auf der Registerkarte **Commands** (Befehle) können Sie Befehle ausführen, die im Rahmen Ihrer Gerätevorlage modelliert wurden.

>[!div class="mx-imgBorder"] 
>![Ansichten für Kniemanschette](media/knee-brace-dashboard.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht mehr verwenden möchten, sollten Sie sie löschen, indem Sie unter **Verwaltung > Anwendungseinstellungen** auf **Löschen** klicken.

>[!div class="mx-imgBorder"] 
>![Löschen einer App](media/admin-delete.png)

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um sich über die Erstellung eines Anbieterdashboards zu informieren, mit dem eine Verbindung mit Ihrer IoT Central-Anwendung hergestellt wird.

> [!div class="nextstepaction"]
> [Erstellen eines Power BI-Anbieterdashboards](howto-health-data-triage.md)