---
title: Erstellen einer Android-App
description: Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung mobiler Azure-App-Back-Ends für die Android-App-Entwicklung.
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 9be9402bff1a1263de3c6f21b78899464c50c823
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77459565"
---
# <a name="create-an-android-app"></a>Erstellen einer Android-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
Dieses Lernprogramm zeigt Ihnen, wie Sie einen cloudbasierten Back-End-Dienst mithilfe eines mobilen Azure-App-Back-Ends zu einer mobilen Android-App hinzufügen.  Sie erstellen sowohl ein neues Mobile App-Back-End als auch eine einfache Android-App des Typs *Aufgabenliste*, die App-Daten in Azure speichert.

Das Absolvieren dieses Lernprogramms ist Voraussetzung für alle anderen Android-Lernprogramme zur Verwendung des Features "Mobile Apps" von Azure App Service.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Die [Android Developer Tools](https://developer.android.com/sdk/index.html) mit der integrierten Entwicklungsumgebung Android Studio sowie der neuesten Android-Plattform
* Azure Mobile Android SDK.
* Ein [aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen mobilen Azure-App-Back-Ends
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-android-app"></a>Ausführen der Android-App
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
