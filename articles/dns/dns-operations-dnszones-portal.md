---
title: Verwalten von DNS-Zonen in Azure DNS – Azure-Portal | Microsoft-Dokumentation
description: Sie können DNS-Zonen mithilfe des Azure-Portals verwalten. In diesem Artikel wird das Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS beschrieben.
services: dns
documentationcenter: na
author: rohinkoul
ms.service: dns
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/28/2021
ms.author: rohink
ms.openlocfilehash: 58118480c16f2e318bab7435a79e27629880acc5
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108203197"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Verwalten von DNS-Zonen im Azure-Portal

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Klassische Azure-Befehlszeilenschnittstelle](./dns-operations-dnszones-cli.md)
> * [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)

In diesem Artikel wird gezeigt, wie DNS-Zonen mithilfe des Azure-Portals verwaltet werden. Sie können Ihre DNS-Zonen auch mithilfe der plattformübergreifenden [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md) oder mit Azure [PowerShell](dns-operations-dnszones.md) verwalten.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen** aus, und suchen Sie nach **DNS-Zone**. Wählen Sie dann **Erstellen** aus.

    :::image type="content" source="./media/dns-operations-dnszones-portal/search-dns-zone.png" alt-text="Screenshot: Erstellen einer Ressourcensuche für eine DNS-Zone":::

1. Geben Sie auf der Seite **DNS-Zone erstellen** die folgenden Werte ein, und wählen Sie dann **Erstellen** aus:

    | Einstellung | Details |
    | --- | --- |
    | **Abonnement** | Wählen Sie ein Abonnement aus, in dem die DNS-Zone erstellt werden soll.|
    | **Ressourcengruppe** | Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine neue. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/management/overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
    | **Name** | Geben Sie einen Namen für die DNS-Zone ein. Beispiel: **contoso.com**. |
    | **Location** | Wählen Sie den Speicherort für die Ressourcengruppe aus. Wenn Sie eine zuvor erstellte Ressourcengruppe verwenden, ist der Speicherort bereits ausgewählt.  |

> [!NOTE]
> Die Ressourcengruppe bezieht sich auf den Standort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Suchen Sie über die Suchressourcen oben im Azure-Portal nach **DNS-Zonen**. Jede DNS-Zone ist eine eigene Ressource. Auf dieser Seite können Informationen wie die Anzahl von Ressourceneintragssätzen und Namenserver angezeigt werden. Die Spalte **Namenserver** ist nicht in der Standardansicht enthalten. Wählen Sie zum Hinzufügen **Ansicht verwalten > Spalten bearbeiten > + Spalte hinzufügen** und anschließend in der Dropdownliste den Eintrag **Namenserver** aus. Wählen Sie **Speichern** aus, um die neue Spalte zu übernehmen.

:::image type="content" source="./media/dns-operations-dnszones-portal/list-zones.png" alt-text="Screenshot: Seite mit der Liste der DNS-Zonen":::

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

Navigieren Sie zu einer DNS-Zone im Portal. Wählen Sie auf der ausgewählten Übersichtsseite **DNS-Zone** die Option **Zone löschen** aus. Sie werden dann aufgefordert, das Löschen der DNS-Zone zu bestätigen. Beim Löschen einer DNS-Zone werden auch alle in der Zone enthaltenen Datensätze gelöscht.

:::image type="content" source="./media/dns-operations-dnszones-portal/delete-zone.png" alt-text="Screenshot: Schaltfläche zum Löschen der DNS-Zone auf der Übersichtsseite":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Arbeit mit Ihrer DNS-Zone und Ihren Datensätzen unter [Erste Schritte mit Azure DNS über das Azure-Portal](dns-getstarted-portal.md).
