---
title: Einrichten der Notfallwiederherstellung für virtuelle Azure-Computer in einer sekundären Region mit Azure Site Recovery
description: Richten Sie in wenigen Schritten die Notfallwiederherstellung für eine Azure-VM zu einer anderen Azure-Region mit dem Dienst „Azure Site Recovery“ ein.
ms.topic: quickstart
ms.date: 03/27/2020
ms.custom: mvc
ms.openlocfilehash: 8681ccaac30402927005e5e8802d7da2bddc2c5c
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135710"
---
# <a name="quickstart-set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Schnellstart: Einrichten der Notfallwiederherstellung in einer sekundären Azure-Region für einen virtuellen Azure-Computer

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem der Onlinezustand Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Notfallwiederherstellung für einen virtuellen Azure-Computer einrichten, indem Sie ihn in einer sekundären Azure-Region replizieren. Im Allgemeinen werden die Standardeinstellungen genutzt, um die Replikation zu ermöglichen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Tutorials benötigen Sie ein Azure-Abonnement und eine VM.

- Falls Sie nicht über ein Azure-Abonnement mit einem aktiven Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine VM mit mindestens 1 GB RAM wird empfohlen. [Erfahren Sie mehr](../virtual-machines/windows/quick-create-portal.md) über die Erstellung einer VM.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-replication-for-the-azure-vm"></a>Aktivieren der Replikation für die Azure-VM

Die folgenden Schritte ermöglichen die Replikation der VM an einem sekundären Ort.

1. Wählen Sie im Azure-Portal unter **Startseite** > **Virtuelle Computer** eine VM aus, die repliziert werden soll.
1. Wählen Sie unter **Vorgänge** die Option **Notfallwiederherstellung** aus.
1. Wählen Sie unter **Grundlagen** > **Zielregion** die Zielregion aus.
1. Wählen Sie zum Anzeigen der Replikationseinstellungen die Option **Replikation überprüfen und starten** aus. Wählen Sie **Erweiterte Einstellungen** aus, falls Sie Standardwerte ändern müssen.
1. Wählen Sie **Replikation starten** aus, um den Auftrag zu starten, mit dem die VM-Replikation durchgeführt wird.

   :::image type="content" source="media/azure-to-azure-quickstart/enable-replication1.png" alt-text="Aktivieren der Replikation":::

## <a name="verify-settings"></a>Überprüfen der Einstellungen

Nach Abschluss des Replikationsauftrags können Sie den Replikationsstatus überprüfen, die Replikationseinstellungen ändern und die Bereitstellung testen.

1. Wählen Sie im Azure-Portal im Menü die Option **Virtuelle Computer** und dann die VM aus, die Sie repliziert haben.
1. Wählen Sie unter **Vorgänge** die Option **Notfallwiederherstellung** aus.
1. Wählen Sie unter **Übersicht** die Option **Zusammenfassung** aus, um die Replikationsdetails anzuzeigen. Ausführlichere Informationen finden Sie unter **Integrität und Status**, **Failoverbereitschaft** und **Infrastrukturansicht**.

   :::image type="content" source="media/azure-to-azure-quickstart/replication-status.png" alt-text="Replikationsstatus":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie müssen die Replikation deaktivieren, um die Replikation der VM in der primären Region zu beenden:

- Die Quellreplikationseinstellungen werden automatisch bereinigt.
- Die Site Recovery-Erweiterung, die während der Replikation auf der VM installiert wird, wird nicht entfernt.
- Die Site Recovery-Abrechnung für den virtuellen Computer wird beendet.

Führen Sie zum Deaktivieren der Replikation die folgenden Schritte aus:

1. Wählen Sie im Azure-Portal im Menü die Option **Virtuelle Computer** und dann die VM aus, die Sie repliziert haben.
1. Wählen Sie unter **Vorgänge** die Option **Notfallwiederherstellung** aus.
1. Wählen Sie unter **Übersicht** die Option **Replikation deaktivieren** aus.
1. Navigieren Sie für die VM zu **Einstellungen** > **Erweiterungen**, um die Site Recovery-Erweiterung zu deinstallieren.

   :::image type="content" source="media/azure-to-azure-quickstart/disable2-replication.png" alt-text="Deaktivieren der Replikation":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine einzelne VM in eine sekundäre Region repliziert. Richten Sie als Nächstes die Replikation für mehrere Azure-VMs ein.

> [!div class="nextstepaction"]
> [Set up disaster recovery for Azure VMs (Einrichten der Notfallwiederherstellung für Azure-VMs)](azure-to-azure-tutorial-enable-replication.md)
