---
title: Suchen nach Beispielen für Azure Service Fabric Mesh
description: Hier finden Sie einen Index der verfügbaren Service Fabric Mesh-Beispielanwendungen. Der Quellcode in diesen Beispielen veranschaulicht, wie Sie mit dem Service Fabric-Ressourcenmodell ein bestimmtes Szenario erzielen.
ms.date: 12/03/2018
ms.topic: conceptual
ms.openlocfilehash: c944ddc26f2e2d099cf9552acb8287c363d0c768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461911"
---
# <a name="find-service-fabric-mesh-samples"></a>Suchen nach Beispielen für Service Fabric Mesh

In dieser Tabelle sind die verfügbaren Service Fabric Mesh-Beispielanwendungen aufgeführt. Der Quellcode in diesen Beispielen veranschaulicht, wie Sie mit dem Service Fabric-Ressourcenmodell ein bestimmtes Szenario erzielen.

Weitere Informationen zum direkten Bereitstellen von Vorlagen in Azure finden Sie auf der [GitHub-Seite mit den Beispielvorlagen](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md).

|Beispielvorlage|Beschreibung des Szenarios|Quellcode|Entwicklertools|
|------------|--------------------|----------|----------------------|
| [Hello World-App](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Statische Webseite, die in einem Container gehostet wird. Für Linux wird „nginx“ und für Windows wird „IIS“ verwendet. | [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Keine Anforderungen |
| [Zähler-App für Azure-Dateivolumes](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.md) | Speichern Sie den Zustand, indem Sie im Container ein Azure Files-basiertes Volume bereitstellen. <br><br> **Hinweis:** Für diese Vorlage wird eine Azure Files-Dateifreigabe benötigt, um zuvor bereitgestellte [Anleitungen](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) freigeben zu können. | [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Mesh-Tools für Visual Studio |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | Erstellen Sie eine Anwendung mit einem Front-End- und Back-End-Dienst, für die die DNS-basierte Auflösung genutzt wird. Dies wird [hier](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore) im Rahmen eines Tutorials verwendet. | [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Mesh-Tools für Visual Studio |
| [App für visuelle Objekte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Ermöglicht die Skalierung und Aktualisierung von Microservices innerhalb einer Anwendung. | [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Mesh-Tools für Visual Studio |
| [Voting-App](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | Erstellen Sie eine Anwendung mit einem Front-End- und Back-End-Dienst, für die die DNS-basierte Auflösung genutzt wird. | [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Mesh-Tools für Visual Studio für die Windows-Version. Für die Linux-Version kann VS Code/Dotnet-CLI verwendet werden. |
| [Zähler-App für zuverlässige Service Fabric-Volumes](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.sfreliablevolume.md)| Speichern Sie den Zustand, indem Sie im Container ein Volume bereitstellen, das auf zuverlässigen Service Fabric-Datenträgern basiert.| [Quellcode](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Mesh-Tools für Visual Studio |
