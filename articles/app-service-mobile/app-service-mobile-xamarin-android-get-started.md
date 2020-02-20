---
title: Erste Schritte mit Xamarin.Android-Apps
description: Befolgen Sie dieses Tutorial mit den ersten Schritte zur Verwendung von Azure Mobile Apps für die Xamarin Android-Entwicklung.
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: b42205436c88f9075423bfcaf9e5a9fd931ee4f4
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461367"
---
# <a name="create-a-xamarinandroid-app"></a>Erstellen einer Xamarin.Android-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
In diesem Tutorial erfahren Sie, wie Sie einer Xamarin.Android-App einen cloudbasierten Back-End-Dienst hinzufügen. Weitere Informationen finden Sie unter [Was sind Mobile Apps?](app-service-mobile-value-prop.md)

Screenshot der fertigen App:

![][0]

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin Android-Apps.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto haben, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio mit Xamarin. Anweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](/visualstudio/cross-platform/setup-and-install) .

## <a name="create-an-azure-mobile-app-backend"></a>Erstellen eines Azure Mobile App-Back-Ends
Führen Sie die folgenden Schritte aus, um ein Mobile App-Back-End zu erstellen.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben nun ein Azure-Back-End für mobile Apps bereitgestellt, das von Ihren mobilen Clientanwendungen verwendet werden kann. Als Nächstes laden Sie ein Serverprojekt für ein einfaches Back-End für „Aufgabenlisten“ herunter und veröffentlichen es in Azure.

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>Ausführen der Xamarin.Android-App
1. Öffnen Sie das Projekt „Xamarin.Android“.

2. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu der mobilen App, die Sie erstellt haben. Suchen Sie auf dem Blatt `Overview` nach der URL, die den öffentlichen Endpunkt für Ihre mobile App darstellt. Beispiel: Der Websitename für meinen App-Namen „test123“ lautet https://test123.azurewebsites.net.

3. Öffnen Sie die Datei `ToDoActivity.cs` in diesem Ordner: „xamarin.android/ZUMOAPPNAME/ToDoActivity.cs“. Der Anwendungsname lautet `ZUMOAPPNAME`.

4. Ersetzen Sie in der Klasse `ToDoActivity` die Variable `ZUMOAPPURL` durch den oben angegebenen öffentlichen Endpunkt.

    `const string applicationURL = @"ZUMOAPPURL";`

    wird zu
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Drücken Sie die Taste F5, um die App bereitzustellen und auszuführen.

6. Geben Sie in der App einen sinnvollen Text wie z.B. *Tutorial fertigstellen* ein, und klicken Sie dann auf die Schaltfläche **Hinzufügen**.

    ![][10]

    Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden von dem mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

   > [!NOTE]
   > Sie können den Code überprüfen, der zum Abfragen und Einfügen von Daten auf das mobile App-Back-End zugreift. Der Code befindet sich in der C#-Datei "ToDoActivity.cs".
   
## <a name="troubleshooting"></a>Problembehandlung
Sollten beim Erstellen der Lösung Probleme auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die `Xamarin.Android`-Unterstützungspakete. Schnellstartprojekte enthalten unter Umständen nicht immer die neuesten Versionen.

Beachten Sie, dass alle Unterstützungspakete, auf die in Ihrem Projekt verwiesen wird, die gleiche Version aufweisen müssen. Das [Azure Mobile Apps-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) hat eine `Xamarin.Android.Support.CustomTabs`-Abhängigkeit für die Android-Plattform. Wenn also das Projekt neuere Unterstützungspakete verwendet, müssen Sie dieses Paket direkt mit der erforderlichen Version installieren, um Konflikte zu vermeiden.

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
