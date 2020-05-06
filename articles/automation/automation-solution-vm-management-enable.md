---
title: Aktivieren von „Starten/Beenden von VMs außerhalb der Geschäftszeiten in Azure Automation“
description: In diesem Artikel wird beschrieben, wie Sie die Azure Automation-Lösung zum Starten/Beenden von VMs für Ihre virtuellen Azure-Computer aktivieren.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: d47daa29c65f847fdeb33b9e24a892ac1f31b52a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82096961"
---
# <a name="enable-azure-automation-startstop-vms-solution"></a>Aktivieren der Lösung zum Starten/Beenden von VMs in Azure Automation

Führen Sie die folgenden Schritte aus, um die Lösung zum **Starten/Beenden von VMs außerhalb der Geschäftszeiten** einem neuen oder vorhandenen Automation-Konto sowie einem verknüpften Log Analytics-Arbeitsbereich hinzuzufügen. Nachdem Sie den Onboardingprozess abgeschlossen haben, konfigurieren Sie die Variablen zum Anpassen der Lösung.

>[!NOTE]
>Wenn Sie diese Lösung mit klassischen VMs verwenden möchten, benötigen Sie ein klassisches ausführendes Konto. Dieses wird nicht standardmäßig erstellt. Anweisungen zum Erstellen eines klassischen ausführenden Kontos finden Sie unter [Erstellen klassischer ausführender Konten](automation-create-standalone-account.md#create-a-classic-run-as-account).
>

## <a name="enable-solution"></a>Aktivieren einer Lösung

1. Melden Sie sich beim Azure-[Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.

3. Wählen Sie auf der Seite „Automation-Konten“ in der entsprechenden Liste Ihr Automation-Konto aus.

4. Wählen Sie in dem Automation-Konto unter **Verwandtes Ressourcen** die Option **VM starten/beenden** aus. Dort können Sie **Weitere Informationen anzeigen und Lösung aktivieren** anklicken. Wenn Sie bereits eine Lösung zum Starten/Beenden virtueller Computer bereitgestellt haben, können Sie diese auswählen, indem Sie auf **Lösung verwalten** klicken und sie in der Liste auswählen.

   ![Aktivieren über ein Automation-Konto](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Sie können Ressourcen auch von überall im Azure-Portal erstellen, indem Sie auf **Ressource erstellen** klicken. Geben Sie auf der Seite „Marketplace“ ein Schlüsselwort ein, z.B. **Starten** oder **Starten/Beenden**. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Alternativ hierzu können Sie ein oder mehrere Schlüsselwörter des vollständigen Namens der Lösung eingeben und dann die EINGABETASTE drücken. Wählen Sie in den Suchergebnissen **VMs außerhalb der Geschäftszeiten starten/beenden** aus.

5. Überprüfen Sie auf der Seite **VMs außerhalb der Geschäftszeiten starten/beenden** die Zusammenfassung der ausgewählten Lösung, und klicken Sie dann auf **Erstellen**.

   ![Azure-Portal](media/automation-solution-vm-management/azure-portal-01.png)

6. Die Seite „Lösung hinzufügen“ wird angezeigt. Sie werden aufgefordert, die Lösung vor dem Importieren in Ihr Automation-Abonnement zu konfigurieren.

   ![Seite „Lösung hinzufügen“ der VM-Verwaltung](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

7. Wählen Sie auf der Seite „Lösung hinzufügen“ die Option **Arbeitsbereich** aus. Wählen Sie einen Log Analytics-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, unter dem sich das Automation-Konto befindet. Wählen Sie die Option **Neuen Arbeitsbereich erstellen**, wenn kein Arbeitsbereich vorhanden ist. Führen Sie auf der Seite „Log Analytics-Arbeitsbereich“ die folgenden Schritte aus:

   - Geben Sie einen Namen für den neuen Log Analytics-Arbeitsbereich an, z. B. **ContosoLAWorkspace**.
   - Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   - Unter **Ressourcengruppe** können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.
   - Wählen Sie einen **Speicherort**aus.
   - Wählen Sie einen **Tarif**aus. Wählen Sie die Option **Pro GB (eigenständig)** aus. Für Azure Monitor-Protokolle wurden die [Preise](https://azure.microsoft.com/pricing/details/log-analytics/) aktualisiert, und der Tarif „Pro GB“ ist die einzige Option.

   > [!NOTE]
   > Wenn Sie Lösungen aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt.
   >
   > Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

8. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen auf der Seite „Log Analytics-Arbeitsbereich“ auf **Erstellen**. Sie können den Status unter **Benachrichtigungen** über das Menü nachverfolgen, und nach Abschluss des Vorgangs gelangen Sie zurück zur Seite „Lösung hinzufügen“.

9. Wählen Sie auf der Seite „Lösung hinzufügen“ die Option **Automation-Konto** aus. Wenn Sie einen neuen Log Analytics-Arbeitsbereich erstellen, können Sie ein neues Automation-Konto erstellen, das diesem zugeordnet wird, oder ein vorhandenes Automation-Konto auswählen, das nicht bereits mit einem Log Analytics-Arbeitsbereich verknüpft ist. Wählen Sie ein vorhandenes Automation-Konto aus, oder klicken Sie auf **Automation-Konto erstellen**, und geben Sie auf der Seite „Automation-Konto hinzufügen“ Folgendes an:
 
   - Geben Sie im Feld **Name** den Namen des Automation-Kontos ein.

     Alle anderen Optionen werden basierend auf dem Log Analytics-Arbeitsbereich automatisch aufgefüllt. Diese Optionen können nicht geändert werden. Ein ausführendes Azure-Konto ist die Standardmethode für die Authentifizierung von Runbooks in dieser Lösung. Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

10. Wählen Sie abschließend auf der Seite „Lösung hinzufügen“die Option **Konfiguration** aus. Die Seite „Parameter“ wird angezeigt.

    ![Seite „Parameter“ für die Lösung](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Hier erhalten Sie folgende Aufforderung:
  
   - Geben Sie einen Wert für die Zielressourcengruppennamen(**Target ResourceGroup Names**) ein. Dies sind Werte von Ressourcengruppen mit virtuellen Computern, die mit dieser Lösung verwaltet werden. Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt). Die Verwendung eines Platzhalterzeichens wird unterstützt, wenn Sie einen Vorgang für VMs in allen Ressourcengruppen des Abonnements durchführen möchten. Dieser Wert wird in den Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames** gespeichert.
  
   - Geben Sie die **VM Exclude List (string)** (VM-Ausschlussliste (Zeichenfolge)) an. Dies ist der Wert von einem oder mehreren virtuellen Computern der Zielressourcengruppe. Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt). Platzhalter können verwendet werden. Dieser Wert wird in der Variablen **External_ExcludeVMNames** gespeichert.
  
   - Wählen Sie einen **Zeitplan** aus. Wählen Sie ein Datum und eine Uhrzeit für Ihren Zeitplan aus. Ein wiederkehrender täglicher Zeitplan wird ab der von Ihnen gewählten Zeit erstellt. Es kann keine andere Region ausgewählt werden. Falls Sie den Zeitplan nach dem Konfigurieren der Lösung für Ihre Zeitzone konfigurieren möchten, helfen Ihnen die Informationen unter [Ändern des Zeitplans für das Starten und Herunterfahren](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules) weiter.
  
   - Um **E-Mail-Benachrichtigungen** von einer Aktionsgruppe zu empfangen, übernehmen Sie den Standardwert **Ja**, und geben Sie eine gültige E-Mail-Adresse an. Wenn Sie **Nein** auswählen, sich aber zu einem späteren Zeitpunkt entscheiden, dass Sie E-Mail-Benachrichtigungen erhalten möchten, können Sie die erstellte [Aktionsgruppe](../azure-monitor/platform/action-groups.md) mit gültigen E-Mail-Adressen, die durch Komma getrennt sind, aktualisieren. Außerdem müssen Sie die folgenden Warnungsregeln aktivieren:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > Der Standardwert für **ResourceGroup-Zielnamen** ist ein **&ast;** . Dies betrifft alle VMs in einem Abonnement. Wenn die Lösung nicht alle VMs als Zielversion in Ihrem Abonnement festlegen soll, muss dieser Wert vor der Aktivierung der Zeitpläne auf eine Liste der Ressourcengruppennamen aktualisiert werden.

11. Nachdem Sie die erforderlichen Anfangseinstellungen für die Lösung konfiguriert haben, klicken Sie auf **OK**, um die Seite „Parameter“ zu schließen, und wählen Sie dann **Erstellen** aus. 

Nachdem alle Einstellungen überprüft wurden, wird die Lösung für Ihr Abonnement bereitgestellt. Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

> [!NOTE]
> Wenn Sie ein Abonnement für Azure Cloud Solution Provider (Azure CSP) besitzen, wechseln Sie nach Abschluss der Bereitstellung in Ihrem Automation-Konto unter **Freigegebene Ressourcen** zu **Variablen**, und legen Sie die Variable [External_EnableClassicVMs](automation-solution-vm-management.md#variables) auf **False** fest. Dadurch hört die Lösung auf, nach klassischen VM-Ressourcen zu suchen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Lösung aktiviert haben, können Sie sie so [konfigurieren,](automation-solution-vm-management-config.md), dass Ihre VM-Verwaltungsanforderungen unterstützt werden.