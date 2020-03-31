---
title: Optimieren von Visual Studio für Azure Service Fabric Mesh
description: In diesem Artikel wird veranschaulicht, wie Sie die Leistung von Visual Studio für Service Fabric Mesh-Projekte optimieren, damit Ihr erster Debuglauf (F5) deutlich schneller durchgeführt wird.
author: dkkapur
ms.author: dekapur
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 40c4e9972a3d83a5dd8247bacac12e9d67a15f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75497981"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Optimieren der Leistung von Visual Studio für Service Fabric Mesh-Projekte

In diesem Artikel wird veranschaulicht, wie Sie die Leistung von Visual Studio für Service Fabric Mesh-Projekte optimieren, damit Ihr erster Debuglauf (F5) deutlich schneller durchgeführt wird.  

## <a name="change-visual-studio-settings"></a>Ändern der Visual Studio-Einstellungen
 
In Visual Studio können Sie unter **Extras** > **Optionen**  > **Service Fabric Mesh-Tools** > **Allgemein** die folgenden Einstellungen anpassen:

- Mit der Option **Erforderliche Docker-Images beim Laden des Projekts mithilfe von Pull übertragen** wird Ihr erster Debuglauf (F5) schneller durchgeführt, indem das Herunterladen des Images während des Ladevorgangs für das Projekt gestartet wird.  
- Mit der Option **Deploy application on project open** (Anwendung beim Öffnen des Projekts bereitstellen) kann Ihr erster Debuglauf (F5) beschleunigt werden, indem der Bereitstellungsprozess gestartet wird, nachdem das Projekt geöffnet wurde.  
- Mit der Option **Remove application on project close** (Anwendung beim Schließen des Projekts entfernen) können die der App zugeordneten Ressourcen (CPU, RAM) wieder beansprucht werden, indem die Mesh-App beim Schließen des Projekts entfernt wird.  

Wenn im Ausgabefenster der Service Fabric-Tools Meldungen mit dem Hinweis angezeigt werden, dass Visual Studio „Images überträgt“, „sich in der Aufwärmphase befindet“ oder „die Anwendung entfernt“, bezieht sich dies auf die obigen Einstellungen. Sie können diese Einstellungen deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Tutorial: Debuggen einer Service Fabric Mesh-Anwendung, die in Ihrem lokalen Entwicklungscluster ausgeführt wird](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md).