---
title: Häufig gestellte Fragen (FAQ) zu privaten Azure Purview-Endpunkten
description: In diesem Artikel werden häufig gestellte Fragen zu privaten Azure Purview-Endpunkten beantwortet.
author: zeinam
ms.author: zeinam
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 05/11/2021
ms.openlocfilehash: 13d9f65a812dcf9cfc45b84758f351e301827173
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110104132"
---
# <a name="frequently-asked-questions-faq-about-azure-purview-private-endpoints"></a>Häufig gestellte Fragen (FAQ) zu privaten Azure Purview-Endpunkten

In diesem Artikel werden Fragen beantwortet, die von Kunden und Außendienstteams häufig zu Azure Purview-Netzwerkkonfigurationen mit [Azure Private Link](../private-link/private-link-overview.md) gestellt werden. Es werden Fragen zu Azure Purview-Firewalleinstellungen, privaten Endpunkten, DNS-Konfigurationen und ähnlichen Konfigurationen geklärt.

Weitere Informationen zum Einrichten von Azure Purview mit Private Link finden Sie unter [Verwenden privater Endpunkte für Ihr Purview-Konto](./catalog-private-link.md).

## <a name="common-questions"></a>Häufig gestellte Fragen

### <a name="what-is-purpose-of-deploying-azure-purview-account-private-endpoint"></a>Welchem Zweck dient die Bereitstellung des privaten Azure Purview-Endpunkts „Account“?

Mit dem privaten Azure Purview-Endpunkt _Account_ wird eine zusätzliche Sicherheitsschicht hinzugefügt, indem Szenarios ermöglicht werden, bei denen nur Clientaufrufe aus dem VNet für den Zugriff auf das Konto zulässig sind. Dieser private Endpunkt ist auch eine Voraussetzung für den privaten Endpunkt „Portal“.
    
### <a name="what-is-purpose-of-deploying--azure-purview-portal-private-endpoint"></a>Welchem Zweck dient die Bereitstellung des privaten Azure Purview-Endpunkts „Portal“?
Mit dem privaten Azure Purview-Endpunkt _Portal_ wird die private Konnektivität zu Azure Purview Studio ermöglicht.
    
### <a name="what-is-purpose-of-deploying-azure-purview-ingestion-private-endpoints"></a>Welchem Zweck dient die Bereitstellung des privaten Azure Purview-Endpunkts „Ingestion“?

Azure Purview kann Datenquellen in Azure oder in einer lokalen Umgebung mit privaten Endpunkten für die Erfassung überprüfen. Drei zusätzliche Ressourcen für private Endpunkte werden bereitgestellt und mit verwalteten Azure Purview-Ressourcen verknüpft, wenn private Endpunkte für die Erfassung erstellt werden:
- Ein mit dem verwalteten Azure Purview-Speicherkonto verknüpftes _Blob_ 
- Eine mit dem verwalteten Azure Purview-Speicherkonto verknüpfte _Warteschlange_  
- Ein mit dem verwalteten Event-Hub-Namespace von Azure Purview verknüpfter _Namespace_
     
### <a name="can-i-scan-data-through-public-endpoint-if-private-endpoint-is-enabled-on-my-azure-purview-account"></a>Kann ich Daten über den öffentlichen Endpunkt überprüfen, wenn der private Endpunkt für mein Azure Purview-Konto aktiviert ist?

Ja, Datenquellen, die nicht über einen privaten Endpunkt verbunden sind, können mithilfe eines öffentlichen Endpunkts überprüft werden, während Azure Purview für die Verwendung eines privaten Endpunkts konfiguriert ist.
    
### <a name="can-i-scan-data-through-service-endpoint-if-private-endpoint-is-enabled"></a>Kann ich Daten über den Dienstendpunkt überprüfen, wenn der private Endpunkt aktiviert ist?

Ja, Datenquellen, die nicht über einen privaten Endpunkt verbunden sind, können mithilfe eines Dienstendpunkts überprüft werden, während Azure Purview für die Verwendung eines privaten Endpunkts konfiguriert ist.
Aktivieren Sie die Option „Vertrauenswürdige Microsoft-Dienste zulassen“, um auf Ressourcen in der Dienstendpunktkonfiguration der Datenquellenressource in Azure zuzugreifen. Wenn Sie beispielsweise eine Azure Blob Storage-Instanz scannen, in der die Einstellungen für Firewalls und virtuelle Netzwerke auf _Ausgewählte Netzwerke_ festgelegt sind, müssen Sie _Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben_ als Ausnahme aktivieren. 
    
###  <a name="can-i-access-azure-purview-studio-from-public-network-if-public-network-access-is-set-to-deny-in-azure-purview-account-networking"></a>Kann ich über ein öffentliches Netzwerk auf Azure Purview Studio zugreifen, wenn der Zugriff auf öffentliche Netzwerke in den Netzwerkeinstellungen des Azure Purview-Kontos auf „Verweigern“ festgelegt ist?

Nein. Wenn Sie über den öffentlichen Endpunkt eine Verbindung mit Azure Purview herstellen und _Öffentlicher Netzwerkzugriff_ auf _Verweigern_ festgelegt ist, wird eine Fehlermeldung wie die folgende ausgegeben:

_Keine Autorisierung für den Zugriff auf dieses Purview-Konto_
_Dieses Purview-Konto befindet sich hinter einem privaten Endpunkt. Greifen Sie über einen Client im selben virtuellen Netzwerk (VNet) auf das Konto zu, das für den privaten Endpunkt des Purview-Kontos konfiguriert wurde._

In diesem Fall müssen Sie zum Starten von Azure Purview Studio einen Computer verwenden, der entweder im selben VNet wie der private Endpunkt des Azure Purview-Portals bereitgestellt wurde, oder eine VM, die mit einem Unternehmensnetzwerk verbunden ist, in dem Hybridkonnektivität zulässig ist.

### <a name="is-it-possible-to-restrict-access-to-azure-purview-managed-storage-account-and-event-hub-namespace-for-private-endpoint-ingestion-only-but-keep-portal-access-enabled-for-end-users-across-the-web"></a>Ist es möglich, den Zugriff auf das verwaltete Azure Purview-Speicherkonto und den Event-Hub-Namespace (nur für die Erfassung privater Endpunkte) einzuschränken, aber den Portalzugriff für Endbenutzer über das Internet aktiviert zu lassen?

Nein. Wenn Sie _Öffentlicher Netzwerkzugriff_ auf _Verweigern_ festlegen, wird der Zugriff auf das verwaltete Azure Purview-Speicherkonto und den Event-Hub-Namespace automatisch nur für die Erfassung privater Endpunkte festgelegt. Wenn Sie _Öffentlicher Netzwerkzugriff_ auf _Zulassen_ festlegen, wird der Zugriff auf das verwaltete Azure Purview-Speicherkonto und den Event-Hub-Namespace automatisch auf _Alle Netzwerke_ festgelegt. Sie können die Erfassung privater Endpunkte für das verwaltete Speicherkonto oder den Event-Hub-Namespace nicht manuell ändern.
    
### <a name="if-public-network-access-is-set-to-allow-does-it-mean-the-managed-storage-account-and-event-hub-can-be-publicly-accessible"></a>Sind das verwaltete Speicherkonto und der Event Hub öffentlich zugänglich, wenn der öffentliche Netzwerkzugriff auf „Zulassen“ festgelegt ist?

Nein. Da es sich beim verwalteten Azure Purview-Speicherkonto und dem Event-Hub-Namespace um geschützte Ressourcen handelt, ist der Zugriff auf Azure Purview beschränkt. Diese Ressourcen werden mit einer Verweigerungszuweisung für alle Prinzipale bereitgestellt, wodurch verhindert wird, dass Anwendungen, Benutzer oder Gruppen Zugriff auf sie erhalten.

Weitere Informationen zur Azure-Verweigerungszuweisung finden Sie unter [Grundlegendes zu Azure-Verweigerungszuweisungen](../role-based-access-control/deny-assignments.md).
    
### <a name="what-are-the-supported-authentication-type-when-using-private-endpoint"></a>Welche Authentifizierungsarten werden bei Verwendung eines privaten Endpunkts unterstützt?

Azure Key Vault oder Dienstprinzipale
  
### <a name="what-are-private-dns-zones-required-for-azure-purview-for-private-endpoint"></a>Welche privaten DNS-Zonen werden für Azure Purview für private Endpunkte benötigt?

**Für Azure Purview-Ressourcen:** 
- `privatelink.purview.azure.com`

**Für verwaltete Azure Purview-Ressourcen:**
- `privatelink.blob.core.windows.net`
- `privatelink.queue.core.windows.net`
- `privatelink.servicebus.windows.net`
    
### <a name="do-i-have-to-use-a-dedicated-virtual-network-and-dedicated-subnet-when-deploying-azure-purview-private-endpoints"></a>Muss ich bei der Bereitstellung privater Azure Purview-Endpunkte ein dediziertes virtuelles Netzwerk und ein dediziertes Subnetz verwenden?

Nein, aber die Option _PrivateEndpointNetworkPolicies_ muss im Zielsubnetz deaktiviert sein, bevor die privaten Endpunkte bereitgestellt werden.
Sie sollten Azure Purview in einem virtuellen Netzwerk bereitstellen, in dem Netzwerkkonnektivität mit Datenquellen-VNets über VNET-Peering und Zugriff auf das lokale Netzwerk besteht, falls Sie Datenquellen standortübergreifend scannen möchten.
    
Lesen Sie mehr über das [Deaktivieren von Netzwerkrichtlinien für private Endpunkte](../private-link/disable-private-endpoint-network-policy.md).

### <a name="can-i-deploy-azure-purview-private-endpoints-and-use-existing-private-dns-zones-in-my-subscription-to-register-the-a-records"></a>Kann ich private Azure Purview-Endpunkte bereitstellen und vorhandene private DNS-Zonen in meinem Abonnement verwenden, um die A-Datensätze zu registrieren?

Ja. Ihre DNS-Zonen für private Endpunkte können in einem Hub- oder Datenverwaltungsabonnement für alle internen DNS-Zonen zentralisiert werden, die für Azure Purview und sämtliche Datenquellen-Datensätze erforderlich sind. Diese Methode wird empfohlen, damit Azure Purview Datenquellen über die internen IP-Adressen ihres privaten Endpunkts auflösen kann.

Darüber hinaus ist es erforderlich, eine [virtuelle Netzwerkverbindung](../dns/private-dns-virtual-network-links.md) für das VNet der vorhandenen privaten DNS-Zone einzurichten.
    
### <a name="can-i-use-azure-integration-runtime-to-scan-data-sources-through-private-endpoint"></a>Kann ich die Azure Integration Runtime verwenden, um Datenquellen über einen privaten Endpunkt zu scannen? 

Nein. Sie müssen eine selbstgehostete Integration Runtime bereitstellen und registrieren, um Daten mithilfe privater Konnektivität zu überprüfen. Azure Key Vault oder ein Dienstprinzipal muss als Authentifizierungsmethode für Datenquellen verwendet werden.
    
### <a name="what-are-the-outbound-ports-and-firewall-requirements-for-virtual-machines-with-self-hosted-integration-runtime-for-azure-purview-when-using-private-endpoint"></a>Welche Anforderungen gelten für ausgehende Ports und Firewalls für VMs mit selbstgehosteter Integration Runtime für Azure Purview, wenn ein privater Endpunkt verwendet wird?

Die VMs, auf denen die selbstgehostete Integration Runtime bereitgestellt wird, müssen über ausgehenden Zugriff auf Azure-Endpunkte und die private Azure Purview-IP-Adresse über Port 443 verfügen. 
    
### <a name="do-i-need-to-enable-outbound-internet-access-from-the-virtual-machine-running-self-hosted-integration-runtime-if-private-endpoint-is-enabled"></a>Muss ich ausgehenden Internetzugriff von der VM aktivieren, auf der die selbstgehostete Integration Runtime ausgeführt wird, wenn der private Endpunkt aktiviert ist?

Nein. Es wird jedoch erwartet, dass die VM, auf der die selbstgehostete Integration Runtime ausgeführt wird, über die interne IP-Adresse und Port 443 eine Verbindung mit Azure Purview herstellen kann. Verwenden Sie gängige Tools wie „nslookup.exe“ für die Namensauflösung und Konnektivitätstests und Test-NetConnection für die Problembehandlung.
    
### <a name="why-do-i-receive-the-following-error-message-when-i-try-to-launch-azure-purview-studio-from-my-machine"></a>Warum wird die folgende Fehlermeldung angezeigt, wenn ich versuche, Azure Purview Studio von meinem Computer aus zu starten?

_Dieses Purview-Konto befindet sich hinter einem privaten Endpunkt. Greifen Sie über einen Client im selben virtuellen Netzwerk (VNet) auf das Konto zu, das für den privaten Endpunkt des Purview-Kontos konfiguriert wurde._

Wahrscheinlich wurde Ihr Azure Purview-Konto mithilfe von Azure Private Link bereitgestellt, und der öffentliche Zugriff ist in Ihrem Konto deaktiviert. Deshalb müssen Sie Azure Purview Studio über den Browser einer VM aufrufen, die über interne Netzwerkkonnektivität mit Azure Purview verfügt.

Wenn Sie eine Verbindung von einer VM hinter einem Hybridnetzwerk herstellen oder einen mit Ihrem VNet verbundenen Bastioncomputer verwenden, sollten Sie gängige Tools wie „nslookup.exe“ für die Namensauflösung und Konnektivitätstests und Test-NetConnection für die Problembehandlung nutzen.

1. Überprüfen Sie, ob Sie die folgenden Adressen über die privaten IP-Adressen Ihres Azure Purview-Kontos auflösen können:

   - `Web.Purview.Azure.com`
   - `<YourPurviewAccountName>.Purview.Azure.com`

2. Überprüfen Sie die Netzwerkkonnektivität mit Ihrem Azure Purview-Konto mithilfe des folgenden PowerShell-Befehls:

   ```powershell
   Test-NetConnection -ComputerName <YourPurviewAccountName>.Purview.Azure.com -Port 443
   ```

3. Überprüfen Sie Ihre standortübergreifende DNS-Konfiguration, wenn Sie Ihre eigene DNS-Auflösungsinfrastruktur verwenden. 

   Weitere Informationen zu den DNS-Einstellungen für private Endpunkte finden Sie unter [DNS-Konfiguration für private Azure-Endpunkte](../private-link/private-endpoint-dns.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Einrichten von Azure Purview mit Private Link finden Sie unter [Verwenden privater Endpunkte für Ihr Purview-Konto](./catalog-private-link.md).
