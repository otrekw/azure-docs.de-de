---
title: Verbinden eines Hybridcomputers mit Servern mit Azure Arc-Unterstützung (Vorschauversion)
description: Hier erfahren Sie, wie Sie Ihren Hybridcomputer mit Servern mit Azure Arc-Unterstützung (Vorschauversion) verbinden und registrieren.
ms.topic: quickstart
ms.date: 08/12/2020
ms.openlocfilehash: eacf75871b1f7cc7fc3b703d8859338578e43456
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88213612"
---
# <a name="quickstart-connect-hybrid-machine-with-azure-arc-enabled-servers-preview"></a>Schnellstart: Verbinden eines Hybridcomputers mit Servern mit Azure Arc-Unterstützung (Vorschauversion)

Mit [Servern mit Azure Arc-Unterstützung](../overview.md) (Vorschauversion) können Sie Ihre in lokalen Umgebungen, in Edgeumgebungen und in Multicloudumgebungen gehosteten Windows- und Linux-Computer verwalten und steuern. In dieser Schnellstartanleitung wird der Connected Machine-Agent auf Ihrem außerhalb von Azure gehosteten Windows- oder Linux-Computer bereitgestellt und für die Verwaltung durch Server mit Azure Arc-Unterstützung (Vorschauversion) konfiguriert.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Zur Bereitstellung des hybriden Connected Machine-Agents für Server mit Azure Arc-Unterstützung (Vorschauversion) müssen Sie auf dem Computer über Administratorberechtigungen verfügen, um den Agent installieren und konfigurieren zu können. Verwenden Sie dazu unter Linux das root-Konto und unter Windows ein Konto, das der Gruppe „Lokale Administratoren“ angehört.

* Machen Sie sich zunächst mit den [Voraussetzungen](../agent-overview.md#prerequisites) für den Agent vertraut, und vergewissern Sie sich, dass Folgendes erfüllt ist:

    * Auf Ihrem Zielcomputer wird ein unterstütztes [Betriebssystem](../agent-overview.md#supported-operating-systems) ausgeführt.

    * Ihrem Konto sind die [erforderlichen Azure-Rollen](../agent-overview.md#required-permissions) zugewiesen.

    * Sollte der Computer für die Kommunikation über das Internet eine Verbindung über eine Firewall oder über einen Proxyserver herstellen, sorgen Sie dafür, dass die [aufgeführten](../agent-overview.md#networking-configuration) URLs nicht blockiert werden.

    * Von Servern mit Azure Arc-Unterstützung (Vorschauversion) werden nur die [hier](../overview.md#supported-regions) angegebenen Regionen unterstützt.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

## <a name="register-azure-resource-providers"></a>Registrieren von Azure-Ressourcenanbietern

Server mit Azure Arc-Unterstützung (Vorschauversion) benötigen folgende Azure-Ressourcenanbieter in Ihrem Abonnement, um diesen Dienst nutzen zu können:

* Microsoft.HybridCompute
* Microsoft.GuestConfiguration

Registrieren Sie sie mithilfe folgender Befehle:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

## <a name="generate-installation-script"></a>Generieren des Installationsskripts

Das Skript zum Automatisieren des Download- und Installationsvorgangs sowie zum Herstellen der Verbindung mit Azure Arc ist über das Azure-Portal verfügbar. Gehen Sie wie folgt vor:

1. Starten Sie den Azure Arc-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Computer – Azure Arc** suchen und diese Option auswählen.

    :::image type="content" source="./media/quick-enable-hybrid-vm/search-machines.png" alt-text="Suchen nach Servern mit Arc-Unterstützung unter „Alle Dienste“" border="false":::

1. Wählen Sie auf der Seite **Computer – Azure Arc** entweder **Hinzufügen** (links oben) oder die Option **Computer erstellen – Azure Arc** (unten im mittleren Bereich) aus.

1. Wählen Sie auf der Seite **Methode auswählen** die Kachel **Computer über ein interaktives Skript hinzufügen** und anschließend **Skript generieren** aus.

1. Wählen Sie auf der Seite **Skript generieren** das Abonnement und die Ressourcengruppe für die Verwaltung des Computers in Azure aus. Wählen Sie einen Azure-Standort zum Speichern der Computermetadaten aus.

1. Wählen Sie auf der Seite **Skript generieren** in der Dropdownliste **Betriebssystem** das Betriebssystem aus, unter dem das Skript ausgeführt wird.

1. Falls der Computer über einen Proxyserver kommuniziert, um eine Internetverbindung herzustellen, wählen Sie **Weiter: Proxyserver** aus.

1. Geben Sie auf der Registerkarte **Proxyserver** die IP-Adresse des Proxyservers oder den Namen und die Portnummer für die Kommunikation mit dem Proxyserver an. Geben Sie den Wert im Format `http://<proxyURL>:<proxyport>` ein.

1. Wählen Sie **Überprüfen + generieren** aus.

1. Überprüfen Sie die auf der Registerkarte **Überprüfen + generieren** zusammengefassten Informationen, und wählen Sie dann **Herunterladen** aus. Sollten noch Änderungen erforderlich sein, wählen Sie **Zurück** aus.

## <a name="install-the-agent-using-the-script"></a>Installieren des Agents mithilfe des Skripts

### <a name="windows-agent"></a>Windows-Agent

1. Melden Sie sich beim Server an.

1. Öffnen Sie eine 64-Bit-PowerShell-Eingabeaufforderung mit erhöhten Rechten.

1. Navigieren Sie zu dem Ordner bzw. zu der Freigabe, in den bzw. in die Sie das Skript kopiert haben, und führen Sie es auf dem Server aus, indem Sie das Skript `./OnboardingScript.ps1` ausführen.

### <a name="linux-agent"></a>Linux-Agent

1. Führen Sie den folgenden Befehl aus, um den Linux-Agent auf dem Zielcomputer zu installieren, der direkt mit Azure kommunizieren kann:

    ```bash
    bash ~/Install_linux_azcmagent.sh
    ```

    * Wenn der Zielcomputer über einen Proxyserver kommuniziert, führen Sie den folgenden Befehl aus:

        ```bash
        bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
        ```

## <a name="verify-the-connection-with-azure-arc"></a>Überprüfen der Verbindung mit Azure Arc

Vergewissern Sie sich im Azure-Portal, dass die Serververbindung erfolgreich hergestellt wurde, nachdem Sie den Agent installiert und für die Verbindungsherstellung mit Servern mit Azure Arc-Unterstützung (Vorschauversion) konfiguriert haben. Sehen Sie sich Ihre Computer im [Azure-Portal](https://aka.ms/hybridmachineportal) an.

:::image type="content" source="./media/quick-enable-hybrid-vm/enabled-machine.png" alt-text="Erfolgreiche Computerverbindung" border="false":::

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren Linux- oder Windows-Hybridcomputer aktiviert und erfolgreich mit dem Dienst verbunden haben, können Sie Azure Policy aktivieren, um sich mit der Compliance in Azure zu beschäftigen.

Im nächsten Tutorial erfahren Sie, wie Sie Server mit Azure Arc-Unterstützung (Vorschauversion) identifizieren, auf denen der Log Analytics-Agent nicht installiert ist:

> [!div class="nextstepaction"]
> [Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen](tutorial-assign-policy-portal.md)
