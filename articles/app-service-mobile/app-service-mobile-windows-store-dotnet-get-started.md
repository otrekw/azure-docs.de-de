---
title: Erstellen einer UWP-App
description: Dieses Tutorial enthält eine Einführung in die Verwendung von Azure Mobile App-Back-Ends zum Entwickeln von Apps für die universelle Windows-Plattform (UWP) in C#, Visual Basic oder JavaScript.
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 9188db19adab9bd46d65fc97f1c62b39141cee90
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461384"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Erstellen einer Windows-App mit einem Azure-Back-End

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie einer App für die universelle Windows-Plattform (UWP) einen cloudbasierten Back-End-Dienst hinzufügen. Weitere Informationen finden Sie unter [Was sind Mobile Apps?](app-service-mobile-value-prop.md) Im Folgenden sehen Sie Screenshots aus der fertigen App:

![Fertige Desktop-App](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Die Absolvierung dieses Tutorials wird für alle anderen Mobile App-Tutorials für UWP-Apps vorausgesetzt.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto besitzen, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10
* Visual Studio Community 2017.
* Kenntnisse in der Entwicklung von UWP-Apps. In der [UWP-Dokumentation](https://docs.microsoft.com/windows/uwp/) finden Sie Informationen zu den [ersten Schritten](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) beim Erstellen von UWP-Apps.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen Azure Mobile App-Back-Ends

Führen Sie die folgenden Schritte aus, um ein neues mobiles App-Back-End zu erstellen.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-client-project"></a>Ausführen des Clientprojekts

1. Öffnen Sie das UWP-Projekt.

2. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu der mobilen App, die Sie erstellt haben. Suchen Sie auf dem Blatt `Overview` nach der URL, die den öffentlichen Endpunkt für Ihre mobile App darstellt. Beispiel: Der Websitename für meinen App-Namen „test123“ lautet https://test123.azurewebsites.net.

3. Öffnen Sie die Datei `App.xaml.cs` in diesem Ordner: „windows-uwp-cs/ZUMOAPPNAME/“. Der Anwendungsname lautet `ZUMOAPPNAME`.

4. Ersetzen Sie in der `App`-Klasse den Parameter `ZUMOAPPURL` durch den oben stehenden öffentlichen Endpunkt.

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    wird zu
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Drücken Sie die Taste F5, um die App bereitzustellen und auszuführen.

6. Geben Sie in der App unter **TodoItem** einfügen einen beschreibenden Text ein, zum Beispiel *Abschließen des Tutorials*. Klicken Sie dann auf **Speichern**.

    ![Windows-Schnellstart – Vollständig für Desktop](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet.
