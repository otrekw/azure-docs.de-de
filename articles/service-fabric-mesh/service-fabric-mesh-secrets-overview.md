---
title: Speichern und Verwenden von Azure Service Fabric Mesh-Anwendungsgeheimnissen
description: Service Fabric Mesh unterstützt Geheimnisse als Azure-Ressourcen. Im Folgenden erfahren Sie, wie Sie Geheimnisse mit Ihren Service Fabric Mesh-Anwendungen speichern und verwalten können.
author: erikadoyle
ms.author: edoyle
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: da4eaa34840f14714616b85e24fd62cf65602b84
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277633"
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
