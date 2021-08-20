---
title: Aktivieren der Überwachung in Cloud Services (erweiterter Support) mithilfe des Azure-Portals
description: Aktivieren der Überwachung in Cloud Services-Instanzen (erweiterter Support) mithilfe des Azure-Portals
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 32f140ae27501b9f4f3b66e5f5815b8f3f20b3ff
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114445109"
---
# <a name="enable-monitoring-for-cloud-services-extended-support-using-the-azure-portal"></a>Aktivieren der Überwachung für Cloud Services (erweiterter Support) mithilfe des Azure-Portals

In diesem Artikel wird erläutert, wie Sie Warnungen für vorhandene Bereitstellungen von Cloud Services (erweiterter Support) aktivieren. 

## <a name="add-monitoring-rules"></a>Hinzufügen von Überwachungsregeln
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie die Bereitstellung von Cloud Services (erweiterter Support) aus, für die Sie Warnungen aktivieren möchten. 
3. Wählen Sie das Blatt **Warnungen** aus. 

    :::image type="content" source="media/enable-alerts-1.png" alt-text="Die Abbildung zeigt das Auswählen der Registerkarte „Warnungen“ im Azure-Portal.":::

4. Wählen Sie das Symbol **Neue Warnung** aus.
     :::image type="content" source="media/enable-alerts-2.png" alt-text="Die Abbildung zeigt das Auswählen der Option „Neue Warnung“.":::

5. Geben Sie die gewünschten Bedingungen und erforderlichen Aktionen basierend auf den Metriken ein, die Sie nachverfolgen möchten. Sie können die Regeln basierend auf einzelnen Metriken oder dem Aktivitätsprotokoll definieren. 

     :::image type="content" source="media/enable-alerts-3.png" alt-text="Die Abbildung zeigt, wo Warnungen Bedingungen hinzugefügt werden.":::

     :::image type="content" source="media/enable-alerts-4.png" alt-text="Die Abbildung zeigt das Konfigurieren der Signallogik.":::

     :::image type="content" source="media/enable-alerts-5.png" alt-text="Die Abbildung zeigt das Konfigurieren der Aktionsgruppenlogik.":::

6. Wenn Sie das Einrichten von Warnungen abgeschlossen haben, speichern Sie die Änderungen. Basierend auf den konfigurierten Metriken wird das Blatt **Warnungen** im Laufe der Zeit mit Daten aufgefüllt.

## <a name="next-steps"></a>Nächste Schritte 
- Sehen Sie sich die [häufig gestellten Fragen](faq.yml) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.