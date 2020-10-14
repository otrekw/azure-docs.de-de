---
author: ericmitt
ms.author: ericmitt
ms.service: iot-pnp
ms.topic: include
ms.date: 07/13/2020
ms.openlocfilehash: 3ab9161c006a88a254b247a921512698ed46fa95
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87351823"
---
1. Öffnen Sie den Azure-IoT-Explorer.

1. Wenn Sie Ihrem IoT-Hub noch keine Verbindung hinzugefügt haben, wählen Sie auf der Seite **IoT-Hubs** die Option **+ Verbindung hinzufügen** aus. Geben Sie die Verbindungszeichenfolge für den zuvor erstellten IoT-Hub ein, und wählen Sie **Speichern** aus.

1. Wählen Sie auf der Seite **IoT Plug and Play Settings** (IoT Plug & Play-Einstellungen) die Optionen **+ Hinzufügen > Lokaler Ordner** und dann den lokalen Ordner *models* aus, in dem Sie Ihre Modelldateien gespeichert haben.

1. Klicken Sie auf der Seite **IoT-Hubs** auf den Namen des Hubs, den Sie verwenden möchten. Eine Liste der Geräte wird angezeigt, die beim IoT-Hub registriert sind.

1. Klicken Sie auf die **Geräte-ID** des Geräts, das Sie zuvor erstellt haben.

1. Im Menü auf der linken Seite werden die verschiedenen Arten von Informationen angezeigt, die für das Gerät verfügbar sind.

1. Wählen Sie **IoT Plug and Play components** (IoT Plug & Play-Komponenten) aus, um die Modellinformationen für Ihr Gerät anzuzeigen.

1. Sie können die verschiedenen Komponenten des Geräts anzeigen. Angezeigt werden die Standardkomponente und alle weiteren Komponenten. Wählen Sie eine Komponente aus, mit der Sie arbeiten möchten.

1. Wählen Sie die Seite **Telemetrie** und dann **Start** aus, um die vom Gerät für diese Komponente gesendeten Telemetriedaten anzuzeigen.

1. Wählen Sie die Seite **Eigenschaften (mit Schreibschutz)** aus, um die für diese Komponente gemeldeten schreibgeschützten Eigenschaften anzuzeigen.

1. Wählen Sie die Seite **Properties (writable)** (Eigenschaften (schreibbar)) aus, um die schreibbaren Eigenschaften anzuzeigen, die Sie für diese Komponente aktualisieren können.

1. Wählen Sie eine Eigenschaft nach ihrem **Namen** aus, geben Sie einen neuen Wert ein, und wählen Sie **Update desired value** (Gewünschten Wert aktualisieren) aus.

1. Wählen Sie die Schaltfläche **Aktualisieren** aus, damit der neue Wert angezeigt wird.

1. Wählen Sie die Seite **Befehle** aus, um alle Befehle für diese Komponente anzuzeigen.

1. Wählen Sie den zu testenden Befehl aus, und legen Sie ggf. den Parameter fest. Wählen Sie **Befehl senden** aus, um den Befehl auf dem Gerät aufzurufen. Sie können sehen, dass Ihr Gerät auf den Befehl im Eingabeaufforderungsfenster antwortet, in dem der Beispielcode ausgeführt wird.
