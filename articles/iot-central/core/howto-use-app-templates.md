---
title: Exportieren einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Ich möchte als Lösungsmanager eine Anwendungsvorlage exportieren, um sie wiederverwenden zu können.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: aec72644f708d6363a80da28c5e571d0165fcdfa
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91651835"
---
# <a name="export-your-application"></a>Exportieren Ihrer Anwendung

In diesem Artikel wird beschrieben, wie Sie als Lösungsmanager eine IoT Central-Anwendung exportieren, um sie wiederverwenden zu können.

Sie haben zwei Möglichkeiten:

- Sie können eine Kopie Ihrer Anwendung erstellen, wenn Sie nur eine duplizierte Kopie davon erstellen müssen.
- Wenn Sie die Erstellung mehrerer Kopien planen, können Sie aus Ihrer Anwendung eine Anwendungsvorlage erstellen.

## <a name="copy-your-application"></a>Kopieren Ihrer Anwendung

Sie können eine Kopie jeder Anwendung erstellen, mit Ausnahme von Geräteinstanzen, Gerätedatenverlauf und Benutzerdaten. Die Kopie verwendet einen Standard-Tarif, der Ihnen in Rechnung gestellt wird. Es ist nicht möglich, eine Anwendung mit dem kostenlosen Tarif zu erstellen, indem Sie eine Anwendung kopieren.

Wählen Sie **Kopieren** aus. Geben Sie im Dialogfeld die Details für die neue Anwendung ein. Wählen Sie dann **Kopieren** aus, um zu bestätigen, dass Sie den Vorgang fortsetzen möchten. Weitere Informationen zu den Feldern im Formular finden Sie im Schnellstart [Erstellen einer Anwendung](quick-deploy-iot-central.md).

![Screenshot, der die Seite „Anwendungseinstellungen kopieren“ zeigt.](media/howto-use-app-templates/appcopy2.png)

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

Wählen Sie Ihren Tarif aus, und füllen Sie die übrigen Felder im Formular aus. Wählen Sie dann **Erstellen** aus, um von der Anwendungsvorlage eine neue IoT Central-Anwendung zu erstellen.

### <a name="manage-application-templates"></a>Verwalten von Anwendungsvorlagen

Auf der Seite zum **Exportieren von Anwendungsvorlagen** können Sie die Anwendungsvorlage löschen oder aktualisieren.

Wenn Sie eine Anwendungsvorlage löschen, ist es nicht mehr möglich, den zuvor generierten freigabefähigen Link zum Erstellen neuer Anwendungen zu verwenden.

Um Ihre Anwendungsvorlage zu aktualisieren, ändern Sie auf der Seite zum **Exportieren von Anwendungsvorlagen** den Namen oder die Beschreibung der Vorlage. Wählen Sie dann erneut die Schaltfläche **Exportieren** aus. Durch diese Aktion wird ein neuer **freigabefähiger Link** generiert. Dadurch werden vorherige URLs für **freigabefähige Links** ungültig.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen jetzt, wie Sie Anwendungsvorlagen verwenden. Lernen Sie als Nächstes, wie Sie [die Gesamtintegrität der Geräte überwachen, die mit einer IoT Central-Anwendung verbunden sind](howto-monitor-application-health.md).
