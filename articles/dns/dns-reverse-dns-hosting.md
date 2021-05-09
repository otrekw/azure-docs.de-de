---
title: Hosten von Reverse-DNS-Lookupzonen in Azure DNS
description: Erfahren Sie, wie Sie mit Azure DNS die Reverse-DNS-Lookupzonen für Ihre IP-Adressbereiche hosten.
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/29/2021
ms.author: rohink
ms.openlocfilehash: 6db63f564d481dd57d1bdbf090678616989f83e1
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108317007"
---
# <a name="host-reverse-dns-lookup-zones-in-azure-dns"></a>Hosten von Reverse-DNS-Lookupzonen in Azure DNS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In diesem Artikel wird erläutert, wie Sie Reverse-DNS-Lookupzonen für Ihre zugewiesenen IP-Adressbereiche mit Azure DNS hosten können. Die durch Reverse-Lookupzonen dargestellten IP-Adressbereiche müssen Ihrer Organisation zugewiesen werden – in der Regel durch Ihren Internetdienstanbieter.

Informationen zum Konfigurieren von Reverse-DNS für eine Azure-eigene IP-Adresse, die Ihrem Azure-Dienst zugewiesen wurde, finden Sie unter [Konfigurieren von Reverse-DNS für in Azure gehostete Dienste](dns-reverse-dns-for-azure-services.md).

Bevor Sie diesen Artikel lesen, sollten Sie sich mit der [Übersicht über Reverse-DNS](dns-reverse-dns-overview.md) vertraut machen, die in Azure unterstützt wird.

In diesem Artikel erfahren Sie, wie Sie Ihre erste Reverse-Lookup-DNS-Zone und den zugehörigen Eintrag über das Azure-Portal, mithilfe von Azure PowerShell, über die klassische Befehlszeilenschnittstelle und die Azure-Befehlszeilenschnittstelle erstellen können.

## <a name="create-a-reverse-lookup-dns-zone"></a>Erstellen einer Reverse-DNS-Lookupzone

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen** aus, und suchen Sie nach **DNS-Zone**. Wählen Sie dann **Erstellen** aus.

      :::image type="content" source="./media/dns-operations-dnszones-portal/search-dns-zone.png" alt-text="Screenshot zum Erstellen einer Ressourcensuche für eine Reverse-DNS-Zone":::

1. Wählen Sie auf der Seite **DNS-Zone erstellen** die folgenden Einstellungen aus, oder geben Sie sie ein:

    | Einstellung | Details |
    | --- | --- |
    | **Abonnement** | Wählen Sie ein Abonnement aus, in dem die DNS-Zone erstellt werden soll.|
    | **Ressourcengruppe** | Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine neue. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/management/overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
    | **Name** | Geben Sie einen Namen für die DNS-Zone ein. Der Name der Zone wird für IPv4- und IPv6-Präfixe unterschiedlich definiert. Gehen Sie zum Benennen der Zone gemäß den Anweisungen für [IPv4](#ipv4) oder [IPv6](#ipv6) vor.  |
    | **Location** | Wählen Sie den Speicherort für die Ressourcengruppe aus. Wenn Sie eine zuvor erstellte Ressourcengruppe verwenden, ist der Speicherort bereits ausgewählt. |

1. Wählen Sie **Überprüfen und erstellen** und nach Abschluss der Überprüfung **Erstellen** aus.

### <a name="ipv4"></a>IPv4

Der Name einer IPv4-Reverse-Lookupzone basiert auf dem IP-Adressbereich, den sie darstellt. Er sollte das folgende Format aufweisen: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Beispiele finden Sie unter [Übersicht über Reverse-DNS](dns-reverse-dns-overview.md#ipv4) für IPv4.

> [!NOTE]
> Wenn Sie klassenlose Reverse-DNS-Lookupzonen in Azure DNS erstellen, müssen Sie im Zonennamen einen Bindestrich (`-`) – statt eines Schrägstrichs (`/`) – verwenden.
>
> Beispiel: Für den IP-Adressbereich „192.0.2.128/26“ verwenden Sie `128-26.2.0.192.in-addr.arpa` als Zonennamen, nicht `128/26.2.0.192.in-addr.arpa`.
>
> Der DNS-Standard unterstützt zwar beide Methoden, Azure DNS unterstützt jedoch keine DNS-Zonennamen mit Schrägstrich (`/`).

Das folgende Beispiel zeigt, wie Sie über das Azure-Portal eine Reverse-DNS-Zone der Klasse C mit dem Namen `2.0.192.in-addr.arpa` in Azure DNS erstellen:

:::image type="content" source="./media/dns-reverse-dns-hosting/ipv4-arpa-zone.png" alt-text="Screenshot zum Erstellen einer IPv4-Arpa-DNS-Zone":::

Die folgenden Beispiele zeigen, wie diese Aufgabe mithilfe von Azure PowerShell und über die Azure-Befehlszeilenschnittstelle ausgeführt wird.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
New-AzDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName mydnsresourcegroup
```

#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns zone create mydnsresourcegroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network dns zone create -g mydnsresourcegroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Der Name einer IPv6-Reverse-Lookupzone sollte folgendes Format aufweisen: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Beispiele finden Sie unter [Übersicht über Reverse-DNS](dns-reverse-dns-overview.md#ipv6) für IPv6.


Das folgende Beispiel zeigt, wie Sie eine IPv6-Reverse-DNS-Lookupzone mit dem Namen `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS über das Azure-Portal erstellen:

:::image type="content" source="./media/dns-reverse-dns-hosting/ipv6-arpa-zone.png" alt-text="Screenshot zum Erstellen einer IPv6-Arpa-DNS-Zone":::

Die folgenden Beispiele zeigen, wie diese Aufgabe mithilfe von Azure PowerShell und über die Azure-Befehlszeilenschnittstelle ausgeführt wird.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName mydnsresourcegroup
```

#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns zone create mydnsresourcegroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
az network dns zone create -g mydnsresourcegroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Delegieren einer Reverse-DNS-Lookupzone

Sobald die Reverse-DNS-Lookupzone erstellt wurde, müssen Sie sicherstellen, dass die Zone von der übergeordneten Zone delegiert wird. Die DNS-Delegierung ermöglicht es dem DNS-Namensauflösungsprozess, nach den Namenservern zu suchen, die Ihre Reverse-DNS-Lookupzone hosten. Diese Namenserver können dann auf DNS-Reverse-Abfragen für die IP-Adressen in Ihrem Adressbereich reagieren.

Für Forward-Lookupzonen wird der Vorgang der Delegierung einer DNS-Zone unter [Delegieren einer Domäne an Azure DNS](dns-delegate-domain-azure-dns.md) beschrieben. Die Delegierung für Reverse-Lookupzonen funktioniert auf dieselbe Weise. Der einzige Unterschied besteht darin, dass Sie die Namenserver beim ISP (Internet Service Provider, Internetdienstanbieter) konfigurieren müssen. Weil der ISP Ihren IP-Adressbereich verwaltet, muss er die Namenserver statt der Domänennamen-Registrierungsstelle aktualisieren.

## <a name="create-a-dns-ptr-record"></a>Erstellen eines DNS-PTR-Eintrags

### <a name="ipv4"></a>IPv4

Im folgenden Beispiel wird der Prozess zur Erstellung eines PTR-Eintrags für eine Reverse-DNS-Zone in Azure DNS erläutert. Weitere Informationen zu Eintragstypen oder zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und -Eintragssätzen](dns-operations-recordsets-portal.md).

1. Wählen Sie oben auf der Übersichtsseite *DNS-Zone* die Option **+ Eintragssatz** aus, um den Bereich *Eintragssatz hinzufügen* zu öffnen.

    :::image type="content" source="./media/dns-reverse-dns-hosting/create-record-set-ipv4.png" alt-text="Screenshot zum Erstellen eines IPv4-Zeigereintragssatzes":::

1. Der Name des Eintragssatzes für einen PTR-Eintrag ist der restliche Teil der IPv4-Adresse in umgekehrter Reihenfolge. 

    In diesem Beispiel wurden die ersten drei Oktette bereits als Teil des Zonennamens `.2.0.192` aufgefüllt. Deshalb ist nur das letzte Oktett im Feld **Name** erforderlich. So geben Sie beispielsweise Ihrem Eintragssatz für eine Ressource mit der IP-Adresse **den Namen** 15`192.0.2.15`.  

    :::image type="content" source="./media/dns-reverse-dns-hosting/create-ipv4-ptr.png" alt-text="Screenshot zum Erstellen eines IPv4-Zeigereintrags":::

1. Wählen Sie unter *Typ* die Option **PTR** aus.

1. Geben Sie für *DOMÄNENNAME* den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) der Ressource an, die die IP-Adresse verwendet.

1. Wählen Sie **OK** aus, um den DNS-Eintrag zu erstellen.

Die folgenden Beispiele zeigen, wie diese Aufgabe mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle durchgeführt wird:

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
New-AzDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName mydnsresourcegroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns record-set add-record mydnsresourcegroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network dns record-set ptr add-record -g mydnsresourcegroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Im folgenden Beispiel wird der Prozess zur Erstellung eines neuen PTR-Eintrags für IPv6 erläutert. Weitere Informationen zu Eintragstypen oder zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und -Eintragssätzen](dns-operations-recordsets-portal.md).

1. Klicken Sie oben im Bereich *DNS-Zone* auf **+ Datensatzgruppe**, um den Bereich *Datensatzgruppe hinzufügen* zu öffnen.

   :::image type="content" source="./media/dns-reverse-dns-hosting/create-record-set-ipv6.png" alt-text="Screenshot zum Erstellen eines IPv6-Zeigereintragssatzes":::

1. Der Name des Eintragssatzes für einen PTR-Eintrag ist der restliche Teil der IPv6-Adresse in umgekehrter Reihenfolge. Er darf keine Nullkomprimierung enthalten. 

    In diesem Beispiel werden die ersten 64 Bit der IPv6-Adresse als Teil des Zonennamens („0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa“) aufgefüllt. Deshalb werden im Feld **Name** nur die letzten 64 Bit angegeben. Die letzten 64 Bit der IP-Adresse werden in umgekehrter Reihenfolge eingegeben, und die einzelnen hexadezimalen Zahlen werden jeweils durch einen Punkt getrennt. Wenn Sie eine Ressource haben, deren IP-Adresse „2001:0db8:abdc:0000:f524:10bc:1af9:405e“ lautet, benennen Sie Ihren Eintragssatz mit **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**.

    :::image type="content" source="./media/dns-reverse-dns-hosting/create-ipv6-ptr.png" alt-text="Screenshot zum Erstellen eines IPv6-Zeigereintrags":::

1. Wählen Sie unter *Typ* die Option **PTR** aus.  

1. Geben Sie für *DOMÄNENNAME* den FQDN der Ressource an, die die IP-Adresse verwendet.

1. Wählen Sie **OK** aus, um den DNS-Eintrag zu erstellen.

Die folgenden Beispiele zeigen, wie diese Aufgabe mit PowerShell oder mit der Azure-Befehlszeilenschnittstelle durchgeführt wird:

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
New-AzDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName mydnsresourcegroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns record-set add-record mydnsresourcegroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network dns record-set ptr add-record -g mydnsresourcegroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Anzeigen von Datensätzen

Navigieren Sie zum Anzeigen der erstellten Einträge im Azure-Portal zu Ihrer DNS-Zone. Die Einträge für die DNS-Zone werden im unteren Teil des Bereichs **DNS-Zone** angezeigt. Dort sollten Sie die standardmäßigen NS- und SOA-Einträge sowie alle neuen Einträge sehen, die Sie erstellt haben. Die NS- und SOA-Einträge werden in jeder Zone erstellt. 

### <a name="ipv4"></a>IPv4

Auf der Seite **DNS-Zone** wird der IPv4-PTR-Eintrag angezeigt:

:::image type="content" source="./media/dns-reverse-dns-hosting/view-ipv4-ptr-record.png" alt-text="Screenshot des IPv4-Zeigereintrags auf der Übersichtsseite" lightbox="./media/dns-reverse-dns-hosting/view-ipv4-ptr-record-expanded.png":::

Die folgenden Beispiele zeigen, wie die PTR-Einträge mithilfe von Azure PowerShell und über die Azure-Befehlszeilenschnittstelle angezeigt werden:

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
Get-AzDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName mydnsresourcegroup
```

#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns record-set list mydnsresourcegroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network dns record-set list -g mydnsresourcegroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Auf der Seite **DNS-Zone** wird der IPv6-PTR-Eintrag angezeigt:

:::image type="content" source="./media/dns-reverse-dns-hosting/view-ipv6-ptr-record.png" alt-text="Screenshot des IPv6-Zeigereintrags auf der Übersichtsseite" lightbox="./media/dns-reverse-dns-hosting/view-ipv6-ptr-record-expanded.png":::

Die folgenden Beispiele zeigen, wie die Einträge mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle angezeigt werden:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName mydnsresourcegroup
```

#### <a name="azure-classic-cli"></a>Klassische Azure-Befehlszeilenschnittstelle

```azurecli
azure network dns record-set list mydnsresourcegroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
az network dns record-set list -g mydnsresourcegroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kann ich Reverse-DNS-Lookupzonen für meine vom ISP zugewiesenen IP-Adressblöcke in Azure DNS hosten?

Ja. Das Hosten der Reverse-Lookupzonen (ARPA) für eigene IP-Adressbereiche in Azure DNS wird vollständig unterstützt.

Erstellen Sie die Reverse-Lookupzone in Azure DNS, wie in diesem Artikel erläutert wird. Arbeiten Sie dann mit Ihrem ISP zusammen, um [die Zone zu delegieren](dns-domain-delegation.md). Sie können dann die PTR-Einträge für jedes Reverse-Lookup auf dieselbe Weise wie andere Eintragstypen verwalten.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Wie viel kostet das Hosten meiner Reverse-DNS-Lookupzone?

Das Hosten der Reverse-DNS-Lookupzone für Ihren vom ISP zugewiesenen IP-Adressblock in Azure DNS wird mit den [Standardsätzen für Azure DNS](https://azure.microsoft.com/pricing/details/dns/) berechnet.

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kann ich Reverse-DNS-Lookupzonen für sowohl IPv4- als auch IPv6-Adressen in Azure DNS hosten?

Ja. In diesem Artikel wird erläutert, wie Reverse-DNS-Lookupzonen für sowohl IPv4 als auch IPv6 in Azure DNS erstellt werden.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Kann ich eine vorhandene Reverse-DNS-Lookupzone importieren?

Ja. Vorhandene DNS-Zonen können über die Azure-Befehlszeilenschnittstelle in Azure DNS importiert werden. Diese Methode kann sowohl für Forward-Lookupzonen als auch für Reverse-Lookupzonen verwendet werden.

Weitere Informationen finden Sie unter [Importieren und Exportieren einer DNS-Zonendatei über die Azure-Befehlszeilenschnittstelle](dns-import-export.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Reverse-DNS finden Sie unter [Reverse DNS](https://en.wikipedia.org/wiki/Reverse_DNS_lookup) in der Wikipedia.

* Erfahren Sie, wie Sie [Reverse-DNS-Einträge für Ihre Azure-Dienste verwalten](dns-reverse-dns-for-azure-services.md).
