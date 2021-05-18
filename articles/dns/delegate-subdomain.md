---
title: Delegieren einer Unterdomäne – Azure DNS
description: Mit diesem Lernpfad können Sie mit dem Delegieren einer Azure DNS-Unterdomäne beginnen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 05/03/2021
ms.author: rohink
ms.openlocfilehash: 11c9fd2e453db37a5aa985bcc53acdabf4a42aaf
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108763305"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Delegieren einer Azure DNS-Unterdomäne

Sie können das Azure-Portal zum Delegieren einer DNS-Unterdomäne verwenden. Wenn Sie beispielsweise Besitzer der Domäne contoso.com sind, können Sie eine Unterdomäne namens *engineering* an eine andere separate Zone delegieren, die Sie separat von der contoso.com-Zone verwalten können.

Falls Sie dies vorziehen, können Sie auch eine Unterdomäne mithilfe von [Azure PowerShell](delegate-subdomain-ps.md) delegieren.

## <a name="prerequisites"></a>Voraussetzungen

Zum Delegieren einer Azure DNS-Unterdomäne müssen Sie zunächst Ihre öffentliche Domäne an Azure DNS delegieren. Anweisungen zum Konfigurieren Ihrer Namenserver für die Delegierung finden Sie unter [Delegieren einer Domäne an Azure DNS](./dns-delegate-domain-azure-dns.md). Sobald Ihre Domäne an Ihre Azure DNS-Zone delegiert ist, können Sie Ihre Unterdomäne delegieren.

> [!NOTE]
> In diesem Artikel wird „contoso.com“ als Beispiel verwendet. Ersetzen Sie Ihren eigenen Domänennamen durch „contoso.com“.

## <a name="create-a-zone-for-your-subdomain"></a>Erstellen einer Zone für die Unterdomäne

Erstellen Sie zuerst die Zone für die Unterdomäne **engineering**.

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus.

1. Suchen Sie nach **DNS-Zone**, und wählen Sie dann **Erstellen** aus.

1. Wählen Sie auf der Seite **DNS-Zone erstellen** die Ressourcengruppe für Ihre Zone aus. Es wird empfohlen, dieselbe Ressourcengruppe wie die übergeordnete Zone zu verwenden, um ähnliche Ressourcen zusammenzuhalten.

1.  Geben Sie für `engineering.contoso.com` als **Name** ein, und wählen Sie dann **Erstellen** aus.

1. Wechseln Sie nach erfolgreicher Bereitstellung zur neuen Zone.

## <a name="note-the-name-servers"></a>Notieren der Namenserver

Notieren Sie sich als Nächstes die vier Namenserver für die engineering-Unterdomäne.

Notieren Sie sich die vier Nameserver für die Zone von der Zonenübersichtsseite **engineering**. Sie benötigen diese Namenserver zu einem späteren Zeitpunkt.

## <a name="create-a-test-record"></a>Erstellen eines Testeintrags

Erstellen Sie einen **A**-Eintrag zum Testen. Erstellen Sie beispielsweise den A-Eintrag **www**, und konfigurieren Sie ihn mit der IP-Adresse **10.10.10.10**.

## <a name="create-an-ns-record"></a>Erstellen eines NS-Eintrags

Als Nächstes erstellen Sie einen Eintrag für den Namenserver (NS) für die Zone **engineering**.

1. Navigieren Sie zu der Zone für die übergeordnete Domäne.

1. Wählen Sie oben auf der Übersichtsseite **+ Ressourceneintragssatz** aus.

1. Geben Sie auf der Seite **Ressourceneintragssatz hinzufügen** den Namen **engineering** in das Textfeld **Name** ein.

1. Wählen Sie unter **Typ** die Option **NS** aus.

1. Geben Sie unter **Namenserver** die vier Namenserver ein, die Sie zuvor in der Zone **engineering** notiert haben.

1. Wählen Sie **OK** aus, um den Eintrag zu speichern.

## <a name="test-the-delegation"></a>Testen der Delegierung

Verwenden Sie „nslookup“ zum Testen der Delegierung.

1. Öffnen Sie ein PowerShell-Fenster.

1. Geben Sie an einer Eingabeaufforderung Folgendes ein: `nslookup www.engineering.contoso.com.`.

1. Sie sollten eine nicht autoritative Antwort mit der Adresse **10.10.10.10** erhalten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Reverse-DNS-Lookups für in Azure gehostete Dienste konfigurieren](dns-reverse-dns-for-azure-services.md).
