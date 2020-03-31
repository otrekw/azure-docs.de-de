---
title: Bereitstellen von Zuordnungen mittels Richtlinien
description: Hier erfahren Sie, wie Sie Zuordnungen für einen benutzerdefinierten Anbieter mithilfe des Azure Policy-Diensts bereitstellen.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: 536b95eb07619d0ce2d02ec01e1f51ed52c1b5e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649202"
---
# <a name="deploy-associations-for-a-custom-provider-using-azure-policy"></a>Bereitstellen von Zuordnungen für einen benutzerdefinierten Anbieter per Azure-Richtlinie

Azure-Richtlinien können zum Bereitstellen von Zuordnungen verwendet werden, um Ressourcen einem benutzerdefinierten Anbieter zuzuordnen. In diesem Artikel wird eine integrierte Richtlinie beschrieben, die Zuordnungen bereitstellt, und es wird erläutert, wie Sie diese Richtlinie verwenden können.

## <a name="built-in-policy-to-deploy-associations"></a>Integrierte Richtlinie zum Bereitstellen von Zuordnungen

„Zuordnungen für einen benutzerdefinierten Anbieter bereitstellen“ ist eine integrierte Richtlinie, mit der eine Zuordnung bereitgestellt werden kann, um eine Ressource einem benutzerdefinierten Anbieter zuzuordnen. Die Richtlinie akzeptiert drei Parameter:

- ID des benutzerdefinierten Anbieters: Diese ID ist die Ressourcen-ID des benutzerdefinierten Anbieters, dem die Ressourcen zugeordnet werden sollen.
- Zuzuordnende Ressourcentypen: Diese Ressourcentypen stellen die Liste der Ressourcentypen dar, die dem benutzerdefinierten Anbieter zugeordnet werden sollen. Einem benutzerdefinierten Anbieter können mehrere Ressourcentypen mit der gleichen Richtlinie zugeordnet werden.
- Namenspräfix der Zuordnung: Diese Zeichenfolge ist das Präfix, das dem Namen der zu erstellenden Zuordnungsressource hinzugefügt werden soll. Der Standardwert lautet „DeployedByPolicy“.

Die Richtlinie verwendet die Auswertung von „DeployIfNotExists“. Sie wird ausgeführt, nachdem ein Ressourcenanbieter eine Anforderung zum Erstellen oder Aktualisieren einer Ressource verarbeitet hat und die Auswertung einen Erfolgsstatuscode zurückgegeben hat. Danach wird die Zuordnungsressource mithilfe einer Vorlagenbereitstellung bereitgestellt.
Weitere Informationen zu Zuordnungen finden Sie unter [Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern – Übersicht](./concepts-resource-onboarding.md).

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Verwenden der integrierten Richtlinie zum Bereitstellen von Zuordnungen 

### <a name="prerequisites"></a>Voraussetzungen
Wenn der benutzerdefinierte Anbieter Berechtigungen für den Richtlinienbereich benötigt, um eine Aktion ausführen zu können, funktioniert die Richtlinienbereitstellung der Zuordnungsressource nur, wenn die entsprechenden Berechtigungen erteilt werden.

### <a name="policy-assignment"></a>Richtlinienzuweisung
Um die integrierte Richtlinie zu verwenden, erstellen Sie eine Richtlinienzuweisung, und weisen Sie die Richtlinie „Zuordnungen für einen benutzerdefinierten Anbieter bereitstellen“ zu. Anschließend identifiziert die Richtlinie nicht kompatible Ressourcen und stellt die Zuordnung für diese Ressourcen bereit.

![Zuweisen der integrierten Richtlinie](media/concepts-built-in-policy/assign-builtin-policy-customprovider.png)

## <a name="getting-help"></a>Hilfe

Wenn Sie Fragen zur Entwicklung benutzerdefinierter Azure-Ressourcenanbieter haben, stellen Sie sie auf [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag ```azure-custom-providers``` hinzu, um schnell eine Antwort zu erhalten!

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die integrierte Richtlinie zum Bereitstellen von Zuordnungen verwenden. Weitere Informationen finden Sie in diesen Artikeln:

- [Konzepte: Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern – Übersicht](./concepts-resource-onboarding.md)
- [Tutorial: Ressourcenonboarding mit benutzerdefinierten Anbietern](./tutorial-resource-onboarding.md)
- [Tutorial: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure](./tutorial-get-started-with-custom-providers.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [Vorgehensweise: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Vorgehensweise: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
