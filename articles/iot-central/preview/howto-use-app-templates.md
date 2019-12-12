---
title: Exportieren einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Ich möchte als Lösungsmanager eine Anwendungsvorlage exportieren, um sie wiederverwenden zu können.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 9870b18be9fb75e25941a7e1d05e4164239aacbb
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977521"
---
# <a name="export-your-application-preview-features"></a>Exportieren Ihrer Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Lösungsmanager eine IoT Central-Anwendung exportieren, um sie wiederverwenden zu können.

Sie haben zwei Möglichkeiten:

- Sie können eine Kopie Ihrer Anwendung erstellen, wenn Sie nur eine duplizierte Kopie davon erstellen müssen.
- Wenn Sie die Erstellung mehrerer Kopien planen, können Sie aus Ihrer Anwendung eine Anwendungsvorlage erstellen.

## <a name="copy-your-application"></a>Kopieren Ihrer Anwendung

Sie können eine Kopie jeder Anwendung erstellen, mit Ausnahme von Geräteinstanzen, Gerätedatenverlauf und Benutzerdaten. Die Kopie ist eine Anwendung, für die Gebühren in Form von nutzungsbasierter Zahlung anfallen. Eine Testversion kann nicht durch das Kopieren einer Anwendung erstellt werden.

Wählen Sie **Kopieren** aus. Geben Sie im Dialogfenster die Details für die neue Anwendung mit nutzungsbasierter Zahlung ein. Wählen Sie dann **Kopieren** aus, um zu bestätigen, dass Sie den Vorgang fortsetzen möchten. Weitere Informationen zu den Feldern im Formular finden Sie im Schnellstart [Erstellen einer Anwendung](quick-deploy-iot-central.md).

> [!NOTE]
> Das **Kopieren Ihrer Anwendung** ist ausschließlich an Standorte in **Europa** und den **USA** möglich.

![Seite „Anwendungseinstellungen“](media/howto-use-app-templates/appcopy2.png)

Nach erfolgreichem Abschluss des App-Kopiervorgangs können Sie über den Link zur neuen Anwendung navigieren.

![Seite „Anwendungseinstellungen“](media/howto-use-app-templates/appcopy3a.png)

Beim Kopieren einer Anwendung wird auch die Definition von Regeln und E-Mail-Aktionen kopiert. Einige Aktionen, z.B. Flow und Logic Apps, sind über die Regel-ID an bestimmte Regeln gebunden. Wenn eine Regel in eine andere Anwendung kopiert wird, erhält sie eine eigene Regel-ID. In diesem Fall müssen Benutzer eine neue Aktion erstellen und diese dann der neuen Regel zuordnen. Generell empfiehlt es sich, die Regeln und Aktionen zu überprüfen, damit sie in der neuen App auf dem neuesten Stand sind.

> [!WARNING]
> Wenn ein Dashboard Kacheln enthält, in denen Informationen zu bestimmten Geräten angezeigt werden, erscheint in der neuen Anwendung die Meldung **Die angeforderte Ressource wurde nicht gefunden** in diesen Kacheln. Sie müssen diese Kacheln erneut konfigurieren, damit in der neuen Anwendung Informationen zu Geräten angezeigt werden.

## <a name="create-an-application-template"></a>Erstellen einer Anwendungsvorlage

Wenn Sie eine Azure IoT Central-Anwendung erstellen, können Sie unter verschiedenen integrierten Beispielvorlagen auswählen. Sie können auch eigene Anwendungsvorlagen von vorhandenen IoT Central-Anwendungen erstellen. Anschließend können Sie bei der Erstellung neuer Anwendung Ihre eigenen Anwendungsvorlagen verwenden.

Beim Erstellen einer Anwendungsvorlage werden die folgenden Elemente aus Ihrer vorhandenen Anwendung einbezogen:

- Das standardmäßige Anwendungsdashboard, einschließlich des Dashboardlayouts und aller von Ihnen definierten Kacheln.
- Gerätevorlagen, einschließlich Messungen, Einstellungen, Eigenschaften, Befehle und Dashboard.
- Regeln – Alle Regeldefinitionen werden eingeschlossen. Aktionen werden jedoch nicht eingeschlossen, mit Ausnahme von E-Mail-Aktionen.
- Gerätegruppen, einschließlich Gerätezustände und Dashboards.

> [!WARNING]
> Wenn ein Dashboard Kacheln enthält, in denen Informationen zu bestimmten Geräten angezeigt werden, erscheint in der neuen Anwendung die Meldung **Die angeforderte Ressource wurde nicht gefunden** in diesen Kacheln. Sie müssen diese Kacheln erneut konfigurieren, damit in der neuen Anwendung Informationen zu Geräten angezeigt werden.

Die folgenden Elemente sind nicht enthalten, wenn Sie eine Anwendungsvorlage erstellen:

- Geräte
- Benutzer
- Auftragsdefinitionen
- Definitionen für den kontinuierlichen Datenexport

Diese Elemente müssen allen von einer Anwendungsvorlage erstellten Anwendungen manuell hinzugefügt werden.

So erstellen Sie eine Anwendungsvorlage von einer vorhandenen IoT Central-Anwendung:

1. Wechseln Sie in Ihrer Anwendung zum Abschnitt **Verwaltung**.
1. Wählen Sie die Option zum **Exportieren von Anwendungsvorlagen** aus.
1. Geben Sie auf der Seite zum **Exportieren von Anwendungsvorlagen** einen Namen und eine Beschreibung für Ihre Vorlage ein.
1. Wählen Sie die Schaltfläche **Exportieren** aus, um die Anwendungsvorlage zu erstellen. Jetzt können Sie den **Freigabefähigen Link** kopieren, mit dem andere Benutzer eine neue Anwendung von der Vorlage erstellen können:

![Erstellen einer Anwendungsvorlage](media/howto-use-app-templates/create-template.png)

### <a name="use-an-application-template"></a>Verwenden einer Anwendungsvorlage

Um eine Anwendungsvorlage zum Erstellen einer neuen IoT Central-Anwendung zu verwenden, benötigen Sie einen zuvor erstellten **Freigabefähigen Link**. Fügen Sie den **Freigabefähigen Link** in die Adressleiste Ihres Browsers ein. Die Seite **Anwendung erstellen** wird angezeigt. Dabei ist Ihre benutzerdefinierte Anwendungsvorlage ausgewählt:

![Erstellen einer Anwendung von einer Vorlage](media/howto-use-app-templates/create-app.png)

Wählen Sie Ihren Zahlungsplan aus, und füllen Sie die übrigen Felder im Formular aus. Wählen Sie dann **Erstellen** aus, um von der Anwendungsvorlage eine neue IoT Central-Anwendung zu erstellen.

> [!NOTE]
> Wenn eine Anwendung mithilfe des **freigabefähigen Links** erstellt wird, sind die verfügbaren Speicherorte **Europa** und **USA**.

### <a name="manage-application-templates"></a>Verwalten von Anwendungsvorlagen

Auf der Seite zum **Exportieren von Anwendungsvorlagen** können Sie die Anwendungsvorlage löschen oder aktualisieren.

Wenn Sie eine Anwendungsvorlage löschen, ist es nicht mehr möglich, den zuvor generierten freigabefähigen Link zum Erstellen neuer Anwendungen zu verwenden.

Um Ihre Anwendungsvorlage zu aktualisieren, ändern Sie auf der Seite zum **Exportieren von Anwendungsvorlagen** den Namen oder die Beschreibung der Vorlage. Wählen Sie dann erneut die Schaltfläche **Exportieren** aus. Durch diese Aktion wird ein neuer **freigabefähiger Link** generiert. Dadurch werden vorherige URLs für **freigabefähige Links** ungültig.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie Sie Anwendungsvorlagen verwenden. Informieren Sie sich als Nächstes, wie Sie [IoT Central über das Azure-Portal verwalten](../core/howto-manage-iot-central-from-portal.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json).
