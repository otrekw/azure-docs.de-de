---
title: Windows Virtual Desktop-Hostpool zum Überwachen von Dienstupdates – Azure
description: Erfahren Sie, wie Sie einen Hostpool für die Überwachung von Dienstupdates erstellen, bevor Updates in der Produktion bereitgestellt werden.
author: Heidilohr
ms.topic: tutorial
ms.date: 12/15/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ead4c0aa7d8d71642fd8a4635edbabcafee5b6c2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97563244"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Tutorial: Erstellen eines Hostpools zum Überprüfen von Dienstupdates

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/create-validation-host-pool-2019.md).

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Windows Virtual Desktop-Umgebungen. Es wird dringend empfohlen, einen Überprüfungshostpool zu erstellen, in dem Dienstupdates zuerst angewendet werden. Auf diese Weise können Sie Dienstupdates überwachen, bevor sie vom Dienst auf Ihre Standardumgebung oder Nicht-Überprüfungsumgebung angewendet werden. Ohne einen Überprüfungshostpool können Sie keine Änderungen erkennen, die Fehler einführen, was zu Downtime für Benutzer in der Standardumgebung führen könnte.

Um sicherzustellen, dass Ihre Apps mit den neuesten Updates funktionieren, sollte der Überprüfungshostpool Hostpools in Ihrer Nicht-Überprüfungsumgebung so ähnlich wie möglich sein. Benutzer sollten so häufig eine Verbindung mit dem Überprüfungshostpool herstellen wie mit dem Standardhostpool. Wenn Sie automatisierte Tests mit Ihrem Hostpool durchführen, sollten Sie auch beim Überprüfungshostpool automatisierte Tests durchführen.

Sie können Probleme beim Überprüfungshostpool entweder mit der [Diagnosefunktion](diagnostics-role-service.md) oder den [Artikeln zur Problembehandlung bei Windows Virtual Desktop](troubleshoot-set-up-overview.md) debuggen.

>[!NOTE]
> Wir empfehlen, dass Sie den Überprüfungshostpool eingerichtet lassen, um alle zukünftigen Updates zu testen.

>[!IMPORTANT]
>Bei Windows Virtual Desktop mit Integration der Azure-Ressourcenverwaltung treten derzeit Probleme beim Aktivieren und Deaktivieren von Überprüfungsumgebungen auf. Dieser Artikel wird aktualisiert, wenn das Problem behoben wurde.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, befolgen Sie die Anweisungen unter [Einrichten des Windows Virtual Desktop PowerShell-Moduls](powershell-module.md), um Ihr PowerShell-Modul einzurichten und sich bei Azure anzumelden.

## <a name="create-your-host-pool"></a>Erstellen Ihres Hostpools

Sie können einen Hostpool erstellen, indem Sie die Anweisungen in diesen Artikeln befolgen:
- [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Definieren Ihres Hostpools als Überprüfungshostpool

Führen Sie die folgenden PowerShell-Cmdlets aus, um den neuen Hostpool als Überprüfungshostpool zu definieren. Ersetzen Sie die Werte in Klammern durch die für Ihre Sitzung relevanten Werte:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -ValidationEnvironment:$true
```

Führen Sie das folgende PowerShell-Cmdlet aus, um zu bestätigen, dass die Überprüfungseigenschaft festgelegt ist. Ersetzen Sie die Werte in Klammern durch die für Ihre Sitzung relevanten Werte.

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | Format-List
```

Die Ergebnisse des Cmdlets sollte der folgenden Ausgabe ähneln:

```powershell
    HostPoolName        : hostpoolname
    FriendlyName        :
    Description         :
    Persistent          : False
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnvironment : True
```

## <a name="enable-your-validation-environment-with-the-azure-portal"></a>Aktivieren Ihrer Überprüfungsumgebung über das Azure-Portal

Sie können auch das Azure-Portal verwenden, um Ihre Überprüfungsumgebung zu aktivieren.

So verwenden Sie das Azure-Portal, um Ihren Überprüfungshostpool zu konfigurieren

1. Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.
2. Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.
3. Wählen Sie auf der Seite „Windows Virtual Desktop“ die Option **Hostpools** aus.
4. Wählen Sie den Namen des Hostpools aus, den Sie bearbeiten möchten.
5. Wählen Sie **Eigenschaften** aus.
6. Wählen Sie im Feld „Überprüfungsumgebung“ **Ja** aus, um die Überprüfungsumgebung zu aktivieren.
7. Wählen Sie **Speichern** aus. Dadurch werden die neuen Einstellungen angewendet.

## <a name="update-schedule"></a>Zeitplan für Updates

Dienstupdates werden monatlich bereitgestellt. Wenn schwerwiegende Probleme vorliegen, werden kritische Updates in häufigeren Abständen bereitgestellt.

Wenn Dienstupdates verfügbar sind, stellen Sie sicher, dass sich jeden Tag zumindest eine kleine Gruppe von Benutzern anmeldet, um die Umgebung zu überprüfen. Es empfiehlt sich, regelmäßig die [TechCommunity-Website](https://techcommunity.microsoft.com/t5/forums/searchpage/tab/message?filter=location&q=wvdupdate&location=forum-board:WindowsVirtualDesktop&sort_by=-topicPostDate&collapse_discussion=true) zu besuchen und alle Beiträge mit dem Tag „WVDUPdate“ zu verfolgen, um über Dienstupdates auf dem Laufenden zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überprüfungshostpool erstellt haben, können Sie sich darüber informieren, wie Sie Azure Service Health zum Überwachen Ihrer Windows Virtual Desktop-Bereitstellung verwenden.

> [!div class="nextstepaction"]
> [Einrichten von Dienstwarnungen](./set-up-service-alerts.md)
