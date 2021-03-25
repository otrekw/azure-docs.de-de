---
title: Konfigurieren von Einstellungen für den automatischen Start eines virtuellen Computers in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Einstellungen für den automatischen Start für virtuelle Computer in einem Lab konfigurieren. Mit dieser Einstellung können virtuelle Computer im Lab nach einem Zeitplan automatisch gestartet werden.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 83e7b0836273a59eaaf66471bd0cb42d63ccf1c3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91328478"
---
# <a name="auto-startup-lab-virtual-machines"></a>Automatisches Starten virtueller Labcomputer  
Mit Azure DevTest Labs können Sie virtuelle Computer in Ihrem Lab so konfigurieren, dass sie basierend auf einem Zeitplan automatisch gestartet und heruntergefahren werden. Weitere Informationen zum Konfigurieren der Einstellungen für automatisches Herunterfahren finden Sie unter [Verwalten von Richtlinien für das automatische Herunterfahren für ein Lab in Azure DevTest Labs](devtest-lab-auto-shutdown.md). 

Im Unterschied zum automatischen Herunterfahren, das alle virtuellen Computer nach der Aktivierung der Richtlinie einschließt, erfordert die Richtlinie für automatischen Start einen Lab-Benutzer, der explizit einen virtuellen Computer auswählt und diesen Zeitplan abonniert. Auf diese Weise werden Sie nicht so leicht in eine Situation geraten, in der unerwünschte VMs versehentlich automatisch gestartet werden und unerwartete Ausgaben verursachen.

In diesem Artikel erfahren Sie, wie Sie die Richtlinie für automatischen Start für ein Lab konfigurieren.

## <a name="configure-autostart-settings-for-a-lab"></a>Konfigurieren von Einstellungen für automatischen Start für ein Lab 
1. Navigieren Sie zur Startseite für Ihr Lab. 
2. Wählen Sie im linken Menü die Option **Konfiguration und Richtlinien** aus. 

    ![Screenshot: Menü „Konfiguration und Richtlinien“ in DevTest Lab](./media/devtest-lab-auto-startup-vm/configuration-policies-menu.png)
3. Führen Sie auf der Seite **Konfiguration und Richtlinien** die folgenden Schritte aus:
    
    1. Wählen Sie **Ein** für **Planung des automatischen Starts virtueller Computer zulassen** aus, um die Funktion für automatischen Start für dieses Lab zu aktivieren. 
    2. Wählen Sie eine Startzeit (Beispiel: 8:00 Uhr) im Feld **Start planen** aus. 
    3. Wählen Sie eine zu verwendende **Zeitzone** aus. 
    4. Wählen Sie die **Wochentage** aus, an denen die VMs automatisch gestartet werden müssen. 
    5. Wählen Sie dann **Speichern** auf der Symbolleiste aus, um die Einstellungen zu speichern. 

        ![Einstellungen für automatischen Start](./media/devtest-lab-auto-startup-vm/auto-start-configuration.png)

        > [!IMPORTANT]
        > Diese Richtlinie wendet nicht automatisch automatischen Start auf virtuelle Computer im Lab an. Wechseln Sie zur Seite des virtuellen Computers, und aktivieren Sie **Automatischer Start** für die betreffende VM, um einzelne virtuelle Computer zu **aktivieren**.

## <a name="enable-autostart-for-a-vm-in-the-lab"></a>Aktivieren von automatischem Start für eine VM im Lab
Im folgenden Verfahren werden die Schritte beschrieben, die für die Aktivierung eines virtuellen Computers in der Richtlinie für automatischen Start des Labs erforderlich sind. 

1. Wählen Sie auf der Startseite für Ihr Lab die **VM** in der Liste **Meine virtuellen Computer** aus. 

    ![Menü „Konfiguration und Richtlinien“](./media/devtest-lab-auto-startup-vm/select-vm.png)
2. Wählen Sie auf der Seite **Virtueller Computer** im linken Menü oder in der Liste **Zeitpläne** die Option **Automatischer Start** aus. 

    ![Menü zum Auswählen des automatischen Starts](./media/devtest-lab-auto-startup-vm/select-auto-start.png)
3. Wählen Sie auf der Seite **Automatischer Start** die Option **Ein** für **Planung des automatischen Starts virtueller Computer zulassen** aus.

    ![Aktivieren von automatischem Start für die VM](./media/devtest-lab-auto-startup-vm/auto-start-vm.png)
4. Wählen Sie dann **Speichern** auf der Symbolleiste aus, um die Einstellung zu speichern. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Konfigurieren der Richtlinie für automatisches Herunterfahren für ein Lab finden Sie unter [Verwalten von Richtlinien für das automatische Herunterfahren für ein Lab in Azure DevTest Labs](devtest-lab-auto-shutdown.md).
