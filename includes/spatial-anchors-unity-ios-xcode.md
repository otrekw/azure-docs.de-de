---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/14/2020
ms.author: rgarcia
ms.openlocfilehash: b93243a537fafce6d865ec207b12dc2654cafd20
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89536256"
---
Wählen Sie **Build** aus. Wählen Sie im dann geöffneten Dialogfeld einen Ordner aus, in den das Xcode-Projekt exportiert wird.

Nach Abschluss des Exports wird ein Ordner angezeigt, der das exportierte Xcode-Projekt enthält.

> [!NOTE]
> Wenn ein Fenster mit der Auswahl zwischen „Ersetzen“ und „Anfügen“ angezeigt wird, empfiehlt es sich, **Anfügen** auszuwählen, da dies schneller ist. Die Auswahl von **Ersetzen** sollte nur erforderlich sein, wenn Sie Ressourcen in Ihrer Szene ändern (wenn Sie beispielsweise Beziehungen zwischen übergeordneten und untergeordneten Elementen hinzufügen, entfernen oder ändern oder wenn Sie Eigenschaften hinzufügen, entfernen oder ändern). Wenn Sie lediglich Änderungen am Quellcode vornehmen, sollte **Anfügen** ausreichend sein.

## <a name="open-the-xcode-project"></a>Öffnen des Xcode-Projekts

Jetzt können Sie `Unity-iPhone.xcodeproj` in Xcode öffnen. Sie können entweder Xcode starten und das exportierte `Unity-iPhone.xcodeproj`-Projekt öffnen oder das Projekt in Xcode starten, indem Sie den folgenden Befehl an dem Ort ausführen, an dem Sie das Projekt exportiert haben:

```bash
open ./Unity-iPhone.xcodeproj
```

Wählen Sie den Hauptknoten **Unity-iPhone** aus, um die Projekteinstellungen anzuzeigen, und wählen Sie dann die Registerkarte **General** (Allgemein) aus.

Vergewissern Sie sich unter **Signing** (Signierung), dass **Automatically manage signing** (Signierung automatisch verwalten) aktiviert ist. Wenn dies nicht der Fall ist, aktivieren Sie diese Option, und wählen Sie im dann angezeigten Dialogfeld die Option **Enable Automatic** (Automatik aktivieren) aus, um die Buildeinstellungen zurückzusetzen.

Vergewissern Sie sich unter **Deployment Info** (Bereitstellungsinformationen), dass das Bereitstellungsziel (**Deployment Target**) auf `11.0` festgelegt ist.

## <a name="deploy-the-app-to-your-ios-device"></a>Bereitstellen der App auf Ihrem iOS-Gerät

Verbinden Sie das iOS-Gerät mit dem Macintosh-Computer, und legen Sie das **aktive Schema** auf Ihr iOS-Gerät fest.

![Auswählen des Geräts](./media/spatial-anchors-unity/select-device.png)

Wählen Sie **Build and then run the current scheme** (Aktuelles Schema erstellen und dann ausführen) aus.

![Bereitstellen und Ausführen](./media/spatial-anchors-unity/deploy-run.png)
