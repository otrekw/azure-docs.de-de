---
author: baanders
description: 'Include-Datei für Azure Digital Twins-Tutorials: Konfigurieren des Beispielprojekts'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 1eceb842947abd8975036c3145c1da755ef59526
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108766753"
---
## <a name="configure-the-sample-project"></a>Konfigurieren des Beispielprojekts

Richten Sie als Nächstes eine Beispielclient-Anwendung ein, die mit Ihrer Azure Digital Twins-Instanz interagiert.

Navigieren Sie auf Ihrem Computer zu der Datei, die Sie zuvor von [Azure Digital Twins-End-to-End-Beispiele](/samples/azure-samples/digital-twins-samples/digital-twins-samples) heruntergeladen haben (und entpacken Sie sie, sofern noch nicht geschehen).

Navigieren Sie dann im Ordner zu _AdtSampleApp_. Öffnen Sie _**AdtE2ESample.sln**_ in Visual Studio 2019. 

Wählen Sie in Visual Studio die Datei _SampleClientApp > **appsettings.json**_ aus, um sie im Bearbeitungsfenster zu öffnen. Sie dient als voreingestellte JSON-Datei mit den erforderlichen Konfigurationsvariablen zum Ausführen des Projekts.

Ändern Sie im Dateitext `instanceUrl` in die *Hostnamen-URL* der Azure Digital Twins-Instanz. (Fügen Sie **_https://_** vor dem *Hostnamen* ein, wie nachfolgend gezeigt.)

```json
{
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-host-name>"
}
```

Speichern und schließen Sie die Datei. 

Konfigurieren Sie als Nächstes die Datei *appsettings.json*, die in das Ausgabeverzeichnis kopiert werden soll, wenn Sie *SampleClientApp* erstellen. Klicken Sie dazu mit der rechten Maustaste auf die Datei *appsettings.json*, und wählen Sie **Eigenschaften** aus. Suchen Sie in der Prüfung der **Eigenschaften** nach der Eigenschaft *In Ausgabeverzeichnis kopieren*. Ändern Sie den Wert in **Kopieren, falls aktueller**, wenn er nicht bereits festgelegt ist.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Auszug aus dem Visual Studio-Fenster mit dem Projektmappen-Explorer-Bereich, in dem „appsettings.json“ hervorgehoben ist, und mit dem Bereich „Eigenschaften“, in dem die Eigenschaft „In Ausgabeverzeichnis kopieren“ auf „Kopieren, falls aktueller“ festgelegt ist" border="false" lightbox="../articles/digital-twins/media/includes/copy-config.png":::

Lassen Sie das Projekt _**AdtE2ESample**_ in Visual Studio geöffnet, um es weiterhin im Tutorial zu verwenden.

[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](digital-twins-local-credentials-outer.md)]
