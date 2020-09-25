---
title: Erstellen eines Datencontrollers in Azure Data Studio
description: Erstellen eines Datencontrollers in Azure Data Studio
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 71678977f899b910a97dbb552233f36ca5a51f26
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931620"
---
# <a name="create-data-controller-in-azure-data-studio"></a>Erstellen eines Datencontrollers in Azure Data Studio

Sie können einen Datencontroller mit Azure Data Studio erstellen, indem Sie den Bereitstellungs-Assistenten und Notebooks verwenden.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Zugriff auf einen Kubernetes-Cluster und müssen die kubeconfig-Datei so konfigurieren, dass sie auf den Kubernetes-Cluster verweist, in dem Sie die Bereitstellung vornehmen möchten.
- Sie müssen die [Clienttools installieren](install-client-tools.md), einschließlich **Azure Data Studio** und der Azure Data Studio-Erweiterungen namens **Azure Arc** und **Azure Data CLI**.
- Sie müssen sich in Azure Data Studio bei Azure anmelden.  Drücken Sie dazu STRG/BEFEHL+UMSCHALT+P, um das Fenster für den Befehlstext zu öffnen, und geben Sie **Azure** ein.  Wählen Sie **Azure: Sign in** (Azure: Anmelden) aus.   Klicken Sie im eingeblendeten Bereich auf das Plussymbol (+) oben rechts, um ein Azure-Konto hinzuzufügen.

## <a name="use-the-deployment-wizard-to-create-azure-arc-data-controller"></a>Erstellen eines Azure Arc-Datencontrollers mit dem Bereitstellungs-Assistenten

Führen Sie die folgenden Schritte aus, um mithilfe des Bereitstellungs-Assistenten einen Azure Arc-Datencontroller zu erstellen.

1. Klicken Sie in Azure Data Studio im linken Navigationsbereich auf die Registerkarte „Verbindungen“.
2. Klicken Sie oben im Bereich „Verbindungen“ auf die Schaltfläche mit den drei Auslassungspunkten **...** , und wählen Sie **Neue Bereitstellung** aus.
3. Wählen Sie im Assistenten für neue Bereitstellungen **Azure Arc Data Controller** (Azure Arc-Datencontroller) aus, aktivieren Sie das Kontrollkästchen für die Zustimmung zur Lizenz, und klicken Sie dann unten auf die Schaltfläche **Auswählen**.
4. Verwenden Sie die standardmäßige kubeconfig-Datei, oder wählen Sie eine andere aus.  Klicken Sie auf **Weiter**.
5. Wählen Sie einen Kubernetes-Clusterkontext aus. Klicken Sie auf **Weiter**.
6. Wählen Sie abhängig von Ihrem Kubernetes-Zielcluster eine Datei für das Bereitstellungskonfigurationsprofil aus. **Klicken Sie auf „Weiter“**.
8. Wählen Sie das gewünschte Abonnement und die gewünschte Ressourcengruppe aus.
9. Geben Sie einen Namen für den Datencontroller und für den Namespace ein, in dem der Datencontroller erstellt wird.  

> [!NOTE]
> Wenn der Namespace bereits vorhanden ist, wird er verwendet, wenn er noch keine anderen Kubernetes-Objekte (Pods usw.) enthält.  Wenn der Namespace nicht vorhanden ist, wird versucht, den Namespace zu erstellen.  Zum Erstellen eines Namespace in einem Kubernetes-Cluster sind Administratorrechte für den Kubernetes-Cluster erforderlich.  Wenn Sie keine Administratorrechte für den Kubernetes-Cluster besitzen, bitten Sie den Kubernetes-Clusteradministrator, die ersten Schritte im Artikel zum [Erstellen eines Datencontrollers mithilfe nativer Kubernetes-Tools](./create-data-controller-using-k8s-native-tools.md) auszuführen. Diese Schritte müssen von einem Kubernetes-Administrator ausgeführt werden, bevor Sie diesen Assistenten abschließen.

> [!NOTE]
> Hinweis: Der Datencontroller- und Namespacename werden verwendet, um eine benutzerdefinierte Ressource im Kubernetes-Cluster zu erstellen. Daher müssen sie den [Namenskonventionen von Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#names) entsprechen.

10. Wählen Sie einen Azure-Standort aus.
   
> [!NOTE]
> Der hier ausgewählte Azure-Standort ist der Ort, an dem die *Metadaten* zum Datencontroller und die davon verwalteten Datenbankinstanzen in Azure gespeichert werden.  Der Datencontroller und die Datenbankinstanzen werden tatsächlich an diesem Ort für Ihren Kubernetes-Cluster erstellt.

11.  Geben Sie einen Benutzernamen und ein Kennwort ein, und bestätigen Sie das Kennwort für das Konto des Datencontrolleradministrators.

> [!NOTE]
> Das Kennwort muss mindestens 8 Zeichen umfassen.

1.  Klicken Sie auf **Weiter**.
2.  Überprüfen Sie dieses, und klicken Sie auf **Skript in Notebook schreiben**.
3.  **Überprüfen Sie das generierte Notebook**.  Nehmen Sie alle erforderlichen Änderungen vor, z. B. an Namen von Speicherklassen oder Diensttypen.
4.  Klicken Sie oben im Notebook auf **Alle ausführen**.

## <a name="monitoring-the-creation-status"></a>Überwachen des Erstellungsstatus

Das Erstellen des Controllers dauert einige Minuten. Mithilfe der folgenden Befehle können Sie den Status in einem anderen Terminalfenster überwachen:

> [!NOTE]
>  Bei den folgenden Beispielbefehlen wird davon ausgegangen, dass Sie einen Datencontroller und Kubernetes-Namespace mit dem Namen „arc“ erstellt haben.  Wenn Sie einen anderen Namespace-/Datencontrollernamen verwendet haben, können Sie „arc“ durch diesen Namen ersetzen.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Sie können auch den Erstellungsstatus eines bestimmten Pods überprüfen, indem Sie einen Befehl wie den folgenden ausführen.  Dies ist besonders bei der Problembehandlung hilfreich.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Beheben von Problemen bei der Erstellung

Wenn Probleme bei der Erstellung auftreten, finden Sie weitere Informationen im [Handbuch zur Problembehandlung](troubleshoot-guide.md).
