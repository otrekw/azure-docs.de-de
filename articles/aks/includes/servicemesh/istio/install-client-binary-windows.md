---
author: paulbouwer
ms.topic: include
ms.date: 10/02/2020
ms.author: pabouwer
ms.openlocfilehash: a02c17013a205ccc0da85536b491d467ef72fa48
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91666706"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Herunterladen und Installieren der Istio-Clientbinärdatei „istioctl“

Verwenden Sie in einer PowerShell-basierten Shell unter Windows `Invoke-WebRequest`, um das Istio-Release herunterzuladen, und führen Sie das Extrahieren dann wie folgt mit `Expand-Archive` durch:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.7.3"

[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istioctl-$ISTIO_VERSION-win.zip" -OutFile "istioctl-$ISTIO_VERSION.zip"
Expand-Archive -Path "istioctl-$ISTIO_VERSION.zip" -DestinationPath .
```

Die Clientbinärdatei `istioctl` wird auf Ihrem Clientcomputer ausgeführt und ermöglicht Ihnen die Installation und Verwaltung von Istio in Ihrem AKS-Cluster. Verwenden Sie die folgenden Befehle, um die Istio-Clientbinärdatei `istioctl` in einer PowerShell-basierten Shell unter Windows zu installieren. Mit diesen Befehlen wird die `istioctl`-Clientbinärdatei in einen Ordner „Istio“ kopiert und dann sowohl sofort (in der aktuellen Shell) als auch dauerhaft (über Neustarts der Shell) über Ihren `PATH` verfügbar gemacht. Sie benötigen keine erhöhten Rechte (Administrator), um diese Befehle auszuführen, und Sie müssen Ihre Shell nicht neu starten.

```powershell
# Copy istioctl.exe to C:\Istio
New-Item -ItemType Directory -Force -Path "C:\Istio"
Move-Item -Path .\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH", "User") + ";C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\Istio\"
```