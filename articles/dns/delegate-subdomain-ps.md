---
title: Delegieren einer Unterdomäne – Azure PowerShell – Azure DNS
description: Mit diesem Lernpfad können Sie mit dem Delegieren einer Azure DNS-Unterdomäne mithilfe der Azure PowerShell beginnen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: rohink
ms.openlocfilehash: 7e019afaae98422b8d5a3c8fa7a5f79e26c6a149
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76937706"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Delegieren einer Azure DNS-Unterdomäne mithilfe von Azure PowerShell

Sie können Azure PowerShell zum Delegieren einer DNS-Unterdomäne verwenden. Wenn Sie beispielsweise die Domäne „contoso.com“ besitzen, können Sie eine Unterdomäne namens *engineering* an eine andere, separate Zone delegieren, die Sie getrennt von der Zone „contoso.com“ verwalten können.

Falls Sie dies vorziehen, können Sie eine Unterdomäne auch mithilfe des [Azure-Portals](delegate-subdomain.md) delegieren.

> [!NOTE]
> In diesem Artikel wird „contoso.com“ als Beispiel verwendet. Ersetzen Sie Ihren eigenen Domänennamen durch „contoso.com“.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zum Delegieren einer Azure DNS-Unterdomäne müssen Sie zunächst Ihre öffentliche Domäne an Azure DNS delegieren. Anweisungen zum Konfigurieren Ihrer Namenserver für die Delegierung finden Sie unter [Delegieren einer Domäne an Azure DNS](./dns-delegate-domain-azure-dns.md). Sobald Ihre Domäne an Ihre Azure DNS-Zone delegiert ist, können Sie Ihre Unterdomäne delegieren.

## <a name="create-a-zone-for-your-subdomain"></a>Erstellen einer Zone für die Unterdomäne

Erstellen Sie zuerst die Zone für die Unterdomäne **engineering**.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Notieren der Namenserver

Notieren Sie sich als Nächstes die vier Namenserver für die engineering-Unterdomäne.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Erstellen eines Testeintrags

Erstellen Sie einen **A**-Eintrag in der engineering-Zone, den Sie zum Testen verwenden.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>Erstellen eines NS-Eintrags

Als Nächstes erstellen Sie einen Eintrag für den Namenserver (NS) für die **engineering**-Zone in der contoso.com-Zone.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Testen der Delegierung

Verwenden Sie „nslookup“ zum Testen der Delegierung.

1. Öffnen Sie ein PowerShell-Fenster.
2. Geben Sie an einer Eingabeaufforderung Folgendes ein: `nslookup www.engineering.contoso.com.`.
3. Sie sollten eine nicht autoritative Antwort mit der Adresse **10.10.10.10** erhalten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Reverse-DNS-Lookups für in Azure gehostete Dienste konfigurieren](dns-reverse-dns-for-azure-services.md).