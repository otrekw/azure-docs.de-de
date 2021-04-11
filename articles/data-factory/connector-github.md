---
title: Herstellen einer Verbindung mit GitHub
description: Verwenden von GitHub zum Angeben von Common Data Model-Entitätsverweisen
author: dcstwh
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: weetok
ms.openlocfilehash: 7461a0332a36509c7bb6dfdd6db5948b056b35a6
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106222111"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Verwenden von GitHub zum Lesen von Common Data Model-Entitätsverweisen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Der GitHub-Connector in Azure Data Factory wird nur zum Empfangen des Entitätsverweisschemas für das [Common Data Model](format-common-data-model.md)-Format im Zuordnungsdatenfluss verwendet.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit GitHub verknüpften Dienst unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **GitHub** festgelegt werden. | ja
| userName | GitHub-Benutzername | ja |
| password | GitHub-Kennwort | ja |

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Quelldatasets](data-flow-source.md) im Zuordnungsdatenfluss