---
title: Herstellen einer Verbindung mit GitHub
description: Verwenden von GitHub zum Angeben von Common Data Model-Entitätsverweisen
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: jingwang
ms.openlocfilehash: 6446bb9e18a8ce9e69c2462570d06154ed4ed8ac
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107904440"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Verwenden von GitHub zum Lesen von Common Data Model-Entitätsverweisen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Der GitHub-Connector in Azure Data Factory wird nur zum Empfangen des Entitätsverweisschemas für das [Common Data Model](format-common-data-model.md)-Format im Zuordnungsdatenfluss verwendet.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit GitHub verknüpften Dienst unterstützt.

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **GitHub** festgelegt werden. | ja
| userName | GitHub-Benutzername | ja |
| password | GitHub-Kennwort | ja |

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Quelldatasets](data-flow-source.md) im Zuordnungsdatenfluss