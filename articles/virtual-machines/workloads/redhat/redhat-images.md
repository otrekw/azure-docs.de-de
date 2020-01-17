---
title: Red Hat Enterprise Linux-Images in Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über Red Hat Enterprise Linux-Images in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 211ac68fd10cd745faf68a5efae7392345008d7b
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941445"
---
# <a name="overview-of-red-hat-enterprise-linux-images"></a>Übersicht über Red Hat Enterprise Linux-Images
Dieser Artikel beschreibt die verfügbaren Red Hat Enterprise Linux-Images (RHEL) im Azure Marketplace sowie Richtlinien für die Benennung und Aufbewahrung.

Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata). Ausführliche Informationen zu Preisen finden Sie auf der Seite [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

>[!IMPORTANT]
> Für RHEL-Images, die derzeit auf dem Azure-Marketplace verfügbar sind, wird entweder das Lizenzierungsmodell Bring-Your-Own-Subscription (BYOS) oder die nutzungsbasierte Zahlung (Pay-As-You-Go, PAYG) unterstützt. Der [Azure-Vorteil bei Hybridnutzung](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) und das dynamische Wechseln zwischen BYOS und PAYG wird nicht unterstützt. Für einen Wechsel des Lizenzierungsmodus ist eine erneute Bereitstellung der VM über das entsprechende Image erforderlich.

>[!NOTE]
> Erstellen Sie ein Supportticket bei Microsoft, wenn bei Ihnen im Azure Marketplace ein Problem mit RHEL-Images auftritt.

## <a name="images-available-in-azure"></a>In Azure verfügbare Images
Wenn Sie im Marketplace nach „Red Hat“ suchen oder eine Ressource auf der Benutzeroberfläche des Azure-Portals erstellen, wird eine Teilmenge der verfügbaren RHEL-Images angezeigt. Sie können immer den vollständigen Satz der verfügbaren VM-Images mithilfe der Azure-Befehlszeilenschnittstelle, von PowerShell oder der API abrufen.

Führen Sie den folgenden Befehl aus, um den vollständigen Satz von verfügbaren Red Hat-Images in Azure anzuzeigen.

```azurecli-interactive
az vm image list --publisher RedHat --all
```

### <a name="naming-convention"></a>Benennungskonvention
VM-Images in Azure sind nach Herausgeber, Angebot, SKU und Version organisiert. Die Kombination aus Herausgeber:Angebot:SKU:Version ist der Image-URN, durch den das zu verwendende Image eindeutig identifiziert wird.

Beispielsweise verweist `RedHat:RHEL:7-LVM:7.6.2018103108` auf ein RHEL 7.6-Image mit LVM-Partitionierung (Logical Volume Manager), das am 31. Oktober 2018 erstellt wurde.

Ein Beispiel zum Erstellen einer RHEL 7.6-VM ist weiter unten dargestellt.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>Der Moniker „latest“
Die Azure-REST-API ermöglicht die Verwendung des Monikers „latest“ für die Version anstelle der genauen Version. Mit „latest“ wird das neueste verfügbare Image für die angegebene Kombination aus Herausgeber, Angebot und SKU bereitgestellt.

Beispielsweise verweist `RedHat:RHEL:7-LVM:latest` auf das neueste verfügbare Image der RHEL 7 Familie mit LVM-Partitionierung.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:latest --no-wait
```

>[!NOTE]
> Im Allgemeinen folgt der Vergleich von Versionen zum Ermitteln der neuesten Version den Regeln der [CompareTo](https://msdn.microsoft.com/library/a5ts8tb6.aspx)-Methode.

### <a name="rhel-6-image-types"></a>RHEL 6-Imagetypen
Für RHEL 6.x-Images stehen die folgenden Imagetypen zur Verfügung:

|Herausgeber | Angebot | SKU-Wert | Version | Details
|----------|-------|-----------|---------|--------
|RedHat | RHEL | Nebenversion (z. B. 6.9) | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 6.9.2018010506) | Alle standardmäßigen RHEL 6.x-Images befolgen diese Konvention.
|RedHat | rhel-byos | rhel-raw69 | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 6.9.20181023) | Dieses Image ist ein RHEL 6.9-BYOS-Image.
|RedHat | RHEL | RHEL-SAP-APPS | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 6.8.2017053118) | Dies ist ein RHEL 6.8-Image für SAP-Anwendungen. Es ist berechtigt, auf SAP-Anwendungsrepositorys sowie auf RHEL-Basisrepositorys zuzugreifen.
|RedHat | RHEL | RHEL-SAP-HANA | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 6.7.2017053121) | Dies ist ein RHEL 6.7-Image für SAP HANA. Es ist berechtigt, auf SAP HANA-Anwendungsrepositorys sowie auf RHEL-Basisrepositorys zuzugreifen.

### <a name="rhel-7-image-types"></a>RHEL 7-Imagetypen
Für RHEL 7.x-Images gibt es ein paar andere Imagetypen. In der folgenden Tabelle sind die verschiedenen Imagetypen aufgeführt, die angeboten werden. Eine vollständige Liste können Sie sich anzeigen lassen, wenn Sie in die Befehlszeilenschnittstelle von Azure den Befehl `az vm image list --publisher redhat --all` eingeben.

>[!NOTE]
> Sofern nicht anders angegeben, sind alle Images mit LVM partitioniert und stellen Verbindungen mit regulären RHEL-Repositorys her (d. h. nicht EUS, nicht E4S). In der Zukunft sollen nur noch LVM-partitionierte Images veröffentlicht werden. Wenn Sie etwas gegen diese Entscheidung einzuwenden haben, können Sie gerne Feedback geben. Weitere Informationen zum erweiterten Updatesupport und zu Updatediensten für SAP finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).

|Herausgeber | Angebot | SKU-Wert | Version | Details
|----------|-------|------------|---------|--------
|RedHat | RHEL | Nebenversion (z. B. 7.6) | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019102813) | Images, die vor April 2019 veröffentlicht wurden, werden an standardmäßige RHEL-Repositorys angefügt. Images, die nach April 2019 veröffentlicht wurden, werden an Repositorys des erweiterten Updatesupports (Extended Update Support, EUS) von Red Hat angefügt, um das Sperren einer bestimmten Nebenversion zu ermöglichen. Kunden, die reguläre Repositorys verwenden möchten, sollten die Images verwenden, die im SKU-Wert „7-LVM“ oder „7-RAW“ enthalten. Weitere Informationen finden Sie unten. Images von RHEL 7.7 und höher werden LVM-partitioniert. Alle anderen Images dieser Kategorie werden rohpartitioniert.
|RedHat | RHEL | 7-RAW | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019102813) | Diese Images werden rohpartitioniert (d. h. es wurden keine logischen Volumen hinzugefügt).
|RedHat | RHEL | 7-RAW-CI | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019072418) | Diese Images werden rohpartitioniert (d. h. es wurden keine logischen Volumen hinzugefügt). Für die Bereitstellung wird „cloud-init“ verwendet.
|RedHat | RHEL | 7-LVM | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019062414) | Dieses Images werden LVM-partitioniert.
|RedHat | rhel-byos | rhel-{lvm,raw} | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.7.20190819) | Bei diesen Images handelt es sich um die RHEL 7-BYOS-Images. Sie werden an keine Repositorys angefügt, und für sie fällt nicht die RHEL-Premiumgebühr an. Wenn Sie an RHEL-BYOS-Images interessiert sind, können Sie [hier](https://aka.ms/rhel-byos) Zugriff anfordern. SKU-Werte enden auf die Nebenversion und geben an, ob ein Image roh- oder LVM-partitioniert wurde. Der SKU-Wert rhel-lvm77 bedeutet beispielsweise, dass es sich um ein LVM-partitioniertes RHEL 7.7-Image handelt.
|RedHat | RHEL | RHEL-SAP | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019071300) | Bei diesen Images handelt es sich um RHEL for SAP-Images. Sie haben Zugriff auf die SAP HANA- und Anwendungsrepositorys sowie auf RHEL E4S-Repositorys. Bei der Abrechnung werden die Premiumtarife von RHEL und SAP mit eingerechnet, zusätzlich zur Computegrundgebühr.
|RedHat | RHEL | RHEL-SAP-HA | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019062320) | Bei diesen Images handelt es sich um RHEL for SAP-Images mit Hochverfügbarkeit und Update-Diensten. Sie haben Zugriff auf die SAP HANA- und Anwendungsrepositorys sowie auf Repositorys mit Hochverfügbarkeit und RHEL E4S-Repositorys. Bei der Abrechnung werden die Premiumtarife für RHEL, SAP und Hochverfügbarkeit mit eingerechnet, zusätzlich zur Computegrundgebühr.
|RedHat | RHEL | RHEL-Hochverfügbarkeit | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.6.2019062019) | Bei diesen Images handelt es sich um RHEL-Images, die Zugriff auf das Hochverfügbarkeits-Add-On haben. Aufgrund des Premiumtarifs für das Hochverfügbarkeits-Add-On entstehen für diese Images zusätzlich zum RHEL-Tarif und den Computegrundgebühren geringe Zusatzkosten.
|RedHat | RHEL | RHEL-SAP-APPS | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.3.2017053118) | Diese Images sind mittlerweile veraltet, da die SAP-Anwendungs- und SAP HANA-Repositorys in SAP-Repositorys zusammengefasst wurden. Bei diesen Images handelt es sich um RHEL for SAP-Anwendungsimages. Sie haben Zugriff auf SAP-Anwendungsrepositorys sowie auf RHEL-Basisrepositorys.
|RedHat | RHEL | RHEL-SAP-HANA | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 7.3.2018051421) | Diese Images sind mittlerweile veraltet, da die SAP-Anwendungs- und SAP HANA-Repositorys in SAP-Repositorys zusammengefasst wurden. Bei diesen Images handelt es sich um RHEL for SAP HANA-Images. Sie haben Zugriff auf SAP HANA-Repositorys sowie auf RHEL-Basisrepositorys.

### <a name="rhel-8-image-types"></a>RHEL 8-Imagetypen
Weitere Informationen zu RHEL 8-Imagetypen finden Sie unten.

|Herausgeber | Angebot | SKU-Wert | Version | Details
|----------|-------|------------|---------|--------
|RedHat | RHEL | 8 | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 8.0.20191023) | Bei diesen Images handelt es sich um LVM-partitionierte RHEL 8.0-Images, die mit Standardrepositorys von Red Hat verbunden sind.
|RedHat | RHEL | 8-gen2 | Verkettete Werte der RHEL-Nebenversion und des Veröffentlichungsdatums (z. B. 8.0.20191024) | Bei diesen Images handelt es sich um LVM-partitionierte Hyper-V Generation 2-RHEL 8.0-Images, die mit Standardrepositorys von Red Hat verbunden sind. Weitere Informationen zu Generation 2-VMs in Azure finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2).

## <a name="extended-update-support-eus"></a>Extended Update Support (EUS)
Seit April 2019 stehen RHEL-Images zur Verfügung, die standardmäßig an die Extended Update Support-Repositorys (EUS) angefügt sind. Weitere Informationen zum Extended Update Support von RHEL finden Sie in der [Red Hat-Dokumentation](https://access.redhat.com/articles/rhel-eus).

Der Wechsel zu EUS-Repositorys ist möglich und wird unterstützt. Anweisungen zum Umstellen Ihrer virtuellen Computer auf EUS und weitere Details zum Ende von EUS finden Sie [hier](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS wird für RHEL Extras nicht unterstützt. Wenn Sie also ein Paket installieren, das in der Regel über den RHEL Extras-Kanal verfügbar ist, können Sie dies nicht installieren, solange Sie EUS nutzen. Informationen zum Produktlebenszyklus von Red Hat Extras finden Sie [hier](https://access.redhat.com/support/policy/updates/extras/).

### <a name="differentiating-between-regular-and-eus-images"></a>Unterscheiden zwischen regulären und EUS-Images
Kunden, die Images verwenden möchten, die EUS-Repositorys zugeordnet sind, sollten das RHEL-Image verwenden, das in der SKU eine RHEL-Nebenversionsnummer enthält.

Beispielsweise könnten die folgenden beiden RHEL 7.4-Images verfügbar sein:
```bash
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7.6:7.6.2019102813
```
In diesem Fall ist `RedHat:RHEL:7.6:7.6.2019102813` standardmäßig an EUS-Repositorys angefügt (ein SKU-Wert von 7.4) und `RedHat:RHEL:7-LVM:7.6.2019062414` an Nicht-EUS-Repositorys (ein SKU-Wert von 7-LVM).

Wenn Sie reguläre Repositorys (nicht EUS) verwenden möchten, führen Sie Bereitstellungen mithilfe eines Image aus, das keine Nebenversionsnummer in der SKU enthält.

#### <a name="rhel-images-with-eus"></a>RHEL-Images mit EUS
Die folgende Tabelle gilt für RHEL-Images, die mit EUS-Repositorys verbunden sind.

>[!NOTE]
> Zum Zeitpunkt des Verfassens verfügen nur RHEL 7.4 und spätere Nebenversionen über EUS-Unterstützung. EUS wird für RHEL-Versionen bis einschließlich 7.3 nicht mehr unterstützt.
>
> Weitere Einzelheiten zur EUS-Verfügbarkeit für RHEL finden Sie [hier](https://access.redhat.com/support/policy/updates/errata).

Nebenversion |EUS-Beispielimage              |EUS-Status                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Images, die ab April 2019 veröffentlicht werden, nutzen EUS standardmäßig|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Images, die ab Juni 2019 veröffentlicht werden, nutzen EUS standardmäßig |
RHEL 7.6      |RedHat:RHEL:7.6:7.6.2019052206 | Images, die ab Mai 2019 veröffentlicht werden, nutzen EUS standardmäßig  |
RHEL 8.0      |–                            | Kein EUS in Red Hat verfügbar                               |





### <a name="other-available-offers-and-skus"></a>Weitere verfügbare Angebote und SKUs
Die vollständige Liste der verfügbaren Angebote und SKUs enthält eventuell weitere Images über die in der obigen Tabelle aufgeführten hinaus, z.B. `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Diese Angebote können zur Unterstützung bestimmter Marketplace-Lösungen verwendet werden, oder sie können für Vorschauversionen und zu Testzwecken veröffentlicht werden. Sie können jederzeit ohne vorherige Warnung geändert oder entfernt werden. Verwenden Sie sie nicht, sofern ihr Vorhandensein nicht öffentlich von Microsoft oder Red Hat dokumentiert wurde.

## <a name="publishing-policy"></a>Veröffentlichungsrichtlinie
Microsoft und Red Hat aktualisieren Images, wenn neue Nebenversionen veröffentlicht werden, nach Bedarf für bestimmte CVEs oder gelegentlich bei Konfigurationsänderungen/-updates. Wir bemühen uns, die aktualisierten Abbilder möglichst bald bereitzustellen – binnen drei Werktagen nach der Veröffentlichung bzw. Verfügbarkeit eines CVE-Updates.

Wir aktualisieren nur die aktuelle Nebenversion in einer bestimmten Imagefamilie. Wenn eine neuere Nebenversion veröffentlicht wird, beenden wir die Aktualisierung älterer Nebenversionen. Beispielsweise werden mit der Veröffentlichung von RHEL 7.6 keine RHEL 7.5-Images mehr aktualisiert.

>[!NOTE]
> Aktive Azure-VMs, die mit RHEL-Images mit nutzungsbasierter Bezahlung bereitgestellt wurden, sind mit der Azure-RHUI verbunden und erhalten Updates und Korrekturen, sobald sie von Red Hat veröffentlicht und in die Azure-RHUI repliziert wurden (normalerweise in weniger als 24 Stunden nach der offiziellen Veröffentlichung durch Red Hat). Für diese VMs ist kein neues veröffentlichte Image erforderlich, um die Updates zu erhalten, und Kunden haben vollständige Kontrolle über den Zeitpunkt des Updates.

## <a name="image-retention-policy"></a>Aufbewahrungsrichtlinie für Images
Nach unserer aktuellen Richtlinie werden alle veröffentlichten Images beibehalten. Wir behalten uns vor, Images zu entfernen, die bekanntermaßen Probleme jeglicher Art verursachen. Beispielsweise können Images mit falschen Konfigurationen aufgrund späterer Plattform- oder Komponentenaktualisierungen entfernt werden. Falls ein Image entfernt wird, gilt die aktuelle Marketplace-Richtlinie, nach der bis zu 30 Tage vor dem Entfernen des Images Benachrichtigungen bereitgestellt werden sollten.

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich die vollständige Liste der [RHEL-Images in Azure](./redhat-imagelist.md) an.
* Erfahren Sie [hier](https://aka.ms/rhui-update) mehr über die Azure Red Hat-Updateinfrastruktur.
* Erfahren Sie mehr über das [RHEL-BYOS-Angebot](./byos.md).
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).
