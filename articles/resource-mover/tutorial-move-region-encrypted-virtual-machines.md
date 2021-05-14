---
title: Verschieben verschlüsselter virtueller Azure-Computer zwischen Regionen mithilfe von Azure Resource Mover
description: Hier erfahren Sie, wie Sie verschlüsselte virtuelle Azure-Computer mithilfe von Azure Resource Mover in eine andere Region verschieben.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: tutorial
ms.date: 02/10/2021
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 457c4c4752b4d78434b1fb90710472b1998f1c4e
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107600691"
---
# <a name="tutorial-move-encrypted-azure-vms-across-regions"></a>Tutorial: Verschieben verschlüsselter virtueller Azure-Computer zwischen Regionen

In diesem Artikel wird erläutert, wie verschlüsselte virtuelle Azure-Computer mithilfe von [Azure Resource Mover](overview.md) in eine andere Azure-Region verschoben werden. 

Verschlüsselte virtuelle Computer können wie folgt beschrieben werden:

- Virtuelle Computer mit Datenträgern, für die Azure Disk Encryption aktiviert ist. Weitere Informationen finden Sie unter [Schnellstart: Erstellen und Verschlüsseln eines virtuellen Windows-Computers mit dem Azure-Portal](../virtual-machines/windows/disk-encryption-portal-quickstart.md).
- Virtuelle Computer, bei denen kundenseitig verwaltete Schlüssel (Customer-Managed Keys, CMKs) für die Verschlüsselung ruhender Daten (serverseitige Verschlüsselung) verwendet werden. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Aktivieren der serverseitigen Verschlüsselung mit kundenseitig verwalteten Schlüsseln für verwaltete Datenträger](../virtual-machines/disks-enable-customer-managed-keys-portal.md).


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen 
> * Bei virtuellen Computern, für die Azure Disk Encryption aktiviert ist: Kopieren von Schlüsseln und Geheimnissen aus dem Schlüsseltresor der Quellregion in den Schlüsseltresor der Zielregion
> * Vorbereiten der Verschiebung von virtuellen Computern und Auswählen von Ressourcen in der Quellregion, aus der sie verschoben werden sollen
> * Auflösen von Ressourcenabhängigkeiten
> * Bei virtuellen Computern, für die Azure Disk Encryption aktiviert ist: Manuelles Zuweisen des Zielschlüsseltresors. Bei virtuellen Computern mit serverseitiger Verschlüsselung und kundenseitig verwalteten Schlüsseln: Manuelles Zuweisen eines Datenträgerverschlüsselungssatzes in der Zielregion
> * Verschieben des Schlüsseltresors oder des Datenträgerverschlüsselungssatzes
> * Vorbereiten und Verschieben der Quellressourcengruppe 
> * Vorbereiten und Verschieben der anderen Ressourcen
> * Verwerfen oder Committen der Verschiebung 
> * Optionales Entfernen von Ressourcen in der Quellregion nach der Verschiebung

> [!NOTE]
> In diesem Tutorial wird der schnellste Weg zum Ausprobieren eines Szenarios veranschaulicht. Es werden nur die Standardoptionen verwendet. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen. Melden Sie sich dann beim [Azure-Portal](https://portal.azure.com) an.

## <a name="prerequisites"></a>Voraussetzungen

Anforderung |Details
--- | ---
**Abonnementberechtigungen** | Vergewissern Sie sich, dass Sie *Besitzerzugriff* auf das Abonnement haben, das die zu verschiebenden Ressourcen enthält.<br/><br/> *Warum benötige ich Besitzerzugriff?* Wenn Sie zum ersten Mal eine Ressource für ein bestimmtes Quelle-Ziel-Paar in einem Azure-Abonnement hinzufügen, erstellt Resource Mover eine [systemseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (früher als Managed Service Identify (MSI) bezeichnet). Diese Identität wird vom Abonnement als vertrauenswürdig eingestuft. Damit Sie die Identität erstellen und ihr die erforderlichen Rollen (*Mitwirkender* und *Benutzerzugriffsadministrator* im Quellabonnement) zuweisen können, benötigt das Konto, das Sie zum Hinzufügen von Ressourcen verwenden, *Besitzerberechtigungen* für das Abonnement. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles).
**VM-Support** | Stellen Sie wie folgt sicher, dass die virtuellen Computer, die Sie verschieben möchten, unterstützt werden:<li>[Überprüfen](support-matrix-move-region-azure-vm.md#windows-vm-support) Sie die unterstützten Windows-VMs.<li>[Überprüfen](support-matrix-move-region-azure-vm.md#linux-vm-support) Sie die unterstützten Linux-VMs und Kernel-Versionen.<li>Überprüfen Sie die unterstützten [Compute-](support-matrix-move-region-azure-vm.md#supported-vm-compute-settings), [Speicher-](support-matrix-move-region-azure-vm.md#supported-vm-storage-settings) und [Netzwerkeinstellungen](support-matrix-move-region-azure-vm.md#supported-vm-networking-settings).
**Schlüsseltresoranforderungen (Azure Disk Encryption)** | Wenn Sie Azure Disk Encryption für virtuelle Computer aktiviert haben, benötigen Sie sowohl in der Quell- als auch in der Zielregion einen Schlüsseltresor. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Schlüsseltresors über das Azure-Portal](../key-vault/general/quick-create-portal.md).<br/><br/> Für die Schlüsseltresore in der Quell- und Zielregion benötigen Sie folgende Berechtigungen:<li>Schlüsselberechtigungen: Schlüsselverwaltungsvorgänge (Abrufen, Auflisten) und kryptografische Vorgänge (Entschlüsseln und Verschlüsseln)<li>Geheimnisberechtigungen: Verwaltungsvorgänge für Geheimnisse (Abrufen, Auflisten und Festlegen)<li>Zertifikat (Auflisten und Abrufen)
**Datenträgerverschlüsselungssatz (serverseitige Verschlüsselung mit CMK)** | Wenn Sie virtuelle Computer mit serverseitiger Verschlüsselung verwenden, die einen CMK verwendet, benötigen Sie sowohl in der Quell- als auch in der Zielregion einen Datenträgerverschlüsselungssatz. Weitere Informationen finden Sie unter [Einrichten des Datenträgerverschlüsselungssatzes](../virtual-machines/disks-enable-customer-managed-keys-portal.md#set-up-your-disk-encryption-set).<br/><br/> Wenn Sie für kundenseitig verwaltete Schlüssel HSM-Schlüssel (Hardware Security Module, Hardwaresicherheitsmodul) verwenden, wird das Verschieben zwischen Regionen nicht unterstützt.
**Kontingent für die Zielregion** | Das Abonnement benötigt ein ausreichendes Kontingent zum Erstellen der Ressourcen, die Sie in die Zielregion verschieben. Wenn kein ausreichendes Kontingent vorhanden ist, [fordern Sie eine Heraufsetzung des Kontingents an](../azure-resource-manager/management/azure-subscription-service-limits.md).
**Gebühren für die Zielregion** | Überprüfen Sie die Preise und Gebühren für die Zielregion, in die Sie virtuelle Computer verschieben. Verwenden Sie dazu den [Preisrechner](https://azure.microsoft.com/pricing/calculator/).


## <a name="verify-permissions-in-the-key-vault"></a>Überprüfen der Berechtigungen im Schlüsseltresor

Wenn Sie virtuelle Computer verschieben, für die Azure Disk Encryption aktiviert ist, müssen Sie ein Skript ausführen, wie im Abschnitt [Kopieren der Schlüssel in den Zielschlüsseltresor](#copy-the-keys-to-the-destination-key-vault) beschrieben. Die Benutzer, die das Skript ausführen, müssen dazu über die entsprechenden Berechtigungen verfügen. Informationen zu den erforderlichen Berechtigungen finden Sie in der Tabelle weiter unten. Navigieren Sie im Azure-Portal zum Schlüsseltresor, um die Optionen zum Ändern der Berechtigungen anzuzeigen. Wählen Sie unter **Einstellungen** die Option **Zugriffsrichtlinien** aus.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png" alt-text="Screenshot: Link „Zugriffsrichtlinien“ im Bereich „Einstellungen“ des Schlüsseltresors" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png":::

Sind keine Benutzerberechtigungen festgelegt, wählen Sie **Zugriffsrichtlinie hinzufügen** aus, und geben Sie dann die Berechtigungen an. Ist für das Benutzerkonto bereits eine Richtlinie vorhanden, legen Sie unter **Benutzer** die Berechtigungen gemäß den Informationen in der im Anschluss bereitgestellten Tabelle fest.

Für virtuelle Azure-Computer mit Azure Disk Encryption sind folgende Varianten möglich, und Sie müssen die Berechtigungen den relevanten Komponenten entsprechend festlegen. Die virtuellen Computer verfügen unter Umständen über Folgendes:
- Standardoption, bei der der Datenträger nur mit Geheimnissen verschlüsselt wird
- Zusätzliche Sicherheit durch [Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK)](../virtual-machines/windows/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek)

### <a name="source-region-key-vault"></a>Schlüsseltresor der Quellregion

Legen Sie für Benutzer, die das Skript ausführen, Berechtigungen für die folgenden Komponenten fest: 

Komponente | Berechtigungen erforderlich
--- | ---
Geheimnisse |  *Get* <br></br> Wählen Sie **Geheimnisberechtigungen** > **Verwaltungsvorgänge für Geheimnisse** und anschließend **Abrufen** aus. 
Schlüssel <br></br> Bei Verwendung eines KEK benötigen Sie diese Berechtigungen zusätzlich zu den Berechtigungen für Geheimnisse. | *Abrufen* und *Entschlüsseln* <br></br> Wählen Sie **Schlüsselberechtigungen** > **Schlüsselverwaltungsvorgänge** und anschließend **Abrufen** aus. Wählen Sie unter **Kryptografische Vorgänge** die Option **Entschlüsseln** aus.

### <a name="destination-region-key-vault"></a>Schlüsseltresor der Zielregion

Vergewissern Sie sich unter **Zugriffsrichtlinien**, dass **Azure Disk Encryption für Volumeverschlüsselung** aktiviert ist. 

Legen Sie für Benutzer, die das Skript ausführen, Berechtigungen für die folgenden Komponenten fest: 

Komponente | Berechtigungen erforderlich
--- | ---
Geheimnisse |  *Set* <br></br> Wählen Sie **Geheimnisberechtigungen** > **Verwaltungsvorgänge für Geheimnisse** und anschließend **Festlegen** aus. 
Schlüssel <br></br> Bei Verwendung eines KEK benötigen Sie diese Berechtigungen zusätzlich zu den Berechtigungen für Geheimnisse. | *Abrufen*, *Erstellen* und *Verschlüsseln* <br></br> Wählen Sie **Schlüsselberechtigungen** > **Schlüsselverwaltungsvorgänge** und anschließend **Abrufen** und **Erstellen** aus. Wählen Sie unter **Kryptografische Vorgänge** die Option **Verschlüsseln** aus.

<br>

Neben den oben genannten Berechtigungen müssen im Zielschlüsseltresor auch Berechtigungen für die [verwaltete Systemidentität](./common-questions.md#how-is-managed-identity-used-in-resource-mover) hinzugefügt werden, die von Resource Mover verwendet wird, um in Ihrem Namen auf die Azure-Ressourcen zuzugreifen. 

1. Wählen Sie unter **Einstellungen** die Option **Zugriffsrichtlinien hinzufügen** aus. 
1. Suchen Sie in unter **Prinzipal auswählen** nach der nach verwalteten Systemidentität (Managed System Identity, MSI). Der Name der MSI lautet ```movecollection-<sourceregion>-<target-region>-<metadata-region>```. 
1. Fügen Sie für die MSI die folgenden Berechtigungen hinzu:

    Komponente | Berechtigungen erforderlich
    --- | ---
    Geheimnisse|  *Abrufen* und *Auflisten* <br></br> Wählen Sie **Geheimnisberechtigungen** > **Verwaltungsvorgänge für Geheimnisse** und anschließend **Abrufen** und **Auflisten** aus. 
    Schlüssel <br></br> Bei Verwendung eines KEK benötigen Sie diese Berechtigungen zusätzlich zu den Berechtigungen für Geheimnisse. | *Abrufen* und *Auflisten* <br></br> Wählen Sie **Schlüsselberechtigungen** > **Schlüsselverwaltungsvorgänge** und anschließend **Abrufen** und **Auflisten** aus.

<br>

### <a name="copy-the-keys-to-the-destination-key-vault"></a>Kopieren der Schlüssel in den Zielschlüsseltresor

Kopieren Sie die Verschlüsselungsgeheimnisse und -schlüssel aus dem Quellschlüsseltresor mithilfe eines von uns bereitgestellten [Skripts](https://raw.githubusercontent.com/AsrOneSdk/published-scripts/master/CopyKeys/CopyKeys.ps1) in den Zielschlüsseltresor.

- Führen Sie das Skript in PowerShell aus. Es wird empfohlen, die neueste PowerShell-Version zu verwenden.
- Für das Skript sind folgende Module erforderlich:
    - Az.Compute
    - Az.KeyVault (Version 3.0.0)
    - Az.Accounts (Version 2.2.3)

Um das Skript auszuführen, führen Sie die folgenden Schritte durch:

1. Öffnen Sie das [Skript](https://raw.githubusercontent.com/AsrOneSdk/published-scripts/master/CopyKeys/CopyKeys.ps1) auf GitHub.
1. Kopieren Sie den Inhalt des Skripts in eine lokale Datei, und nennen Sie sie *Copy-keys.ps1*.
1. Führen Sie das Skript aus.
1. Melden Sie sich beim Azure-Portal an.
1. Wählen Sie in den Dropdownlisten im Fenster **Benutzereingaben** das Quellabonnement, die Ressourcengruppe und die virtuellen Quellcomputer aus. Wählen Sie anschließend den Zielspeicherort und die Zieltresore für die Datenträger- und Schlüsselverschlüsselung aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/script-input.png" alt-text="Screenshot: Fenster „Benutzereingaben“ zum Eingeben der Skriptwerte" :::

1. Wählen Sie die Schaltfläche **Auswählen** aus. 
   
   Nach Abschluss der Skriptausführung werden Sie in einer Meldung darüber informiert, dass „CopyKeys“ erfolgreich war.

## <a name="prepare-vms"></a>Vorbereiten der VMs

1. Nachdem Sie sich vergewissert haben, dass die virtuellen Computer die [Voraussetzungen](#prerequisites) erfüllen, müssen Sie sicherstellen, dass die zu verschiebenden virtuellen Computer aktiviert sind. Alle VM-Datenträger, die in der Zielregion verfügbar sein sollen, müssen an den virtuellen Computer angefügt und initialisiert werden.
1. Führen Sie folgende Schritte aus, um sich zu vergewissern, dass die virtuellen Computer über die neuesten vertrauenswürdigen Stammzertifikate sowie über eine aktualisierte Zertifikatssperrliste (Certificate Revocation List, CRL) verfügen:
    - Installieren Sie auf Windows-VMs die aktuellen Windows-Updates.
    - Befolgen Sie auf virtuellen Linux-Computern den Leitfaden für den Linux-Distributoren, damit die Computer über die aktuellen Zertifikate und CRL verfügen. 
1. Um ausgehende Verbindungen von den virtuellen Computern zuzulassen, gehen Sie wie folgt vor:
    - [Lassen Sie den Zugriff auf die URLs zu](support-matrix-move-region-azure-vm.md#url-access), wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden.
    - Wenn Sie Regeln für Netzwerksicherheitsgruppen (NSG) zum Steuern der ausgehenden Verbindung verwenden, erstellen Sie diese [Diensttagregeln](support-matrix-move-region-azure-vm.md#nsg-rules).

## <a name="select-the-resources-to-move"></a>Auswählen der zu verschiebenden Ressourcen

- Sie können einen beliebigen unterstützten Ressourcentyp aus einer beliebigen Ressourcengruppe in der ausgewählten Quellregion auswählen.  
- Sie können Ressourcen in eine Zielregion verschieben, die dem gleichen Abonnement angehört wie die Quellregion. Wenn Sie das Abonnement ändern möchten, können Sie dies nach dem Verschieben der Ressourcen tun.

Gehen Sie zum Auswählen der Ressourcen wie folgt vor:

1. Suchen Sie im Azure-Portal nach **Resource Mover**. Wählen Sie anschließend unter **Dienste** die Option **Azure Resource Mover** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/search.png" alt-text="Screenshot: Suchergebnisse für Azure Resource Mover im Azure-Portal" :::

1. Wählen Sie in Azure Resource Mover im Bereich **Übersicht** die Option **Regionsübergreifend verschieben** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png" alt-text="Screenshot: Schaltfläche „Regionsübergreifend verschieben“ zum Hinzufügen von Ressourcen zum Verschieben in eine andere Region" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png":::

1. Wählen Sie im Bereich **Ressourcen verschieben** die Registerkarte **Quelle + Ziel** aus. Wählen Sie dann in den Dropdownlisten das Quellabonnement und die Quellregion aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/source-target.png" alt-text="Seite zum Auswählen der Quell- und Zielregion" :::

1. Wählen Sie unter **Ziel** die Region aus, in die Sie die virtuellen Computer verschieben möchten, und wählen Sie dann **Weiter** aus.

1. Wählen Sie die Registerkarte **Zu verschiebende Ressourcen** und dann **Ressourcen auswählen** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-resources.png" alt-text="Screenshot: Bereich „Ressourcen verschieben“ und Schaltfläche „Ressourcen auswählen“" :::

1. Wählen Sie im Bereich **Ressourcen auswählen** die virtuellen Computer aus, die Sie verschieben möchten. Wie im Abschnitt [Auswählen der zu verschiebenden Ressourcen](#select-the-resources-to-move) erwähnt, können Sie nur Ressourcen hinzufügen, für die die Verschiebung unterstützt wird.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-vm.png" alt-text="Screenshot: Bereich „Ressourcen auswählen“ zum Auswählen der zu verschiebenden virtuellen Computer" :::

    > [!NOTE]
    >  In diesem Tutorial werden ein virtueller Computer mit serverseitiger Verschlüsselung (rayne-vm) und kundenseitig verwaltetem Schlüssel sowie ein virtueller Computer mit aktivierter Datenträgerverschlüsselung (rayne-vm-ade) ausgewählt.

1. Wählen Sie **Fertig** aus.
1. Wählen Sie die Registerkarte **Zu verschiebende Ressourcen** und dann **Weiter** aus.
1. Wählen Sie die Registerkarte **Überprüfen** aus, und überprüfen Sie dann die Quell- und Zieleinstellungen. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/review.png" alt-text="Screenshot: Bereich zum Überprüfen der Quell- und Zieleinstellungen" :::

1. Wählen Sie **Fortsetzen** aus, um die Ressourcen hinzuzufügen.
1. Wählen Sie das Benachrichtigungssymbol aus, um den Status zu überwachen. Wählen Sie nach erfolgreichem Abschluss des Vorgangs im Bereich **Benachrichtigungen** die Option **Zu verschiebende Ressourcen wurden hinzugefügt.** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png" alt-text="Screenshot: Bereich „Benachrichtigungen“ mit der Bestätigung, dass Ressourcen erfolgreich hinzugefügt wurden" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png":::
    
1. Nachdem Sie die Benachrichtigung ausgewählt haben, überprüfen Sie die Ressourcen auf der Seite **Regionsübergreifend**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-prepare-pending.png" alt-text="Screenshot: Hinzugefügte Ressourcen mit dem Status „Vorbereitung ausstehend“" :::

> [!NOTE]
> - Die von Ihnen hinzugefügten Ressourcen haben den Status *Vorbereitung ausstehend*.
> - Die Ressourcengruppe für die virtuellen Computer wird automatisch hinzugefügt.
> - Wenn Sie die Einträge für **Zielkonfiguration** ändern, um eine Ressource zu verwenden, die bereits in der Zielregion vorhanden ist, wird der Ressourcenstatus auf *Commit ausstehend* festgelegt, da für die betreffende Ressource keine Verschiebung initiiert werden muss.
> - Wenn Sie eine hinzugefügte Ressource entfernen möchten, hängt die verwendete Methode davon ab, an welchem Punkt im Verschiebevorgang Sie sich befinden. Weitere Informationen finden Sie unter [Verwalten von Verschiebungssammlungen und Ressourcengruppen](remove-move-resources.md).


## <a name="resolve-dependencies"></a>Auflösen von Abhängigkeiten

1. Wenn für Ressourcen in der Spalte **Probleme** die Meldung *Abhängigkeiten überprüfen* angezeigt wird, wählen Sie die Schaltfläche **Abhängigkeiten überprüfen** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png" alt-text="Screenshot: Schaltfläche „Abhängigkeiten überprüfen“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png":::

    Der Überprüfungsprozess wird daraufhin begonnen.
1. Wenn Abhängigkeiten gefunden werden, wählen Sie **Add dependencies** (Abhängigkeiten hinzufügen) aus.  

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png" alt-text="Screenshot: Schaltfläche „Abhängigkeiten hinzufügen“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png":::


1. Übernehmen Sie im Bereich **Abhängigkeiten hinzufügen** die Standardoption **Alle Abhängigkeiten anzeigen**.

    - Bei Verwendung von **Alle Abhängigkeiten anzeigen** werden alle direkten und indirekten Abhängigkeiten für eine Ressource durchlaufen. Für einen virtuellen Computer werden beispielsweise die NIC, das virtuelle Netzwerk, die Netzwerksicherheitsgruppen (NSGs) usw. angezeigt.
    - Bei Verwendung von **Show first level dependencies only** (Nur Abhängigkeiten der ersten Ebene anzeigen) werden nur direkte Abhängigkeiten angezeigt. Für einen virtuellen Computer wird beispielsweise die NIC, aber nicht das virtuelle Netzwerk angezeigt.
 
1. Wählen Sie die abhängigen Ressourcen, die Sie hinzufügen möchten, und anschließend **Abhängigkeiten hinzufügen** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png" alt-text="Screenshot: Abhängigkeitsliste und Schaltfläche „Abhängigkeiten hinzufügen“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png":::

1. Überprüfen Sie die Abhängigkeiten noch einmal. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png" alt-text="Screenshot: Bereich zum erneuten Überprüfen der Abhängigkeiten" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png":::

## <a name="assign-destination-resources"></a>Zuweisen von Zielressourcen

Zielressourcen, die der Verschlüsselung zugeordnet sind, müssen manuell zugewiesen werden.

- Wenn Sie einen virtuellen Computer verschieben, für den Azure Disk Encryption aktiviert ist, wird der Schlüsseltresor in der Zielregion als Abhängigkeit angezeigt.
- Wenn Sie einen virtuellen Computer mit serverseitiger Verschlüsselung und kundenseitig verwalteten Schlüsseln verschieben, wird der Datenträgerverschlüsselungssatz in der Zielregion als Abhängigkeit angezeigt. 
- Da in diesem Tutorial das Verschieben eines virtuellen Computers veranschaulicht wird, für den Azure Disk Encryption aktiviert ist und ein CMK verwendet wird, werden sowohl der Zielschlüsseltresor als auch die Datenträgerverschlüsselung als Abhängigkeiten angezeigt.

Gehen Sie wie folgt vor, um die Zielressourcen manuell zuzuweisen:

1. Wählen Sie im Eintrag für den Datenträgerverschlüsselungssatz in der Spalte **Zielkonfiguration** die Option **Ressource nicht zugewiesen** aus.
1. Wählen Sie unter **Konfigurationseinstellungen** den Ziel-Datenträgerverschlüsselungssatz und dann **Änderungen speichern** aus.
1. Sie können für die Ressource, die Sie ändern, die Änderungen speichern und Abhängigkeiten überprüfen oder die Änderungen einfach nur speichern und später alle Änderungen auf einmal überprüfen.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png" alt-text="Screenshot: Bereich „Zielkonfiguration“ zum Speichern von Änderungen in der Zielregion" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png":::

    Nach dem Hinzufügen der Zielressource ändert sich der Status des Datenträgerverschlüsselungssatzes in *Commit für Verschiebung ausstehend*.

1. Wählen Sie im Eintrag für den Schlüsseltresor in der Spalte **Zielkonfiguration** die Option **Ressource nicht zugewiesen** aus. Wählen Sie unter **Konfigurationseinstellungen** den Zielschlüsseltresor aus, und speichern Sie Ihre Änderungen. 

Der Status des Datenträgerverschlüsselungssatzes und des Schlüsseltresors ändert sich nun in *Commit für Verschiebung ausstehend*.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png" alt-text="Screenshot: Bereich zum Vorbereiten anderer Ressourcen" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png":::

Gehen Sie wie folgt vor, um die Verschiebung der Verschlüsselungsressourcen zu committen und den Prozess abzuschließen:

1. Wählen Sie unter **Regionsübergreifend** die Ressource (Datenträgerverschlüsselungssatz oder Schlüsseltresor) und anschließend **Commit für Verschiebung ausführen** aus.
1. Wählen Sie unter **Ressourcen verschieben** die Option **Committen** aus.

> [!NOTE]
> Nachdem Sie die Verschiebung committet haben, ändert sich der Ressourcenstatus in *Löschung der Quelle ausstehend*.


## <a name="move-the-source-resource-group"></a>Verschieben der Quellressourcengruppe 

Bevor Sie virtuelle Computer vorbereiten und verschieben können, muss die VM-Ressourcengruppe in der Zielregion vorhanden sein. 

### <a name="prepare-to-move-the-source-resource-group"></a>Vorbereiten zum Verschieben der Quellressourcengruppe

Beim Vorbereitungsprozess generiert Resource Mover ARM-Vorlagen (Azure Resource Manager) über die Ressourcengruppeneinstellungen. Die Ressourcen innerhalb der Ressourcengruppe sind nicht betroffen.

Gehen Sie wie folgt vor, um das Verschieben der Quellressourcengruppe vorzubereiten:

1. Wählen Sie unter **Regionsübergreifend** die Quellressourcengruppe und anschließend **Vorbereiten** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png" alt-text="Screenshot: Schaltfläche „Vorbereiten“ im Bereich „Ressourcen vorbereiten“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png":::

1. Wählen Sie unter **Prepare resources** (Ressourcen vorbereiten) die Option **Prepare** (Vorbereiten) aus.

> [!NOTE]
> Nachdem Sie die Verschiebung vorbereitet haben, ändert sich der Status der Ressourcengruppe in *Einleitung der Verschiebung ausstehend*. 

 
### <a name="move-the-source-resource-group"></a>Verschieben der Quellressourcengruppe

Beginnen Sie mit dem Verschieben der Quellressourcengruppe, indem Sie folgende Schritte ausführen:

1. Wählen Sie im Bereich **Regionsübergreifend** die Ressourcengruppe und dann **Verschiebung einleiten** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png" alt-text="Screenshot: Schaltfläche „Verschiebung einleiten“ im Bereich „Regionsübergreifend“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png":::

1. Wählen Sie im Bereich **Ressourcen verschieben** die Option **Verschiebung einleiten** aus. Der Ressourcengruppenstatus ändert sich in *Verschiebung wird eingeleitet*.   
1. Nachdem Sie die Verschiebung initiiert haben, wird die Zielressourcengruppe basierend auf der generierten ARM-Vorlage erstellt. Der Status der Quellressourcengruppe ändert sich in *Commit für Verschiebung ausstehend*.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png" alt-text="Screenshot: Bereich „Ressourcen verschieben“, in dem sich der Status der Ressourcengruppe in „Commit für Verschiebung ausstehend“ geändert hat" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png":::

Gehen Sie wie folgt vor, um die Verschiebung zu committen und den Prozess abzuschließen:

1. Wählen Sie im Bereich **Regionsübergreifend** die Ressourcengruppe und dann **Commit für Verschiebung ausführen** aus.
1. Wählen Sie im Bereich **Ressourcen verschieben** die Option **Committen** aus.

> [!NOTE]
> Nachdem Sie die Verschiebung committet haben, ändert sich der Status der Quellressourcengruppe in *Löschung der Quelle ausstehend*.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png" alt-text="Screenshot: Quellressourcengruppe, für die sich der Status in „Löschung der Quelle ausstehend“ geändert hat" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png":::

## <a name="prepare-resources-to-move"></a>Vorbereiten der Ressource zum Verschieben

Nach dem Verschieben der Verschlüsselungsressourcen und der Quellressourcengruppe können Sie als Nächstes die Verschiebung anderer Ressourcen mit dem Status *Vorbereitung ausstehend* vorbereiten.


1. Überprüfen Sie im Bereich **Regionsübergreifend** die Verschiebung noch einmal, und beheben Sie mögliche Probleme.
1. Wenn Sie die Zieleinstellungen vor dem Verschieben bearbeiten möchten, wählen Sie den Link in der Spalte **Destination configuration** (Zielkonfiguration) der Ressource aus, und bearbeiten Sie dann die Einstellungen. Wenn Sie die Ziel-VM-Einstellungen bearbeiten, darf die Größe der Ziel-VM nicht kleiner sein als die Größe der Quell-VM.
1. Wählen Sie für Ressourcen mit dem Status *Vorbereitung ausstehend*, die Sie verschieben möchten, die Option **Vorbereiten** aus.
1. Wählen Sie im Bereich **Ressourcen vorbereiten** die Option **Vorbereiten** aus.

    - Während der Vorbereitung wird der Mobilitäts-Agent von Azure Site Recovery auf den virtuellen Computern installiert, um diese zu replizieren.
    - Die VM-Daten werden in regelmäßigen Abständen in der Zielregion repliziert. Dies wirkt sich nicht auf die Quell-VM aus.
    - Beim Verschieben von Ressourcen werden ARM-Vorlagen für die anderen Quellressourcen generiert.

> [!NOTE]
> Nachdem Sie die Ressourcen vorbereitet haben, ändert sich ihr Status in *Einleitung der Verschiebung ausstehend*.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png" alt-text="Screenshot: Bereich „Ressourcen vorbereiten“ mit den Ressourcen im Status „Einleitung der Verschiebung ausstehend“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png":::



## <a name="initiate-the-move"></a>Initiieren der Verschiebung

Nachdem Sie die Ressourcen vorbereitet haben, können Sie die Verschiebung nun initiieren. 

1. Wählen Sie im Bereich **Regionsübergreifend** die Ressourcen aus, deren Status *Einleitung der Verschiebung ausstehend* lautet, und wählen Sie dann **Verschiebung einleiten** aus.
1. Wählen Sie im Bereich **Ressourcen verschieben** die Option **Verschiebung einleiten** aus.
1. Sie können den Status der Verschiebung auf der Benachrichtigungsleiste nachverfolgen.

    - Bei virtuellen Computern werden Replikat-VMs in der Zielregion erstellt. Die Quell-VM wird heruntergefahren, und es kommt zu einer gewissen Downtime (in der Regel einige Minuten).
    - Andere Ressourcen werden von Resource Mover unter Verwendung der vorbereiteten ARM-Vorlagen neu erstellt. Dabei gibt es in der Regel keine Downtime.
    - Nachdem Sie die Ressourcen verschoben haben, ändert sich ihr Status in *Commit für Verschiebung ausstehend*.

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" alt-text="Screenshot: Liste von Ressourcen mit dem Status „Commit für Verschiebung ausstehend“" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" :::


## <a name="discard-or-commit"></a>Verwerfen oder committen?

Nach der anfänglichen Verschiebung können Sie entscheiden, ob Sie einen Commit für die Verschiebung durchführen oder sie verwerfen möchten. 

- **Verwerfen:** Sie können eine Verschiebung verwerfen, wenn Sie Tests durchführen und die Quellressource letztendlich nicht verschoben werden soll. Beim Verwerfen der Verschiebung wird die Ressource in den Status *Einleitung der Verschiebung ausstehend* zurückgesetzt.
- **Commit**: Beim Commit wird die Verschiebung in die Zielregion abgeschlossen. Nach dem Commit für eine Quellressource ändert sich ihr Status in *Löschung der Quelle ausstehend*, und Sie können entscheiden, ob Sie sie löschen möchten.


## <a name="discard-the-move"></a>Verwerfen der Verschiebung 

Gehen Sie wie folgt vor, um die Verschiebung zu verwerfen:

1. Wählen Sie im Bereich **Regionsübergreifend** Ressourcen aus, deren Status *Commit für Verschiebung ausstehend* lautet, und wählen Sie dann **Discard move** (Verschiebung verwerfen) aus.
1. Wählen Sie im Bereich **Discard move** (Verschiebung verwerfen) die Option **Verwerfen** aus.
1. Sie können den Status der Verschiebung auf der Benachrichtigungsleiste nachverfolgen.


> [!NOTE]
> Nachdem Sie die Ressourcen verworfen haben, ändert sich der Status der virtuellen Computer in *Einleitung der Verschiebung ausstehend*.

## <a name="commit-the-move"></a>Committen der Verschiebung

Committen Sie zum Abschließen der Verschiebung diese wie folgt: 

1. Wählen Sie im Bereich **Regionsübergreifend** Ressourcen aus, deren Status *Commit für Verschiebung ausstehend* lautet, und wählen Sie dann **Commit für Verschiebung ausführen** aus.
1. Wählen Sie im Bereich **Commit für Ressourcen ausführen** die Option **Committen** aus.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" alt-text="Screenshot: Liste der Ressourcen zum Committen von Ressourcen zum Abschließen der Verschiebung" lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" :::

1. Verfolgen Sie den Fortschritt des Commits in der Benachrichtigungsleiste nach.

> [!NOTE]
> - Nach dem Committen der Verschiebung wird die Replikation der virtuellen Computer beendet. Der virtuelle Quellcomputer ist von dem Commit nicht betroffen.
> - Der Commitprozess wirkt sich nicht auf die Quellnetzwerkressourcen aus.
> - Nachdem Sie die Verschiebung committet haben, ändert sich der Ressourcenstatus in *Löschung der Quelle ausstehend*.



## <a name="configure-settings-after-the-move"></a>Konfigurieren der Einstellungen nach dem Verschieben

- Der Mobilitätsdienst wird auf virtuellen Computern nicht automatisch deinstalliert. Deinstallieren Sie ihn manuell, oder lassen Sie ihn installiert, wenn Sie planen, den Server erneut zu verschieben.
- Ändern Sie nach dem Verschieben die Regeln der rollenbasierten Zugriffssteuerung (RBAC) in Azure.

## <a name="delete-source-resources-after-commit"></a>Löschen von Quellressourcen nach einem Commit

Nach dem Verschieben können Sie die Ressourcen in der Quellregion löschen, dies ist jedoch optional. 

1. Wählen Sie im Bereich **Regionsübergreifend** alle Quellressourcen aus, die Sie löschen möchten, und wählen Sie dann **Quelle löschen** aus.
1. Überprüfen Sie unter **Quelle löschen** die zu löschenden Elemente, und geben Sie unter **Löschvorgang bestätigen** den Text **Ja** ein. Die Aktion kann nicht rückgängig gemacht werden. Prüfen Sie daher sorgfältig.
1. Wählen Sie nach Eingabe von **Ja** die Option **Quelle löschen** aus.

> [!NOTE]
>  Im Portal für die Ressourcenverschiebung können keine Ressourcengruppen, Schlüsseltresore oder SQL Server-Instanzen gelöscht werden. Diese Elemente müssen einzeln über die Eigenschaftenseite der jeweiligen Ressource gelöscht werden.


## <a name="delete-resources-that-you-created-for-the-move"></a>Löschen von Ressourcen, die für das Verschieben erstellt wurden

Nach dem Verschieben können Sie die Verschiebungssammlung und die während dieses Prozesses erstellten Site Recovery-Ressourcen manuell löschen.

- Die Sammlung für die Verschiebung ist standardmäßig ausgeblendet. Blenden Sie ausgeblendete Ressourcen ein, um sie anzuzeigen.
- Der Cachespeicher verfügt über eine Sperre, die zunächst gelöscht werden muss.

Führen Sie zum Löschen der Ressourcen die folgenden Schritte aus: 
1. Suchen Sie die Ressourcen in der Ressourcengruppe ```RegionMoveRG-<sourceregion>-<target-region>```.
1. Vergewissern Sie sich, dass alle virtuellen Computer und andere Quellressourcen in der Quellregion verschoben oder gelöscht wurden. Mit diesem Schritt wird sichergestellt, dass sie nicht von ausstehenden Ressourcen verwendet werden.
1. Löschen der Ressourcen:

    - Name der Verschiebungssammlung: ```movecollection-<sourceregion>-<target-region>```
    - Name des Cachespeicherkontos: ```resmovecache<guid>```
    - Tresorname: ```ResourceMove-<sourceregion>-<target-region>-GUID```
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Verschieben verschlüsselter virtueller Azure-Computer und ihrer abhängigen Ressourcen in eine andere Azure-Region


Versuchen Sie als Nächstes, Azure SQL-Datenbanken und Pools für elastische Datenbanken in eine andere Region zu verschieben.

> [!div class="nextstepaction"]
> [Verschieben von Azure SQL-Ressourcen](./tutorial-move-region-sql.md)
