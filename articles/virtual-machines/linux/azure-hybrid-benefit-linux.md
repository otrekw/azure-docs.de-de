---
title: Azure-Hybridvorteil und virtuelle Linux-Computer
description: Mit dem Azure-Hybridvorteil können Sie bei virtuellen Linux-Computern, die in Azure ausgeführt werden, Geld sparen.
services: virtual-machines
documentationcenter: ''
author: mathapli
manager: westonh
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/22/2020
ms.author: alsin
ms.openlocfilehash: d62eaf96354627e0c1e4e0a31bb16fb3265f66ac
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91279772"
---
# <a name="preview-azure-hybrid-benefit--how-it-applies-for-linux-virtual-machines"></a>Vorschau: Azure-Hybridvorteil – Anwendung bei virtuelle Linux-Computern

## <a name="overview"></a>Übersicht

Mit dem Azure-Hybridvorteil können Sie die virtuellen Computer (VMs) Ihrer lokalen RHEL- (Red Hat Enterprise Linux) und SLES-Server (SUSE Linux Enterprise Server) ganz einfach zu Azure migrieren, indem Sie Ihr eigenes, bereits vorhandenes Red Hat- oder SUSE-Softwareabonnement verwenden. Dank diesem Vorteil bezahlen Sie lediglich die Infrastrukturkosten Ihres virtuellen Computers, da die Softwaregebühr durch Ihr RHEL- bzw. SLES-Abonnement abgedeckt ist. Der Vorteil gilt für alle RHEL und SLES Marketplace-Images mit nutzungsbasierter Bezahlung.

> [!IMPORTANT]
> Der Azure-Hybridvorteil für Linux-VMs befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="benefit-description"></a>Beschreibung des Vorteils

Mit dem Azure-Hybridvorteil können Sie Ihre lokalen RHEL- und SLES-Server ganz einfach zu Azure migrieren, indem Sie die nutzungsbasierte Bezahlung (PAYG, Pay As You Go) von vorhandenen RHEL- und SLES-VMs in Azure in eine BYOS-Abrechnung (Bring Your Own Subscription) umwandeln. In der Regel wird für virtuelle Computer, die über PAYG-Images in Azure bereitgestellt werden, nicht nur eine Infrastrukturgebühr, sondern auch eine Softwaregebühr berechnet. Mit dem Azure-Hybridvorteil können virtuelle Computer mit nutzungsbasierter Bezahlung ohne erneute Bereitstellung auf ein BYOS-Abrechnungsmodell umgestellt werden, wodurch das Risiko eines Ausfalls vermieden wird.

:::image type="content" source="./media/ahb-linux/azure-hybrid-benefit-cost.png" alt-text="Darstellung der Kosten für den Azure-Hybridvorteil auf virtuellen Linux-Computern.":::

Wenn Sie den Vorteil für einen virtuellen RHEL- oder SLES-Computer aktivieren, wird Ihnen keine zusätzliche Softwaregebühr berechnet, die normalerweise für einen virtuellen PAYG-Computer anfällt. Stattdessen wird für Ihren virtuellen Computer eine BYOS-Gebühr berechnet, die nur die Hardwaregebühr, jedoch keine Softwaregebühr beinhaltet.

Wenn Sie möchten, können Sie einen virtuellen Computer, für den der Vorteil aktiviert wurde, wieder auf ein PAYG-Abrechnungsmodell umstellen.

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux-vms"></a>Umfang der Azure-Hybridvorteilsberechtigung für virtuelle Linux-Computer

Der Azure-Hybridvorteil ist für alle RHEL und SLES Marketplace-Images mit nutzungsbasierte Bezahlung verfügbar. Für RHEL oder SLES Marketplace-BYOS-Images oder benutzerdefinierte Images ist der Vorteil noch nicht verfügbar.

Die Hybridvorteile von reservierten Instanzen, Dedicated Hosts und SQL berechtigen nicht zum Azure-Hybridvorteil, wenn Sie bereits den Vorteil für virtuellen Linux-Computer nutzen.

## <a name="how-to-get-started"></a>Erste Schritte

Der Azure-Hybridvorteil befindet sich derzeit für virtuelle Linux-Computer in einer Vorschauphase. Sobald Sie Zugriff auf die Vorschauversion haben, können Sie den Vorteil über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle aktivieren.

### <a name="preview"></a>Vorschau

In dieser Phase können Sie den Vorteil nutzen, indem Sie das Formular [hier](https://aka.ms/ahb-linux-form) ausfüllen. Nachdem Sie das Formular ausgefüllt haben, werden Ihre Azure-Abonnements für den Vorteil aktiviert und Sie erhalten innerhalb von drei Werktagen eine Bestätigung von Microsoft.

### <a name="red-hat-customers"></a>Red Hat-Kunden

1.    Füllen Sie das oben erwähnte Formular zur Beantragung der Vorschauversion aus.
1.    Registrieren Sie sich beim [Red Hat Cloud Access-Programm](https://aka.ms/rhel-cloud-access).
1.    Aktivieren Sie für Ihre Azure-Abonnements den Cloudzugriff, und aktivieren Sie die Abonnements für die virtuellen Computer, die Sie nutzen möchten.
1.    Wenden Sie den Vorteil über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle auf Ihre vorhandenen virtuellen Computer an.
1.    Registrieren Sie die virtuellen Computer, auf die der Vorteil angewendet wird, bei einer separaten Quelle für Updates.

### <a name="suse-customers"></a>SUSE-Kunden

1.    Füllen Sie das oben erwähnte Formular zur Beantragung der Vorschauversion aus.
1.    Registrieren Sie sich beim SUSE Public Cloud-Programm.
1.    Wenden Sie den Vorteil über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle auf Ihre vorhandenen virtuellen Computer an.
1.    Registrieren Sie die virtuellen Computer, auf die der Vorteil angewendet wird, bei einer separaten Quelle für Updates.

### <a name="enable-and-disable-the-benefit-in-the-azure-portal"></a>Aktivieren und Deaktivieren des Vorteils im Azure-Portal

Sie können den Vorteil für vorhandene virtuelle Computer aktivieren, indem Sie den Anweisungen auf dem Blatt **Konfiguration** befolgen. Sie können den Vorteil auf neuen virtuellen Computern beim Erstellen des virtuellen Computers aktivieren.

### <a name="enable-and-disable-the-benefit-in-the-azure-cli"></a>Aktivieren und Deaktivieren des Vorteils über die Azure-Befehlszeilenschnittstelle

Mit dem Befehl „az vm update“ können Sie vorhandene virtuelle Computer aktualisieren. Führen Sie bei virtuellen RHEL-Computern den Befehl mit einem „--license-type“-Parameter von „RHEL_BYOS“ aus. Führen Sie bei virtuellen SLES-Computern den Befehl mit einem „--license-type“-Parameter von „SLES_BYOS“ aus.

#### <a name="cli-example-to-enable-the-benefit"></a>Beispiel für die Aktivierung des Vorteils über die Befehlszeilenschnittstelle:
```azurecli
# This will enable the benefit on a RHEL VM
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS

# This will enable the benefit on a SLES VM
az vm update -g myResourceGroup -n myVmName --license-type SLES_BYOS
```
#### <a name="cli-example-to-disable-the-benefit"></a>Beispiel für die Deaktivierung des Vorteils über die Befehlszeilenschnittstelle:
Verwenden Sie zum Deaktivieren des Vorteils den „license-type“-Wert „None“.
```azurecli
# This will disable the benefit on a VM
az vm update -g myResourceGroup -n myVmName --license-type None
```

#### <a name="cli-example-to-enable-the-benefit-on-a-large-number-of-vms"></a>Beispiel für die Aktivierung des Vorteils für eine Vielzahl von virtuellen Computern über die Befehlszeilenschnittstelle
Zum Aktivieren des Vorteils für eine Vielzahl von virtuellen Computern können Sie den `--ids`-Parameter in der Azure-Befehlszeilenschnittstelle verwenden.

```azurecli
# This will enable the benefit on a RHEL VM. In this example, ids.txt is an
# existing text file containing a delimited list of resource IDs corresponding
# to the VMs using the benefit
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS --ids $(cat ids.txt)
```

Anhand der folgenden Beispiele werden zwei Methoden zum Abrufen einer Liste mit Ressourcen-IDs vorgestellt: eine auf der Ressourcengruppenebene, die andere auf der Abonnementebene.
```azurecli
# To get a list of all the resource IDs in a resource group:
$(az vm list -g MyResourceGroup --query "[].id" -o tsv)

# To get a list of all the resource IDs of VMs in a subscription:
az vm list -o json | jq '.[] | {VMName: .name, ResourceID: .id}'
```

## <a name="check-ahb-status-of-a-vm"></a>Prüfen des AHB-Status eines virtuellen Computers
Es gibt drei Möglichkeiten, den AHB-Status (Azure Hybrid Benefit, Azure-Hybridvorteil) anzuzeigen: über das Portal, über die Azure-Befehlszeilenschnittstelle und mithilfe von Azure Instance Metadata Service (Azure IMDS).


### <a name="portal"></a>Portal

Wenn Sie feststellen möchten, ob für Ihren virtuellen Computer AHB aktiviert ist, rufen Sie das Blatt „Konfiguration“ auf und zeigen den Lizenzierungsstatus an.

### <a name="azure-cli"></a>Azure CLI

Hierfür können Sie den Befehl `az vm get-instance-view` verwenden. Suchen Sie in der Antwort nach einem „licenseType“-Feld. Wenn das „licenseType“-Feld vorhanden ist und der Wert „RHEL_BYOS“ oder „SLES_BYOS“ lautet, ist für Ihren virtuellen Computer der Vorteil aktiviert.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Azure-Instanzmetadatendienst

Über den virtuellen Computer selbst können Sie die bestätigten IMDS-Metadaten abfragen, um den Wert für „licenseType“ für Ihren virtuellen Computer zu ermitteln. Wenn der „licenseType“-Wert „RHEL_BYOS“ oder „SLES_BYOS“ lautet, ist der Vorteil für Ihren virtuellen Computer aktiviert. Weitere Informationen zu bestätigten Metadaten finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/linux/instance-metadata-service#attested-data).

## <a name="compliance"></a>Compliance

### <a name="red-hat"></a>Red Hat

Wenn Sie den Azure-Hybridvorteil für Ihre virtuellen RHEL-Computer verwenden möchten, müssen Sie sich zunächst beim Red Hat Cloud Access-Programm registrieren. Die Registrierung können Sie über die Red Hat Cloud Access-Website hier durchführen. Nachdem Sie den Vorteil für Ihren virtuellen Computer aktiviert haben, müssen Sie den virtuellen Computer bei Ihrer eigenen Quelle für Updates entweder bei Red Hat Subscription Manager oder bei Red Hat Satellite registrieren. Durch die Registrierung für Updates wird sichergestellt, dass Sie in einem unterstützten Zustand bleiben.

### <a name="suse"></a>SUSE

Wenn Sie den Azure-Hybridvorteil für Ihre virtuellen SLES-Computer verwenden möchten, müssen Sie sich zunächst beim SUSE Public Cloud-Programm registrieren. Weitere Informationen zum Programm finden Sie hier. Nachdem Sie SUSE-Abonnements erworben haben, müssen Sie Ihre virtuellen Computer mit diesen Abonnements bei Ihrer eigenen Quelle für Updates über SUSE Customer Center, den Subscription Management Tool-Server oder SUSE Manager registrieren.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
*F: Kann ich den Lizenztyp „RHEL_BYOS“ mit einem SLES-Image oder umgekehrt verwenden?*

A: Nein, das ist nicht möglich. Wenn Sie versuchen, einen Lizenztyp einzugeben, der nicht mit der auf Ihrem virtuellen Computer ausgeführten Distribution übereinstimmt, werden die Metadaten für die Abrechnung nicht aktualisiert. Wenn Sie jedoch versehentlich den falschen Lizenztyp eingeben und Ihren virtuellen Computer dann noch mal mit dem richtigen Lizenztyp aktualisieren, wird der Vorteil dagegen aktiviert.

*F: Ich habe mich bei Red Hat Cloud Access registriert, kann aber den Vorteil auf meinen virtuellen RHEL-Computern dennoch nicht aktivieren. Wie gehe ich vor?*

A: Es kann einige Zeit in Anspruch nehmen, bis die Registrierung Ihres Red Hat Cloud Access-Abonnements von Red Hat an Azure weitergegeben wird. Wenn der Fehler nach einem Werktag weiterhin auftritt, wenden Sie sich an den Microsoft Support.

## <a name="common-errors"></a>Häufige Fehler
Dieser Abschnitt enthält eine Liste mit häufigen Fehlern und Anweisungen zur Problemlösung.

| Fehler | Minderung |
| ----- | ---------- |
| „Das Abonnement wurde nicht für die Linux-Vorschauversion des Azure-Hybridvorteils registriert. Schritt-für-Schritt-Anleitungen finden Sie unter https://aka.ms/ahb-linux.“ | Füllen Sie das Formular unter https://aka.ms/ahb-linux-form aus, um sich für die Linux-Vorschauversion des Azure-Hybridvorteils zu registrieren.
| „Die Aktion konnte nicht durchgeführt werden, da Red Hat Cloud Access laut unseren Datensätzen nicht für Ihr Azure-Abonnement aktiviert wurde.“ | Um den Vorteil mit virtuellen RHEL-Computern zu nutzen, müssen Sie zunächst Ihre Azure-Abonnements bei Red Hat Cloud Access registrieren. Weitere Informationen zum Registrieren von Azure-Abonnements für Red Hat Cloud Access finden Sie unter diesem Link.

## <a name="next-steps"></a>Nächste Schritte
* Unternehmen Sie die ersten Schritte mit der Vorschauversion, indem Sie das Formular [hier](https://aka.ms/ahb-linux-form) ausfüllen.
