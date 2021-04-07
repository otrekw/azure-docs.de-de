---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 1a023475de1ce2891916807632d9ee15e382326c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "81735484"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Herunterladen und Installieren der Linkerd-Clientbinärdatei „linkerd“

Verwenden Sie in einer PowerShell-basierten Shell unter Windows `Invoke-WebRequest` wie folgt, um das Linkerd-Release herunterzuladen:

```powershell
# Specify the Linkerd version that will be leveraged throughout these instructions
$LINKERD_VERSION="stable-2.6.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-windows.exe" -OutFile "linkerd2-cli-$LINKERD_VERSION-windows.exe"
```

Die Clientbinärdatei `linkerd` wird auf dem Clientcomputer ausgeführt und ermöglicht die Interaktion mit dem Linkerd-Dienstnetz. Verwenden Sie die folgenden Befehle, um die Linkerd-Clientbinärdatei `linkerd` in einer PowerShell-basierten Shell unter Windows zu installieren. Mit diesen Befehlen wird die `linkerd`-Clientbinärdatei in einen Linkerd-Ordner kopiert und dann sowohl sofort (in der aktuellen Shell) als auch dauerhaft (über Neustarts der Shell) über Ihren `PATH` verfügbar gemacht. Sie benötigen keine erhöhten Rechte (Administrator), um diese Befehle auszuführen, und Sie müssen Ihre Shell nicht neu starten.

```powershell
# Copy linkerd.exe to C:\Linkerd
New-Item -ItemType Directory -Force -Path "C:\Linkerd"
Copy-Item -Path ".\linkerd2-cli-$LINKERD_VERSION-windows.exe" -Destination "C:\Linkerd\linkerd.exe"

# Add C:\Linkerd to PATH. 
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH", "User") + ";C:\Linkerd\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\Linkerd\"
```