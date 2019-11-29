---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: a1a608b4f4e1afe4a3cf99229561a91cc3f9fa96
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74170809"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Herunterladen und Installieren der Istio-Clientbinärdatei „istioctl“

Verwenden Sie in einer PowerShell-basierten Shell unter Windows `Invoke-WebRequest`, um das Istio-Release herunterzuladen, und führen Sie das Extrahieren dann wie folgt mit `Expand-Archive` durch:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.4.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

Die Clientbinärdatei `istioctl` wird auf dem Clientcomputer ausgeführt und ermöglicht die Interaktion mit dem Istio-Dienstmesh. Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer PowerShell-basierten Shell unter Windows zu installieren. Mit diesen Befehlen wird die `istioctl`-Clientbinärdatei in einen Ordner „Istio“ kopiert und dann sowohl sofort (in der aktuellen Shell) als auch dauerhaft (über Neustarts der Shell) über Ihren `PATH` verfügbar gemacht. Sie benötigen keine erhöhten Rechte (Administrator), um diese Befehle auszuführen, und Sie müssen Ihre Shell nicht neu starten.

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```