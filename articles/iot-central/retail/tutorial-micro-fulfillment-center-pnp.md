---
title: 'Tutorial: Micro-Fulfillment-Center-App-Vorlage | Microsoft-Dokumentation'
description: Ein Tutorial zur IoT Central-Anwendungsvorlage für Micro-Fulfillment-Center.
author: avneet723
ms.author: avneets
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 01/09/2020
ms.openlocfilehash: 2ed6f37bc3b00397fa6331a501e1b16c8d622b5f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026223"
---
# <a name="tutorial-deploy-and-walk-through-a-micro-fulfillment-center-application-template"></a>Tutorial: Bereitstellen und Durchlaufen einer Anwendungsvorlage für Micro-Fulfillment-Center

In diesem Tutorial wird anhand der Azure IoT Central-Anwendungsvorlage ***Micro-Fulfillment-Center*** gezeigt, wie Sie eine Einzelhandelslösung erstellen. Sie erfahren, wie Sie die MFC-Vorlage bereitstellen, was standardmäßig enthalten ist und welche nächsten Schritte sich anbieten.

In diesem Tutorial lernen Sie Folgendes: 
> [!div class="checklist"]
> * Erstellen einer Einzelhandelsanwendung mithilfe der Azure IoT Central-Vorlage **Micro-Fulfillment-Center**
> * Durchlaufen der Anwendung 

## <a name="prerequisites"></a>Voraussetzungen
Zum Abschließen dieses Tutorials benötigen Sie Folgendes:
* Ein Azure-Abonnement. Optional können Sie eine kostenlose 7-Tage-Testversion verwenden. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eines erstellen.

## <a name="create-an-application"></a>Erstellen einer Anwendung 
In diesem Abschnitt erstellen Sie eine neue Azure IoT Central-Anwendung aus einer Vorlage. Sie verwenden diese Anwendung in der gesamten Tutorialreihe, um eine umfassende Lösung zu erstellen.

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Navigieren Sie zur Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral).
1. Wenn Sie ein Azure-Abonnement haben, melden Sie sich mit den üblichen Anmeldeinformationen an. Verwenden Sie andernfalls ein Microsoft-Konto:

   ![Eingeben Ihres Organisationskontos](./media/tutorial-in-store-analytics-create-app-pnp/sign-in.png)

1. Wählen Sie **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen.

1. Wählen Sie **Einzelhandel** aus.  Auf der Seite „Einzelhandel“ werden verschiedene Vorlagen für Einzelhandelsanwendungen angezeigt.

So erstellen Sie eine neue Micro-Fulfillment-Center-Anwendung mit Previewfunktionen:  
1. Wählen Sie die Anwendungsvorlage **Micro-Fulfillment-Center** aus. Diese Vorlage enthält Gerätevorlagen für alle im Tutorial verwendeten Geräte. Darüber hinaus stellt die Vorlage ein Bedienerdashboard zur Überwachung von Bedingungen innerhalb Ihres Fulfillment-Centers sowie zur Überwachung der Bedingungen für Ihre Transportroboter bereit. 

    > [!div class="mx-imgBorder"]
    > ![Anwendung vom Typ „Micro-Fulfillment-Center“](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-homepage-mfc.png)
    
1. Wählen Sie optional einen benutzerfreundlichen **Anzeigenamen** aus.  Die Anwendungsvorlage basiert auf dem fiktiven Unternehmen Northwind Traders. 

    > [!NOTE]
    > Wenn Sie einen benutzerfreundlichen **Anwendungsnamen** verwenden, müssen Sie dennoch einen eindeutigen Wert für die Anwendungs-**URL** angeben.

1. Wenn Sie ein Azure-Abonnement haben, geben Sie die entsprechenden Angaben für *Verzeichnis, Azure-Abonnement und Region* ein. Wenn Sie kein Abonnement haben, können Sie eine **7-tägige kostenlose Testversion** aktivieren und die erforderlichen Kontaktinformationen eingeben.  

    Weitere Informationen zu Verzeichnissen und Abonnements finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](../preview/quick-deploy-iot-central.md).

1. Klicken Sie auf **Erstellen**.

> [!div class="mx-imgBorder"]
> ![Erstellen einer Anwendung für ein Micro-Fulfillment-Center](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-create-app-mfc.png)

## <a name="walk-through-the-application"></a>Einführung in die Anwendung 

### <a name="dashboard"></a>Dashboard 

Nach erfolgreicher Bereitstellung der App-Vorlage wird zunächst das **Micro-Fulfillment-Center-Dashboard für Northwind Traders** angezeigt. Northwind Trader ist ein fiktiver Einzelhändler, der über ein in dieser IoT Central-Anwendung verwaltetes Micro-Fulfillment-Center verfügt. Dieses Bedienerdashboard enthält Informationen und Telemetriedaten zu den Geräten in dieser Vorlage sowie die verfügbaren Befehle, Aufträge und Aktionen. Das Dashboard ist logisch in zwei Abschnitte unterteilt (links und rechts). Auf der linken Seite können Sie die Umgebungsbedingungen innerhalb der Fulfillment-Struktur und auf der rechten Seite den Status eines Transportroboters in Ihrer Einrichtung überwachen.  

Über das Dashboard können Sie Folgendes durchführen:
   * Anzeigen von Gerätetelemetriedaten (etwa die Anzahl von Entnahmen und verarbeiteten Aufträgen) und Eigenschaften (etwa den Struktursystemstatus)  
   * Anzeigen des **Grundrisses** und der Position der Transportroboter innerhalb der Fulfillment-Struktur
   * Auslösen von Befehlen – etwa zum Zurücksetzen des Steuerungssystems, zum Aktualisieren der Firmwareversion eines Transportroboters oder zum Ändern der Netzwerkkonfiguration

> [!div class="mx-imgBorder"]
> ![Dashboard des Micro-Fulfillment-Centers](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard1.png)
   * Betrachten eines Beispiels für das Dashboard, über das ein Bediener die Bedingungen innerhalb des Fulfillment-Centers überwachen kann 
   * Überwachen der Integrität der Nutzlasten, die auf dem Gatewaygerät innerhalb des Fulfillment-Centers ausgeführt werden    

> [!div class="mx-imgBorder"]
> ![Dashboard des Micro-Fulfillment-Centers](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard2.png)

## <a name="device-template"></a>Gerätevorlage
Wenn Sie auf die Registerkarte „Gerätevorlagen“ klicken, werden für die Vorlage zwei unterschiedliche Gerätetypen angezeigt: 
   * **Robotic Carrier** (Transportroboter): Diese Gerätevorlage stellt die Definition eines funktionierenden Transportroboters dar, der in der Fulfillment-Struktur eingesetzt wird und ordnungsgemäß funktioniert. Wenn Sie auf die Vorlage klicken, sehen Sie, dass von dem Roboter Gerätedaten wie Temperatur und Achsenposition sowie Eigenschaften wie etwa der Status des Transportroboters gesendet werden. 
   * **Structure Condition Monitoring** (Überwachung der Strukturbedingungen): Diese Gerätevorlage stellt eine Sammlung von Geräten dar, mit deren Hilfe Sie sowohl Umgebungsbedingungen als auch das Gatewaygerät überwachen können, von dem verschiedene Edgeworkloads für den Betrieb Ihres Fulfillment-Centers gehostet werden. Das Gerät sendet Telemetriedaten wie Temperatur, Anzahl von Entnahmen, Anzahl von Aufträgen usw. sowie Informationen zum Zustand und zur Integrität der in Ihrer Umgebung ausgeführten Computeworkloads. 

> [!div class="mx-imgBorder"]
> ![Gerätevorlagen für das Micro-Fulfillment-Center](./media/tutorial-micro-fulfillment-center-app-pnp/device-templates.png)

Wenn Sie auf die Registerkarte „Gerätegruppen“ klicken, sehen Sie auch, dass für diese Gerätevorlagen automatisch Gerätegruppen erstellt werden.

## <a name="rules"></a>Regeln
Auf der Registerkarte „Regeln“ sehen Sie eine Beispielregel, die in der Anwendungsvorlage vorhanden ist, um die Temperaturbedingungen für den Transportroboter zu überwachen. Diese Regel kann verwendet werden, um den Bediener zu benachrichtigen, wenn ein bestimmter Roboter in der Einrichtung zu heiß wird und zu Wartungszwecken offline geschaltet werden muss. 

Nutzen Sie die Beispielregel als Orientierungshilfe, um Regeln zu definieren, die besser zu Ihren geschäftlichen Funktionen passen.

   - **Robotic carrier too warm** (Transportroboter zu warm): Diese Regel wird ausgelöst, wenn der Transportroboter für einen bestimmten Zeitraum einen Temperaturschwellen erreicht. 

> [!div class="mx-imgBorder"]
> ![Micro-Fulfillment-Center-Regeln](./media/tutorial-micro-fulfillment-center-app-pnp/rules.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht mehr verwenden möchten, löschen Sie die Anwendungsvorlage, indem Sie **Verwaltung** > **Anwendungseinstellungen** besuchen und auf **Löschen** klicken.

> [!div class="mx-imgBorder"]
> ![Bereinigen der Micro-Fulfillment-Center-Anwendung](./media/tutorial-micro-fulfillment-center-app-pnp/delete.png)

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über die [Lösungsarchitektur von Micro-Fulfillment-Centern](./architecture-micro-fulfillment-center-pnp.md).
* Weitere Informationen zu anderen [IoT Central-Einzelhandelsvorlagen](./overview-iot-central-retail-pnp.md)
* Lesen Sie die [Übersicht über IoT Central](../preview/overview-iot-central.md).