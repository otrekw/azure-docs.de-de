---
title: Zertifizieren von IoT Plug & Play-Geräten | Microsoft-Dokumentation
description: Erfahren Sie als Geräteentwickler, wie Sie Tests durchführen und ein Gerät zur Zertifizierung übermitteln können.
author: konichi3
ms.author: koichih
ms.date: 08/21/2020
ms.topic: how-to
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: bdb6bf166e84bb9134bbd14454899bcefbf0a887
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88949897"
---
# <a name="how-to-certify-iot-plug-and-play-devices"></a>Zertifizieren von IoT Plug & Play-Geräten

Das IoT Plug & Play-Gerätezertifizierungsprogramm umfasst Tools zum Überprüfen, ob ein Gerät die IoT Plug & Play-Zertifizierungsanforderungen erfüllt. Die Tools helfen Organisationen auch, das Bewusstsein für die Verfügbarkeit ihrer IoT Plug & Play-Geräte zu steigern. Diese zertifizierten Geräte sind auf IoT-Lösungen zugeschnitten und tragen dazu bei, die Zeit bis zur Markteinführung zu verkürzen.

In diesem Artikel lernen Sie Folgendes:

- Installieren der Erweiterung für das Azure IoT-Befehlszeilentool für die Azure CLI
- Führen Sie die IoT Plug & Play-Tests aus, um Ihre Geräteanwendung während der Entwicklungsphase zu überprüfen.  
- Verwenden des Azure Certified Device-Portals zum Überprüfen der Geräteanwendung

## <a name="prepare-your-device"></a>Vorbereiten Ihres Geräts

Der Anwendungscode, der auf Ihrem IoT Plug & Play ausgeführt wird, muss Folgendes erreichen:

- Eine Verbindung mit Azure IoT Hub mithilfe des [Gerätebereitstellungsdienst (Device Provisioning Service, DPS)](../iot-dps/about-iot-dps.md) herstellen.
- Die [IoT Plug & Play-Konventionen](concepts-convention.md) zur Implementierung von Telemetrie, Eigenschaften und Befehlen befolgen.

Bei der Anwendung handelt es sich um Software, die separat vom Betriebssystem installiert oder mit dem Betriebssystem in einem Firmwareimage gebündelt wird, das per Flashvorgang auf das Gerät übertragen wird.

Der Zertifizierungsprozess überprüft, ob das Gerät mit IoT Plug & Play kompatibel ist, indem geprüft wird, ob die Geräteimplementierung mit der Telemetrie, den Eigenschaften und Befehlen übereinstimmt, die im [DTDL](https://github.com/Azure/opendigitaltwins-dtdl)-Gerätemodell (Digital Twins Definition Language) definiert sind, und ob das Modell im [öffentlichen Azure IoT-Modellrepository](concepts-model-repository.md) verfügbar ist.

Ihr Gerät muss Folgendes durchführen, um die Anforderungen an die Zertifizierung zu erfüllen:

- Eine Verbindung mit Azure IoT Hub mithilfe des [Gerätebereitstellungsdiensts](../iot-dps/about-iot-dps.md) (DPS) herstellen.
- Telemetrie, Eigenschaften oder Befehle implementieren, die der IoT Plug & Play-Konvention folgen.
- Die Geräteinteraktionen mit einem [DTDL v2](https://aka.ms/dtdl)-Modell beschreiben.
- Das Modell und alle erforderlichen Schnittstellen im [öffentlichen Azure IoT-Modellrepository](https://devicemodels.azureiotsolutions.com/) veröffentlichen.
- Die Modell-ID während der [DPS-Registrierung](concepts-developer-guide.md#dps-payload) in den DPS-Bereitstellungsnutzdaten senden.
- Die Modell-ID während der [MQTT-Verbindung](concepts-developer-guide.md#model-id-announcement) ankündigen.

## <a name="test-with-the-azure-iot-extension-cli"></a>Testen mit der Befehlszeilenschnittstelle der Azure IoT-Erweiterung

Mit der [Azure IoT CLI-Erweiterung](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/product?view=azure-cli-latest) können Sie überprüfen, ob die Geräteimplementierung mit dem Modell übereinstimmt, bevor Sie das Gerät zur Zertifizierung über das Azure Certified Device-Portal übermitteln.

In den folgenden Schritten wird gezeigt, wie Sie die Zertifizierungstests mithilfe der CLI vorbereiten und ausführen:

### <a name="install-the-azure-iot-extension-for-the-azure-cli"></a>Installieren der Azure IoT-Erweiterung für die Azure CLI

Folgen Sie den [Installationsanleitungen](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) zum Einrichten der Azure CLI in Ihrer Umgebung.

Führen Sie zum Installieren der Azure IoT-Erweiterung den folgenden Befehl aus:

```azurecli
az extension add --name azure-iot
```

Weitere Informationen finden Sie unter [Azure CLI für Azure IoT](https://docs.microsoft.com/cli/azure/azure-cli-reference-for-iot?view=azure-cli-latest).

### <a name="create-a-new-product-test"></a>Erstellen eines neuen Produkttests

Der folgende Befehl erstellt einen Test unter Verwendung von DPS mit einer symmetrischen Schlüsselnachweismethode:

- Erstellt ein neues Produkt zum Testen und generiert eine Testkonfiguration. Die Ausgabe zeigt die DPS-Informationen an, die das Gerät für die Bereitstellung verwenden muss: den Primärschlüssel, die Geräte-ID und den ID-Bereich.
- Gibt den Ordner mit den DTDL-Dateien an, die Ihr Modell beschreiben.

```azurecli
az iot product test create --badge-type Pnp --at SymmetricKey --device-type FinishedProduct --models {local folder name}
```

> [!NOTE]
> Sie müssen sich bei Ihrem Abonnement [anmelden](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest), wenn Sie die Befehlszeilenschnittstelle (CLI) verwenden.

Die JSON-Ausgabe des Befehls enthält `primaryKey`, `registrationId` und `scopeID`, die beim Herstellen einer Verbindung für Ihr Gerät zu verwenden sind.

Erwartete Ausgabe:

```json
"deviceType": "FinishedProduct",
"id": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
"provisioningConfiguration": {
  "symmetricKeyEnrollmentInformation": {
    "primaryKey":"Ci/Ghpqp0n4m8FD5PTicr6dEikIfM3AtVWzAhObU7yc=",
    "registrationId": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
    "scopeId": "0ne000FFA42"
  }
}
```

### <a name="connect-your-device"></a>Verbinden Ihres Geräts

Verwenden Sie die vom vorherigen Befehl ausgegebenen DPS-Informationen, um Ihr Gerät mit der Testinstanz des IoT Hubs zu verbinden.

### <a name="manage-and-configure-the-product-tests"></a>Verwalten und Konfigurieren der Produkttests

Wenn das Gerät verbunden und bereit ist, mit dem IoT Hub zu interagieren, generieren Sie eine Konfigurationsdatei für den Produkttest. So erstellen Sie die Datei

- Verwenden Sie die Test-`id` aus der Ausgabe des vorherigen Befehls.
- Verwenden Sie den `--wait`-Parameter, um den Testfall abzurufen.

```azurecli
az iot product test task create --type GenerateTestCases --test-id d45d53d9-656d-4be7-9bbf-140bc87e98dc --wait
```

Erwartete Ausgabe:

```json
{
  "deviceTestId": "d45d53d9-656d-4be7-9bbf-140bc87e98dc",
  "error": null,
  "id": "526da38e-91fc-4e20-a761-4f04b392c42b",
  "resultLink": "/deviceTests/d45d53d9-656d-4be7-9bbf-140bc87e98dc/TestCases",
  "status": "Completed",
  "type": "GenerateTestCases"
}
```

Sie können den `az iot product test case update`-Befehl verwenden, um die Testkonfigurationsdatei zu ändern.

### <a name="run-the-tests"></a>Ausführen der Tests

Nachdem Sie die Testkonfiguration generiert haben, werden im nächsten Schritt die Tests durchgeführt. Verwenden Sie dieselbe `devicetestId` aus den vorherigen Befehlen als Parameter, um die Tests auszuführen. Überprüfen Sie die Testergebnisse, um sicherzustellen, dass alle Tests den Status `Passed` aufweisen.

```azurecli
az iot product test task create --type QueueTestRun --test-id d45d53d9-656d-4be7-9bbf-140bc87e98dc --wait
```

Beispiel für die Ausgabe eines Testlaufs

```json
      "validationTasks": [
        {
          "componentName": "Default component",
          "endTime": "2020-08-25T05:18:49.5224772+00:00",
          "interfaceId": "dtmi:com:example:TemperatureController;1",
          "logs": [
            {
              "message": "Waiting for telemetry from the device",
              "time": "2020-08-25T05:18:37.3862586+00:00"
            },
            {
              "message": "Validating PnP properties",
              "time": "2020-08-25T05:18:37.3875168+00:00"
            },
            {
              "message": "Validating PnP commands",
              "time": "2020-08-25T05:18:37.3894343+00:00"
            },
            {
              "message": "{\"propertyName\":\"serialNumber\",\"expectedSchemaType\":null,\"actualSchemaType\":null,\"message\":\"Property is successfully validated\",\"passed\":true,\"time\":\"2020-08-25T05:18:37.4205985+00:00\"}",
              "time": "2020-08-25T05:18:37.4205985+00:00"
            },
            {
              "message": "PnP interface properties validation passed",
              "time": "2020-08-25T05:18:37.4206964+00:00"
            },
```

## <a name="test-using-the-azure-certified-device-portal"></a>Testen über das Azure Certified Device-Portal

Die folgenden Schritte zeigen Ihnen, wie Sie das [Azure Certified Device-Portal](https://aka.ms/acdp) verwenden, um Produktdetails zu registrieren, einen Leitfaden für die ersten Schritte zu übermitteln und die Zertifizierungstests durchzuführen.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Artikels unterstützt das Portal nicht die Veröffentlichung im [Katalog mit Certified for Azure IoT-Geräten](https://aka.ms/devicecatalog).

### <a name="onboarding"></a>Onboarding

Um das [Zertifizierungsportal](https://aka.ms/acdp) zu nutzen, müssen Sie ein Azure Active Directory von Ihrem Geschäfts-, Schul- oder Unimandanten verwenden.

Um die Modelle im öffentlichen Azure IoT-Modellrepository zu veröffentlichen, muss Ihr Account ein Mitglied des [Microsoft Partner Network](https://partner.microsoft.com) sein. Das System prüft, ob die Microsoft Partner Network-ID vorhanden ist, und das Konto wird vor der Veröffentlichung im Gerätekatalog vollständig überprüft.

### <a name="company-profile"></a>Unternehmensprofil

Sie können Ihr Unternehmensprofil über das linke Navigationsmenü verwalten. Das Firmenprofil umfasst die Firmen-URL, die E-Mail-Adresse und das Firmenlogo. Die Programmvereinbarung muss auf dieser Seite akzeptiert werden, bevor Sie Zertifizierungsvorgänge durchführen können.

Die Unternehmensprofilinformationen werden in der im Gerätekatalog dargestellten Gerätebeschreibung verwendet.

### <a name="create-new-project"></a>Erstellen eines neuen Projekts

Zum Zertifizieren eines Geräts müssen Sie zunächst ein neues Projekt erstellen.

Navigieren Sie zum [Zertifizierungsportal](https://aka.ms/acdp). Wählen Sie auf der Seite **Projekte** die Option *+ Neues Projekt erstellen* aus. Geben Sie dann einen Namen für das Projekt und den Gerätenamen ein, und wählen Sie eine Geräteklasse aus.

Die Produktinformationen, die Sie während des Zertifizierungsprozesses bereitstellen, können in vier Kategorien eingeteilt werden:

- Geräteinformationen Sammelt Informationen zum Gerät wie Name, Beschreibung, Zertifizierungen und Betriebssystem.
- Der Leitfaden **Erste Schritte** Sie müssen den Leitfaden als PDF-Dokument zur Genehmigung durch den Systemadministrator übermitteln, bevor Sie das Gerät veröffentlichen.
- Marketingdetails Stellen Sie kundenspezifische Marketinginformationen für Ihr Gerät bereit. Die Marketinginformationen umfassen eine Beschreibung, ein Foto und Vertriebspartner.
- Zusätzliche Industriezertifizierungen In diesem optionalen Abschnitt können Sie zusätzliche Informationen zu weiteren Zertifizierungen angeben, die das Gerät erhalten hat.

### <a name="connect-and-test"></a>Verbinden und Testen

Der Schritt „Verbinden und Testen“ prüft, ob Ihr Gerät die Anforderungen der IoT Plug & Play-Zertifizierung erfüllt.

Es müssen drei Schritte ausgeführt werden:

1. Schnittstellen verbinden und ermitteln Das Gerät muss über DPS mit dem Azure IoT-Zertifizierungsdienst verbunden werden. Wählen Sie die Authentifizierungsmethode (X.509-Zertifikat, symmetrische Schlüssel oder Trusted Platform Module) aus, um die Geräteanwendung mit den DPS-Informationen zu verwenden und zu aktualisieren.
1. Schnittstellen überprüfen Überprüfen Sie die Schnittstelle und vergewissern Sie sich, dass jede Schnittstelle Nutzdateneingaben aufweist, die für Tests sinnvoll sind.
1. Tests durchführen Das System testet jedes Gerätemodell, um zu überprüfen, ob die Telemetrie, Eigenschaften und Befehle, die im Modell beschrieben sind, den IoT Plug & Play-Konventionen entsprechen. Wenn der Test abgeschlossen ist, wählen Sie den Link **Protokolle anzeigen** aus, um die Telemetrie vom Gerät und die an die IoT Hub-Gerätezwillingseigenschaften gesendeten Rohdaten anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Geräteübermittlung abgeschlossen ist, können Sie sich unter [iotcert@microsoft.com](mailto:iotcert@microsoft.com) an das Gerätezertifizierungsteam wenden, um mit den nächsten Schritten fortzufahren, zu denen die Validierung der Mitgliedschaft im Microsoft Partner Network und eine Überprüfung der Leitfäden für die ersten Schritte gehören. Wenn alle Anforderungen erfüllt sind, können Sie wählen, ob Ihr Gerät in den [Katalog mit Certified for Azure IoT-Geräten](https://aka.ms/devicecatalog) aufgenommen werden soll.
