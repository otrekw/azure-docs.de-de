---
title: Bereitstellen von Zuordnungen für verwaltete Anwendungen mithilfe einer Richtlinie
description: Informationen zum Bereitstellen von Zuordnungen für verwaltete Azure-Anwendungen mithilfe des Azure Policy-Diensts.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: ec0fe8f66ef2ad2458b4ffad0e848591793e5b05
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "75649346"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>Bereitstellen von Zuordnungen für eine verwaltete Anwendung mithilfe von Azure Policy

Azure-Richtlinien können zum Bereitstellen von Zuordnungen verwendet werden, um Ressourcen eine verwaltete Anwendung zuzuordnen. In diesem Artikel wird eine integrierte Richtlinie beschrieben, die Zuordnungen bereitstellt, und es wird erläutert, wie Sie diese Richtlinie verwenden können.

## <a name="built-in-policy-to-deploy-associations"></a>Integrierte Richtlinie zum Bereitstellen von Zuordnungen

„Zuordnungen für eine verwaltete Anwendung bereitstellen“ ist eine integrierte Richtlinie, die zum Bereitstellen einer Zuordnung verwendet werden kann, um einer verwalteten Anwendung eine Ressource zuzuordnen. Die Richtlinie akzeptiert drei Parameter:

- Verwaltete Anwendungs-ID: Diese ID ist die Ressourcen-ID der verwalteten Anwendung, der die Ressourcen zugeordnet werden müssen.
- Zuzuordnende Ressourcentypen: Diese Ressourcentypen stellen die Liste der Ressourcentypen dar, die der verwalteten Anwendung zugeordnet werden sollen. Sie können einer verwalteten Anwendung mit derselben Richtlinie mehrere Ressourcentypen zuordnen.
- Namenspräfix der Zuordnung: Diese Zeichenfolge ist das Präfix, das dem Namen der zu erstellenden Zuordnungsressource hinzugefügt werden soll. Der Standardwert lautet „DeployedByPolicy“.

Die Richtlinie verwendet die Auswertung von „DeployIfNotExists“. Sie wird ausgeführt, nachdem ein Ressourcenanbieter eine Anforderung zum Erstellen oder Aktualisieren einer Ressource der ausgewählten Ressourcentypen verarbeitet und die Auswertung einen Erfolgsstatuscode zurückgegeben hat. Danach wird die Zuordnungsressource mithilfe einer Vorlagenbereitstellung bereitgestellt.
Weitere Informationen zu Zuordnungen finden Sie unter [Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern – Übersicht](../custom-providers/concepts-resource-onboarding.md).

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Verwenden der integrierten Richtlinie zum Bereitstellen von Zuordnungen 

### <a name="prerequisites"></a>Voraussetzungen
Wenn die verwaltete Anwendung Berechtigungen für das Abonnement benötigt, um eine Aktion auszuführen, funktioniert die Richtlinienbereitstellung der Zuordnungsressource nicht, ohne die Berechtigungen zu erteilen.

### <a name="policy-assignment"></a>Richtlinienzuweisung
Um die integrierte Richtlinie zu verwenden, erstellen Sie eine Richtlinienzuweisung, und weisen Sie die Richtlinie „Zuordnungen für eine verwaltete Anwendung bereitstellen“ zu. Nachdem die Richtlinie erfolgreich zugewiesen wurde, identifiziert die Richtlinie nicht kompatible Ressourcen und stellt die Zuordnung für diese Ressourcen bereit.

![Zuweisen der integrierten Richtlinie](media/concepts-built-in-policy/assign-builtin-policy-managedapp.png)

## <a name="getting-help"></a>Hilfe

Wenn Sie Fragen zur Entwicklung benutzerdefinierter Azure-Ressourcenanbieter haben, stellen Sie sie auf [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag ```azure-custom-providers``` hinzu, um schnell eine Antwort zu erhalten!

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die integrierte Richtlinie zum Bereitstellen von Zuordnungen verwenden. Weitere Informationen finden Sie in diesen Artikeln:

- [Konzepte: Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern – Übersicht](../custom-providers/concepts-resource-onboarding.md)
- [Tutorial: Ressourcenonboarding mit benutzerdefinierten Anbietern](../custom-providers/tutorial-resource-onboarding.md)
- [Tutorial: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure](../custom-providers/tutorial-get-started-with-custom-providers.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](../custom-providers/create-custom-provider.md)
- [Vorgehensweise: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](../custom-providers/custom-providers-action-endpoint-how-to.md)
- [Vorgehensweise: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](../custom-providers/custom-providers-resources-endpoint-how-to.md)
