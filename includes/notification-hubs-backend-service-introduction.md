---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 98c9d83424e7d347bb6e1166a3b00304d39e0cce
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095278"
---
Ein [ASP.NET Core-Web-API](https://dotnet.microsoft.com/apps/aspnet/apis)-Backend wird verwendet, um die [Geräteregistrierung](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) für den Client unter Verwendung des neuesten und besten [Installationsansatzes](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations) zu verarbeiten. Der Dienst senden auch plattformübergreifend Pushbenachrichtigungen. 

Diese Vorgänge werden mithilfe des [Notification Hubs SDKs für Back-End-Vorgänge](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) verarbeitet. Weitere Details zum allgemeinen Ansatz finden Sie in der Dokumentation zur [Registrierung Ihres App-Back-Ends](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend).
