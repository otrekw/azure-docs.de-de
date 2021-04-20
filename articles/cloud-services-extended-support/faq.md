---
title: Häufig gestellte Fragen zu Azure Cloud Services (erweiterter Support)
description: Häufig gestellte Fragen zu Azure Cloud Services (erweiterter Support)
ms.topic: conceptual
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 33bafac9247f007978fef568469d643f1a1098df
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106383585"
---
# <a name="frequently-asked-questions-for-azure-cloud-services-extended-support"></a>Häufig gestellte Fragen zu Azure Cloud Services (erweiterter Support)
In diesem Artikel werden häufig gestellte Fragen zu Azure Cloud Services (erweiterter Support) behandelt.

## <a name="general"></a>Allgemein

### <a name="what-is-the-resource-name-for-cloud-services-classic--cloud-services-extended-support"></a>Wie lautet der Ressourcenname für Cloud Services (klassisch) und Cloud Services (erweiterter Support)?
- Cloud Services (klassisch): `microsoft.classiccompute/domainnames`
- Cloud Services (erweiterter Support): `microsoft.compute/cloudservices`

### <a name="what-locations-are-available-to-deploy-cloud-services-extended-support"></a>Welche Standorte sind zum Bereitstellen von Cloud Services (erweiterter Support) verfügbar?
Cloud Services (erweiterter Support) ist in allen Public Cloud-Regionen verfügbar.

### <a name="how-does-my-quota-change"></a>Wie ändert sich mein Kontingent? 
Kunden müssen ein Kontingent mit denselben Prozessen wie jedes andere Azure Resource Manager-Produkt anfordern. Das Kontingent in Azure Resource Manager ist regional, und für jede Region wird eine separate Kontingentanforderung benötigt.

### <a name="why-dont-i-see-a-production--staging-slot-anymore"></a>Warum wird kein Produktions- und Stagingslot mehr angezeigt?
Cloud Services (erweiterter Support) unterstützt nicht das logische Konzept eines gehosteten Diensts, der zwei Slots enthält (Produktion und Staging). Jede Bereitstellung ist eine unabhängige Bereitstellung des Clouddiensts (erweiterter Support). Um eine neue Version eines Clouddiensts zu testen und bereitzustellen, stellen Sie einen Clouddienst (erweiterter Support) bereit und markieren ihn als VIP-austauschbar mit einem anderen Clouddienst (erweiterter Support).

### <a name="why-cant-i-create-an-empty-cloud-service-anymore"></a>Warum kann ich keinen leeren Clouddienst mehr erstellen?
Das Konzept der Namen von gehosteten Diensten ist nicht mehr vorhanden. Sie können keinen leeren Clouddienst (erweiterter Support) erstellen.

### <a name="does-cloud-services-extended-support-support-resource-health-check-rhc"></a>Unterstützt Cloud Services (erweiterter Support) Resource Health Check (RHC)?
Nein, Cloud Services (erweiterter Support) unterstützt Resource Health Check (RHC) nicht.

### <a name="how-are-role-instance-metrics-changing"></a>Wie ändern sich Rolleninstanzmetriken?
Es gibt keine Änderungen bei den Rolleninstanzmetriken. 

### <a name="how-are-web--worker-roles-changing"></a>Wie ändern sich Web- und Workerrollen?
Es gibt keine Änderungen am Entwurf, an der Architektur oder den Komponenten von Web- und Workerrollen. 

### <a name="how-are-role-instances-changing"></a>Wie ändern sich Rolleninstanzen?
Es gibt keine Änderungen am Entwurf, an der Architektur oder den Komponenten der Rolleninstanzen. 

### <a name="how-will-guest-os-updates-change"></a>Wie ändern sich Updates von Gastbetriebssystemen?
 Es gibt keine Änderungen bei der Rolloutmethode. Cloud Services (klassisch) und Cloud Services (erweiterter Support) erhalten dieselben Updates.
 
### <a name="does-cloud-services-extended-support-support-stopped-allocated-and-stopped-deallocated-states"></a>Unterstützt Cloud Services (erweiterter Support) die Status „stopped-allocated“ (beendet zugewiesen) und „stopped-deallocated“ (beendet Zuordnung aufgehoben)?

Die Bereitstellung von Cloud Services (erweiterter Support) unterstützt nur den Status „stopped-allocated“, der im Azure-Portal als „Beendet“ angezeigt wird. Der Status „stopped-deallocated“ wird nicht unterstützt. 

### <a name="do-cloud-services-extended-support-deployments-support-scaling-across-clusters-availability-zones-and-regions"></a>Unterstützen Bereitstellungen von Cloud Services (erweiterter Support) die Skalierung über Cluster, Verfügbarkeitszonen und Regionen hinweg?
Bereitstellungen von Cloud Services (erweiterter Support) können nicht über mehrere Cluster, Verfügbarkeitszonen und Regionen hinweg skaliert werden. 

### <a name="how-can-i-get-the-deployment-id-for-my-cloud-service-extended-support"></a>Wie kann ich die Bereitstellungs-ID für meinen Clouddienst erhalten (erweiterter Support)?
Auf die Bereitstellungs-ID, die als private ID bezeichnet wird, können Sie mit der [CloudServiceInstanceView](https://docs.microsoft.com/rest/api/compute/cloudservices/getinstanceview#cloudserviceinstanceview) -API zugreifen. Sie ist auch auf dem Azure-Portal auf dem Blatt „Rollen und Instanzen“ des Clouddienstes verfügbar (erweiterter Support).

### <a name="are-there-any-pricing-differences-between-cloud-services-classic-and-cloud-services-extended-support"></a>Gibt es Preisunterschiede zwischen Cloud Services (klassisch) und Cloud Services (erweiterter Support)?
Cloud Services (erweiterter Support) verwendet Azure Key Vault und öffentliche Basic-IP-Adressen (ARM). Kunden, die Zertifikate benötigen, müssen Azure Key Vault für die Zertifikatverwaltung verwenden ([weitere Informationen](https://azure.microsoft.com/pricing/details/key-vault/) zu Azure Key Vault-Preisen).  Jede öffentliche IP-Adresse für Cloud Services (erweiterter Support) wird separat abgerechnet ([weitere Informationen](https://azure.microsoft.com/pricing/details/ip-addresses/) zu Preisen für öffentliche IP-Adressen). 
## <a name="resources"></a>Ressourcen 

### <a name="what-resources-linked-to-a-cloud-services-extended-support-deployment-need-to-live-in-the-same-resource-group"></a>Welche Ressourcen, die mit einer Cloud Services-Bereitstellung (erweiterter Support) verknüpft sind, müssen sich in derselben Ressourcengruppe befinden?
Load Balancer, Netzwerksicherheitsgruppen und Routingtabellen müssen sich in der gleichen Region und Ressourcengruppe befinden. 

### <a name="what-resources-linked-to-a-cloud-services-extended-support-deployment-need-to-live-in-the-same-region"></a>Welche Ressourcen, die mit einer Cloud Services-Bereitstellung (erweiterter Support) verknüpft sind, müssen sich in derselben Region befinden?
Key Vault, das virtuelle Netzwerk, öffentliche IP-Adressen, Netzwerksicherheitsgruppen und Routingtabellen müssen sich in der gleichen Region befinden.

### <a name="what-resources-linked-to-a-cloud-services-extended-support-deployment-need-to-live-in-the-same-virtual-network"></a>Welche Ressourcen, die mit einer Cloud Services-Bereitstellung (erweiterter Support) verknüpft sind, müssen sich in derselben virtuellen Netzwerk befinden?
Öffentliche IP-Adressen, Load Balancer, Netzwerksicherheitsgruppen und Routingtabellen müssen sich im gleichen virtuellen Netzwerk befinden. 

## <a name="deployment-files"></a>Bereitstellungsdateien 

### <a name="how-can-i-use-a-template-to-deploy-or-manage-my-deployment"></a>Wie kann ich eine Vorlage verwenden, um meine Bereitstellung bereitzustellen oder zu verwalten?
Vorlagen- und Parameterdateien können mithilfe von REST, PowerShell und der CLI als Parameter übergeben werden. Sie können auch über das Azure-Portal hochgeladen werden.  

### <a name="do-i-need-to-maintain-four-files-now-template-parameter-csdef-cscfg"></a>Muss ich jetzt vier Dateien verwalten? (Vorlagendatei, Parameterdatei, CSDEF-Datei, CSCFG-Datei)
Vorlagen- und Parameterdateien werden nur für die Bereitstellungsautomatisierung verwendet. Wie in Cloud Services (klassisch) können Sie zunächst manuell abhängige Ressourcen und dann eine Bereitstellung von Cloud Services (erweiterter Support) mithilfe von PowerShell, CLI-Befehlen oder über das Portal mit vorhandenen CSDEF- und CSCFG-Dateien erstellen.

### <a name="how-does-my-application-code-change-on-cloud-services-extended-support"></a>Wie ändert sich mein Anwendungscode für Cloud Services (erweiterter Support)?
Für den in einer CSPKG-Datei enthaltenen Anwendungscode sind keine Änderungen erforderlich. Ihre vorhandenen Anwendungen funktionieren auch weiterhin wie zuvor. 

### <a name="does-cloud-services-extended-support-allow-ctp-package-format"></a>Lassen die Clouddienste (erweiterter Support) das CTP (Community Technology Preview)-Paketformat zu?
Das CTP-Paketformat wird in Clouddiensten (erweiterter Support) nicht unterstützt. Die Paketgrößenbeschränkung wurde aber auf 800 MB erweitert.

## <a name="migration"></a>Migration

### <a name="will-cloud-services-extended-support-mitigate-the-failures-due-to-allocation-failures"></a>Werden Fehler aufgrund von Zuordnungsfehlern durch Cloud Services (erweiterter Support) verringert?
Nein, Bereitstellungen von Cloud Services (erweiterter Support) sind wie Cloud Services (klassisch) an einen Cluster gebunden. Daher treten Zuordnungsfehler weiterhin auf, wenn der Cluster voll ist. 

### <a name="when-do-i-need-to-migrate"></a>Wann muss ich eine Migration durchführen? 
Das Einschätzen der erforderlichen Zeit und der Komplexität der Migration hängt von mehreren Variablen ab. Planung ist der effektivste Schritt, um den Umfang der Arbeiten, mögliche Hindernisse und die Komplexität der Migration nachzuvollziehen.

## <a name="networking"></a>Netzwerk 

### <a name="why-cant-i-create-a-deployment-without-virtual-network"></a>Warum kann ich keine Bereitstellung ohne ein virtuelles Netzwerk erstellen?
Virtuelle Netzwerke sind eine erforderliche Ressource für jede Bereitstellung unter Azure Resource Manager. Die Bereitstellung von Cloud Services (erweiterter Support) muss sich in einem virtuellen Netzwerk befinden. 

### <a name="why-am-i-now-seeing-so-many-networking-resources"></a>Warum werden jetzt so viele Netzwerkressourcen angezeigt? 
In Azure Resource Manager werden Komponenten Ihrer Bereitstellung von Cloud Services (erweiterter Support) als Ressource zur besseren Sichtbarkeit und optimierten Kontrolle zur Verfügung gestellt. Der gleiche Typ von Ressourcen wurde in Cloud Services (klassisch) verwendet, diese Ressourcen waren jedoch ausgeblendet. Ein Beispiel für eine solche Ressource ist die öffentliche Load Balancer, bei der es sich nun um eine explizite „schreibgeschützte“ Ressource handelt, die automatisch von der Plattform erstellt wird.

### <a name="what-restrictions-apply-for-a-subnet-with-respective-to-cloud-services-extended-support"></a>Welche Einschränkungen gelten für ein Subnetz bezüglich Cloud Services (erweiterter Support)?
Ein Subnetz mit Bereitstellungen von Cloud Services (erweiterter Support) kann nicht für Bereitstellungen aus anderen Computeprodukten wie Virtual Machines, Virtual Machines-Skalierungsgruppen, Service Fabric usw. freigegeben werden.

### <a name="what-ip-allocation-methods-are-supported-on-cloud-services-extended-support"></a>Welche IP-Zuordnungsmethoden werden für Cloud Services (erweiterter Support) unterstützt?
Cloud Services (erweiterter Support) unterstützt dynamische und statische IP-Zuordnungsmethoden. Auf statische IP-Adressen wird in der CSCFG-Datei als reservierte IP-Adressen verwiesen.

### <a name="why-am-i-getting-charged-for-ip-addresses"></a>Warum werden mir IP-Adressen in Rechnung gestellt?
Die Nutzung von IP-Adressen für Cloud Services (erweiterter Support) wird Kunden in Rechnung gestellt, genauso wie Benutzern IP-Adressen für virtuelle Computer in Rechnung gestellt werden. 

### <a name="can-i-use-a-dns-name-with-cloud-services-extended-support"></a>Kann ich einen DNS-Namen mit Cloud Services (erweiterter Support) verwenden? 
Ja. Cloud Services (erweiterter Support) kann auch ein DNS-Name zugewiesen werden. Bei Azure Resource Manager ist die DNS-Bezeichnung eine optionale Eigenschaft der öffentlichen IP-Adresse, die dem Clouddienst zugewiesen wird. Das Format des DNS-Namens für auf Azure Resource Manager basierende Bereitstellungen ist `<userlabel>.<region>.cloudapp.azure.com`.

### <a name="can-i-update-or-change-the-virtual-network-reference-for-an-existing-cloud-service-extended-support"></a>Kann ich den Verweis auf ein virtuelles Netzwerk für einen vorhandenen Clouddienst (erweiterter Support) aktualisieren oder ändern? 
Nein. Der Verweis auf ein virtuelles Netzwerk ist bei der Erstellung eines Clouddienstes obligatorisch. Für einen vorhandenen Clouddienst kann der Verweis auf ein virtuelles Netzwerk nicht geändert werden. Der Adressbereich des virtuellen Netzwerks selbst kann mithilfe von VNet-APIs geändert werden. 

## <a name="certificates--key-vault"></a>Zertifikate und Key Vault

### <a name="why-do-i-need-to-manage-my-certificates-on-cloud-services-extended-support"></a>Warum muss ich meine Zertifikate unter Cloud Services (erweiterter Support) verwalten?
Cloud Services (erweiterter Support) hat den gleichen Prozess wie andere Computeangebote übernommen, bei denen die Zertifikate in vom Kunden verwalteten Key Vaults gespeichert werden. Dadurch haben Kunden umfassende Kontrolle über Ihre Geheimnisse und Zertifikate. 

### <a name="can-i-use-one-key-vault-for-all-my-deployments-in-all-regions"></a>Kann ich einen Key Vault für alle meine Bereitstellungen in allen Regionen verwenden?
Nein. Key Vault ist eine regionale Ressource, und Kunden benötigen einen Key Vault in jeder Region. Allerdings kann ein Key Vault für alle Bereitstellungen innerhalb einer bestimmten Region verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Einstieg in Cloud Services (erweiterter Support) finden Sie unter [Bereitstellen einer Cloud Services-Instanz (erweiterter Support) mithilfe von PowerShell](deploy-powershell.md).
