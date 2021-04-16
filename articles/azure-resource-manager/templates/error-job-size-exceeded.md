---
title: Fehler „Auftragsgröße überschritten“
description: Informationen dazu, wie Sie Fehler beheben, wenn die Auftragsgröße oder die Vorlage zu groß ist.
ms.topic: troubleshooting
ms.date: 03/23/2021
ms.openlocfilehash: b39a0bba15e73bab1a85cbd9e36efebf82d6cf42
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104889364"
---
# <a name="resolve-errors-for-job-size-exceeded"></a>Beheben von Fehlern des Typs „Auftragsgröße überschritten“

In diesem Artikel wird beschrieben, wie die Fehler **JobSizeExceededException** und **DeploymentJobSizeExceededException** behoben werden.

## <a name="symptom"></a>Symptom

Bei der Bereitstellung einer Vorlage erhalten Sie eine Fehlermeldung, die besagt, dass die Bereitstellung Grenzwerte überschritten hat.

## <a name="cause"></a>Ursache

Dieser Fehler wird angezeigt, wenn die Bereitstellung einen der zulässigen Grenzwerte überschreitet. Dies tritt in der Regel auf, wenn Ihre Vorlage oder der Auftrag für die Bereitstellung zu groß ist.

Der Bereitstellungsauftrag darf nicht größer als 1 MB sein. Der Auftrag umfasst Metadaten zur Anforderung. Bei großen Vorlagen können die Metadaten, die mit der Vorlage kombiniert werden, die zulässige Größe für einen Auftrag überschreiten.

Die Vorlage darf 4 MB nicht überschreiten. Die Beschränkung von 4 MB gilt für den endgültigen Status der Vorlage, nachdem sie für Ressourcendefinitionen erweitert wurde, die [copy](copy-resources.md) zum Erstellen von Instanzen verwenden. Der endgültige Zustand enthält auch die aufgelösten Werte für Variablen und Parameter.

Die folgenden weiteren Grenzwerte gelten für die Vorlage:

* 256 Parameter
* 256 Variablen
* 800 Ressourcen (einschließlich copy-Anzahl)
* 64 Ausgabewerte
* 24.576 Zeichen in einem Vorlagenausdruck

Verwenden Sie bei der Bereitstellung von Ressourcen mithilfe von Kopierschleifen nicht den Schleifennamen als Abhängigkeit:

```json
dependsOn: [ "nicLoop" ]
```

Verwenden Sie stattdessen die Instanz der Ressource aus der Schleife, zu der die Abhängigkeit besteht. Beispiel:

```json
dependsOn: [
    "[resourceId('Microsoft.Network/networkInterfaces', concat('nic-', copyIndex()))]"
]
```

## <a name="solution-1---simplify-template"></a>Lösung 1: Vereinfachen der Vorlage

Ihre erste Option besteht darin, die Vorlage zu vereinfachen. Diese Option funktioniert, wenn die Vorlage viele verschiedene Ressourcentypen bereitstellt. Sie können die Vorlage z. B. in [verknüpfte Vorlagen](linked-templates.md) aufteilen. Unterteilen Sie Ihre Ressourcentypen in logische Gruppen, und fügen Sie eine verknüpfte Vorlage für jede Gruppe hinzu. Wenn Sie z. B. viele Netzwerkressourcen bereitstellen müssen, können Sie diese Ressourcen in eine verknüpfte Vorlage verschieben.

Sie können andere Ressourcen als abhängig von der verknüpften Vorlage festlegen und [Werte aus der Ausgabe der verknüpften Vorlage](linked-templates.md#get-values-from-linked-template) abrufen.

## <a name="solution-2---reduce-name-size"></a>Lösung 2: Reduzieren der Größe des Namens

Versuchen Sie, die Länge der Namen zu kürzen, die Sie für [Parameter](template-parameters.md), [Variablen](template-variables.md) und [Ausgaben](template-outputs.md) verwenden. Wenn diese Werte durch Kopierschleifen wiederholt werden, wird ein großer Name mehrmals vervielfacht.