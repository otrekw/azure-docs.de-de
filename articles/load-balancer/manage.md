---
title: Azure Load Balancer – Portaleinstellungen
description: Erfahren Sie mehr über Portaleinstellungen für Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/8/2020
ms.author: allensu
ms.openlocfilehash: e1080aea12e70f4312fbee07b063d5a5cfbd1201
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89596214"
---
# <a name="azure-load-balancer-portal-settings"></a>Azure Load Balancer – Portaleinstellungen

Wenn Sie eine Azure Load Balancer-Instanz erstellen, erfahren Sie anhand der Informationen in diesem Artikel mehr über die einzelnen Einstellungen und die richtige Konfiguration.

## <a name="create-load-balancer"></a>Erstellen eines Load Balancers

Azure Load Balancer ist ein Netzwerk-Lastenausgleich, der Datenverkehr über VM-Instanzen im Back-End-Pool verteilt.

### <a name="basics"></a>Grundlagen

Auf der Registerkarte **Grundlagen** der Portalseite zum Erstellen eines Lastenausgleichs sehen Sie folgende Informationen:



| Einstellung |  Details |
| ---------- | ---------- |
| Subscription  | Wählen Sie Ihr Abonnement aus. Diese Auswahl ist das Abonnement, in dem Ihr Lastenausgleich bereitgestellt werden soll. |
| Resource group | Wählen Sie **Neu erstellen** aus, und geben Sie den Namen für Ihre Ressourcengruppe in das Textfeld ein. Wenn Sie bereits über eine Ressourcengruppe verfügen, wählen Sie sie aus. |
| Name | Diese Einstellung ist der Name für Ihre Azure Load Balancer-Instanz. |
| Region | Wählen Sie eine Azure-Region aus, in der Sie Ihren Lastenausgleich bereitstellen möchten. |
| Typ | Es gibt zwei Lastenausgleichstypen: </br> **Intern (privat)** </br> **Öffentlich (extern)** .</br> Ein interner Lastenausgleich (Internal Load Balancer, ILB) leitet Datenverkehr über eine private IP-Adresse an die Back-End-Pool Mitglieder weiter.</br> Ein öffentlicher Lastenausgleich leitet Anforderungen von Clients über das Internet an den Back-End-Pool weiter.</br> Hier erfahren Sie mehr zu [Lastenausgleichstypen](components.md#frontend-ip-configuration-).|
| SKU  | Wählen Sie **Standard** aus. </br> Der Lastenausgleich bietet zwei SKUs: **Basic** und **Standard**. </br> „Basic“ hat eine eingeschränkte Funktionalität. </br> Für Produktionsworkloads wird **Standard** empfohlen. </br> Hier erfahren Sie mehr zu [SKUs](skus.md). |

Wenn Sie **Öffentlich** als Typ auswählen, werden die folgenden Informationen angezeigt:

| Einstellung |  Details |
| ---------- | ---------- |
| Öffentliche IP-Adresse | Wählen Sie **Neu erstellen** aus, um eine öffentliche IP-Adresse für den öffentlichen Lastenausgleich zu erstellen. </br> Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, wählen Sie **Vorhandene verwenden** aus.  |
| Name der öffentlichen IP-Adresse | Der Name der öffentlichen IP-Adresse.|
| SKU der öffentlichen IP-Adresse | Öffentliche IP-Adressen haben zwei SKUs: **Basic** und **Standard**. </br> „Basic“ unterstützt weder Zonenresilienz noch zonale Attribute. </br> Für Produktionsworkloads wird **Standard** empfohlen. </br> Die SKUs von Lastenausgleich und öffentlicher IP-Adresse **müssen übereinstimmen**. |
| Zuordnung | **Statisch** wird automatisch als Standard ausgewählt. </br> Es gibt zwei Typen öffentlicher Basic-IP-Adressen: **Dynamisch** und **Statisch**. </br> Dynamische öffentliche IP-Adressen werden erst nach der Erstellung zugewiesen. </br> IP-Adressen können verloren gehen, wenn die Ressource gelöscht wird. </br> Statische IP-Adressen werden empfohlen. |
| Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus, um einen resilienten Lastenausgleich zu erstellen. </br> Wählen Sie zum Erstellen eines zonalen Lastenausgleichs eine bestimmte Zone aus **1**, **2** oder **3** aus. </br> Standard Load Balancer und öffentliche IP-Adressen unterstützen Zonen. </br> Weitere Informationen zu [Load Balancer und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md) finden Sie hier. </br> Für „Basic“ wird keine Zonenauswahl angezeigt. Von Load Balancer Basic-Instanzen werden keine Zonen unterstützt. |
| Öffentliche IPv6-Adresse hinzufügen | Load Balancer unterstützt **IPv6**-Adressen für Ihr Front-End. </br> Hier erfahren Sie mehr über [Load Balancer und IPv6](load-balancer-ipv6-overview.md).
| Routingpräferenz | Wählen Sie **Microsoft-Netzwerk** aus. </br> Microsoft-Netzwerk bedeutet, dass Datenverkehr über das globale Microsoft-Netzwerk weitergeleitet wird. </br> Internet bedeutet, dass Datenverkehr über das Internetdienstanbieter-Netzwerk weitergeleitet wird. </br> Hier erfahren Sie mehr zu [Routingeinstellungen](../virtual-network/routing-preference-overview.md).|

:::image type="content" source="./media/manage/create-public-load-balancer-basics.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

Wenn Sie **Intern** als Typ auswählen, werden die folgenden Informationen angezeigt:

| Einstellung |  Details |
| ---------- | ---------- |
| Virtuelles Netzwerk | Das virtuelle Netzwerk, zu dem Ihr interner Lastenausgleich gehören soll. </br> Die private Front-End-IP-Adresse, die Sie für Ihren internen Lastenausgleich auswählen, kommt aus diesem virtuellen Netzwerk. |
| IP-Adresszuweisung | Die Optionen sind **Statisch** oder **Dynamisch**. </br> „Statisch“ stellt sicher, dass die IP-Adresse sich nicht ändert. Eine dynamische IP-Adresse könnte sich ändern. |
| Verfügbarkeitszone | Folgende Optionen sind verfügbar: </br> **Zonenredundant** </br> **Zone 1** </br> **Zone 2** </br> **Zone 3** </br> Wählen Sie zum Erstellen eines Lastenausgleichs mit hoher Verfügbarkeit und Resilienz vor Verfügbarkeitszonenausfällen eine **zonenredundante** IP aus. |

:::image type="content" source="./media/manage/create-internal-load-balancer-basics.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="frontend-ip-configuration"></a>Front-End-IP-Konfiguration

Die IP-Adresse Ihres Azure Load Balancers. Sie ist der Kontaktpunkt für Clients. 

Sie können über eine oder mehrere Front-End-IP-Konfigurationen verfügen. Um ein Front-End für den Lastenausgleich zu erstellen, müssen Sie nur den oben aufgeführten Grundlagenabschnitt durcharbeiten. 

Wenn Sie Ihrem Lastenausgleich eine Front-End-IP-Konfiguration hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **Front-End-IP-Konfiguration** aus, und wählen Sie dann **+Hinzufügen** aus.

| Einstellung |  Details |
| ---------- | ---------- |
| Name | Der Name Ihrer Front-End-IP-Konfiguration. |
| IP-Version | Die IP-Adressversion, die ihr Front-End haben soll. </br> Load Balancer unterstützt IPv4- und IPv6-Front-End-Konfigurationen. |
| IP-Typ | Der IP-Typ bestimmt, ob mit Ihrem Front-End eine einzelne IP-Adresse oder ein IP-Adressbereich mit einem Präfix für IP-Adressen verknüpft ist. </br> Ein [Präfix für öffentliche IP-Adressen](../virtual-network/public-ip-address-prefix.md) ist hilfreich, wenn Sie wiederholt eine Verbindung mit demselben Endpunkt herstellen müssen. Mit dem Präfix wird sichergestellt, dass genügend Ports zur Unterstützung bei SNAT-Portproblemen angegeben werden. |
| Öffentliche IP-Adresse (oder Präfix, wenn Sie oben das Präfix gewählt haben) | Wählen oder erstellen Sie eine neue öffentliche IP-Adresse (oder ein Präfix) für das Lastenausgleichs-Front-End. |

:::image type="content" source="./media/manage/frontend.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="backend-pools"></a>Back-End-Pools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen Netzwerkschnittstellen im Back-End-Pool. 

Wenn Sie Ihrem Lastenausgleich einen Back-End-Pool hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **Back-End-Pools** aus, und wählen Sie dann **+Hinzufügen** aus.

| Einstellung | Details |
| ---------- |  ---------- |
| Name | Der Name des Back-End-Pools. |
| Virtuelles Netzwerk | Das virtuelle Netzwerk, zu dem Ihre Back-End-Instanzen gehören. |
| IP-Version | Ihre Optionen sind **IPv4** oder **IPv6**. |

Sie können dem Back-End-Pool Ihrer Azure Load Balancer-Instanz VMs oder VM-Skalierungsgruppen hinzufügen. Erstellen Sie zuerst die VMs oder VM-Skalierungsgruppen. Fügen Sie sie dann im Portal dem Lastenausgleich hinzu.

:::image type="content" source="./media/manage/backend.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="health-probes"></a>Integritätstests

Mit einem Integritätstest wird der Status Ihrer Back-End-VMs oder -Instanzen überwacht. Der Integritätsteststatus bestimmt, wann neue Verbindungen auf der Grundlage von Integritätsprüfungen an eine Instanz gesendet werden. 

Wenn Sie Ihrem Lastenausgleich einen Integritätstest hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **Integritätstests** aus, und wählen Sie dann **+Hinzufügen** aus.

| Einstellung | Details |
| ---------- | ---------- |
| Name | Der Name des Integritätstests. |
| Protocol | Das von Ihnen gewählte Protokoll bestimmt den Typ der Überprüfung, mit dem ermittelt wird, ob die Back-End-Instanzen fehlerfrei sind. </br> Folgende Optionen sind verfügbar: </br> **TCP** </br> **HTTPS** </br> **HTTP** </br> Stellen Sie sicher, dass Sie das richtige Protokoll verwenden. Diese Auswahl hängt von der Art der Anwendung ab. </br> Die Konfiguration des Integritätstests und der Testantworten bestimmt, welche Back-End-Poolinstanzen neue Flows empfangen. </br> Anhand von Integritätstests können Sie einen Fehler in einer Anwendung für einen Back-End-Endpunkt erkennen. </br> Hier erfahren Sie mehr zu [Integritätstests](load-balancer-custom-probe-overview.md). |
| Port | Der Zielport für den Integritätstest. </br> Diese Einstellung ist der Port auf der Back-End-Instanz, mit dem der Integritätstest die Integrität der Instanz bestimmt. |
| Intervall | Die Anzahl der Sekunden zwischen Testversuchen. </br> Mit dem Intervall wird bestimmt, wie häufig der Integritätstest versucht, die Back-End-Instanz zu erreichen. </br> Wenn Sie 5 auswählen, wird der zweite Testversuch nach 5 Sekunden durchgeführt usw. |
| Fehlerhafter Schwellenwert | Die Anzahl aufeinander folgender Testfehler, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.</br> Wenn Sie 2 auswählen, werden nach zwei aufeinanderfolgenden Fehlern keine neuen Flows auf diese Back-End-Instanz festgelegt. |

:::image type="content" source="./media/manage/health-probe.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="load-balancing-rules"></a>Lastenausgleichsregeln

Definiert, wie eingehender Datenverkehr auf alle Instanzen innerhalb des Back-End-Pools verteilt wird. Eine Lastenausgleichsregel ordnet mehreren Back-End-IP-Adressen und Ports eine bestimmte Front-End-IP-Konfiguration und einen Port zu.

Wenn Sie Ihrem Lastenausgleich eine Lastenausgleichsregel hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **Lastenausgleichsregeln** aus, und wählen Sie dann **+Hinzufügen** aus.
    
| Einstellung | Details |
| ---------- | ---------- |
| Name | Der Name der Lastenausgleichsregel. |
| IP-Version | Ihre Optionen sind **IPv4** oder **IPv6**.  |
| Front-End-IP-Adresse | Wählen Sie die Front-End-IP-Adresse aus. </br> Die Front-End-IP-Adresse Ihres Lastenausgleichs, dem Sie die Lastenausgleichsregel zuordnen möchten.|
| Protocol | Azure Load Balancer ist ein Netzwerk-Lastenausgleich auf Schicht 4. </br> Die Optionen sind wie folgt: **TCP** oder **UDP**. |
| Port | Diese Einstellung ist der Port, der der Front-End-IP-Adresse zugeordnet ist, an die der Datenverkehr auf Basis dieser Lastenausgleichsregel verteilt werden soll. |
| Back-End-Port | Diese Einstellung ist der Port auf den Instanzen im Back-End-Pool, an den der Lastenausgleich Datenverkehr senden soll. Diese Einstellung kann mit dem Front-End-Port identisch sein, oder anders, wenn Sie die Flexibilität für Ihre Anwendung benötigen. |
| Back-End-Pool | Der Back-End-Pool, auf den diese Lastenausgleichsregel angewendet werden soll. |
| Integritätstest | Der Integritätstest, den Sie erstellt haben, um den Integritätsstatus der Instanzen im Back-End-Pool zu ermitteln. </br> Nur fehlerfreie Instanzen empfangen neuen Datenverkehr. |
| Sitzungspersistenz |  Folgende Optionen sind verfügbar: </br> **None** </br> **Client-IP** </br> **Client-IP und Protokoll**</br> </br> Verwalten den Datenverkehr zwischen einem Client und demselben virtuellen Computer im Back-End-Pool. Dieser Datenverkehr wird für die Dauer der Sitzung beibehalten. </br> **Keine** gibt an, dass aufeinander folgende Anforderungen vom selben Client von jedem virtuellen Computer verarbeitet werden können. </br> **Client-IP** gibt an, dass aufeinanderfolgende Anforderungen von derselben Client-IP-Adresse vom selben virtuellen Computer verarbeitet werden. </br> **Client-IP und Protokoll** stellt sicher, dass aufeinanderfolgende Anforderungen von derselben Client-IP-Adresse und demselben Protokoll vom selben virtuellen Computer verarbeitet werden. </br> Erfahren Sie mehr zu [Verteilungsmodi](load-balancer-distribution-mode.md). |
| Leerlaufzeitüberschreitung (Minuten) | Eine **TCP**- oder **HTTP**-Verbindung wird offen gelassen, unabhängig von Clients, die Erhaltungsmeldungen senden. |  
| TCP-Zurücksetzung | Der Lastenausgleich kann **TCP-Zurücksetzungen** senden, damit das Anwendungsverhalten vorhersagbarer wird, wenn sich die Verbindung im Leerlauf befindet. </br> Hier erfahren Sie mehr zur [TCP-Zurücksetzung](load-balancer-tcp-reset.md).|
| Unverankerte IP | Floating IP ist die in Azure verwendete Benennung für die Komponente **Direct Server Return (DSR)** . </br> DSR besteht aus zwei Teilen: <br> 1. Datenflusstopologie </br> 2. Ein IP-Adressen-Zuordnungsschema auf Plattformebene. </br></br> Azure Load Balancer wird immer in einer DSR-Datenflusstopologie betrieben, unabhängig davon, ob Floating IP aktiviert ist. </br> Dieser Vorgang bedeutet, dass der ausgehende Teil eines Datenflusses immer ordnungsgemäß so umgeschrieben wird, dass er direkt wieder an den Ursprung übermittelt wird. </br> Ohne Floating IP wird von Azure ein herkömmliches Zuordnungsschema für IP-Adressen für den Lastenausgleich (IP-Adresse für VM-Instanzen) verfügbar gemacht. </br> Durch die Aktivierung von Floating IP wird die Zuordnung der IP-Adressen in die Front-End-IP-Adresse des Lastenausgleichsmoduls geändert, um für zusätzliche Flexibilität zu sorgen. </br> Weitere Informationen finden Sie unter [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).|
| Erstellen impliziter Ausgangsregeln | Wählen Sie also **Nein**. </br> Standard: **disableOutboundSnat = false**  </br> In diesem Fall erfolgt der ausgehende Datenverkehr über dieselbe Front-End-IP. </br></br> **disableOutboundSnat = true** </br>In diesem Fall werden Ausgangsregeln für ausgehenden Datenverkehr benötigt. |

:::image type="content" source="./media/manage/load-balancing-rule.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="inbound-nat-rules"></a>Eingehende NAT-Regeln

Eine NAT-Regel für eingehenden Datenverkehr leitet eingehenden Datenverkehr weiter, der an eine Kombination aus Front-End-IP-Adresse und Port gesendet wird. 

Der Datenverkehr wird an einen bestimmten virtuellen Computer oder eine Instanz im Back-End-Pool gesendet. Der Portweiterleitung liegt die gleiche hashbasierte Verteilung zugrunde wie dem Lastenausgleich.

Wenn Ihr Szenario Remotedesktopprotokoll- oder Secure Shell-Sitzungen (RDP, SSH) zum Trennen von VM-Instanzen innerhalb eines Back-End-Pools erfordert. Ports können mehrere interne Endpunkte unter derselben Front-End-IP-Adresse zugeordnet werden. 

Sie können die Front-End-IP-Adressen verwenden, um für Ihre VMs die Remoteverwaltung ohne zusätzliche Jumpbox durchzuführen.

Wenn Sie Ihrem Lastenausgleich eine NAT-Regel für eingehenden Datenverkehr hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **NAT-Regeln für eingehenden Datenverkehr** aus, und wählen Sie dann **+Hinzufügen** aus.

| Einstellung | Details |
| ---------- | ---------- |
| Name | Der Name Ihrer NAT-Regel für eingehenden Datenverkehr. |
| Front-End-IP-Adresse | Wählen Sie die Front-End-IP-Adresse aus. </br> Die Front-End-IP-Adresse Ihres Lastenausgleichs, dem Sie die NAT-Regel für eingehenden Datenverkehr	zuordnen möchten. |
| IP-Version | Ihre Optionen sind IPv4 und IPv6. |
| Dienst | Der Typ des Diensts, den Sie auf der Azure Load Balancer-Instanz ausführen. </br> Wenn Sie hier eine Auswahl treffen, werden die Portdaten entsprechend aktualisiert. |
| Protocol | Azure Load Balancer ist ein Netzwerk-Lastenausgleich auf Schicht 4. </br> Die Optionen sind wie folgt: TCP oder UDP. |
| Leerlaufzeitüberschreitung (Minuten) | Eine TCP- oder HTTP-Verbindung wird offen gelassen, unabhängig von Clients, die Erhaltungsmeldungen senden. |
| TCP-Zurücksetzung | Der Lastenausgleich kann TCP-Zurücksetzungen senden, damit das Anwendungsverhalten vorhersagbarer wird, wenn sich die Verbindung im Leerlauf befindet. </br> Hier erfahren Sie mehr zur [TCP-Zurücksetzung](load-balancer-tcp-reset.md). |
| Port | Diese Einstellung ist der Port, der der Front-End-IP-Adresse zugeordnet ist, an die der Datenverkehr auf Basis dieser NAT-Regel für eingehenden Datenverkehr verteilt werden soll. |
| Virtueller Zielcomputer | Der VM-Teil des Back-End-Pools, dem diese Regel zugeordnet werden soll. |
| Portzuordnung | Diese Einstellung kann basierend auf der Anwendungseinstellung standardmäßig oder benutzerdefiniert sein. |

:::image type="content" source="./media/manage/inbound-nat-rule.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="outbound-rules"></a>Ausgangsregeln

Ausgangsregeln für den Lastenausgleich konfigurieren das ausgehende SNAT für virtuelle Computer im Back-End-Pool.

Wenn Sie Ihrem Lastenausgleich eine Regel für ausgehenden Datenverkehr hinzufügen möchten, navigieren Sie im Azure-Portal zu Ihrem Lastenausgleich, wählen Sie **Regeln für ausgehenden Datenverkehr** aus, und wählen Sie dann **+Hinzufügen** aus.

| Einstellung | Details |
| ------- | ------ |
| Name | Der Name der Regel für ausgehenden Datenverkehr. |
| Front-End-IP-Adresse | Wählen Sie die Front-End-IP-Adresse aus. </br> Die Front-End-IP-Adresse Ihres Lastenausgleichs, dem Sie die Regel für ausgehenden Datenverkehr	zuordnen möchten. |
| Protocol | Azure Load Balancer ist ein Netzwerk-Lastenausgleich auf Schicht 4. </br> Die Optionen sind wie folgt: **Alle**, **TCP** oder **UDP**. |
| Leerlaufzeitüberschreitung (Minuten) | Eine **TCP**- oder **HTTP**-Verbindung wird offen gelassen, unabhängig von Clients, die Erhaltungsmeldungen senden. |
| TCP-Zurücksetzung | Der Lastenausgleich kann **TCP-Zurücksetzungen** senden, damit das Anwendungsverhalten vorhersagbarer wird, wenn sich die Verbindung im Leerlauf befindet. </br> Hier erfahren Sie mehr zur [TCP-Zurücksetzung](load-balancer-tcp-reset.md). |
| Back-End-Pool | Der Back-End-Pool, auf den diese Ausgangsregel angewendet werden soll. |

### <a name="port-allocation"></a>Portzuordnung

| Einstellung | Details |
| ------- | ------ |
| Portzuordnung | Sie sollten **Anzahl ausgehender Ports manuell auswählen** auswählen.|

### <a name="outbound-ports"></a>Ausgehende Ports

| Einstellung | Details |
| ------- | ------ |
| Auswählen nach | Wählen Sie **Ports pro Instanz** aus. |
| Ports pro Instanz | Geben Sie **10.000** ein. |

:::image type="content" source="./media/manage/outbound-rule.png" alt-text="Erstellen eines öffentlichen Lastenausgleichs." border="true":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die unterschiedlichen Begriffe und Einstellungen im Azure-Portal für Azure Load Balancer kennengelernt.

* [Erfahren Sie mehr](./load-balancer-overview.md) über Azure Load Balancer.
* [Häufig gestellte Fragen](./load-balancer-faqs.md) zu Azure Load Balancer.