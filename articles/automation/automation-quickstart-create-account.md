---
title: Azure-Schnellstart – Erstellen eines Azure Automation-Kontos | Microsoft-Dokumentation
description: Dieser Artikel hilft Ihnen bei der Erstellung eines Azure Automation-Kontos und der Ausführung eines Runbooks.
services: automation
ms.date: 04/04/2019
ms.topic: quickstart
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: a07c6ac524aa213519ace1ae204ac2d76db802aa
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836701"
---
# <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Ein Azure Automation-Konto kann über Azure mithilfe des Azure-Portals erstellt werden. Hierbei handelt es sich um eine browserbasierten Benutzeroberfläche für den Zugriff auf eine Reihe von Ressourcen. Von einem Automation-Konto können Ressourcen in allen Regionen und Abonnements für einen bestimmten Mandanten verwaltet werden. 

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Automation-Konto erstellen und ein Runbook in dem Konto ausführen. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

[Melden Sie sich bei Azure an.](https://portal.azure.com)

## <a name="create-automation-account"></a>Erstellen eines Automation-Kontos

1. Wählen Sie einen Namen für Ihr Azure-Konto. Automation-Kontonamen sind für jede Region und Ressourcengruppe eindeutig. Namen von gelöschten Automation-Konten sind möglicherweise nicht sofort verfügbar.

    > [!NOTE]
    > Nach der Eingabe auf der Benutzeroberfläche kann der Kontoname nicht mehr geändert werden. 

2. Klicken Sie links oben im Azure-Portal auf die Schaltfläche **Ressource erstellen**.

3. Wählen Sie **IT- und Verwaltungstools** und dann **Automation** aus.

4. Geben Sie die Kontoinformationen einschließlich des gewählten Kontonamens ein. Wählen Sie für **Ausführendes Azure-Konto erstellen** die Option **Ja**, damit die Artefakte zum Vereinfachen der Authentifizierung für Azure automatisch aktiviert werden. Klicken Sie nach Angabe aller Informationen auf **Erstellen**, um die Bereitstellung des Automation-Kontos zu starten.

    ![Eingeben von Informationen zu Ihrem Automation-Konto auf der Seite](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Eine aktualisierte Liste mit Standorten, an denen Sie ein Automation-Konto bereitstellen können, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all).

5. Klicken Sie nach Abschluss der Bereitstellung auf **Alle Dienste**.

6. Wählen Sie **Automation-Konten** und anschließend das von Ihnen erstellte Automation-Konto aus.

    ![Automation-Konto – Übersicht](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Ausführen eines Runbooks

Führen Sie ein Runbook des Tutorials aus.

1. Klicken Sie unter **Prozessautomatisierung** auf **Runbooks**. Die Liste mit den Runbooks wird angezeigt. Standardmäßig sind im Konto mehrere Tutorial-Runbooks aktiviert.

    ![Liste mit Runbooks des Automation-Kontos](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus. Die Seite mit der Runbookübersicht wird geöffnet.

    ![Runbookübersicht](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Klicken Sie auf **Starten** und anschließend auf der Seite „Runbook starten“ auf **OK**, um das Runbook zu starten.

    ![Seite für Runbookauftrag](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Sobald der Auftragsstatus `Running` lautet, können Sie auf **Ausgabe** oder **Alle Protokolle** klicken, um die Ausgabe des Runbookauftrags anzuzeigen. Für dieses Tutorial-Runbook ist die Ausgabe eine Liste mit Ihren Azure-Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Automation-Konto bereitgestellt, einen Runbookauftrag gestartet und die Auftragsergebnisse angezeigt. Weitere Informationen zu Azure Automation finden Sie in der Schnellstartanleitung zum Erstellen Ihres ersten Runbooks.

> [!div class="nextstepaction"]
> [Automation-Schnellstart: Erstellen eines Azure Automation-Runbooks](./automation-quickstart-create-runbook.md)

