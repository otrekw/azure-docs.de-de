---
title: Übersicht über GPU-VMs auf Ihrem Azure Stack Edge Pro-Gerät und deren Bereitstellung
description: Erfahren Sie, wie GPU-VMs auf einem Azure Stack Edge Pro-Gerät anhand von Vorlagen erstellt und verwaltet werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: ff805b758dce05a66764ab1ff08e53378c946362
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102438181"
---
# <a name="gpu-vms-for-your-azure-stack-edge-pro-device"></a>GPU-VMs für Ihr Azure Stack Edge Pro-Gerät

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

Dieser Artikel enthält eine Übersicht über GPU-VMs (virtuelle Computer) auf Ihrem Azure Stack Edge Pro-Gerät. In diesem Artikel wird beschrieben, wie Sie eine GPU-VM erstellen und dann die GPU-Treibererweiterung installieren, um die entsprechenden Nvidia-Treiber zu installieren. Sie verwenden dabei Azure Resource Manager-Vorlagen, um die GPU-VM zu erstellen und die GPU-Treibererweiterung zu installieren. 

Die Informationen in diesem Artikel beziehen sich auf Geräte vom Typ „Azure Stack Edge Pro GPU“ und „Azure Stack Edge Pro R“.

## <a name="about-gpu-vms"></a>Informationen zu GPU-VMs

Ihre Azure Stack Edge Pro-Geräte sind mit 1 oder 2 Tesla T4-GPUs von Nvidia ausgestattet. Verwenden Sie für die Bereitstellung von VMs mit GPU-Beschleunigung auf diesen Geräten die GPU-optimierten VM-Größen. Beispielsweise sollte für das Bereitstellen von Rückschlussworkloads mit T4-GPUs die Serie NC T4 v3 verwendet werden. 

Weitere Informationen finden Sie unter [VMs der NC T4 v3-Serie](../virtual-machines/nct4-v3-series.md).

## <a name="supported-os-and-gpu-drivers"></a>Unterstützte Betriebssysteme und GPU-Treiber 

Um die GPU-Funktionen von Azure-VMs der N-Serie nutzen zu können, müssen Nvidia-GPU-Treiber installiert werden. 

Mit der Nvidia-GPU-Treibererweiterung werden die entsprechende Nvidia-CUDA- oder -GRID-Treiber installiert. Sie können die Erweiterung mithilfe von Azure Resource Manager-Vorlagen installieren und verwalten.

### <a name="supported-os-for-gpu-extension-for-windows"></a>Von der GPU-Erweiterung für Windows unterstützte Betriebssysteme

Diese Erweiterung unterstützt die folgenden Betriebssysteme. Andere Versionen funktionieren zwar möglicherweise ebenfalls, wurden jedoch nicht intern auf GPU-VMs getestet, die auf Azure Stack Edge Pro-Geräten ausgeführt werden.

| Distribution | Version |
|---|---|
| Windows Server 2019 | Core |
| Windows Server 2016 | Core |

### <a name="supported-os-for-gpu-extension-for-linux"></a>Von der GPU-Erweiterung für Linux unterstützte Betriebssysteme

Diese Erweiterung unterstützt die folgenden Betriebssystem-Distributionen, abhängig von der Treiberunterstützung für bestimmte Betriebssystemversionen. Andere Versionen funktionieren zwar möglicherweise ebenfalls, wurden jedoch nicht intern auf GPU-VMs getestet, die auf Azure Stack Edge Pro-Geräten ausgeführt werden.


| Distribution | Version |
|---|---|
| Ubuntu | 18.04 LTS |
| Red Hat Enterprise Linux | 7.4 |


## <a name="gpu-vms-and-kubernetes"></a>GPU-VMs und Kubernetes

Beachten Sie vor der Bereitstellung von GPU-VMs auf Ihrem Gerät die folgenden Überlegungen, wenn darauf Kubernetes konfiguriert ist.

#### <a name="for-1-gpu-device"></a>Für Geräte mit einer GPU: 

- **Sie erstellen zuerst eine GPU-VM auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario werden sowohl die GPU-VM-Erstellung als auch die Kubernetes-Konfiguration erfolgreich abgeschlossen. Kubernetes hat in diesem Fall keinen Zugriff auf die GPU.

- **Sie konfigurieren zuerst Kubernetes auf Ihrem Gerät und erstellen danach eine GPU-VM:** In diesem Szenario beansprucht Kubernetes die GPU auf Ihrem Gerät, sodass die VM-Erstellung zu einem Fehler führt, da keine GPU-Ressourcen verfügbar sind.

#### <a name="for-2-gpu-device"></a>Für Geräte mit zwei GPUs:

- **Sie erstellen zuerst eine GPU-VM auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario beansprucht die erstellte GPU-VM eine GPU auf Ihrem Gerät, und die Kubernetes-Konfiguration ist ebenfalls erfolgreich und beansprucht die verbleibende GPU. 

- **Sie erstellen zuerst zwei GPU-VMs auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario beanspruchen die beiden GPU-VMs die beiden GPUs auf dem Gerät. Kubernetes wird zwar erfolgreich konfiguriert, kann aber keine GPUs nutzen. 

- **Sie konfigurieren zuerst Kubernetes auf Ihrem Gerät und erstellen danach eine GPU-VM:** In diesem Szenario beansprucht Kubernetes beide GPUs auf Ihrem Gerät, sodass die VM-Erstellung zu einem Fehler führt, da keine GPU-Ressourcen mehr verfügbar sind.

Wenn auf Ihrem Gerät GPU-VMs ausgeführt werden und Kubernetes ebenfalls konfiguriert ist, besteht jedes Mal, wenn die Zuordnung der VMs aufgehoben wird (Sie beenden eine VM mithilfe von Stop-AzureRmVM oder entfernen sie mit Remove-AzureRmVM), die Gefahr, dass der Kubernetes-Cluster alle auf dem Gerät verfügbaren GPUs beansprucht. In diesem Fall können Sie die auf Ihrem Gerät bereitgestellten GPU-VMs nicht neu starten und auch keine GPU-VMs erstellen.


## <a name="create-gpu-vms"></a>Erstellen von GPU-VMs

Führen Sie die folgenden Schritte aus, um auf Ihrem Gerät GPU-VMs bereitzustellen:

1. Ermitteln Sie, ob auf Ihrem Gerät auch Kubernetes ausgeführt wird. Wenn auf dem Gerät Kubernetes ausgeführt wird, müssen Sie zuerst die GPU-VM erstellen und erst danach Kubernetes konfigurieren. Wenn Kubernetes zuerst konfiguriert wird, werden alle verfügbaren GPU-Ressourcen beansprucht, und die Erstellung der GPU-VM führt zu einem Fehler.

1. [Laden Sie die VM-Vorlagen und Parameterdateien](https://aka.ms/ase-vm-templates) auf Ihrem Clientcomputer herunter. Entpacken Sie die Vorlagen und Dateien in einem Verzeichnis, das Sie als Arbeitsverzeichnis verwenden.

1. Führen Sie zum Erstellen von GPU-VMs alle Schritte unter [Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-Gerät mithilfe von Vorlagen](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) aus – mit Ausnahme der folgenden Abweichungen: 

    1. Aktivieren Sie beim Konfigurieren des Computenetzwerks den Port, der mit dem Internet verbunden ist, für Compute. Damit können Sie die für GPU-Erweiterungen erforderlichen GPU-Treiber für Ihre GPU-VMs herunterladen.

        Im folgenden Beispiel ist Port 2 mit dem Internet verbunden und wird zum Aktivieren des Computenetzwerks verwendet. Wenn Sie im vorausgehenden Schritt festgestellt haben, dass Kubernetes nicht benötigt wird, können Sie die IP-Adresse des Kubernetes-Knotens und die IP-Zuweisung für externe Dienste überspringen.

        ![Aktivieren von Computeeinstellungen für den mit dem Internet verbundenen Port](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/enable-compute-network-1.png)

            
    1. Erstellen Sie mithilfe der Vorlagen eine VM. Verwenden Sie bei der Angabe der GPU-VM-Größen die NCasT4-v3-Serie in der Datei `CreateVM.parameters.json`, da diese für GPU-VMs unterstützt wird. Weitere Informationen finden Sie unter [Unterstützte VM-Größen für GPU-VMs](azure-stack-edge-gpu-virtual-machine-sizes.md#ncast4_v3-series-preview).

        ```json
            "vmSize": {
          "value": "Standard_NC4as_T4_v3"
        },
        ```

    1. Nachdem die GPU-VM erstellt wurde, können Sie sie in der Liste der VMs in Ihrer Azure Stack Edge-Ressource im Azure-Portal anzeigen.

        ![GPU-VM in der Liste der VMs im Azure-Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/list-virtual-machine-1.png)

1. Wählen Sie die VM aus, und zeigen Sie die Detailinformationen an. Kopieren Sie die der VM zugewiesene IP-Adresse.

    ![Die der GPU-VM zugewiesene IP-Adresse im Azure-Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/get-ip-gpu-virtual-machine-1.png)

1. Nachdem die VM erstellt wurde, stellen Sie die GPU-Erweiterung mithilfe der Erweiterungsvorlage bereit. Informationen für Linux-VMs finden Sie unter [Installieren der GPU-Erweiterung für Linux](#gpu-extension-for-linux) und für Windows-VMs unter [Installieren der GPU-Erweiterung für Windows](#gpu-extension-for-windows).

1. Stellen Sie zum Überprüfen der Installation der GPU-Erweiterung eine Verbindung mit der GPU-VM her:
    1. Wenn Sie eine Windows-VM verwenden, befolgen Sie die Schritte unter [Herstellen einer Verbindung mit einer Windows-VM](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-a-windows-vm). [Überprüfen Sie die Installation.](#verify-windows-driver-installation)
    1. Wenn Sie eine Linux-VM verwenden, befolgen Sie die Schritte unter [Herstellen einer Verbindung mit einer Linux-VM](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-a-linux-vm). [Überprüfen Sie die Installation.](#verify-linux-driver-installation)

1. Bei Bedarf können Sie das Computenetzwerk wieder auf die gewünschte Konfiguration umstellen. 


> [!NOTE]
> Wenn Sie die Softwareversion Ihres Geräts von 2012 auf eine höhere Version aktualisieren, müssen Sie die GPU-VMs manuell beenden.


## <a name="install-gpu-extension"></a>Installieren der GPU-Erweiterung

Je nach Betriebssystem Ihrer VM können Sie die GPU-Erweiterung für Windows oder für Linux installieren.

> [!NOTE]
> Stellen Sie vor dem Installieren der GPU-Erweiterung sicher, dass der für das Computenetzwerk auf Ihrem Gerät aktivierte Port mit dem Internet verbunden ist und Zugriff hat. Die GPU-Treiber werden über das Internet heruntergeladen.

### <a name="gpu-extension-for-windows"></a>GPU-Erweiterung für Windows

Um Nvidia-GPU-Treiber für eine vorhandene VM bereitzustellen, bearbeiten Sie die Parameterdatei `addGPUExtWindowsVM.parameters.json` und stellen dann die Vorlage `addGPUextensiontoVM.json` bereit.

#### <a name="edit-parameters-file"></a>Bearbeiten der Parameterdatei

Die Datei `addGPUExtWindowsVM.parameters.json` kann die folgenden Parameter enthalten:

```json
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: windowsGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverWindows" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DriverURL" : "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
    "DriverCertificateUrl" : "https://go.microsoft.com/fwlink/?linkid=871664",
    "DriverType":"CUDA"
    }
    }
    }
```

Im Folgenden finden Sie die Beispielparameterdatei, die in diesem Artikel verwendet wurde:

```powershell
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\CreateVM\CreateVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\CreateVM\CreateVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment2"

DeploymentName          : deployment2
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:02:56 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          adminUsername    String                     Administrator
                          password         String                     Password1
                          imageName        String                     myasewindowsimg
                          vmSize           String                     Standard_NC4as_T4_v3
                          vnetName         String                     ASEVNET
                          vnetRG           String                     aserg
                          subnetName       String                     ASEVNETsubNet
                          nicName          String                     nic6
                          ipConfigName     String                     ipconfig6
                          privateIPAddress  String

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```
#### <a name="deploy-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage `addGPUextensiontoVM.json` bereit. Diese Vorlage stellt die Erweiterung für eine vorhandene VM bereit. Führen Sie den folgenden Befehl aus:

```powershell
$templateFile = "<Path to addGPUextensiontoVM.json>" 
$templateParameterFile = "<Path to addGPUExtWindowsVM.parameters.json>" 
$RGName = "<Name of your resource group>"
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
```
> [!NOTE]
> Die Bereitstellung der Erweiterung ist ein zeitintensiver Auftrag, der etwa 10 Minuten dauert.

Hier ist eine Beispielausgabe:

```powershell
PS C:\WINDOWS\system32> "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtWindowsVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment3"

DeploymentName          : deployment3
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:18:50 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          extensionName    String                     windowsgpuext
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverWindows
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
                          }

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```
#### <a name="track-deployment"></a>Nachverfolgen der Bereitstellung

Führen Sie den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für eine bestimmte VM anzuzeigen: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName <Name of resource group> -VMName <Name of VM> -Name <Name of the extension>
```
Hier ist eine Beispielausgabe:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM2 -Name windowsgpuext

ResourceGroupName       : myasegpuvm1
VMName                  : VM2
Name                    : windowsgpuext
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverWindows
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/myasegpuvm1/providers/Microsoft.Compute/virtualMachines/VM2/extensions/windowsgpuext
PublicSettings          : {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert. Sie können den Installationsstatus in dieser Datei `C:\Packages\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverWindows\1.3.0.0\Status` nachverfolgen. 


Eine erfolgreiche Installation wird durch eine `message` vom Typ `Enable Extension` und einen `status` mit dem Wert `success` angegeben.

```powershell
"status":  {
                       "formattedMessage":  {
                                                "message":  "Enable Extension",
                                                "lang":  "en"
                                            },
                       "name":  "NvidiaGpuDriverWindows",
                       "status":  "success",
```

#### <a name="verify-windows-driver-installation"></a>Überprüfen der Treiberinstallation unter Windows

Melden Sie sich auf der VM an, und führen Sie das mit dem Treiber installierte Befehlszeilen-Hilfsprogramm nvidia-smi aus. Die Datei `nvidia-smi.exe` befindet sich unter `C:\Program Files\NVIDIA Corporation\NVSMI\nvidia-smi.exe`. Wenn die Datei nicht angezeigt wird, läuft die Treiberinstallation möglicherweise immer noch im Hintergrund. Warten Sie 10 Minuten, und überprüfen Sie dann den Status erneut.

Wenn der Treiber installiert wurde, wird eine Ausgabe ähnlich der folgenden angezeigt: 

```powershell
PS C:\Users\Administrator> cd "C:\Program Files\NVIDIA Corporation\NVSMI"
PS C:\Program Files\NVIDIA Corporation\NVSMI> ls

    Directory: C:\Program Files\NVIDIA Corporation\NVSMI

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/26/2020  12:00 PM         849640 MCU.exe
-a----        2/26/2020  12:00 PM         443104 nvdebugdump.exe
-a----        2/25/2020   2:06 AM          81823 nvidia-smi.1.pdf
-a----        2/26/2020  12:01 PM         566880 nvidia-smi.exe
-a----        2/26/2020  12:01 PM         991344 nvml.dll

PS C:\Program Files\NVIDIA Corporation\NVSMI> .\nvidia-smi.exe
Wed Dec 16 00:35:51 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 442.50       Driver Version: 442.50       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla T4            TCC  | 0000503C:00:00.0 Off |                    0 |
| N/A   35C    P8    11W /  70W |      8MiB / 15205MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
PS C:\Program Files\NVIDIA Corporation\NVSMI>
```

Weitere Informationen finden Sie unter [Nvidia-GPU-Treibererweiterung für Windows](../virtual-machines/extensions/hpccompute-gpu-windows.md).

### <a name="gpu-extension-for-linux"></a>GPU-Erweiterung für Linux

Um Nvidia-GPU-Treiber für eine vorhandene VM bereitzustellen, bearbeiten Sie die Parameterdatei und stellen dann die Vorlage `addGPUextensiontoVM.json` bereit. Es gibt spezielle Parameterdateien für Ubuntu und Red Hat Enterprise Linux (RHEL), wie in den folgenden Abschnitten erläutert.

#### <a name="edit-parameters-file"></a>Bearbeiten der Parameterdatei

Wenn Sie Ubuntu verwenden, kann die Datei `addGPUExtLinuxVM.parameters.json` die folgenden Parameter enthalten:

```powershell
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: linuxGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverLinux" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
    "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
    "CUDA_ver": "10.0.130",
    "InstallCUDA": "true"
    }
    }
    }
```
Wenn Sie Red Hat Enterprise Linux (RHEL) verwenden, akzeptiert die Datei `addGPUExtensionRHELVM.parameters.json` die folgenden Parameter:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<name of the VM>" 
        },
        "extensionName": {
            "value": "<name for the extension. Example: linuxGpu>" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
                    "isCustomInstall":true,
                    "DRIVER_URL":"https://go.microsoft.com/fwlink/?linkid=874273",
                    "CUDA_ver":"10.0.130",
                    "PUBKEY_URL":"http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                    "DKMS_URL":"https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm",
                    "LIS_URL":"https://aka.ms/lis",
                    "LIS_RHEL_ver":"3.10.0-1062.9.1.el7"
            }
        }
    }
}
```


Im Folgenden finden Sie die Beispielparameterdatei für Ubuntu, die in diesem Artikel verwendet wurde:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "VM1" 
        },
        "extensionName": {
            "value": "gpuLinux" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
            "CUDA_ver": "10.0.130",
            "InstallCUDA": "true"
            }
        }
    }
}
```


#### <a name="deploy-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage `addGPUextensiontoVM.json` bereit. Diese Vorlage stellt die Erweiterung für eine vorhandene VM bereit. Führen Sie den folgenden Befehl aus:

```powershell
$templateFile = "Path to addGPUextensiontoVM.json" 
$templateParameterFile = "Path to addGPUExtLinuxVM.parameters.json" 
$RGName = "<Name of your resource group>" 
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
``` 

> [!NOTE]
> Die Bereitstellung der Erweiterung ist ein zeitintensiver Auftrag, der etwa 10 Minuten dauert.

Hier ist eine Beispielausgabe:

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtLinuxVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "rg2"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "delpoyment7"

DeploymentName          : delpoyment7
ResourceGroupName       : rg2
ProvisioningState       : Succeeded
Timestamp               : 12/10/2020 10:43:23 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM1
                          extensionName    String                     gpuLinux
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverLinux
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL":
                          "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
                          }

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```

#### <a name="track-deployment-status"></a>Nachverfolgen des Bereitstellungsstatus    
    
Die Vorlagenbereitstellung ist ein zeitintensiver Auftrag. Um den Bereitstellungsstatus von Erweiterungen für eine bestimmte VM zu überprüfen, öffnen Sie eine weitere PowerShell-Sitzung (als Administrator). Führen Sie den folgenden Befehl aus: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName <VM Name> -Name <Extension Name>
```
Hier ist eine Beispielausgabe: 

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName rg2 -VMName VM1 -Name gpulinux

ResourceGroupName       : rg2
VMName                  : VM1
Name                    : gpuLinux
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverLinux
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/rg2/providers/Microsoft.Compute/virtualMachines/VM1/extensions/gpuLinux
PublicSettings          : {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

> [!NOTE]
> Nach Abschluss der Bereitstellung wird der `ProvisioningState` in `Succeeded` geändert.

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert: `/var/log/azure/nvidia-vmext-status`.

#### <a name="verify-linux-driver-installation"></a>Überprüfen der Treiberinstallation unter Linux

Führen Sie diese Schritte aus, um die Treiberinstallation zu überprüfen:

1. Stellen Sie eine Verbindung mit der GPU-VM her. Befolgen Sie die Anweisungen unter [Herstellen einer Verbindung mit einer Linux-VM](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-a-linux-vm). 

    Hier ist eine Beispielausgabe:

    ```powershell
    PS C:\WINDOWS\system32> ssh -l Administrator 10.57.50.60
    Administrator@10.57.50.60's password:
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
      System information as of Thu Dec 10 22:57:01 UTC 2020
    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
    
    249 packages can be updated.
    140 updates are security updates.
    
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage    
      System information as of Thu Dec 10 22:57:01 UTC 2020    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
        
    249 packages can be updated.
    140 updates are security updates.
    
    New release '20.04.1 LTS' available.
    Run 'do-release-upgrade' to upgrade to it.
        
    *** System restart required ***
    Last login: Thu Dec 10 21:49:29 2020 from 10.90.24.23
    To run a command as administrator (user "root"), use "sudo <command>".
    See "man sudo_root" for details.
    
    Administrator@VM1:~$
    ```
2. Führen Sie das mit dem Treiber installierte Befehlszeilen-Hilfsprogramm nvidia-smi aus. Wenn der Treiber erfolgreich installiert wurde, können Sie das Hilfsprogramm ausführen, das die folgende Ausgabe anzeigt:

    ```powershell
    Administrator@VM1:~$ nvidia-smi
    Thu Dec 10 22:58:46 2020
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            Off  | 0000941F:00:00.0 Off |                    0 |
    | N/A   48C    P0    27W /  70W |      0MiB / 15109MiB |      5%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    Administrator@VM1:~$
    ```

Weitere Informationen finden Sie unter [Nvidia-GPU-Treibererweiterung für Linux](../virtual-machines/extensions/hpccompute-gpu-linux.md).

## <a name="remove-gpu-extension"></a>Entfernen der GPU-Erweiterung

Verwenden Sie zum Entfernen der GPU-Erweiterung den folgenden Befehl:

`Remove-AzureRmVMExtension -ResourceGroupName <Resource group name> -VMName <VM name> -Name <Extension name>`

Hier ist eine Beispielausgabe:

```powershell
PS C:\azure-stack-edge-deploy-vms> Remove-AzureRmVMExtension -ResourceGroupName rgl -VMName WindowsVM -Name windowsgpuext
Virtual machine extension removal operation
This cmdlet will remove the specified virtual machine extension. Do you want to continue? [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): y
Requestld IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------    
          True                OK         OK
```




## <a name="next-steps"></a>Nächste Schritte

[Azure Resource Manager-Cmdlets](/powershell/module/azurerm.resources/?view=azurermps-6.13.0&preserve-view=true)