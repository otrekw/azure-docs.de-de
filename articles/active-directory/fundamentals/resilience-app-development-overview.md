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
ms.openlocfilehash: f6078ed96106d6a53b55195ee950e2165334221d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919076"
---
# <a name="increase-resilience-of-authentication-and-authorization-applications-you-develop"></a>Erhöhen der Resilienz bei der Authentifizierung und Autorisierung in von Ihnen entwickelten Anwendungen

Microsoft Identity nutzt moderne tokenbasierte Authentifizierungs- und Autorisierungsverfahren. Das bedeutet, dass eine Anwendung Token von einem Identitätsanbieter abruft, um den Benutzer zu authentifizieren und die Anwendung für das Aufrufen geschützter APIs zu autorisieren.

![Abbildung: eine Anwendung, die Microsoft Identity aufruft](media/resilience-app-development-overview/app-overview.png)

Ein Token ist eine bestimmte Zeit lang gültig, bevor die App ein neues Token abrufen muss. In seltenen Fällen kann es beim Tokenabruf zu einem Fehler aufgrund eines Ausfalls des Netzwerks, der Infrastruktur oder des Authentifizierungsdiensts kommen. In diesem Dokument finden Sie Schritte, die Entwickler ausführen können, um die Resilienz in ihren Anwendungen zu erhöhen, falls beim Tokenabruf ein Fehler auftritt.

Die folgenden Artikel bieten Informationen dazu, wie sich die Resilienz in Apps mithilfe der Microsoft Identity-Plattform und von Azure Active Directory erhöhen lässt. Es gibt Anleitungen sowohl für Clientanwendungen, die im Namen eines angemeldeten Benutzers funktionieren, als auch für Daemonanwendungen, die im eigenen Namen arbeiten. Die Anleitungen enthalten Best Practices für die Verwendung von Token sowie für den Aufruf von Ressourcen.

- [Erzielen von Resilienz in Anwendungen, die Benutzer anmelden](resilience-client-app.md)
- [Erzielen von Resilienz in Anwendungen ohne Benutzer](resilience-daemon-app.md)
- [Erzielen von Resilienz in Ihrer Identitäts- und Zugriffsverwaltungsinfrastruktur](resilience-in-infrastructure.md)
- [Erzielen von Resilienz in der Kundenidentitäts- und Zugriffsverwaltung mit Azure Active Directory B2C](resilience-b2c.md)