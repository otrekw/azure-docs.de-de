---
title: 'Upgraden einer zusammengesetzten Entität: LUIS'
description: Hier erfahren Sie, wie Sie mithilfe des Upgradeprozesses im LUIS-Portal eine zusammengesetzte Entität zu einer durch maschinelles Lernen erworbenen Entität upgraden.
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: 9bbb03a2009bdcb6e2fa5d20aefadd4c7c99f025
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599314"
---
# <a name="upgrade-composite-entity-to-machine-learned-entity"></a>Upgraden einer zusammengesetzten Entität zu einer durch maschinelles Lernen erworbenen Entität

Upgraden Sie eine zusammengesetzte Entität zu einer durch maschinelles Lernen erworbenen Entität, um eine Entität zu erstellen, die ausführlichere, besser aufschlüsselbare Vorhersagen für das Debuggen der Entität erhält.

## <a name="current-version-model-restrictions"></a>Modelleinschränkungen der aktuellen Version

Mithilfe des Upgradeprozesses werden durch maschinelles Lernen erworbene Entitäten in einer neuen Version Ihrer App erstellt (basierend auf den vorhandenen zusammengesetzten Entitäten in Ihrer App). Dabei werden untergeordnete Elemente und Rollen der zusammengesetzten Entität mit einbezogen. Außerdem werden die Beschriftungen in Beispieläußerungen ausgetauscht, um die neue Entität zu verwenden.

## <a name="upgrade-process"></a>Upgradeprozess

Der Upgradeprozess umfasst Folgendes:
* Erstellen einer neuen, durch maschinelles Lernen erworbenen Entität für jede zusammengesetzte Entität
* Untergeordnete Entitäten:
    * Wenn eine untergeordnete Entität nur in der zusammengesetzten Entität verwendet wird, wird sie nur der durch maschinelles Lernen erworbenen Entität hinzugefügt.
    * Wird eine untergeordnete Entität in der zusammengesetzten Entität _und_ als separate Entität (in Beispieläußerungen beschriftet) verwendet, wird sie der Version als Entität und der neuen, durch maschinelles Lernen erworbenen Entität als untergeordnete Entität hinzugefügt.
    * Wenn die untergeordnete Entität eine Rolle verwendet, werden die einzelnen Rollen jeweils in eine untergeordnete Entität gleichen Namens konvertiert.
    * Handelt es sich bei der untergeordneten Entität um eine nicht durch maschinelles Lernen erworbene Entität (regulärer Ausdruck, Listenentität oder eine vordefinierte Entität), wird eine neue untergeordnete Entität gleichen Namens erstellt, und die neue untergeordnete Entität verfügt über ein Feature, bei dem der nicht durch maschinelles Lernen erworbenen Entität das erforderliche Feature hinzugefügt wird.
* Namen werden beibehalten, müssen aber auf der gleichen Ebene (Unterentität/gleichgeordnetes Element) eindeutig sein. Weitere Informationen finden Sie unter [Beschränkungen für eindeutige Benennungen](luis-boundaries.md#name-uniqueness).
* Beschriftungen in Beispieläußerungen werden auf die neue, durch maschinelles Lernen erworbene Entität mit untergeordneten Entitäten umgestellt.

Die Modelländerungen werden im folgenden Diagramm veranschaulicht:

|Altes Objekt|Neues Objekt|Hinweise|
|--|--|--|
|Zusammengesetzte Entität|Durch maschinelles Lernen erworbene Entität mit Struktur|Beide Objekte sind übergeordnete Objekte.|
|Die untergeordnete Entität der zusammengesetzten Entität ist eine **einfache Entität**.|Untergeordnete Entität|Beide Objekte sind untergeordnete Objekte.|
|Die untergeordnete Entität der zusammengesetzten Entität ist eine **vordefinierte Entität** (beispielsweise eine Zahl).|Untergeordnete Entität mit dem Namen der vordefinierten Entität (beispielsweise eine Zahl). Die untergeordnete Entität verfügt außerdem über ein _Feature_ in Form einer vordefinierten Zahlenentität, und die Einschränkungsoption ist auf _true_ festgelegt.|Die untergeordnete Entität enthält ein Feature mit Einschränkung auf der Ebene der untergeordneten Entität.|
|Die untergeordnete Entität der zusammengesetzten Entität ist eine **vordefinierte Entität** (beispielsweise eine Zahl), und die vordefinierte Entität verfügt über eine **Rolle**.|Untergeordnete Entität mit dem Namen der Rolle. Die untergeordnete Entität verfügt außerdem über ein Feature in Form einer vordefinierten Zahlenentität, und die Einschränkungsoption ist auf „true“ festgelegt.|Die untergeordnete Entität enthält ein Feature mit Einschränkung auf der Ebene der untergeordneten Entität.|
|Role|Untergeordnete Entität|Der Rollenname wird zum Namen der untergeordneten Entität. Die untergeordnete Entität ist ein direkter Nachfolger der durch maschinelles Lernen erworbenen Entität.|

## <a name="begin-upgrade-process"></a>Starten des Upgradeprozesses

Führen Sie vor der Aktualisierung Folgendes aus:

* Wechseln Sie die Version, falls Sie sich nicht in der korrekten Version für das Upgrade befinden.


1. Starten Sie den Upgradeprozess über die Benachrichtigung, oder warten Sie bis zur nächsten geplanten Aufforderung.

    > [!div class="mx-imgBorder"]
    > ![Starten des Upgrades über Benachrichtigungen](./media/update-composite-entity/notification-begin-update.png)

1. Wählen Sie im Popupelement die Option **Jetzt aktualisieren**.

1. Lesen Sie, **was passiert, wenn Sie das Upgrade** durchführen, und wählen Sie anschließend **Weiter** aus.

1. Wählen Sie in der Liste die zusammengesetzten Entitäten aus, für die das Upgrade durchgeführt werden soll, und wählen Sie anschließend **Weiter** aus.

1. Durch Aktivieren des entsprechenden Kontrollkästchens können untrainierte Änderungen aus der aktuellen Version in die Upgradeversion übertragen werden.

1. Wählen Sie **Weiter** aus, um den Upgradeprozess zu starten.

1. Die Statusleiste gibt Aufschluss über den Status des Upgradevorgangs.

1. Nach Abschluss des Prozesses befinden Sie sich in einer neuen trainierten Version mit den neuen, durch maschinelles Lernen erworbenen Entitäten. Wählen Sie **Try your new entities** (Neue Entitäten testen) aus, um die neue Entität anzuzeigen.

    Falls das Upgrade oder Training für die neue Entität nicht erfolgreich war, erhalten Sie eine Benachrichtigung mit weiteren Informationen.

1. Auf der Seite mit der Entitätsliste wird neben dem Namen der neuen Entitäten **NEU** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Ersteller und Mitwirkende](luis-how-to-collaborate.md)