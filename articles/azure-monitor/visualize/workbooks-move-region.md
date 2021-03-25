---
title: 'Azure Monitor-Arbeitsmappen: Verschieben in Regionen'
description: Verschieben einer Arbeitsmappe in eine andere Region
services: azure-monitor
author: gardnerjr
manager: acearun
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/12/2020
ms.author: jgardner
ms.openlocfilehash: d58d9ab48fa16fb5258f097ed4567e539e21c72c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100601132"
---
# <a name="move-an-azure-workbook-to-another-region"></a>Verschieben einer Azure-Arbeitsmappe in eine andere Region

In diesem Artikel wird beschrieben, wie Sie Azure-Arbeitsmappenressourcen in eine andere Azure-Region verschieben. Sie können Ihre Ressourcen aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, um Features oder Dienste bereitzustellen, die nur in bestimmten Regionen verfügbar sind, um interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf die Anforderungen, die bei der Kapazitätsplanung bestimmt wurden.

## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass Arbeitsmappen in der Zielregion unterstützt werden.

* Diese Anweisungen gelten sowohl für freigegebene Arbeitsmappen (`microsoft.insights/workbooks`) als auch für in Azure Monitor und den meisten Ressourcentypen gespeicherte private Arbeitsmappen (`microsoft.insights/myworkbooks`).

  Arbeitsmappen, die speziell mit dem Ressourcentyp Application Insights verknüpft sind, werden jedoch in der Azure-Region gespeichert, in der die Application Insights-Ressource gespeichert ist.

  Diese Arbeitsmappen können nicht einzeln in eine andere Region verschoben werden.

## <a name="move"></a>Move

Die einfachste Möglichkeit zum Verschieben einer Azure-Arbeitsmappe ist die Verwendung von „Speichern unter“ im Arbeitsmappentool auf der Portal-Benutzeroberfläche:

1. Öffnen Sie die Zielarbeitsmappe in der Arbeitsmappenanzeige.
2. Wechseln Sie über die Symbolleistenschaltfläche „Bearbeiten“ in den Bearbeitungsmodus.
3. Verwenden Sie die Symbolleistenschaltfläche „Speichern unter“.
4. Wählen Sie im Speicherformular einen Namen und die gewünschte Region für die Arbeitsmappe aus. Stellen Sie sicher, dass die anderen Felder für Abonnement, Ressourcengruppe und Freigabe geeignete Werte enthalten.

   > [!NOTE]
   > Möglicherweise müssen Sie einen neuen Namen für die Arbeitsmappe verwenden, um doppelte Namen zu vermeiden.

5. Speichern Sie. 

## <a name="verify"></a>Überprüfung

Suchen Sie die neue Arbeitsmappe über die Benutzeroberfläche zum Durchsuchen von Azure-Arbeitsmappen. Vergewissern Sie sich, dass der Speicherort der Zielspeicherort ist.

## <a name="clean-up"></a>Bereinigen

Nachdem die Arbeitsmappe in der neuen Region erstellt wurde, löschen Sie die ursprüngliche Arbeitsmappe in der vorherigen Region.
1. Öffnen Sie die ursprüngliche Arbeitsmappe in der Arbeitsmappenanzeige.
2. Wechseln Sie über die Symbolleistenschaltfläche „Bearbeiten“ in den Bearbeitungsmodus.
3. Wählen Sie im Dropdownmenü für Bearbeitungstools (Stiftsymbol) die Option „Arbeitsmappe löschen“ aus.

Wenn Sie Ihre Arbeitsmappe umbenannt haben, um sie in eine neue Region zu importieren, können Sie die Arbeitsmappe in den vorherigen Namen umbenennen, nachdem die ursprüngliche Arbeitsmappe mit der Option „Umbenennen“ in der Dropdownliste der Bearbeitungstools auf der Symbolleiste des Bearbeitungsmodus gelöscht wurde.

## <a name="next-steps"></a>Nächste Schritte

Sie müssen eine Arbeitsmappenvorlage anstelle einer Arbeitsmappe verschieben? Informieren Sie sich, wie Sie [eine Azure-Arbeitsmappenvorlage in eine andere Region verschieben](./workbook-templates-move-region.md).

Informieren Sie sich über das [Bereitstellen von Arbeitsmappen und Arbeitsmappenvorlagen](../visualize/workbooks-automate.md) über ARM-Vorlagen.
