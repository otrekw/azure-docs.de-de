---
title: Schnelle Problembehebung für Advisor-Empfehlungen
description: Durchführen einer Massenwartung mithilfe der schnellen Problembehebung in Advisor
ms.topic: article
ms.date: 03/13/2020
ms.author: sagupt
ms.openlocfilehash: a9c86a7ae510d9657f64c71db2aa8c4e3e558f52
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90968585"
---
# <a name="quick-fix-remediation-for-advisor"></a>Schnelle Problembehebung für Advisor
Die **schneller Problembehebung** ermöglicht eine schnellere und einfachere Wartung für Empfehlungen für mehrere Ressourcen. Sie bietet eine Möglichkeit für die Massenwartung von Ressourcen und unterstützt Sie bei der schnelleren Optimierung Ihrer Abonnements durch eine bedarfsgerechte Wartung Ihrer Ressourcen.
Die Funktion ist über das Azure-Portal nur für bestimmte Empfehlungen verfügbar.


## <a name="steps-to-use-quick-fix"></a>Schritte zur Verwendung der „schnellen Problembehebung“

1. Klicken Sie in der Liste der Empfehlungen, die mit **Schnelle Problembehebung** gekennzeichnet sind, auf die entsprechende Empfehlung.

   :::image type="content" source="./media/quick-fix-1.png" alt-text="{Screenshot: Azure Advisor zeigt Bezeichnungen für die schnelle Problembehebung in den Empfehlungen.}":::
   
   *Die Preise in der Abbildung dienen nur als Beispiel*

2. Auf der Seite mit den Empfehlungsdetails sehen Sie eine Liste der Ressourcen, für die Sie diese Empfehlung erhalten. Wählen Sie alle Ressourcen aus, die Sie für die Empfehlung warten möchten.

   :::image type="content" source="./media/quick-fix-2.png" alt-text="Screenshot: Fenster „Betroffene Ressourcen“ mit hervorgehobenen Listenelementen und der Schaltfläche „Schnelle Problembehebung“.":::
   
   *Die Preise in der Abbildung dienen nur als Beispiel*

3. Nachdem Sie die Ressourcen ausgewählt haben, klicken Sie auf die Schaltfläche **Schnelle Problembehebung**, um eine Massenwartung auszuführen.

   > [!NOTE]
   > Einige der aufgelisteten Ressourcen sind möglicherweise deaktiviert, da Sie nicht über die entsprechenden Berechtigungen zum Ändern verfügen.
   
   > [!NOTE]
   > Wenn sich zusätzlich zu den Vorteilen, die in Advisor erwähnt werden, weitere Implikationen ergeben, werden Sie über die Benutzeroberfläche darüber informiert, damit Sie fundierte Entscheidungen zur Wartung treffen können.
   
4. Sie erhalten eine Benachrichtigung zum Wartungsabschluss. Es wird ein Fehler angezeigt, wenn Ressourcen, die nicht gewartet werden, sowie Ressourcen im ausgewählten Modus in der Ressourcenlistenansicht vorhanden sind.  


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie unter:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
* [Advisor-Empfehlungen für einen optimalen Betrieb](advisor-operational-excellence-recommendations.md)
* [Advisor-REST-API](/rest/api/advisor/)
