---
title: Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services | Microsoft-Dokumentation
description: 'Informationen zum Erstellen einer VM-Vorlage, die mehrere VMs enthält  Das heißt: Aktivieren Sie die geschachtelte Virtualisierung auf einer VM-Vorlage in Azure Lab Services.'
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 64097a5b3b62bcd5a84f4472a844bb95cf24cd6f
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555086"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services

Momentan können Sie mithilfe von Azure Lab Services in einem Lab eine Vorlage für virtuelle Computer einrichten und jedem Ihrer Benutzer jeweils eine Kopie zur Verfügung zu stellen. Wenn Sie aber ein Dozent sind und Unterricht zu Netzwerken, Sicherheit oder IT geben, müssen Sie möglicherweise für jeden Ihrer Kursteilnehmer eine Umgebung bereitstellen, in der mehrere VMs über ein Netzwerk miteinander kommunizieren können.

Eine geschachtelte Virtualisierung ermöglicht Ihnen, innerhalb einer Vorlage für virtuelle Computer in einem Lab eine Umgebung mit mehreren virtuellen Computern zu erstellen. Wenn diese Vorlage veröffentlicht wird, wird für jeden Benutzer im Lab ein eingerichteter virtueller Computer bereitgestellt, in dem weitere virtuelle Computer eingeschlossen sind.  Dieser Artikel behandelt die Vorgehensweise zum Einrichten der geschachtelten Virtualisierung auf einem Vorlagencomputer in Azure Lab Services.

## <a name="what-is-nested-virtualization"></a>Was ist geschachtelte Virtualisierung?

Die geschachtelte Virtualisierung ermöglicht es Ihnen, virtuelle Computer innerhalb eines virtuellen Computers zu erstellen. Geschachtelte Virtualisierung erfolgt über Hyper-V, und steht nur für Windows-VMs zur Verfügung.

Weitere Informationen zur geschachtelten Virtualisierung finden Sie in den folgenden Artikeln:

- [Nested Virtualization in Azure (Geschachtelte Virtualisierung in Azure)](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Aktivieren der geschachtelten Virtualisierung auf einer Azure-VM](../../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Überlegungen

Vor dem Einrichten eines Labs mit geschachtelter Virtualisierung sind folgende Punkte zu berücksichtigen.

- Wenn Sie ein neues Lab erstellen, wählen Sie als Größe des virtuellen Computers **Medium (Nested virtualization)** (Mittel (geschachtelte Virtualisierung)) oder **Large (Nested virtualization)** (Groß (geschachtelte Virtualisierung)) aus. Diese Größen virtueller Computer unterstützen geschachtelte Virtualisierung.
- Wählen Sie eine Größe aus, die sowohl für den Host als auch für den virtuellen Clientcomputer gute Leistung bereitstellt.  Denken Sie daran, dass bei der Verwendung von Virtualisierung die Größe, die Sie auswählen, nicht nur für einen Computer ausreichend sein muss, sondern sowohl für den Host als auch für alle Clientcomputer, die gleichzeitig ausgeführt werden sollen.
- Virtuelle Clientcomputer haben keinen Zugriff auf Azure-Ressourcen, z. B. DNS-Server im virtuellen Azure-Netzwerk.
- Beim virtuellen Hostcomputer muss die Einrichtung berücksichtigen, dass der Clientcomputer eine Internetverbindung besitzen muss.
- Virtuelle Clientcomputer werden als unabhängige Computer lizenziert. Informationen zur Lizenzierung für Microsoft-Betriebssysteme und -Produkte finden Sie unter [Microsoft-Lizenzierung](https://www.microsoft.com/licensing/default). Überprüfen Sie die Lizenzverträge für jegliche andere verwendete Software, bevor Sie den Vorlagencomputer einrichten.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer

In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Labkonto erstellt haben.  Weitere Informationen zum Erstellen eines neuen Labkontos finden Sie im [Tutorial zum Einrichten eines Labkontos](tutorial-setup-lab-account.md). Weitere Informationen zum Erstellen eines Labs finden Sie im Tutorial [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md).

>[!IMPORTANT]
>Wählen Sie bei der Erstellung des Labs als Größe für die VM **Large (nested virtualization)** (Groß (geschachtelte Virtualisierung)) oder **Medium (nested virtualization)** (Mittel (geschachtelte Virtualisierung)) aus.  Andernfalls funktioniert die verschachtelte Virtualisierung nicht.  

Informationen zum Herstellen einer Verbindung mit dem Vorlagencomputer finden Sie unter [Erstellen und Verwalten einer Classroom-Vorlage](how-to-create-manage-template.md). 

Die Schritte in diesem Abschnitt konzentrieren sich auf das Einrichten der geschachtelten Virtualisierung für Windows Server 2016 bzw. Windows Server 2019. Sie verwenden dabei ein Skript, um den Vorlagencomputer mit Hyper-V einzurichten.  Führen Sie die folgenden Schritte aus, um zu erfahren, wie Sie [Hyper-V-Skripts für Lab Services](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV) verwenden können.

1. Wenn Sie Internet Explorer verwenden, müssen Sie der Liste vertrauenswürdiger Sites möglicherweise `https://github.com` hinzufügen.
    1. Öffnen Sie Internet Explorer.
    1. Wählen Sie das Zahnradsymbol aus, und wählen Sie **Internetoptionen**.  
    1. Wenn das Dialogfeld **Internetoptionen** angezeigt wird, wählen Sie **Sicherheit**, **Vertrauenswürdige Sites** aus, und klicken Sie auf die Schaltfläche **Sites**.
    1. Wenn das Dialogfeld "**Vertrauenswürdige Sites** angezeigt wird, fügen Sie der Liste vertrauenswürdiger Websites `https://github.com` hinzu, und wählen Sie **Schließen** aus.

        ![Vertrauenswürdige Sites](../media/how-to-enable-nested-virtualization-template-vm/trusted-sites-dialog.png)
1. Laden Sie die Git-Repositorydateien herunter, wie in den folgenden Schritten beschrieben.
    1. Wechseln Sie zu [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/).
    1. Klicken Sie auf die Schaltfläche **Clone or Download** (Klonen oder herunterladen).
    1. Klicken Sie auf **ZIP herunterladen**.
    1. Extrahieren Sie die ZIP-Datei.

    >[!TIP]
    >Sie können das Git-Repository unter [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git) auch klonen.

1. Starten Sie die **PowerShell** im **Administrator**modus.
1. Navigieren Sie im PowerShell-Fenster zu dem Ordner mit dem heruntergeladenen Skript. Wenn Sie vom obersten Ordner der Repositorydateien aus navigieren, befindet sich das Skript in `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`.
1. Möglicherweise müssen Sie die Ausführungsrichtlinie ändern, damit das Skript erfolgreich ausgeführt wird. Führen Sie den folgenden Befehl aus:

    ```powershell
    Set-ExecutionPolicy bypass -force
    ```

1. Ausführen des Skripts:

    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > Das Skript erfordert möglicherweise, dass der Computer neu gestartet wird. Befolgen Sie die Anweisungen des Skripts, und führen Sie das Skript erneut aus, bis in der Ausgabe **Skript abgeschlossen** angezeigt wird.
1. Vergessen Sie nicht, die Ausführungsrichtlinie zurückzusetzen. Führen Sie den folgenden Befehl aus:

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Zusammenfassung

Jetzt ist Ihr Vorlagencomputer bereit, um virtuelle Hyper-V-Computer zu erstellen. Anweisungen zum Erstellen von virtuellen Hyper-V-Computern finden Sie unter [Erstellen eines virtuellen Computers in Hyper-V](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v). Verfügbare Betriebssysteme und Software finden Sie außerdem im [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/).  
