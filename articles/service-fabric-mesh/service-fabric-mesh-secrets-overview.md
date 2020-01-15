---
title: Speichern und Verwenden von Azure Service Fabric Mesh-Anwendungsgeheimnissen
description: Service Fabric Mesh unterstützt Geheimnisse als Azure-Ressourcen. Im Folgenden erfahren Sie, wie Sie Geheimnisse mit Ihren Service Fabric Mesh-Anwendungen speichern und verwalten können.
author: v-steg
ms.author: jeconnoc
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: d45b9e98c1f325e5dbd656c85655a4ac72e4c3e3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459117"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-Anwendungsgeheimnisse
Service Fabric Mesh unterstützt Geheimnisse als Azure-Ressourcen. Bei einem Service Fabric Mesh-Geheimnis kann es sich um beliebige sensible Informationen in Textform handeln, z.B. Speicherverbindungszeichenfolgen, Kennwörter oder andere Werte, die sicher gespeichert und übertragen werden sollten.

![Übersicht über Mesh-Geheimnisse][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Mesh-Geheimnisressourcen
Ein Mesh-Anwendungsgeheimnis umfasst Folgendes:
* Eine Ressource **Geheimnisse**, bei der es sich um einen Container zum Speichern von Textgeheimnissen handelt. In der Ressource **Geheimnisse** enthaltene Geheimnisse werden auf sichere Weise gespeichert und übertragen.
* Mindestens eine Ressource vom Typ **Geheimnisse/Werte**, die im Ressourcencontainer **Geheimnisse** gespeichert ist. Jede Ressource **Geheimnisse/Werte** verfügt über eine Versionsnummer.

## <a name="next-steps"></a>Nächste Schritte 
Weitere Informationen zu Service Fabric Mesh-Geheimnissen finden Sie in den folgenden Artikeln:
- [Verwalten von Service Fabric Mesh-Anwendungsgeheimnissen](service-fabric-mesh-howto-manage-secrets.md)
- [Einführung in das Service Fabric-Ressourcenmodell](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
