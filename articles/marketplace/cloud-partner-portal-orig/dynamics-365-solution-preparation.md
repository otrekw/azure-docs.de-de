---
title: Dynamics 365 Solution Preparation
description: Framework zum Verpacken, Installieren und Deinstallieren von Komponenten
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: ac1e4fa541e945f20904ced114a36b58d14585ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278585"
---
# <a name="dynamics-365-solution-preparation"></a>Dynamics 365 Solution Preparation

Das Dynamics 365-Lösungssystem ist ein Framework zum Verpacken, Installieren und Deinstallieren von Komponenten, die bestimmte Geschäftsfunktionalität bieten. Lösungen werden von unabhängigen Softwareanbietern und anderen Partnern von Microsoft Dynamics 365 verwendet, um Erweiterungen zu verteilen, die sie erstellen.

Wenn Sie bereits ein unabhängiger Softwareanbieter für Dynamics 365 (xRM) sind, haben Sie wahrscheinlich bereits eine verwaltete Lösung erstellt und verfügen über eine solution.zip-Datei. Stellen Sie sicher, dass die Felder „Anzeigename“ und „Beschreibung“ in Ihrer Lösung enthalten, was Ihre Kunden erfahren sollen. Diese Felder werden in der CRM Online-Verwaltungskonsole angezeigt.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Hinweis:** Im folgenden Paketbeispiel wird davon ausgegangen, dass der Name der Lösung „SampleSolution.zip“_ lautet.

Wenn Sie ein neuer unabhängiger Softwareanbieter sind, erhalten Sie hier weitere Informationen zum Erstellen einer Lösung: [https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Wenn Ihre Lösung unterstützende Daten erfordert:

* Erstellen der Beispieldaten in der Testumgebung
* Verwenden Sie das Tool für die Konfigurationsmigration, um ein Schema mit Vergleichsregeln für Ihre Daten zu erstellen.
* Speichern Sie Ihr Konfigurationsschema mit Ihren Projektdateien. Sie werden dies später benötigen, wenn Sie Ihre Konfigurationsdaten aktualisieren.
* Exportieren Sie Ihre Konfigurationsdaten. Achten Sie darauf, dass Sie der Exportdatei einen Namen geben, der für den Export sinnvoll ist.
