---
title: Neuerungen in Custom Vision
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Neuigkeiten zu Custom Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 06/29/2020
ms.author: pafarley
ms.openlocfilehash: 019a9264beddafbd4585810967551e064592e94d
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90602522"
---
# <a name="whats-new-in-custom-vision"></a>Neuerungen in Custom Vision

Informieren Sie sich über die Neuerungen im Dienst. Dabei kann es sich um Versionshinweise, Videos, Blogbeiträge und andere Informationen handeln. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.

## <a name="july-2020"></a>Juli 2020

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

* Custom Vision unterstützt die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC), ein Autorisierungssystem für die Verwaltung des individuellen Zugriffs auf Azure-Ressourcen. Weitere Informationen zum Verwalten des Zugriffs auf Ihre Custom Vision-Projekte finden Sie unter [Rollenbasierte Zugriffssteuerung](./role-based-access-control.md).

### <a name="subset-training"></a>Teilmengentraining

* Beim Training eines Objekterkennungsprojekts können Sie optional auch nur mit einer Teilmenge Ihrer angewendeten Tags trainieren. Das kann sinnvoll sein, wenn bestimmte Tags noch nicht ausreichend angewendet wurden, andere jedoch schon. Sehen Sie sich den Artikel [Schnellstart: Erstellen eines Objekterkennungsprojekts mit dem Custom Vision SDK](./quickstarts/object-detection.md) zu C# oder Python an, um weitere Informationen zu erhalten.

### <a name="azure-storage-notifications"></a>Speicherbenachrichtigungen in Azure

* Sie können Ihr Custom Vision-Projekt in eine Azure-Blobspeicher-Warteschlange integrieren, um Pushbenachrichtigungen zu Trainings- und Exportaktivitäten für Projekte und zu Sicherungskopien von veröffentlichten Modellen zu erhalten. Dieses Feature ist hilfreich, um das ständige Abfragen des Diensts nach Ergebnissen zu vermeiden, wenn lange Vorgänge ausgeführt werden. Stattdessen können Sie die Benachrichtigungen für die Speicherwarteschlange in Ihren Workflow integrieren. Unter [Integrieren von Azure-Speicher für Benachrichtigungen und Sicherungen](./storage-integration.md) finden Sie weitere Informationen.

### <a name="copy-and-move-projects"></a>Kopieren und Verschieben von Projekten

* Sie können nun Projekte von einem Custom Vision-Konto in andere kopieren. Beispielsweise können Sie ein Projekt aus einer Entwicklungsumgebung in eine Produktionsumgebung verschieben oder ein Projekt in einem Konto in einer anderen Azure-Region sichern, um die Datensicherheit zu erhöhen. Unter [Kopieren und Verschieben von Custom Vision-Projekten](./copy-move-projects.md) finden Sie weitere Informationen.

## <a name="september-2019"></a>September 2019

### <a name="suggested-tags"></a>Vorgeschlagene Tags

* Die intelligente Bezeichnungserstellung auf der [Custom Vision-Website](https://www.customvision.ai/) generiert vorgeschlagene Tags für Ihre Trainingsbilder. Auf diese Weise können Sie eine große Anzahl von Bildern schneller beschriften, wenn Sie ein Custom Vision-Modell trainieren. Eine Anleitung für dieses Feature finden Sie unter [Schnelleres Beschriften von Bildern mit der intelligenten Bezeichnungserstellung](./suggested-tags.md).

## <a name="cognitive-service-updates"></a>Cognitive Services-Updates

[Azure-Updateankündigungen für Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)