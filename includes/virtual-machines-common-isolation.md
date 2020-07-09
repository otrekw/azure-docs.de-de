---
title: include file
description: include file
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 033ec0f4011cae657fc464849aac5c5d9a850907
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83343319"
---
Azure Compute bietet VM-Größen, die für einen bestimmten Hardwaretyp isoliert und für einen einzelnen Kunden bestimmt sind.  Diese VM-Größen eignen sich am besten für Workloads, die ein hohes Maß an Isolation von anderen Kunden erfordern, wenn es um Workloads mit Elementen wie Konformität und gesetzlichen Anforderungen geht.  Kunden können auch die Ressourcen dieser isolierten virtuellen Computer weiter unterteilen, indem sie die [Azure-Unterstützung für geschachtelte virtuelle Computer](https://azure.microsoft.com/blog/nested-virtualization-in-azure/) verwenden.

Durch die Verwendung isolierter Größen wird sichergestellt, dass Ihr virtueller Computer als einziger in der jeweiligen Serverinstanz ausgeführt wird.  Zu den aktuellen Angebote isolierter virtueller Computer zählen Folgende:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2 <sup>*</sup>
* Standard_D15_v2 <sup>*</sup>
* Standard_F72s_v2

<sup>*</sup>Die Isolationsgarantie wird ab dem 15. Mai 2020 eingestellt.

## <a name="retiring-d15_v2ds15_v2-isolation-on-may-15-2020"></a>Außerbetriebnahme der D15_v2/DS15_v2-Isolation am 15. Mai 2020
**Update am 10. Februar 2020: Die Außerbetriebnahme-Zeitachse für „Isolation“ wurde bis zum 15. Mai 2020 verlängert**

Azure Dedicated Host ist jetzt allgemein verfügbar, sodass Sie die virtuellen Linux- und Windows-Computer Ihrer Organisation auf physischen Servern mit einem einzigen Mandanten ausführen können. Eine vollständige Ersetzung der isolierten Azure-VMs durch Azure Dedicated Host ist geplant. Nach dem **15. Mai 2020** sind die Azure-VMs der Serie D15_v2/DS15_v2 nicht mehr hardwareisoliert.

## <a name="how-does-this-affect-me"></a>Welche Folgen hat das für mich?
Ab dem 15. Mai 2020 wird keine Isolationsgarantie für Ihre virtuellen Azure-Computer des Typs D15_v2/DS15_v2 gewährleistet. 

## <a name="what-actions-should-i-take"></a>Welche Aktionen zieht das für mich nach sich?
Wenn die Hardwareisolation für Sie nicht erforderlich ist, müssen Sie keine Maßnahmen ergreifen. 

Wenn die Isolation für Sie jedoch erforderlich ist, müssen Sie eine der folgenden Maßnahmen vor dem 15. Mai 2020 ergreifen:

•   [Migrieren](https://azure.microsoft.com/blog/introducing-azure-dedicated-host) Sie Ihre Workload zu Azure Dedicated Host.

•   [Fordern Sie Zugriff](https://aka.ms/D15iRequestAccess) auf eine D15i_v2- und eine DS15i_v2-Azure-VM an, um dieselbe Leistung zum gleichen Preis zu erhalten. Diese Option ist nur in Szenarien für reservierte Instanzen mit nutzungsbasierter Bezahlung oder einer Laufzeit von einem Jahr verfügbar.    

•   [Migrieren](https://azure.microsoft.com/blog/resize-virtual-machines/) Sie Ihre Workload zu einer anderen isolierten Azure-VM. 

Weitere Informationen finden Sie unten:

## <a name="timeline"></a>Zeitachse
| Date | Aktion | 
| --- | --- |
| 18. November 2019 | Verfügbarkeit von D/DS15i_v2 (PAYG, 1 Jahr RI) |
| 14. Mai 2020  | Letzter Tag zum Erwerb von D/DS15i_v2 RI für 1 Jahr | 
| 15. Mai 2020   | D/DS15_v2-Isolationsgarantie entfernt | 
| 15. Mai 2021  | Außerbetriebnahme von D/DS15i_v2 (alle Kunden mit Ausnahme derjenigen, die vor dem 18. November 2019 RIs von D/DS15_v2 für 3 Jahre erworben haben)| 
| 17. November 2022  | Außerbetriebnahme von D/DS15i_v2 nach Ablauf von RIs für 3 Jahre (für Kunden, die vor dem 18. November 2019 RIs von D/DS15_v2 für 3 Jahre erworben haben) | 

## <a name="faq"></a>Häufig gestellte Fragen
### <a name="q-is-the-size-dds15_v2-going-to-get-retired"></a>F: Soll die Größe D/DS15_v2 eingestellt werden?
**A:** Nein, nur die Funktion „Isolation“ wird eingestellt. Wenn Sie keine Isolation benötigen, brauchen Sie keine Maßnahmen zu ergreifen.

### <a name="q-is-the-size-dds15i_v2-going-to-get-retired"></a>F: Soll die Größe D/DS15i_v2 eingestellt werden?
**A:** Ja, die Größe ist nur bis zum 15. Mai 2021 verfügbar. Kunden die 3-jährige reservierte Instanzen von D/DS15_v2 vor dem 18. November 2019 erworben haben, haben Zugriff auf D/DS15i_v2 bis zum 17. November 2022.

### <a name="q-why-am-i-not-seeing-the-new-dds15i_v2-sizes-in-the-portal"></a>F: Warum sehe ich keine neuen D/DS15i_v2-Größen im Portal?
**A:** Wenn Sie ein aktueller D/DS15_v2-Kunde sind und die neuen D/DS15i_v2-Größen verwenden möchten, füllen Sie dieses [Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUMkZUWEFPNjFPNVgyMkhZS05FSzlPTzRIOS4u) aus.

### <a name="q-why-i-am-not-seeing-any-quota-for-the-new-dds15i_v2-sizes"></a>F: Warum sehe ich kein Kontingent für die neuen D/DS15i_v2-Größen?
**A:** Wenn Sie ein aktueller D/DS15_v2-Kunde sind und die neuen D/DS15i_v2-Größen verwenden möchten, füllen Sie dieses [Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUNU1XUkhZWkNXQUFMNEJWUk9VWkRRVUJPMy4u) aus.

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>F: Wann werden die anderen isolierten Größen außer Betrieb genommen?
**A:** Wir werden Erinnerungen 12 Monate vor der offiziellen Außerbetriebnahme der Größen bereitstellen.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>F: Gibt es eine Ausfallzeit, wenn meine VM auf nicht isolierte Hardware verschoben wird?
**A:** Wenn Sie keine Isolation benötigen, müssen Sie keine Maßnahmen ergreifen, und es treten keine Ausfallzeiten auf.

### <a name="q-are-there-any-cost-changes-for-moving-to-a-non-isolated-virtual-machine"></a>F: Gibt es Kostenänderungen beim Umstieg auf einen nicht isolierten virtuellen Computer?
**A:** Nein 

### <a name="q-i-already-purchased-1--or-3-year-reserved-instance-for-d15_v2-or-ds15_v2-how-will-the-discount-be-applied-to-my-vm-usage"></a>F: Ich habe bereits eine reservierte Instanz für 1 oder 3 Jahre für D15_v2 oder Ds15_v2 erworben. Wie wird der Rabatt auf meine VM-Nutzung angewendet?
**A:** RIs, die vor dem 18. November 2019 erworben wurden, werden automatisch auf die neue isolierte VM-Reihe erweitert. 

| RI |  Flexibilität bei der Instanzgröße | Vorteilsberechtigung |   
| --- | --- | --- |
|   D15_v2  |   Aus     |   D15_v2 und D15i_v2 |    
|   D15_v2  |   Andererseits  |   Die D15_v2-Serie und D15i_v2 erhalten alle den RI-Vorteil. |    
|   D14_v2  |   Andererseits  |   Die D15_v2-Serie und D15i_v2 erhalten alle den RI-Vorteil. |    
 
Gleiches gilt für die Dsv2-Serie.
 
### <a name="q-i-want-to-purchase-additional-reserved-instances-for-dv2-which-one-should-i-choose"></a>F: Ich möchte weitere reservierte Instanzen für Dv2 erwerben. Welche sollte ich auswählen?
**A:** Für alle RIs, die nach dem 18. November 2019 erworben wurden, gilt das folgende Verhalten. 

| RI |  Flexibilität bei der Instanzgröße | Vorteilsberechtigung |   
| --- | --- | --- |
| D15_v2 |  Aus |   Nur D15_v2  
| D15_v2 |  Andererseits |    Die D15_v2-Serie erhält den RI-Vorteil. Die neue D15i_v2-Serie ist für den RI-Vorteil dieses RI-Typs nicht berechtigt. | 
| D15i_v2 |     Aus | Nur D15i_v2 |  
| D15i_v2 |     Andererseits  | Nur D15i_v2 | 
 
Flexibilität der Instanzgröße kann nicht verwendet werden, um auf andere Größen wie etwa D2_v2, D4_v2 oder D15_v2 angewendet zu werden. Gleiches gilt für die Dsv2-Serie.  
 
### <a name="q-can-i-buy-a-new-3-year-ri-for-d15i_v2-and-ds15i_v2"></a>F: Kann ich eine neue RI für 3 Jahre für D15i_v2 und DS15i_v2 erwerben?
**A:** Leider ist bei einem Neuerwerb nur eine RI mit einjähriger Laufzeit verfügbar.
 
### <a name="q-can-i-move-my-existing-d15_v2ds15_v2-reserve-instance-to-an-isolated-size-reserved-instance"></a>F: Kann ich meine vorhandene reservierte D15_v2/DS15_v2-Instanz in eine reservierte Instanz der isolierten Größe verschieben?
**A:** Diese Aktion ist nicht erforderlich, da der Vorteil sowohl für isolierte als auch für nicht isolierte Größen gilt. Azure unterstützt jedoch das Ändern vorhandener reservierter D15_v2/DS15_v2-Instanzen in D15i_v2/DS15i_v2. Verwenden Sie für alle anderen reservierten Dv2/Dsv2-Instanzen die vorhandene reservierte Instanz, oder erwerben Sie neue reservierte Instanzen für die isolierten Größen.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>F: Ich bin ein Azure Service Fabric-Kunde, der die Dauerhaftigkeitsstufen Silver oder Gold verwendet. Wirkt sich diese Änderung auf mich aus?
**A:** Nein. Die von den [Dauerhaftigkeitsstufen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) von Service Fabric bereitgestellten Garantien werden auch nach dieser Änderung weiterhin funktionieren. Wenn Sie aus anderen Gründen physische Hardwareisolierung benötigen, müssen Sie möglicherweise dennoch eine der oben beschriebenen Aktionen ausführen. 
