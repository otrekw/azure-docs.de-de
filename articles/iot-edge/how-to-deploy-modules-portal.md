---
title: Bereitstellen von Modulen über das Azure-Portal – Azure IoT Edge
description: Über Ihren IoT Hub im Azure-Portal können Sie – entsprechend der Konfiguration durch ein Bereitstellungsmanifest – ein IoT Edge-Modul per Push von Ihrem IoT Hub auf Ihr IoT Edge-Gerät übertragen.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 754c106db42f3f0695ad023e736993bee82e9757
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82133916"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal

Nachdem Sie IoT Edge-Module mit Ihrer Geschäftslogik erstellen, sollten Sie sie auf Ihren Geräten für den Betrieb im Edge-Bereich bereitstellen. Wenn bei Ihnen mehrere Module gemeinsam Daten erfassen und verarbeiten, können Sie alle auf einmal bereitstellen und die Routingregeln, mit denen sie verbunden werden, deklarieren.

In diesem Artikel wird gezeigt, wie das Azure-Portal Sie durch das Erstellen eines Bereitstellungsmanifests und die Übertragung der Bereitstellung auf einem IoT Edge-Gerät führt. Informationen zum Erstellen einer Bereitstellung für mehrere Geräte – basierend auf deren freigegebenen Tags – finden Sie unter [Deploy and monitor IoT Edge modules at scale](how-to-deploy-at-scale.md) (Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab).

## <a name="prerequisites"></a>Voraussetzungen

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement.
* Ein [IoT Edge-Gerät](how-to-register-device.md#register-in-the-azure-portal) mit installierter IoT Edge-Runtime.

## <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Weitere Informationen zur Funktionsweise und Erstellung von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

Das Azure-Portal verfügt über einen Assistenten, der Sie durch das Erstellen des Bereitstellungsmanifests führt, sodass Sie das JSON-Dokument nicht manuell erstellen müssen. Der Assistent umfasst drei Schritte: **Hinzufügen von Modulen**, **Angeben von Routen** und **Überprüfen der Bereitstellung**.

### <a name="select-device-and-add-modules"></a>Auswählen des Geräts und Hinzufügen von Modulen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.
1. Wählen Sie im linken Bereich aus dem Menü die Option **IoT Edge** aus.
1. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts.
1. Wählen Sie in der oberen Leiste **Module festlegen** aus.
1. Geben Sie im Abschnitt **Container Registry-Einstellungen** der Seite die Anmeldeinformationen für den Zugriff auf private Containerregistrierungen mit Ihren Modulimages an.
1. Wählen Sie im Abschnitt **IoT Edge-Module** der Seite die Option **Hinzufügen** aus.
1. Sehen Sie sich die Modultypen im Dropdownmenü an:

   * **IoT Edge-Modul** – Geben Sie den Modulnamen und den URI des Containerimages an. Beispielsweise lautet der Image-URI für das Beispielmodul „SimulatedTemperatureSensor“ `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`. Wenn das Modulimage in einer privaten Containerregistrierung gespeichert ist, fügen Sie die Anmeldeinformationen auf dieser Seite hinzu, um auf das Image zugreifen zu können.
   * **Marketplace-Modul** – Module, die im Azure Marketplace gehostet werden. Weil einige Marketplace-Module eine zusätzliche Konfiguration erfordern, überprüfen Sie die Moduldetails in der Liste [Azure Marketplace IoT-Edge-Module](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).
   * **Azure Stream Analytics-Modul** – Module, die aus einer Azure Stream Analytics-Workload generiert wurden.

1. Nachdem Sie ein Modul hinzugefügt haben, wählen Sie dessen Namen aus der Liste aus, um die Moduleinstellungen zu öffnen. Füllen Sie bei Bedarf die optionalen Felder aus. Weitere Informationen zu Containererstellungsoptionen, Neustartrichtlinien und gewünschtem Status finden Sie unter [Gewünschte EdgeAgent-Eigenschaften](module-edgeagent-edgehub.md#edgeagent-desired-properties). Weitere Informationen zum Modulzwilling finden Sie unter [Definieren oder Aktualisieren gewünschter Eigenschaften](module-composition.md#define-or-update-desired-properties).
1. Wiederholen Sie bei Bedarf die Schritte 5 bis 8, um Ihrer Bereitstellung weitere Module hinzuzufügen.
1. Klicken Sie auf **Weiter: Routen**, um mit dem Abschnitt über Routen fortzufahren.

### <a name="specify-routes"></a>Angeben von Routen

Auf der Registerkarte **Routen** definieren Sie, wie Nachrichten zwischen Modulen und dem IoT Hub übergeben werden sollen. Nachrichten werden mit Name-Wert-Paaren erstellt. Standardmäßig wird eine Route mit **route** bezeichnet und als **FROM /messages/\* INTO $upstream** definiert. Dies bedeutet, dass alle von sämtlichen Modulen ausgegebenen Nachrichten an Ihren IoT-Hub gesendet werden.  

Fügen Sie die Routen mit Informationen aus [Routen deklarieren](module-composition.md#declare-routes) hinzu, oder aktualisieren Sie sie damit. Wählen Sie dann **Weiter: Überprüfen + erstellen** aus, um mit dem nächsten Schritt des Assistenten fortzufahren.

### <a name="review-deployment"></a>Überprüfen der Bereitstellung

Im Abschnitt zur Überprüfung wird das JSON-Bereitstellungsmanifest angezeigt, das anhand Ihrer Auswahl in den vorherigen beiden Abschnitten erstellt wurde. Beachten Sie, dass zwei Module deklariert wurden, die Sie nicht hinzugefügt haben: **$edgeAgent** und **$edgeHub**. Diese beiden Module bilden die [IoT Edge-Runtime](iot-edge-runtime.md) und sind in jeder Bereitstellung erforderliche Standardvorgaben.

Überprüfen Sie Ihre Bereitstellungsinformationen, und wählen Sie **Erstellen** aus.

## <a name="view-modules-on-your-device"></a>Anzeigen von Modulen auf dem Gerät

Nachdem Sie Module auf Ihrem Gerät bereitgestellt haben, können Sie alle auf der Seite „Gerätedetails“ Ihres IoT Hubs anzeigen. Auf dieser Seite werden die Namen der einzelnen bereitgestellten Modul sowie nützliche Informationen wie der Bereitstellungsstatus und der Exitcode angezeigt.

## <a name="deploy-modules-from-azure-marketplace"></a>Bereitstellen von Modulen aus Azure Marketplace

[Azure Marketplace](https://azuremarketplace.microsoft.com/) ist ein Onlinemarktplatz für Anwendungen und Dienste, in dem sie eine breite Palette an Unternehmensanwendungen und -lösungen durchsuchen können, die für die Ausführung unter Azure, einschließlich [IoT Edge-Modulen](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules), zertifiziert und optimiert sind.

Sie können ein IoT Edge-Modul aus Azure-Marketplace und über Ihren IoT Hub bereitstellen.

### <a name="deploy-from-azure-marketplace"></a>Bereitstellen aus Azure Marketplace

Gehen Sie die IoT Edge-Module im Marketplace durch, und wenn Sie das gewünschte finden, können Sie es durch Auswählen von **Erstellen** oder **Jetzt abrufen** bereitstellen. Fahren Sie mit den Schritten des Bereitstellungs-Assistenten fort, die je nach dem ausgewählten IoT Edge-Modul variieren können:

1. Bestätigen Sie die Nutzungsbedingungen und Datenschutzrichtlinie des Anbieters durch Auswählen von **Weiter**. Möglicherweise müssen Sie zuerst Kontaktinformationen angeben.
1. Wählen Sie Ihr Abonnement und den IoT-Hub aus, der mit dem Zielgerät verbunden ist.
1. Wählen Sie **Für Gerät bereitstellen** aus.
1. Geben Sie den Namen des Geräts ein, oder wählen Sie **Gerät suchen** aus, um die mit dem Hub registrierten Geräte zu durchsuchen.
1. Wählen Sie **Erstellen** aus, um den Standardprozess des Konfigurieren eines Bereitstellungsmanifests fortzusetzen. Dazu gehört auch das Hinzufügen weiterer Module bei Bedarf. Details für das neue Modul, z.B. Image-URI, Erstellungsoptionen und gewünschte Eigenschaften, sind vordefiniert, können aber geändert werden.

Vergewissern Sie sich, dass das Modul in Ihrem IoT Hub im Azure-Portal bereitgestellt wird. Wählen Sie Ihr Gerät und **Module festlegen** aus. Dann sollte das Modul im Abschnitt **IoT Edge-Module** aufgeführt werden.

### <a name="deploy-from-azure-iot-hub"></a>Bereitstellen über Azure IoT Hub

Sie können ein Modul aus dem Azure Marketplace auf Ihrem Gerät in Ihrem IoT Hub im Azure-Portal schnell bereitstellen.

1. Navigieren Sie im Azure-Portal zu Ihrem IoT-Hub.
1. Wählen Sie im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.
1. Wählen Sie das IoT Edge-Gerät aus, auf dem die Bereitstellung erfolgen soll.
1. Wählen Sie in der oberen Leiste **Module festlegen** aus.
1. Klicken Sie im Abschnitt **IoT Edge-Module** auf **Hinzufügen**, und wählen Sie im Dropdownmenü die Option **Marketplace-Modul** aus.

![Hinzufügen eines Moduls in IoT Hub](./media/how-to-deploy-modules-portal/iothub-add-module.png)

Wählen Sie auf der Seite **IoT Edge-Modul aus Marketplace** das gewünschte Modul aus. Das ausgewählte Modul wird für Ihr Abonnement, Ihre Ressourcengruppe und Ihr Gerät automatisch konfiguriert. Dann wird es in Ihrer Liste von IoT Edge-Modulen angezeigt. Bei einigen Modulen ist möglicherweise eine zusätzliche Konfiguration erforderlich.

> [!TIP]
> Die Informationen zu IoT Edge-Modulen aus dem Azure IoT Hub sind eingeschränkt. Weitere Informationen zu [IoT Edge-Modulen](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) erhalten Sie zunächst im Azure Marketplace.

Klicken Sie auf **Weiter: Routen**, und fahren Sie mit der Bereitstellung fort, wie weiter oben in diesem Artikel unter [Angeben von Routen](#specify-routes) und [Überprüfen der Bereitstellung](#review-deployment) beschrieben wird.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [IoT Edge-Module im großen Maßstab bereitstellen und überwachen](how-to-deploy-at-scale.md).
