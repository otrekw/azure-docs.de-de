---
title: Erhöhen der Resilienz bei der Authentifizierung und Autorisierung in von Ihnen entwickelten Anwendungen
titleSuffix: Microsoft identity platform
description: Übersicht über die wichtigsten Artikel zur Resilienz bei der Anwendungsentwicklung mit Azure Active Directory und der Microsoft Identity-Plattform
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: 5d469ed875a33aa6e451775521cb3ab734a9cdc7
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107929613"
---
# <a name="increase-resilience-of-authentication-and-authorization-applications-you-develop"></a>Erhöhen der Resilienz bei der Authentifizierung und Autorisierung in von Ihnen entwickelten Anwendungen

Microsoft Identity nutzt moderne tokenbasierte Authentifizierungs- und Autorisierungsverfahren. Dies bedeutet, dass eine Client-Anwendung Token von einem Identity Provider erwirbt, um den Benutzer zu authentifizieren und die Anwendung zum Aufruf geschützter APIs zu autorisieren. Ein Dienst wird Token validieren.

Ein Token ist eine bestimmte Zeit lang gültig, bevor die App ein neues Token abrufen muss. In seltenen Fällen kann es beim Tokenabruf zu einem Fehler aufgrund eines Ausfalls des Netzwerks, der Infrastruktur oder des Authentifizierungsdiensts kommen. In diesem Dokument finden Sie Schritte, die Entwickler ausführen können, um die Resilienz in ihren Anwendungen zu erhöhen, falls beim Tokenabruf ein Fehler auftritt.

Die folgenden Artikel bieten Informationen dazu, wie sich die Resilienz in Apps mithilfe der Microsoft Identity-Plattform und von Azure Active Directory erhöhen lässt. Es sind Anleitungen verfügbar, sowohl für Client- und Service-Anwendungen, die im Auftrag eines angemeldeten Benutzers arbeiten, als auch für Daemon-Anwendungen, die im eigenen Auftrag arbeiten. Die Anleitungen enthalten Best Practices für die Verwendung von Token sowie für den Aufruf von Ressourcen.

- [Erzielen von Resilienz in Anwendungen, die Benutzer anmelden](resilience-client-app.md)
- [Erzielen von Resilienz in Anwendungen ohne Benutzer](resilience-daemon-app.md)
- [Erzielen von Resilienz in Ihrer Identitäts- und Zugriffsverwaltungsinfrastruktur](resilience-in-infrastructure.md)
- [Erzielen von Resilienz für Ihre CIAM-Systeme](resilience-b2c.md)
- [Erstellen Sie Dienste, die resistent gegen die Aktualisierung von Metadaten sind](../develop/howto-build-services-resilent-to-metadata-refresh.md)
