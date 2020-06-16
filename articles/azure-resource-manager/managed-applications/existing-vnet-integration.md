---
title: Bereitstellen von Containergruppen in einem vorhandenen virtuellen Netzwerk
description: Beschreibt, wie Sie Benutzern Ihrer verwalteten Anwendung ermöglichen, ein vorhandenes virtuelles Netzwerk auszuwählen. Das virtuelle Netzwerk kann sich außerhalb der verwalteten Anwendung befinden.
author: tfitzmac
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: tomfitz
ms.openlocfilehash: fa5e59b96aada06c2dd486094d9be6a52c79e43e
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260683"
---
# <a name="use-existing-virtual-network-with-azure-managed-applications"></a>Verwenden eines vorhandenen virtuellen Netzwerks mit Azure Managed Applications

In diesem Artikel erfahren Sie, wie Sie eine verwaltete Azure-Anwendung definieren, die in ein vorhandenes virtuelles Netzwerk im Abonnement des Consumers integriert ist. Mit der verwalteten Anwendung kann der Consumer entscheiden, ob ein neues virtuelles Netzwerk erstellt oder ein vorhandenes virtuelles Netzwerk verwendet werden soll. Das vorhandene virtuelle Netzwerk kann sich außerhalb der verwalteten Ressourcengruppe befinden.

## <a name="main-template"></a>Hauptvorlage

Betrachten Sie zunächst die Datei **mainTemplate.json**. Die gesamte Vorlage zum Bereitstellen eines virtuellen Computers und der zugehörigen Ressourcen ist nachstehend dargestellt. Später untersuchen Sie die Teile der Vorlage genauer, die sich auf die Verwendung eines vorhandenen virtuellen Netzwerks beziehen.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/mainTemplate.json":::

Beachten Sie, dass das virtuelle Netzwerk [bedingt bereitgestellt](../templates/conditional-resource-deployment.md) wird. Der Consumer übergibt einen Parameterwert, der angibt, ob ein neues virtuelles Netzwerk erstellt oder ein vorhandenes verwendet werden soll. Wenn der Consumer ein neues virtuelles Netzwerk auswählt, wird die Ressource bereitgestellt. Andernfalls wird die Ressource während der Bereitstellung übersprungen.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/mainTemplate.json" range="111-132" highlight="2":::

Die Variable für die ID des virtuellen Netzwerks weist zwei Eigenschaften auf. Eine Eigenschaft gibt die Ressourcen-ID zurück, wenn ein neues virtuelles Netzwerk bereitgestellt wird. Die andere Eigenschaft gibt die Ressourcen-ID zurück, wenn ein vorhandenes virtuelles Netzwerk verwendet wird. Die Ressourcen-ID für das vorhandene virtuelle Netzwerk enthält den Namen der Ressourcengruppe mit dem virtuellen Netzwerk.

Die Subnetz-ID wird aus dem Wert für die ID des virtuellen Netzwerks generiert. Dabei wird der Wert entsprechend der Auswahl durch den Consumer verwendet.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/mainTemplate.json" range="98-109" highlight="6-10":::

Die Netzwerkschnittstelle wird auf die Variable mit der Subnetz-ID festgelegt.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/mainTemplate.json" range="142-163" highlight="16":::

## <a name="ui-definition"></a>Benutzeroberflächendefinition

Sehen Sie sich nun die Datei **createUiDefinition.json** an. Die gesamte Datei lautet:

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/createUiDefinition.json":::

Die Datei enthält ein Element für ein virtuelles Netzwerk.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/createUiDefinition.json" range="53-81":::

Mit diesem Element kann der Consumer entweder ein neues oder ein vorhandenes virtuelles Netzwerk auswählen.

:::image type="content" source="./media/existing-vnet-integration/new-or-existing-vnet.png" alt-text="Neues oder vorhandenes virtuelles Netzwerk":::

Sie schließen in die Ausgaben einen Wert ein, der angibt, ob der Consumer ein neues oder ein vorhandenes virtuelles Netzwerk ausgewählt hat. Außerdem gibt es einen Wert für die verwaltete Identität.

> [!NOTE]
> Der Ausgabewert für die verwaltete Identität muss den Namen **managedIdentity** tragen.

:::code language="json" source="~/resourcemanager-templates/managed-app-existing-vnet/createUiDefinition.json" range="136-148" highlight="6,12":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen der Benutzeroberflächen-Definitionsdatei finden Sie unter [Die Datei „CreateUiDefinition.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen](create-uidefinition-overview.md).
