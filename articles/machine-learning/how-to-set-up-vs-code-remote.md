---
title: 'Interaktives Debuggen: VS Code- und ML Compute-Instanzen (Vorschau)'
titleSuffix: Azure Machine Learning
description: Richten Sie VS Code Remote ein, um Ihren Code interaktiv mit Azure Machine Learning zu debuggen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: jmartens
author: j-martens
ms.date: 08/06/2020
ms.openlocfilehash: 37d0ec0295d76f740b2e8bf70ae72f0c95e68d14
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904478"
---
# <a name="debug-interactively-on-an-azure-machine-learning-compute-instance-with-vs-code-remote-preview"></a>Interaktives Debuggen auf einer Azure Machine Learning Compute-Instanz mit VS Code Remote (Vorschau)

In diesem Artikel erfahren Sie, wie Sie die Visual Studio Code-Remoteerweiterung auf einer Azure Machine Learning Compute-Instanz einrichten, sodass Sie Ihren **Code über VS Code interaktiv debuggen** können.

* Eine [Azure Machine Learning Compute-Instanz](concept-compute-instance.md) ist eine vollständig verwaltete cloudbasierte Arbeitsstation für Datenanalysten und bietet IT-Administratoren Funktionen für die Verwaltung und Unternehmensbereitschaft. 

* Mit[ Visual Studio Code Remote](https://code.visualstudio.com/docs/remote/remote-overview) Development können Sie einen Container, einen Remotecomputer oder das Windows-Subsystem für Linux (WSL) als voll ausgestattete Entwicklungsumgebung verwenden. 

## <a name="prerequisite"></a>Voraussetzung  

* SSH-fähige Compute-Instanz. Weitere Informationen finden Sie unter [Erstellen einer Compute-Instanz](https://docs.microsoft.com/azure/machine-learning/concept-compute-instance#create).
* Auf Windows-Plattformen müssen Sie ggf. [einem mit OpenSSH kompatiblen SSH-Client installieren](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client). 

> [!Note]
> PuTTY wird unter Windows nicht unterstützt, da sich der SSH-Befehl im Pfad befinden muss. 

## <a name="get-the-ip-and-ssh-port-for-your-compute-instance"></a>Abrufen der IP-Adresse und des SSH-Ports für die Compute-Instanz

1. Wechseln Sie zum Azure Machine Learning-Studio unter https://ml.azure.com/.

2. Wählen Sie Ihren [Arbeitsbereich](concept-workspace.md) aus.
1. Aktualisieren der Registerkarte **Compute-Instanzen**.
1. Klicken Sie in der Spalte **Anwendungs-URI** auf den **SSH**-Link der Compute-Instanz, die Sie als Remotecompute-Instanz verwenden möchten. 
1. Notieren Sie die IP-Adresse und den SSH-Port aus dem angezeigten Dialogfeld. 
1. Speichern Sie den privaten Schlüssel im Verzeichnis „~/.ssh/“ auf dem lokalen Computer. Öffnen Sie beispielsweise einen Editor für eine neue Datei, und fügen Sie den Schlüssel in ein: 

   **Linux:**

   ```sh
   vi ~/.ssh/id_azmlcitest_rsa  
   ```

   **Windows**:

   ```cmd
   notepad C:\Users\<username>\.ssh\id_azmlcitest_rsa
   ```

   Der private Schlüssel sieht in etwa wie folgt aus:

   ```text
   -----BEGIN RSA PRIVATE KEY-----

   MIIEpAIBAAKCAQEAr99EPm0P4CaTPT2KtBt+kpN3rmsNNE5dS0vmGWxIXq4vAWXD
   ..... 
   ewMtLnDgXWYJo0IyQ91ynOdxbFoVOuuGNdDoBykUZPQfeHDONy2Raw==

   -----END RSA PRIVATE KEY-----
   ```

1. Ändern Sie die Berechtigungen für die Datei, um sicherzustellen, dass nur Sie die Datei lesen können.  

   ```sh
   chmod 600 ~/.ssh/id_azmlcitest_rsa
   ```

## <a name="add-instance-as-a-host"></a>Hinzufügen von Instanz als Host

Öffnen Sie die Datei `~/.ssh/config` (Linux) oder `C:\Users<username>.ssh\config` (Windows) in einem Editor, und fügen Sie einen neuen Eintrag ähnlich dem folgenden hinzu:

```
Host azmlci1 

    HostName 13.69.56.51 

    Port 50000 

    User azureuser 

    IdentityFile ~/.ssh/id_azmlcitest_rsa
```

Hier einige Details zu den Feldern:

|Feld|BESCHREIBUNG|
|----|---------|
|Host|Verwenden Sie einen beliebigen Kurznamen für die Compute-Instanz. |
|HostName|Dies ist die IP-Adresse der Compute-Instanz. |
|Port|Dies ist der im obigen SSH-Dialogfeld angezeigte Port. |
|Benutzer|Dies muss  `azureuser` sein. |
|IdentityFile|Muss auf die Datei verweisen, in der Sie den privaten Schlüssel gespeichert haben. |

Jetzt sollten Sie mithilfe des oben verwendeten Kurznamens `ssh azmlci1` eine SSH-Verbindung mit Ihrer Compute-Instanz herstellen können.

## <a name="connect-vs-code-to-the-instance"></a>Verbinden von VS Code mit der Instanz

1. [Installieren Sie Visual Studio Code.](https://code.visualstudio.com/)

1. [Installieren Sie die Remote-SSH-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).

1. Klicken Sie auf das Remote-SSH-Symbol auf der linken Seite, um Ihre SSH-Konfigurationen anzuzeigen.

1. Klicken Sie mit der rechten Maustaste auf die soeben erstellte SSH-Hostkonfiguration.

1. Wählen Sie **Im aktuellem Fenster eine Verbindung mit dem Host herstellen** aus. 

Ab diesem Punkt arbeiten Sie komplett auf der Compute-Instanz und können nun Bearbeitungen durchführen, Debuggen, Git benutzen, Erweiterungen verwenden, etc. – genauso, wie das mit Ihrem lokalen Visual Studio Code möglich ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Visual Studio Code-Remoteerweiterung eingerichtet haben, können Sie eine Compute-Instanz als Remotecompute-Instanz aus Visual Studio Code verwenden, um den [Code interaktiv zu debuggen](how-to-debug-visual-studio-code.md).

[Tutorial: Trainieren Ihres ersten ML-Modells](tutorial-1st-experiment-sdk-train.md) zeigt, wie eine Compute-Instanz mit einem integrierten Notebook verwendet wird.