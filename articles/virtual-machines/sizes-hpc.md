---
title: 'Azure-VM-Größen: HPC | Microsoft-Dokumentation'
description: Liste der verschiedenen verfügbaren Größen für virtuelle HPC-Computer (High Performance Computing) in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: amverma
ms.reviewer: jonbeck
ms.openlocfilehash: b900a95df00ccdd0ad9b5bee3887364195c7d1c2
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78226725"
---
# <a name="high-performance-compute-vm-sizes"></a>Größen von virtuellen HPC-Computern (High Performance Computing)

Virtuelle Azure-Computer (Azure-VMs) der H-Serie bieten eine herausragende Leistung, MPI-Skalierbarkeit und Kosteneffizienz für verschiedene HPC-Workloads in der Praxis.

VMs der [HBv2-Serie](hbv2-series.md) unterstützen Mellanox HDR InfiniBand mit 200 GBit/s, und VMs der Serien HB und HC unterstützen Mellanox EDR InfiniBand mit 100 GBit/s. Jeder dieser VM-Typen ist für eine optimierte und konsistente RDMA-Leistung in einer FAT-Struktur ohne Blocks verbunden. HBv2-VMs unterstützen adaptives Routing und DCT (Dynamic Connected Transport, zusätzlich zum standardmäßigen RC- und UD-Transport). Diese Features verbessern die Anwendungsleistung, Skalierbarkeit und Konsistenz, und ihre Verwendung wird dringend empfohlen.

VMs der [HB-Serie](hb-series.md) sind für Anwendungen mit hohen Anforderungen an die Speicherbandbreite optimiert, z. B. Strömungssimulationen, explizite Finite-Elemente-Analysen oder Wettermodelle. HB-VMs bieten 60 AMD-Prozessorkerne des Modells EPYC 7551, 4 GB RAM pro CPU-Kern und kein Hyperthreading. Die AMD EPYC-Plattform bietet eine Speicherbandbreite von mehr als 260 GB/Sekunde.

VMs der [HC-Serie](hc-series.md) sind für Anwendungen mit hohen Anforderungen an die Rechenleistung optimiert, z. B. implizite Finite-Elemente-Analysen, Molekulardynamik oder Chemoinformatik. HC-VMs bieten 44 Intel-Prozessorkerne des Modells Xeon Platinum 8168, 8 GB RAM pro CPU-Kern und kein Hyperthreading. Die Intel Xeon Platinum-Plattform unterstützt Intels umfangreiches Ökosystem von Softwaretools, etwa die Intel Math Kernel Library.

VMs der [H-Serie](h-series.md) sind für Anwendungen optimiert, für die Anforderungen hinsichtlich hoher CPU-Frequenzen oder großem Arbeitsspeicher pro Kern gelten. Virtuelle Computer der H-Serie bieten 8 oder 16 Intel-Prozessorkerne des Modells Xeon E5 2667 v3, 7 oder 14 GB RAM pro CPU-Kern und kein Hyperthreading. Die H-Serie unterstützt Mellanox EDR InfiniBand mit 56 GBit/s in einer FAT-Strukturkonfiguration ohne Blocks für einheitliche RDMA-Leistung. Virtuelle Computer der H-Serie unterstützen Intel MPI 5.x und MS-MPI.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

- **Azure-Abonnement:** Um eine größere Anzahl von rechenintensiven Instanzen bereitzustellen, sollten Sie ein Abonnement mit nutzungsbasierter Bezahlung oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.

- **Preise und Verfügbarkeit:** Diese VM-Größen werden nur im Tarif „Standard“ angeboten. Informationen zur Verfügbarkeit in den Azure-Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/) .
- **Kontingent Speicherkerne:** Es kann sein, dass Sie das Kontingent für die Speicherkerne in Ihrem Azure-Abonnement anpassen müssen, indem Sie den Standardwert erhöhen. Möglicherweise ist bei Ihrem Abonnement auch die Anzahl von Kernen beschränkt, die in bestimmten VM-Größenkategorien bereitgestellt werden können. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. (Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.)

  > [!NOTE]
  > Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support. Azure-Kontingente sind Angebotsbeschränkungen, keine Kapazitätsgarantien. Unabhängig von Ihrem Kontingent werden nur die tatsächlich verwendeten Kerne in Rechnung gestellt.
  
- **Virtuelles Netzwerk:** Ein [virtuelles Azure-Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) ist nicht erforderlich, um die rechenintensiven Instanzen zu verwenden. Für viele Bereitstellungen benötigen Sie jedoch mindestens ein cloudbasiertes virtuelles Azure-Netzwerk oder auch eine Site-to-Site-Verbindung für den Zugriff auf lokale Ressourcen. Erstellen Sie ggf. ein neues virtuelles Netzwerk zum Bereitstellen der Instanzen. Das Hinzufügen rechenintensiver virtueller Computer zu einem virtuellen Netzwerk in einer Affinitätsgruppe wird nicht unterstützt.
- **Größenanpassung:** Aufgrund der speziellen Hardware können Sie die Größe rechenintensiver Instanzen nur innerhalb der gleichen Größenfamilie (H-Serie oder rechenintensive A-Serie) anpassen. So können Sie beispielsweise die Größe eines virtuellen Computers der H-Serie nur auf eine andere Größe der H-Serie festlegen. Wechsel von einer nicht rechenintensiven Größe zu einer rechenintensiven Größe werden nicht unterstützt.  

> [!NOTE]
> Die Außerbetriebnahme von A8- bis A11-VMs ist für März 2021 geplant. Weitere Informationen finden Sie im [HPC-Migrationsleitfaden](https://azure.microsoft.com/resources/hpc-migration-guide/).

## <a name="rdma-capable-instances"></a>RDMA-fähige Instanzen

Eine Teilmenge der rechenintensiven Instanzen (A8, A9, H16r, H16mr, HB und HC) verfügt über eine Netzwerkschnittstelle für RDMA-Verbindungen (Remote Direct Memory Access). Ausgewählte N-Seriengrößen mit dem Zusatz „r“, z. B. die NC24rs-Konfigurationen (NC24rs_v2 und NC24rs_v3), sind ebenfalls RDMA-fähig. Diese Schnittstelle steht zusätzlich zur standardmäßigen Azure-Netzwerkschnittstelle anderer VM-Größen zur Verfügung.

Mithilfe dieser Schnittstelle können die RDMA-fähigen Instanzen über ein InfiniBand-Netzwerk (IB) kommunizieren, das mit EDR-Raten für virtuelle Computer der Größen HB und HC sowie mit FDR-Raten für virtuelle Computer der Größen H16r und H16mr sowie für RDMA-fähige virtuelle Computer der N-Serie und mit QDR-Raten für virtuelle Computer der Größe A8 und A9 betrieben wird. Mit diesen RDMA-Funktionen können Skalierbarkeit und Leistung von bestimmten MPI-Anwendungen (Message Passing Interface) gesteigert werden. Weitere Informationen zur Geschwindigkeit finden Sie in den Details in den Tabellen auf dieser Seite.

> [!NOTE]
> In Azure wird „IP over IB“ nur auf SR-IOV-fähigen virtuellen Computern unterstützt (SR-IOV für InfiniBand, derzeit HB und HC). „RDMA over IB“ wird für alle RDMA-fähigen Instanzen unterstützt.
- **Betriebssystem**: Windows Server 2016 auf allen oben genannten virtuellen HPC-Computern. Windows Server 2012 R2 und Windows Server 2012 werden auch auf virtuellen Computern unterstützt, die nicht SR-IOV-fähig sind (und daher nicht zur HB- oder HC-Reihe gehören).

- **MPI**: Die SR-IOV-fähigen VM-Größen (HB, HC) in Azure gestatten die Verwendung nahezu jeder Variante von MPI mit Mellanox OFED.
Auf nicht SR-IOV-fähigen virtuellen Computern verwenden unterstützte MPI-Implementierungen die Microsoft Network Direct-Schnittstelle (ND) für die Kommunikation zwischen Instanzen. Daher werden nur die Versionen Microsoft MPI (MS-MPI) 2012 R2 oder höher und Intel MPI 5.x unterstützt. Höhere Versionen (2017, 2018) der Intel MPI-Laufzeitbibliothek sind möglicherweise mit den Azure-RDMA-Treibern kompatibel.

- **InfiniBandDriverWindows-VM-Erweiterung**: Fügen Sie auf RDMA-fähigen virtuellen Computern die Erweiterung „InfiniBandDriverWindows“ hinzu, um InfiniBand zu aktivieren. Diese Windows-Erweiterung für VMs installiert Windows Network Direct-Treiber (auf VMs ohne SR-IOV) oder Mellanox OFED-Treiber (auf VMs mit SR-IOV), um RDMA-Konnektivität herzustellen.
In bestimmten Bereitstellungen von A8- und A9-Instanzen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt. Beachten Sie, dass die VM-Erweiterung „HpcVmDrivers“ eingestellt wird. Sie wird nicht mehr aktualisiert. Um die VM-Erweiterung einer VM hinzuzufügen, können Sie [Azure PowerShell](/powershell/azure/overview)-Cmdlets verwenden. 

  Der folgende Befehl installiert die neueste Version 1.0 der Erweiterung „InfiniBandDriverWindows“ auf einer vorhandenen, RDMA-fähigen VM mit dem Namen *myVM*, die in der Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Westen* bereitgestellt ist:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  ```

  Alternativ können VM-Erweiterungen zur einfachen Bereitstellung mithilfe des folgenden JSON-Elements in Azure Resource Manager-Vorlagen aufgenommen werden:

  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverWindows",
  "typeHandlerVersion": "1.0",
  } 
  ```

  Durch folgenden Befehl wird die neueste Version (1.0) der Erweiterung „InfiniBandDriverWindows“ auf allen RDMA-fähigen VMs in einer vorhandenen VM-Skalierungsgruppe mit dem Namen *myVMSS* installiert, die in der Ressourcengruppe mit dem Namen *myResourceGroup* bereitgestellt wurde:

  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```

  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer](./extensions/overview.md). Erweiterungen können auch für virtuelle Computer verwendet werden, die mit dem [klassischen Bereitstellungsmodell](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic) bereitgestellt wurden.

- **RDMA-Netzwerkadressbereich:** Für das RDMA-Netzwerk in Azure wird der Adressbereich 172.16.0.0/16 reserviert. Wenn Sie MPI-Anwendungen auf Instanzen ausführen möchten, die in einem virtuellen Azure-Netzwerk bereitgestellt wurden, vergewissern Sie sich, dass der Adressraum des virtuellen Netzwerks sich nicht mit dem RDMA-Netzwerk überschneidet.

## <a name="cluster-configuration-options"></a>Konfigurationsoptionen für Cluster

Azure bietet mehrere Optionen zum Erstellen von Clustern von Windows-HPC-VMs, die über das RDMA-Netzwerk kommunizieren können, einschließlich der folgenden: 

- **Virtuelle Computer**: Stellen Sie die RDMA-fähigen HPC-VMs in der gleichen Verfügbarkeitsgruppe bereit (wenn Sie das Azure Resource Manager-Bereitstellungsmodell verwenden). Stellen Sie die VMs bei Verwendung des klassischen Bereitstellungsmodells im gleichen Clouddienst bereit. 

- **VM-Skalierungsgruppen**: Stellen Sie bei Verwendung einer VM-Skalierungsgruppe sicher, dass Sie die Bereitstellung auf eine einzelne Platzierungsgruppe beschränken. Legen Sie z. B. in einer Resource Manager-Vorlage die Eigenschaft `singlePlacementGroup` auf `true` fest. 

- **MPI zwischen virtuellen Computern**: Wenn MPI-Kommunikation zwischen virtuellen Computern (VMs) erforderlich ist, sollten Sie sicherstellen, dass sich die VMs in derselben Verfügbarkeitsgruppe oder derselben VM-Skalierungsgruppe befinden.

- **Azure CycleCloud**: Erstellen Sie in [Azure CycleCloud](/azure/cyclecloud/) einen HPC-Cluster zum Ausführen von MPI-Aufträgen auf Windows-Knoten.

- **Azure Batch**: Erstellen Sie einen [Azure Batch](/azure/batch/)-Pool zum Ausführen von MPI-Workloads auf Windows Server-Computeknoten. Weitere Informationen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](../batch/batch-pool-compute-intensive-sizes.md). Informationen zum Ausführen containerbasierter Workloads in Batch finden Sie im Projekt [Batch Shipyard](https://github.com/Azure/batch-shipyard).

- **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) enthält eine Laufzeitumgebung für MS-MPI, die das Azure-RDMA-Netzwerk bei der Bereitstellung auf RDMA-fähigen Linux-VMs verwendet. Beispielbereitstellungen finden Sie unter [Einrichten eines Linux-RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

- Prüflisten zur Verwendung rechenintensiver Instanzen mit HPC Pack unter Windows Server finden Sie unter [Einrichten eines Linux-RDMA-Clusters mit HPC Pack zum Ausführen von MPI-Anwendungen](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

- Informationen zur Verwendung rechenintensiver Instanzen zum Ausführen von MPI-Anwendungen mit Azure Batch finden Sie unter [Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch](../batch/batch-mpi.md).

- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.