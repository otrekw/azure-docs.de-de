---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: a4090172e926f21db01a8e374d8c4bb1c80402c7
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74197361"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Herunterladen und Installieren der Linkerd-Clientbinärdatei „linkerd“

Verwenden Sie in einer Bash-basierten Shell unter macOS `curl` wie folgt, um das Linkerd-Release herunterzuladen:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-darwin"
```

Die Clientbinärdatei `linkerd` wird auf dem Clientcomputer ausgeführt und ermöglicht die Interaktion mit dem Linkerd-Dienstnetz. Verwenden Sie die folgenden Befehle, um die Linkerd-Clientbinärdatei `linkerd` in einer Bash-basierten Shell unter macOS zu installieren. Diese Befehle kopieren die `linkerd`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-darwin /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Wenn Sie die Befehlszeilen-Vervollständigung für die Linkerd-Clientbinärdatei `linkerd` nutzen möchten, können Sie dies wie folgt einrichten:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```
