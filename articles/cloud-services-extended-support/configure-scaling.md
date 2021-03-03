---
title: Konfigurieren der Skalierung für Azure Cloud Services (erweiterter Support)
description: 'Vorgehensweise: Aktivieren der Skalierungsoptionen für Azure Cloud Services (erweiterter Support)'
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: cfa5be01a0d36764086c6c9adf97e6cb166d2bb6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101728160"
---
# <a name="configure-scaling-options-with-azure-cloud-services-extended-support"></a>Konfigurieren der Skalierungsoptionen für Azure Cloud Services (erweiterter Support) 

Bedingungen können so konfiguriert werden, dass Bereitstellungen von Cloud Services (erweiterter Support) abskaliert und aufskaliert werden können. Diese Bedingungen können auf der Auslastung von CPU, Datenträger und Netzwerk basieren. 

Beachten Sie beim Konfigurieren der Skalierung Ihrer Cloud Service-Bereitstellungen die folgenden Informationen:
- Die Skalierung wirkt sich auf die Kernauslastung aus. Größere Rolleninstanzen verbrauchen mehr Kerne, und Sie können nur innerhalb des Kerngrenzwerts Ihres Abonnements skalieren. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Auf dem Schwellenwert für Warteschlangennachrichten basierende Skalierung wird unterstützt. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Queue Storage](../storage/queues/storage-dotnet-how-to-use-queues.md).
- Um die Hochverfügbarkeit Ihrer Anwendungen unter Cloud Services (erweiterter Support) sicherzustellen, ist unbedingt eine Bereitstellung mit mindestens zwei Rolleninstanzen erforderlich.
- Die benutzerdefinierte automatische Skalierung ist nur möglich, wenn sich alle Rollen im Status **Bereit** befinden.

## <a name="configure-and-manage-scaling"></a>Konfigurieren und Verwalten der Skalierung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie die Bereitstellung von Cloud Services (erweiterter Support) aus, für die Sie die Skalierung konfigurieren möchten. 
3. Wählen Sie das Blatt **Skalieren** aus. 

    :::image type="content" source="media/enable-scaling-1.png" alt-text="Die Abbildung zeigt das Auswählen der Option „Remotedesktop“ im Azure-Portal.":::

4. Auf einer Seite wird eine Liste aller Rollen angezeigt, in denen die Skalierung konfiguriert werden kann. Wählen Sie die Rolle aus, die Sie konfigurieren möchten. 
5. Wählen Sie den Skalierungstyp aus, den Sie konfigurieren möchten.
    - Mit **Manuelle Skalierung** wird die absolute Anzahl von Instanzen festgelegt.
        1. Wählen Sie **Manuelle Skalierung** aus.
        2. Geben Sie die Anzahl der Instanzen ein, die Sie hoch- oder herunterskalieren möchten.
        3. Klicken Sie auf **Speichern**.

        :::image type="content" source="media/enable-scaling-2.png" alt-text="Abbildung zeigt das Einrichten der manuellen Skalierung im Azure-Portal":::

        4. Die Skalierung wird sofort gestartet. 
        
    - Mit **Benutzerdefinierte Autoskalierung** können Sie Regeln festlegen, die bestimmen, in welchem Maße skaliert werden soll. 
        1. Wählen Sie **Benutzerdefinierte Autoskalierung** aus.
        2. Wählen Sie die Skalierung auf Basis einer Metrik oder Instanzenanzahl aus.

        :::image type="content" source="media/enable-scaling-3.png" alt-text="Abbildung zeigt das Einrichten der benutzerdefinierten Autoskalierung im Azure-Portal":::

        3. Fügen Sie bei Skalierung auf Basis einer Metrik nach Bedarf Regeln hinzu, um die gewünschten Skalierungsergebnisse zu erzielen.

        :::image type="content" source="media/enable-scaling-4.png" alt-text="Abbildung zeigt das Einrichten der Regeln für benutzerdefinierte Autoskalierung im Azure-Portal":::

        4. Klicken Sie auf **Speichern**.
        5. Die Skalierungsvorgänge werden gestartet, sobald eine Regel ausgelöst wird.
        
6. Sie können durch Auswahl der Registerkarte **Skalieren** auf Ihre Bereitstellungen angewandte vorhandene Skalierungsregeln anzeigen oder anpassen.

    :::image type="content" source="media/enable-scaling-5.png" alt-text="Abbildung zeigt das Anpassen einer vorhandenen Skalierungsregel im Azure-Portal":::

## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.