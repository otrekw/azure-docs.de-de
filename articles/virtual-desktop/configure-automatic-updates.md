---
title: 'Konfigurieren von Microsoft Endpoint Configuration Manager: Azure'
description: Erfahren Sie, wie Sie Microsoft Endpoint Configuration Manager konfigurieren, um Softwareupdates für Windows 10 Enterprise (mehrere Sitzungen) unter Windows Virtual Desktop bereitzustellen.
author: Heidilohr
ms.topic: how-to
ms.date: 06/12/2020
ms.author: helohr
ms.reviewer: v-cawood; clemr
manager: lizross
ms.openlocfilehash: bfc797e0c11a1804d27c6c97fea8cb2ba62dca7a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88010122"
---
# <a name="configure-microsoft-endpoint-configuration-manager"></a>Konfigurieren von Microsoft Endpoint Configuration Manager

In diesem Artikel wird erläutert, wie Microsoft Endpoint Configuration Manager so konfiguriert wird, dass Updates automatisch auf einen Windows Virtual Desktop-Host angewendet werden, auf dem Windows 10 Enterprise (mehrere Sitzungen) ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Um diese Einstellung zu konfigurieren, benötigen Sie Folgendes:

   - Stellen Sie sicher, dass Sie den Microsoft Endpoint Configuration Manager-Agent auf Ihren virtuellen Computern installiert haben.
   - Vergewissern Sie sich, dass sich Ihre Version von Microsoft Endpoint Configuration Manager mindestens auf Verzweigungsebene 1906 befindet. Optimale Ergebnisse erzielen Sie ab Verzweigungsebene 1910.

## <a name="configure-the-software-update-point"></a>Konfigurieren des Softwareupdatepunkts

Um Updates für Windows 10 Enterprise (mehrere Sitzungen) zu erhalten, müssen Sie Windows Server, Version 1903 und höher, als Produkt innerhalb von Microsoft Endpoint Configuration Manager aktivieren. Diese Produkteinstellung gilt auch, wenn Sie Updates mit Windows Server Update Service auf Ihren Systemen bereitstellen.

So erhalten Sie Updates

1. Öffnen Sie Microsoft Endpoint Configuration Manager, und wählen Sie **Standorte** aus.
2. Wählen Sie **Standortkomponenten konfigurieren** aus.
3. Wählen Sie im Dropdown Menü **Softwareupdatepunkt** aus.
4. Wählen Sie die Registerkarte **Produkte** aus.
5. Aktivieren Sie das Kontrollkästchen **Windows Server, Version 1903 und höher**.
6. Wechseln Sie zu **Softwarebibliothek** > **Übersicht** > **Softwareupdates** > **Alle Softwareupdates**, und wählen Sie **Softwareupdates synchronisieren** aus.
7. Überprüfen Sie die Datei wsyncmgr.log unter **Program Files** > **Microsoft Configuration Manager** > **Logs**, um sicherzustellen, dass Ihre Änderungen gespeichert wurden. Die Synchronisierung der Updates kann einige Minuten dauern.

## <a name="create-a-query-based-collection"></a>Erstellen einer abfragebasierten Sammlung

Um eine Sammlung virtueller Computer mit Windows 10 Enterprise (mehrere Sitzungen) zu erstellen, kann mithilfe einer abfragebasierten Sammlung die spezifische SKU des Betriebssystems ermittelt werden.

So erstellen Sie eine Sammlung

1. Wählen Sie **Bestand und Kompatibilität** aus.
2. Wechseln Sie zu **Übersicht** > **Gerätesammlungen**, klicken Sie mit der rechten Maustaste auf **Gerätesammlungen**, und wählen Sie im Dropdownmenü **Gerätesammlung erstellen** aus.
3. Geben Sie im eingeblendeten Menü auf der Registerkarte **Allgemein** in das Feld **Name** einen Namen ein, der Ihre Sammlung beschreibt. Im Feld **Kommentar** können Sie zusätzliche Informationen angeben, die beschreiben, um welche Sammlung es sich handelt. Legen Sie in **Begrenzende Sammlung** fest, welche Computer in die Sammlungsabfrage einbezogen werden sollen.
4. Fügen Sie auf der Registerkarte **Mitgliedschaftsregeln** eine Regel für Ihre Abfrage hinzu, indem Sie **Regel hinzufügen** und dann **Abfrageregel** auswählen.
5. Geben Sie in **Eigenschaften für Abfrageregel** einen Namen für Ihre Regel ein, und legen Sie dann die Parameter der Regel fest, indem Sie **Abfrageanweisung bearbeiten** auswählen.
6. Wählen Sie **Abfrageanweisung anzeigen** aus.
7. Geben Sie in die Anweisung die folgende Zeichenfolge ein:

    ```syntax
    select
    SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client
    from SMS_R_System inner join SMS_G_System_OPERATING_SYSTEM on
    SMS_G_System_OPERATING_SYSTEM.ResourceId = SMS_R_System.ResourceId where
    SMS_G_System_OPERATING_SYSTEM.OperatingSystemSKU = 175
    ```

8. Wählen Sie **OK** aus, um die Sammlung zu erstellen.
9. Um zu überprüfen, ob Sie die Sammlung erfolgreich erstellt haben, wechseln Sie zu **Bestand und Kompatibilität** > **Übersicht** > **Gerätesammlungen**.
