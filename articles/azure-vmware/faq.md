---
title: Häufig gestellte Fragen
description: Hier finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure VMware Solution.
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: dikamath
ms.openlocfilehash: 8868f86f0cf46ff82e37cd433d7b5bca0d69567d
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078934"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution"></a>Häufig gestellte Fragen zu Azure VMware Solution

Antworten auf häufig gestellte Fragen zu Azure VMware Solution

## <a name="general"></a>Allgemein

#### <a name="what-is-azure-vmware-solution"></a>Was ist Azure VMware Solution?

Im Zuge der IT-Modernisierungsstrategien von Unternehmen zur Verbesserung der Geschäftsflexibilität, Kostensenkung und Innovationsbeschleunigung haben sich hybride Cloudplattformen als Schlüsselfaktoren für die digitale Transformation von Kunden erwiesen. Azure VMware Solution kombiniert die SDDC-Software (Software Defined Data Center) von VMware mit dem globalen Clouddienstökosystem von Microsoft Azure. Azure VMware Solution wird verwaltet, um die Anforderungen an Leistung, Verfügbarkeit, Sicherheit und Compliance zu erfüllen.

## <a name="azure-vmware-solution-service"></a>Azure VMware Solution-Dienst

#### <a name="where-is-azure-vmware-solution-available-today"></a>Wo ist Azure VMware Solution aktuell verfügbar?

Der Dienst wird fortlaufend neuen Regionen hinzugefügt. Details finden Sie auf der Seite mit den [neuesten Informationen zur Dienstverfügbarkeit](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware). 

#### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>Können Workloads, die in einer Azure VMware Solution-Instanz ausgeführt werden, Azure-Dienste nutzen oder in diese integriert werden?

Alle Azure-Dienste stehen Azure VMware Solution-Kunden zur Verfügung. Leistungs- und Verfügbarkeitseinschränkungen für bestimmte Dienste müssen von Fall zu Fall behandelt werden.

#### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>Verwende ich die gleichen Tools, die ich jetzt zum Verwalten von privaten Cloudressourcen verwende?

Ja. Das Azure-Portal wird für die Bereitstellung und eine Reihe von Verwaltungsvorgängen verwendet. vCenter und NSX Manager werden zum Verwalten von vSphere- und NSX-T-Ressourcen verwendet.

#### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>Kann ich eine private Cloud mit meiner lokalen vCenter-Instanz verwalten?

Beim Start unterstützt Azure VMware Solution keine einzelne Verwaltungsumgebung für lokale und private Cloudumgebungen. Cluster der privaten Cloud werden mit vCenter und NSX Manager lokal für eine private Cloud verwaltet.

#### <a name="can-i-use-vrealize-suite-running-on-premises"></a>Kann ich eine lokal ausgeführte vRealize-Suite verwenden? 

Bestimmte Integrationen und Anwendungsfälle können von Fall zu Fall ausgewertet werden.

#### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>Kann ich vSphere-VMs aus lokalen Umgebungen zu privaten Azure VMware Solution-Clouds migrieren?

Ja. Die VM-Migration und vMotion können verwendet werden, um VMs in eine private Cloud zu verschieben, wenn die standardmäßigen vCenter-übergreifenden [vMotion-Anforderungen](https://kb.vmware.com/s/article/210695) erfüllt sind.

#### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>Ist eine bestimmte Version von vSphere in lokalen Umgebungen erforderlich?

Da alle Cloudumgebungen VMware HCX bereitstellen, verwenden Sie vSphere 5.5 oder höher in lokalen Umgebungen für vMotion.

#### <a name="what-does-the-change-control-process-look-like"></a>Wie sieht der Änderungssteuerungsvorgang aus?

Updates, die am Dienst selbst vorgenommen werden, befolgen dem Change Management-Standardprozess von Microsoft Azure. Kunden sind für alle Workloadverwaltungsaufgaben und die zugehörigen Change Management-Prozesse verantwortlich.

#### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>Wie unterscheidet sich dies von Azure VMware Solution by CloudSimple?

Mit dem neuen Dienst Azure VMware Solution verfügen Microsoft und VMware über eine direkte Cloudanbieterpartnerschaft. Es handelt sich dabei um eine vollständig von Microsoft entworfene, erstellte und unterstützte Lösung, die von VMware unterstützt wird. Die Architektur der beiden Lösungen ist konsistent. Bei beiden wird der VMware-Technologiestapel in einer dedizierten Azure-Infrastruktur ausgeführt.

#### <a name="are-red-hat-solutions-supported-on-azure-vmware-solution"></a>Werden Red Hat-Lösungen unter Azure VMware Solution unterstützt?

Microsoft und Red Hat teilen sich ein integriertes, zusammengefasstes Supportteam, das eine einheitliche Anlaufstelle für Red Hat-Ökosysteme bietet, die auf der Azure-Plattform ausgeführt werden.  Wie andere Dienste der Azure-Plattform, die mit Red Hat Enterprise Linux zusammenarbeiten, fällt Azure VMware Solution unter den Bereich von Cloudzugriff und integriertem Support, und Red Hat Enterprise Linux wird für die Ausführung über Azure VMware Solution innerhalb von Azure unterstützt.

#### <a name="is-vmware-hcx-enterprise-edition-available-and-if-so-how-much-does-it-cost"></a>Ist VMware HCX Enterprise Edition verfügbar und wie viel kostet es, wenn dies der Fall ist?

VMware HCX Enterprise Edition (EE) ist mit Azure VMware Solution als *Vorschau*-Funktion/Dienst verfügbar. Solange die VMware HCX EE für Azure VMware Solution in Vorschau vorliegt, ist sie eine kostenlose Funktion/ein kostenloser Dienst und unterliegt den Bestimmungen eines Diensts in Vorschau. Sobald der VMware HCX EE-Dienst die Phase „Allgemeine Verfügbarkeit“ erreicht hat, erhalten Sie eine Benachrichtigung, dass in 30 Tagen die Abrechnung umgestellt wird. Sie haben auch die Möglichkeit, den Dienst zu deaktivieren bzw. abzuwählen.

#### <a name="can-azure-vmware-solution-vms-be-managed-by-vmrc"></a>Können virtuelle Computer von Azure VMware Solution durch VMRC verwaltet werden?
Ja, sofern das System, auf dem es installiert ist, auf das vCenter für die private Cloud zugreifen kann und es öffentliches DNS verwendet (sodass es ESXi-Hostnamen auflösen kann).

#### <a name="are-there-special-instructions-for-installing-and-using-vmrc-with-azure-vmware-solution-vms"></a>Gibt es spezielle Anweisungen für die Installation und Verwendung von VMRC mit virtuellen Azure VMware Solution-Computern (VMs)?
Nein, verwenden Sie die von [VMware bereitgestellten Anweisungen](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-89E7E8F0-DB2B-437F-8F70-BA34C505053F.html), und erfüllen Sie die in diesen Anweisungen angegebenen VM-Voraussetzungen. 

#### <a name="is-vmware-hcx-supported-on-vpns"></a>Wird VMware HCX auf VPNs unterstützt?
Nein, aufgrund der Anforderungen an Bandbreite und Wartezeit.

#### <a name="can-azure-bastion-be-used-for-connecting-to-avs-vms"></a>Kann Azure Bastion zum Herstellen einer Verbindung mit virtuellen AVS-Computern verwendet werden?
Azure Bastion ist der Dienst, der empfohlen wird, um eine Verbindung mit der Jumpbox herzustellen, damit die Azure VMware Solution-Instanz nicht im Internet veröffentlicht wird. Sie können Azure Bastion nicht verwenden, um eine Verbindung mit Azure VMware Solution-VMs herzustellen, da es sich hierbei nicht um Azure-IaaS-Objekte handelt.

#### <a name="can-an-existing-expressroute-gateway-be-used-to-connect-to-azure-vmware-solution"></a>Kann ein vorhandenes ExpressRoute Gateway verwendet werden, um eine Verbindung mit Azure VMware Solution herzustellen?
Ja, Sie können ein vorhandenes ExpressRoute-Gateway verwenden, um eine Verbindung mit Azure VMware Solution herzustellen, sofern der Grenzwert von vier ExpressRoute-Verbindungen pro virtuellem Netzwerk nicht überschritten wird.  Für den lokalen Zugriff auf Azure VMware Solution über ExpressRoute benötigen Sie ExpressRoute Global Reach, da das ExpressRoute-Gateway kein transitives Routing zwischen den Verbindungen unterstützt.

## <a name="compute-network-storage-and-backup"></a>Compute, Netzwerk, Speicher und Sicherung

#### <a name="is-there-more-than-one-type-of-host-available"></a>Sind mehrere Hosttypen verfügbar?

Es ist nur ein Hosttyp verfügbar.

#### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>Wie lauten die CPU-Spezifikationen in den einzelnen Hosttypen?

Die Server verfügen über Intel Dual-CPUs mit 18 Kernen mit 2,3 GHz.

#### <a name="how-much-memory-is-in-each-host"></a>Wie viel Arbeitsspeicher ist auf jedem Host vorhanden?

Die Server verfügen über 576 GB RAM.

#### <a name="what-is-the-storage-capacity-of-each-host"></a>Wie groß ist die Speicherkapazität jedes Hosts?

Jeder ESXi-Host verfügt über zwei vSAN-Datenträgergruppen mit einer Kapazitätsebene von 15,2 TB und einer NVMe-Cacheebene von 3,2 TB (1,6 TB in jeder Datenträgergruppe).

#### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>Wie viel Netzwerkbandbreite ist in jedem ESXi-Host verfügbar?

Jeder ESXi-Host in Azure VMware Solution ist mit vier Netzwerkadaptern mit 25 GBit/s konfiguriert. Dabei werden zwei Netzwerkadapter für ESXi-Systemdatenverkehr und zwei Netzwerkadapter für Workloaddatenverkehr bereitgestellt. 

#### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>Werden Daten, die in den vSAN-Datenspeichern gespeichert sind, im Ruhezustand verschlüsselt?

Ja, alle vSAN-Daten werden standardmäßig mithilfe der in Azure Key Vault gespeicherten Schlüssel verschlüsselt.

#### <a name="you-document-that-commvault-veritas-and-veeam-have-extended-their-backup-solutions-to-work-with-azure-vmware-solution-what-about-other-independent-software-vendor-isv-backup-solutions"></a>Sie dokumentieren, dass Commvault, Veritas und Veeam ihre Sicherungslösungen erweitert haben, um mit Azure VMware Solution zu arbeiten. Was ist mit anderen Sicherungslösungen unabhängiger Softwareanbieter (ISVs)?

Soweit uns bekannt ist, sollte jede Sicherungslösung, die VMware VADP mit dem HotAdd-Transportmodus verwendet, sofort mit Azure VMware Solution funktionieren.

#### <a name="what-about-support-for-isv-backup-solutions"></a>Was ist mit der Unterstützung für ISV-Sicherungslösungen?

Da diese Sicherungslösungen von Kunden installiert und verwaltet werden, können sie sich an den jeweiligen unabhängigen Softwareanbieter wenden, um Unterstützung zu erhalten. 

#### <a name="what-is-the-correct-storage-policy-for-the-dedup-set-up"></a>Welche ist die richtige Speicherrichtlinie für die Dedup-Einrichtung?

Verwenden Sie die Richtlinie *thin_provision* für die Speicherung Ihrer VM-Vorlage.  Der Standardwert ist *thick_provision*.

#### <a name="are-the-snmp-infrastructure-logs-shared"></a>Werden die Protokolle der SNMP-Infrastruktur freigegeben?

Nein.

## <a name="hosts-clusters-and-private-clouds"></a>Hosts, Cluster und private Clouds

#### <a name="is-the-underlying-infrastructure-shared"></a>Wird die zugrunde liegende Infrastruktur gemeinsam verwendet?

Nein, private Cloudhosts und Cluster sind dediziert und werden vor und nach der Verwendung sicher gelöscht.

#### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>Wie hoch ist die minimale und maximale Anzahl von Hosts pro Cluster?

Cluster können zwischen 3 und 16 ESXi-Hosts skaliert werden. Testcluster sind auf drei Hosts beschränkt.

#### <a name="can-i-scale-my-private-cloud-clusters"></a>Kann ich die Cluster meiner privaten Cloud skalieren?

Ja, Cluster werden zwischen der minimalen und maximalen Anzahl von ESXi-Hosts skaliert. Testcluster sind auf drei Hosts beschränkt.

#### <a name="what-are-trial-clusters"></a>Was sind Testcluster?

Testcluster sind drei Hostcluster, die für Auswertungen eines Monats von privaten Azure VMware Solution-Clouds verwendet werden.

#### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>Kann ich High-End-Hosts für Testcluster verwenden?

Nein. High-End-ESXi-Hosts sind für die Verwendung in Produktionsclustern reserviert.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware Solution und VMware-Software

#### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>Welche Versionen von VMware-Software werden in privaten Clouds verwendet?

Private Clouds verwenden vSphere 6.7, vSAN 6.7, VMware HCX und Version 2.5 von NSX-T.  

#### <a name="do-private-clouds-use-vmware-nsx"></a>Verwenden private Clouds VMware NSX?

Ja, NSX-T 2.5 wird für softwaredefinierte Netzwerke in privaten Azure VMware Solution-Clouds verwendet.

#### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>Kann ich VMware NSX-V in einer privaten Cloud verwenden?

Nein. NSX-T ist die einzige unterstützte Version von NSX.

#### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>Ist NSX in lokalen Umgebungen oder Netzwerken erforderlich, die eine Verbindung mit einer privaten Cloud herstellen?

Nein, Sie müssen NSX nicht lokal verwenden.

#### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>Wie sieht der Upgrade- und Updatezeitplan für VMware-Software in einer privaten Cloud aus?

Die Upgrades der Softwarepakete der privaten Cloud werden durchgeführt, um die Software innerhalb einer Version des neuesten Release des Softwarepakets von VMware zu halten. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter, vSAN) unterscheiden.

#### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>Wie häufig wird der Softwarestapel der privaten Cloud aktualisiert?

Das Upgrade der Software der privaten Cloud erfolgt nach einem Zeitplan, der sich nach der Veröffentlichung des Softwarepakets von VMware richtet. Ihre private Cloud erfordert keine Ausfallzeiten für Upgrades.

## <a name="connectivity"></a>Konnektivität

#### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>Welche Planung von Netzwerk-IP-Adressen ist erforderlich, um private Clouds in lokale Umgebungen einzubinden?

Ein /22-Adressraum eines privaten Netzwerks ist erforderlich, um eine private Azure VMware Solution-Cloud bereitzustellen. Dieser private Adressraum darf sich nicht mit anderen virtuellen Netzwerken in einem Abonnement oder mit lokalen Netzwerken überlappen.
 
#### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>Wie kann ich eine Verbindung aus lokalen Umgebungen mit einer privaten Azure VMware Solution-Cloud herstellen?

Sie können mit einer von zwei Methoden eine Verbindung mit dem Dienst herstellen: 

- Mit einer VM oder einem Anwendungsgateway in einem virtuellen Azure-Netzwerk, die bzw. das über ExpressRoute und Peering mit der privaten Cloud verbunden ist.
- Über ExpressRoute Global Reach aus Ihrem lokalen Rechenzentrum mit einer Azure ExpressRoute-Verbindung.

#### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>Wie verbinde ich eine Workload-VM mit dem Internet oder einem Azure-Dienstendpunkt?

Aktivieren Sie im Azure-Portal Internetkonnektivität für eine private Cloud. Erstellen Sie mit NSX-T Manager einen NSX-T T1-Router und einen logischen Switch. Sie verwenden dann vCenter, um eine VM in dem Netzwerksegment bereitzustellen, das durch den logischen Switch definiert wird. Diese VM verfügt über Netzwerkzugriff auf das Internet und auf Azure-Dienste.

#### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>Muss ich den Zugriff aus dem Internet auf VMs in logischen Netzwerken in einer privaten Cloud einschränken?

Nein. Eingehender Netzwerksdatenverkehr aus dem Internet direkt in private Clouds ist unzulässig.

#### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>Muss ich Internetzugriff von VMs in logischen Netzwerken auf das Internet in einer privaten Cloud einschränken?

Ja. Sie müssen NSX-T Manager zum Erstellen einer Firewall verwenden, die den VM-Zugriff auf das Internet einschränkt.



## <a name="accounts-and-privileges"></a>Konten und Berechtigungen

#### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>Welche Konten und Berechtigungen erhalte ich mit meiner neuen privaten Azure VMware Solution-Cloud?

Sie erhalten Anmeldeinformationen für einen Cloudadministratorbenutzer in vCenter und Administratorzugriff auf NSX-T Manager. Es gibt auch eine Cloudadministratorgruppe, die verwendet werden kann, um Azure Active Directory zu integrieren. Weitere Informationen finden Sie unter [Zugriffs- und Identitätskonzepte](concepts-identity.md).

#### <a name="can-have-administrator-access-to-esxi-hosts"></a>Kann Administratorzugriff auf ESXi-Hosts erhalten?

Nein, Administratorzugriff auf ESXi ist darauf beschränkt, die Sicherheitsanforderungen der Lösung zu erfüllen.

#### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>Welche Zugriffsberechtigungen und Berechtigungen erhalte ich in vCenter?

Sie verfügen über CloudAdmin-Gruppenberechtigungen. Weitere Informationen finden Sie unter [Zugriffs- und Identitätskonzepte](concepts-identity.md).

#### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>Welche Zugriffsberechtigungen und Berechtigungen erhalte ich für NSX-T Manager?

Sie verfügen über vollständige Administratorrechte für NSX-T und können rollenbasierte Zugriffssteuerung wie im lokalen NSX-T-Rechenzentrum verwalten. Weitere Informationen finden Sie unter [Zugriffs- und Identitätskonzepte](concepts-identity.md).

> [!NOTE]
> Ein T0-Router wird im Rahmen der Bereitstellung einer privaten Cloud erstellt und konfiguriert. Änderungen an diesem logischen Router oder den NSX-T-Edgeknoten-VMs können sich auf die Konnektivität mit Ihrer privaten Cloud auswirken.

## <a name="billing-and-support"></a>Abrechnung und Support

#### <a name="how-will-pricing-be-structured-for-azure-vmware-solution"></a>Wie werden die Preise für Azure VMware Solution strukturiert sein?

Allgemeine Fragen zu Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/azure-vmware) für Azure VMware Solution. 

#### <a name="who-supports-azure-vmware-solution"></a>Wer unterstützt Azure VMware Solution?

Unterstützung für Azure VMware Solution wird von Microsoft bereitgestellt. Sie können eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) übermitteln.

#### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>Welche Konten benötige ich zum Erstellen einer privaten Azure VMware Solution-Cloud?

Sie benötigen ein Azure-Konto in einem Azure-Abonnement.

#### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>Wie beantrage ich eine Erhöhung des Hostkontingents für eine Azure-VMware-Lösung?

* Sie benötigen einen [Azure Konzernvertrag (Enterprise Agreement, EA)](../cost-management-billing/manage/ea-portal-agreements.md) mit Microsoft.
* Sie benötigen ein Azure-Konto in einem Azure-Abonnement.

Bevor Sie Ihre Azure VMware Solution-Ressource erstellen, müssen Sie ein Supportticket einreichen, damit ihre Knoten zugewiesen werden. Sobald das Supportteam Ihre Anforderung erhalten hat, dauert es bis zu fünf Werktage, bis die Anforderung bestätigt wird und ihre Knoten zugewiesen werden. Wenn Sie eine vorhandene private Azure VMware Solution-Cloud haben und möchten, dass weitere Knoten zugeordnet werden, durchlaufen Sie denselben Prozess.


1. Erstellen Sie im Azure-Portal unter **Hilfe und Support** eine **[Neue Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support)** , und geben Sie die folgenden Informationen für das Ticket an:
   - **Problemtyp:** Technisch
   - **Abonnement:** Wählen Sie Ihr Abonnement aus.
   - **Dienst:** Alle Dienste > Azure VMware Solution
   - **Ressource:** Allgemeine Frage 
   - **Zusammenfassung:** Kapazität erforderlich
   - **Problemtyp:** Capacity Management Issues (Kapazitätsverwaltungsprobleme)
   - **Problemuntertyp:** Customer Request for Additional Host Quota/Capacity (Kundenanfrage für zusätzliches Hostkontingent/zusätzliche Hostkapazität)

1. Geben Sie in der **Beschreibung** des Supporttickets auf der Registerkarte **Details** Folgendes an:

   - Proof of Concept oder Produktion 
   - Name der Region
   - Anzahl von Knoten
   - Weitere Details

   >[!NOTE]
   >Für Azure VMware Solution werden mindestens drei Knoten empfohlen, um Ihre private Cloud zu starten, und für Redundanz werden N + 1 Knoten empfohlen. 

1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.

   Es dauert bis zu fünf Werktage, bis ein Supportmitarbeiter Ihre Anforderung bestätigt.

   >[!IMPORTANT] 
   >Wenn Sie bereits eine Azure VMware Solution haben und zusätzliche Knoten anfordern möchten, beachten Sie bitte, dass wir fünf Werktage für die Zuordnung der Knoten benötigen. 

1. Stellen Sie sicher, dass Sie den Ressourcenanbieter **Microsoft.AVS** im Azure-Portal registriert haben, damit Sie Ihre Knoten bereitstellen können.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Informationen zu weiteren Möglichkeiten für die Ressourcenanbieterregistrierung finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md