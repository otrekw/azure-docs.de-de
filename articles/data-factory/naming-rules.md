---
title: Regeln für die Benennung von Azure Data Factory-Entitäten
description: Beschreibt die Benennungsregeln für Data Factory-Entitäten.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: a1a0622c0736bad5f6c205fab01f4405577ed67a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104783352"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory – Benennungsregeln

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Die folgende Tabelle enthält Benennungsregeln für Data Factory-Artefakte.

| Name | Eindeutigkeit des Namens | Überprüfungen |
|:--- |:--- |:--- |
| Data Factory | Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt, sodass `MyDF` und `mydf` auf die gleiche Data Factory verweisen. |<ul><li>Jede Data Factory ist an genau ein Azure-Abonnement gebunden.</li><li>Objektnamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten.</li><li>Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer vorangestellt werden und er muss von diesen gefolgt sein. Aufeinanderfolgende Bindestriche sind in Containernamen nicht zulässig.</li><li>Namen können 3 bis 63 Zeichen lang sein.</li></ul> |
| Verknüpfte Dienste/Datasets/Pipelines/Datenflüsse | Eindeutig innerhalb einer Data Factory. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Objektnamen müssen mit einem Buchstaben beginnen.</li><li>Die folgenden Zeichen sind nicht zulässig: „.“, „+“, „?“, „/“, „<“, „>“, „*“, „%“, „&“, „:“, „\\“</li><li>Bindestriche („-“) sind in den Namen von verknüpften Diensten, Datenflüssen und Datasets nicht zulässig.</li></ul>  |
| Integration Runtime |Eindeutig innerhalb einer Data Factory. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Der Name der Integration Runtime darf nur Buchstaben, Zahlen und den Bindestrich (-) enthalten.</li><li>Das erste und das letzte Zeichen müssen Buchstaben oder Zahlen sein. Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer vorangestellt werden und er muss von diesen gefolgt sein.</li><li>Aufeinanderfolgende Bindestriche sind im Namen der Integration Runtime nicht zulässig. </li></ul> |
| Datenflusstransformationen | Eindeutig innerhalb eines Datenflusses. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. | <ul><li>Namen von Datenflusstransformationen dürfen nur Buchstaben und Ziffern enthalten.</li><li>Das erste Zeichen muss ein Buchstabe sein. </li></ul> |
| Ressourcengruppe |Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. | Weitere Informationen finden Sie unter [Benennungsregeln und -einschränkungen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). |
| Pipelineparameter und -variable  |Eindeutig innerhalb der Pipeline. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. | <ul><li>Die Überprüfung von Parametername und Variablenname ist aus Gründen der Abwärtskompatibilität auf die Eindeutigkeit beschränkt.</li><li>Wenn Sie Parameter oder Variablen verwenden, um auf Entitätsnamen zu verweisen (z. B. verknüpfter Dienst), gelten die Regeln für die Entitätsbenennung.</li><li>Bei der Benennung Ihrer Pipelineparameter und -variablen sollten Sie die Benennungsregeln für die Datenflusstransformation befolgen.</li></ul> |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Data Factorys erstellen, indem Sie die detaillierten Anweisungen im Artikel [Schnellstart: Erstellen einer Data Factory](quickstart-create-data-factory-powershell.md) befolgen. 
