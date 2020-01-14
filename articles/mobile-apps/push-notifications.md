---
title: Die Wichtigkeit von Pushbenachrichtigungen in Ihren mobilen Apps mit Visual Studio App Center und Azure Notification Hubs
description: Erfahren Sie mehr über Dienste wie Visual Studio App Center, mit denen Sie Kontakt mit den Benutzern Ihrer mobilen Anwendung aufnehmen können.
author: elamalani
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: 1717bf6da849218f60bae7dafca20cb304286976
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453142"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Kontaktaufnahme mit Benutzern Ihrer Anwendung durch Senden von Pushbenachrichtigungen

Unabhängig davon, ob Sie Endkunden- oder Unternehmensanwendungen erstellen, möchten Sie, dass Benutzer Ihre Anwendung aktiv verwenden. Es gibt häufig Anlässe, zu denen Sie aktuelle Nachrichten, Spieleupdates, spannende Angebote, ein Produktupdate oder jegliche andere wichtige Informationen mit Ihren Benutzern teilen möchten. Um die Kundenbindung Ihrer Benutzer zu erhöhen und sie immer wieder zu Ihrer Anwendung zurückzubringen, benötigen Sie eine Möglichkeit, mit Ihren Benutzern in Echtzeit zu kommunizieren.

Pushbenachrichtigungen bieten eine schnelle und effiziente Möglichkeit, mit den Benutzern Ihrer Anwendung zu kommunizieren. Sie können Ihre Benutzer zum richtigen Zeitpunkt erreichen und ihnen die gewünschten Informationen zukommen lassen, meist in einem Popupelement oder Dialogfeld auf einem mobilen Gerät, und zwar unabhängig davon, welche Aktion die Benutzer gerade ausführen.

## <a name="value-of-push-notifications"></a>Wert von Pushbenachrichtigungen
Pushbenachrichtigungen sind sowohl für Endbenutzer als auch für das Unternehmen wertvoll.

Unternehmen können:
- Direkt mit Benutzern kommunizieren, indem sie Nachrichten über eine unterstützte Plattform zum richtigen Zeitpunkt senden.
- Die Kundenbindung steigern, indem sie Echtzeitupdates und Erinnerungen an Benutzer senden, um diese regelmäßig zu bestärken, sich mit der Anwendung zu beschäftigen.
- Benutzer halten und neuen Kontakt zu inaktiven Benutzer herstellen, die die Anwendung heruntergeladen haben, diese aber nicht verwenden, um wieder aktiv zu werden.
- Die Wechselraten erhöhen, indem sie das Verhalten der Benutzer analysieren, die Benutzer segmentieren und Kampagnen auf der Grundlage von mobilen Pushbenachrichtigungen realisieren.
- Produkte und Dienste voranbringen, indem sie Pushnachrichten und rechtzeitige Updates an Benutzer senden.
- Die Benutzer gezielt ansprechen, indem Sie personalisierte Pushnachrichten senden.

Für Anwendungsbenutzer ermöglichen Pushbenachrichtigungen:
- Bereitstellen von Wert und Informationen in Echtzeit.
- Erinnern der Benutzer, die Anwendung zu verwenden.

Mit den folgenden Diensten können Sie Pushbenachrichtigungen in ihren mobilen Apps aktivieren.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
Mit [App Center Push](/appcenter/push/) können Sie gezielte Nachrichten an iOS-, Android- und Windows-Benutzer senden, ohne dieses Senden von Benachrichtigungen an Geräte mit Push Notification Services (PNS) verwalten zu müssen. Dieser auf Microsoft Azure Notification Hubs aufsetzende Dienst eliminiert durch Bereitstellen eines leistungsfähigen Dashboards die Komplexität, die mit manueller Pushübertragung von Benachrichtigungen verbunden ist.

**Wichtige Features**
- Senden von Pushbenachrichtigungen an mobile Geräte über eine Vielzahl von Plattformen.
- Verwenden von Benachrichtigungen, um Daten an eine Anwendung zu senden, eine Meldung für den Benutzer anzuzeigen oder eine Aktion durch die Anwendung auszulösen
- Verwenden Sie Benachrichtigungsziele für Folgendes: 
    - Senden von Broadcastmeldungen an alle registrierten Geräte.
    - Senden von Benachrichtigungen an Zielgruppen, basierend auf Geräteinformationen und benutzerdefinierten Eigenschaften.
    - Senden von Benachrichtigungen an bestimmte Benutzer.
    - Senden von Benachrichtigungen an bestimmte Geräte.
- Nutzen Sie die umfangreichen Telemetriedaten zu Pushbenachrichtigungen, Geräten und Fehlern, die im App Center-Portal verfügbar sind.
- Erhalten Sie Plattformunterstützung für iOS, Android, macOS, Xamarin, React Native, Unity und Cordova.

**Referenzen**
- [Registrieren bei Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit App Center Push](/appcenter/push/)

## <a name="azure-notification-hubs"></a>Azure Notification Hubs
[Notification Hubs](/azure/notification-hubs/notification-hubs-push-notification-overview) bietet eine einfach zu bedienende und horizontal skalierte Push-Engine. Sie können es verwenden, um Pushbenachrichtigungen an jede Plattform und von jedem Back-End, egal, ob in der Cloud oder lokal, zu senden.

**Wichtige Features**
- Senden von Pushbenachrichtigungen an jede Plattform und von jedem Back-End, egal, ob in der Cloud oder lokal.
- Schnelles Übertragen von Pushbenachrichtigungen mit einem einzigen API-Aufruf an Millionen von mobilen Geräten.
- Anpassen von Pushbenachrichtigungen an Kunden, Sprache und Standort
- Dynamisches Definieren und Benachrichtigen von Kundensegmenten.
- Sofortiges Skalieren auf Millionen von mobilen Geräten.
- Erhalten Sie Plattformunterstützung für iOS, Android, Windows, Kindle und Baidu.
        
**Referenzen**
- [Azure portal](https://portal.azure.com) 
- [Erste Schritte mit Azure Notification Hubs](/azure/notification-hubs/)
- [Schnellstarts](/azure/notification-hubs/create-notification-hub-portal)
- [Beispiele](/azure/notification-hubs/samples)
