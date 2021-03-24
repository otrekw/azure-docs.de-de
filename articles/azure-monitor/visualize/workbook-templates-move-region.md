---
title: 'Azure Monitor-Arbeitsmappenvorlagen: Verschieben von Regionen'
description: Verschieben einer Arbeitsmappenvorlage in eine andere Region
services: azure-monitor
author: gardnerjr
manager: acearun
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/12/2020
ms.author: jgardner
ms.openlocfilehash: 9787118b1912a5697c02e116be99b1f396748937
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100599520"
---
# <a name="move-an-azure-workbook-template-to-another-region"></a>Verschieben einer Azure-Arbeitsmappenvorlage in eine andere Region

In diesem Artikel wird beschrieben, wie Sie Ressourcen von Azure-Arbeitsmappenvorlagen in eine andere Azure-Region verschieben. Sie können Ihre Ressourcen aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, um Features oder Dienste bereitzustellen, die nur in bestimmten Regionen verfügbar sind, um interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf die Anforderungen, die bei der Kapazitätsplanung bestimmt wurden.

Zurzeit gibt es keine Portal-Benutzeroberfläche zum Erstellen von Arbeitsmappenvorlagen-Ressourcen. Die einzige Möglichkeit, diese zu erstellen, bieten derzeit [Azure Resource Manager-Vorlagenbereitstellungen (ARM-Vorlagen)](../visualize/workbooks-automate.md). Die einfachste Methode zum Verschieben einer Vorlage ist, die vorherige ARM-Vorlage wiederzuverwenden und für die Bereitstellung in der neuen Region zu aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Arbeitsmappenvorlagen in der Zielregion unterstützt werden.

## <a name="prepare"></a>Vorbereiten

* Identifizieren Sie die zuvor verwendete ARM-Vorlage für die Arbeitsmappenvorlage.

## <a name="move"></a>Move

1. Aktualisieren Sie die zuvor verwendete Vorlage so, dass sie auf die neue Region verweist.

   > [!NOTE]
   > Möglicherweise müssen Sie einen neuen Namen für die Arbeitsmappenvorlage verwenden, um doppelte Namen zu vermeiden.

2. Stellen Sie die aktualisierte Vorlage über eine ARM-Vorlagenbereitstellung bereit, um eine neue Arbeitsmappenvorlage in der gewünschten Region zu erstellen.

## <a name="verify"></a>Überprüfung

Suchen Sie die neu bereitgestellte Arbeitsmappenvorlage über die Benutzeroberfläche zum Durchsuchen von Azure-Arbeitsmappen. Vergewissern Sie sich, dass der Speicherort der Zielspeicherort ist.

## <a name="clean-up"></a>Bereinigen

Nachdem die Arbeitsmappenvorlage in der neuen Region erstellt wurde, löschen Sie die ursprüngliche Arbeitsmappenvorlage in der vorherigen Region.
1. Suchen Sie die Arbeitsmappenvorlage über die Benutzeroberfläche zum Durchsuchen von Azure-Arbeitsmappen.
2. Wählen Sie die zu löschende Arbeitsmappenvorlage aus.
3. Wählen Sie den Befehl „Löschen“ aus.

Wenn Sie Ihre Arbeitsmappenvorlage umbenannt haben, um sie in eine neue Region zu importieren, können Sie die Arbeitsmappenvorlage in den vorherigen Namen umbenennen, nachdem das ursprüngliche Element mit dem Befehl „Umbenennen“ in der Ressourcenansicht für Azure-Arbeitsmappenvorlagen gelöscht wurde.

## <a name="next-steps"></a>Nächste Schritte

Sie müssen eine Arbeitsmappe anstelle einer Vorlage verschieben? Erfahren Sie, wie Sie [eine Azure-Arbeitsmappenvorlage in eine andere Region verschieben](./workbooks-move-region.md).

