---
title: Verwalten von DNS-Ressourceneintragssätzen und -Einträgen in Azure DNS
description: Mit Azure DNS können Sie beim Hosten Ihrer Domäne DNS-Ressourceneintragssätze und -Einträge verwalten.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 04/28/2021
ms.author: rohink
ms.openlocfilehash: 8ac76671dc16fb51cea35154cd7862ad1dee66f6
ms.sourcegitcommit: 49bd8e68bd1aff789766c24b91f957f6b4bf5a9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108226975"
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal

In diesem Artikel wird gezeigt, wie Sie Ressourceneintragssätze und Einträge für die DNS-Zone im Azure-Portal verwalten.

Es ist wichtig, den Unterschied zwischen DNS-Ressourceneintragssätzen und einzelnen DNS-Ressourceneinträgen zu verstehen. Bei einem Ressourceneintragssatz handelt es sich um die Sammlung von Einträgen in einer Zone, die den gleichen Namen und den gleichen Typ aufweisen. Weitere Informationen finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal](./dns-getstarted-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Weitere Informationen zum Erstellen eines Ressourceneintragssatzes im Azure-Portal finden Sie unter [Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal](./dns-getstarted-portal.md).

## <a name="view-a-record-set"></a>Anzeigen eines Ressourceneintragssatzes

1. Wechseln Sie im Azure-Portal zur Übersichtsseite **DNS-Zone**.

1. Suchen Sie nach dem Ressourceneintragssatz, und wählen Sie ihn aus, um seine Eigenschaften zu öffnen.

    :::image type="content" source="./media/dns-operations-recordsets-portal/overview.png" alt-text="Screenshot der Übersichtsseite für die Zone „contosotest.com“":::

## <a name="add-a-new-record-to-a-record-set"></a>Hinzufügen eines Eintrags zu einem Ressourceneintragssatz

Sie können einem Ressourceneintragssatz bis zu 20 Einträge hinzufügen. Ein Ressourceneintragssatz darf nicht zwei identische Einträge enthalten. Leere Ressourceneintragssätze (ohne Einträge) können erstellt werden, werden auf den Azure DNS-Namenservern aber nicht angezeigt. Ressourceneintragssätze vom Typ CNAME können höchstens einen Eintrag enthalten.

1. Wählen Sie auf der Seite **Eigenschaften des Ressourceneintragssatzes** für Ihre DNS-Zone den Ressourceneintragssatz aus, dem Sie einen Eintrag hinzufügen möchten.

    :::image type="content" source="./media/dns-operations-recordsets-portal/select-record.png" alt-text="Screenshot: Auswählen des Ressourceneintragssatzes „www“":::

1. Geben Sie die Einstellungen des Ressourceneintragssatzes an, indem Sie die Felder ausfüllen.

    :::image type="content" source="./media/dns-operations-recordsets-portal/record-page.png" alt-text="Screenshot der Seite „Add a record“ (Eintrag hinzufügen)":::

1. Wählen Sie oben auf der Seite **Speichern** aus, um Ihre Einstellungen zu speichern. Schließen Sie dann die Seite.

Nachdem der Eintrag gespeichert wurde, spiegeln die Werte auf der Seite **DNS-Zone** den neuen Eintrag wider.

## <a name="update-a-record"></a>Update a record (Datensatz aktualisieren)

Wenn Sie einen Eintrag innerhalb eines vorhandenen Ressourceneintragssatzes aktualisieren, hängt vom Eintragstyp ab, welche Felder Sie aktualisieren können.

1. Suchen Sie auf der Seite **Eigenschaften des Ressourceneintragssatzes** für Ihren Ressourceneintragssatz nach dem Eintrag.

1. Ändern Sie den Eintrag. Wenn Sie einen Eintrag ändern, können Sie die verfügbaren Einstellungen für den Eintrag ändern. Im folgenden Beispiel ist das Feld **IP-Adresse** ausgewählt, und die IP-Adresse wird gerade geändert.

    :::image type="content" source="./media/dns-operations-recordsets-portal/update-record-page.png" alt-text="Screenshot: Seite zum Aktualisieren eines Eintrags":::

1. Wählen Sie oben auf der Seite **Speichern** aus, um Ihre Einstellungen zu speichern. In der oberen rechten Ecke wird die Benachrichtigung angezeigt, dass der Eintrag gespeichert wurde.

    :::image type="content" source="./media/dns-operations-recordsets-portal/record-saved.png" alt-text="Screenshot eines erfolgreich gespeicherten Eintrags":::

Nachdem der Eintrag gespeichert wurde, spiegeln die Werte für den Ressourceneintragssatz auf der Seite **DNS-Zone** den aktualisierten Eintrag wider.

## <a name="remove-a-record-from-a-record-set"></a>Entfernen eines Eintrags aus einem Ressourceneintragssatz

Sie können Einträge mit dem Azure-Portal aus einem Ressourceneintragssatz entfernen. Durch das Entfernen des letzten Eintrags aus einem Ressourceneintragssatz wird der Eintragssatz nicht gelöscht.

1. Suchen Sie auf der Seite **Eigenschaften des Ressourceneintragssatzes** für Ihren Ressourceneintragssatz nach dem Eintrag.

1. Wählen Sie neben dem Eintrag **...** und dann **Entfernen** aus, um den Eintrag aus dem Ressourceneintragssatz zu löschen.

    :::image type="content" source="./media/dns-operations-recordsets-portal/delete-record.png" alt-text="Screenshot zum Löschen eines Eintrags":::

1. Wählen Sie oben auf der Seite **Speichern** aus, um Ihre Einstellungen zu speichern.

1. Nachdem der Eintrag entfernt wurde, spiegeln die Werte für den Eintrag auf der Seite **DNS-Zone** diesen Vorgang wider.

## <a name="delete-a-record-set"></a><a name="delete"></a>Löschen eines Eintragssatzes

1. Wählen Sie auf der Seite **Eigenschaften des Ressourceneintragssatzes** für Ihren Ressourceneintragssatz **Löschen** aus.

    :::image type="content" source="./media/dns-operations-recordsets-portal/delete-record-set.png" alt-text="Screenshot zum Löschen eines Ressourceneintragssatzes":::

1. Sie werden in einer Meldung gefragt, ob Sie den Ressourceneintragssatz wirklich löschen möchten.

1. Überprüfen Sie, ob der Name des Ressourceneintragssatzes mit dem übereinstimmt, den Sie löschen möchten, und wählen Sie **Ja** aus.

1. Überprüfen Sie auf der Seite **DNS-Zone**, ob der Ressourceneintragssatz nicht mehr angezeigt wird.

## <a name="work-with-ns-and-soa-records"></a>Arbeiten mit NS- und SOA-Einträgen

Automatisch erstellte NS- und SOA-Einträge werden anderes verwaltet als andere Ressourceneintragssätze.

### <a name="modify-soa-records"></a>Ändern von SOA-Einträgen

Sie können dem automatisch erstellten SOA-Eintragssatz an der Zonenspitze (Name = „\@“) weder Einträge hinzufügen noch Einträge daraus entfernen. Allerdings können Sie jeden Parameter im SOA-Eintrag (mit Ausnahme von „Host“) und die Gültigkeitsdauer des Eintragssatzes ändern.

### <a name="modify-ns-records-at-the-zone-apex"></a>Ändern von NS-Einträgen an der Zonenspitze

Der für die Zonenspitze festgelegte NS-Eintrag wird für jede DNS-Zone automatisch erstellt. Er enthält die Namen der Azure DNS-Namenserver, die der Zone zugewiesen sind.

Sie können diesem NS-Eintragssatz weitere Namenserver hinzufügen, um das gemeinsame Hosten von Domänen mit mehr als einem DNS-Anbieter zu unterstützen. Sie können auch die Gültigkeitsdauer und die Metadaten für diesen Datensatz ändern. Sie können aber die vorab mit Daten aufgefüllten Azure DNS-Namenserver nicht entfernen oder ändern.

Diese Einschränkung gilt nur für den NS-Eintragssatz an der Zonenspitze. Andere NS-Eintragssätze in Ihrer Zone (zur Delegierung von untergeordneten Zonen) können ohne Einschränkungen geändert werden.

### <a name="delete-soa-or-ns-record-sets"></a>Löschen von SOA- oder NS-Ressourceneintragssätzen

Sie können die SOA- und NS-Eintragssätze an der Zonenspitze (Name = „\@“), die beim Erstellen der Zone automatisch erstellt werden, nicht löschen. Wenn Sie die Zone löschen, werden sie automatisch gelöscht.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).
* Weitere Informationen zum Automatisieren von DNS finden Sie unter [Erstellen von DNS-Zonen und -Datensatzgruppen mithilfe des .NET SDK](dns-sdk.md).
* Weitere Informationen zu Reverse-DNS-Einträgen finden Sie unter [Übersicht über Reverse-DNS und die Unterstützung in Azure](dns-reverse-dns-overview.md).
* Weitere Informationen zu Azure DNS-Aliaseinträgen finden Sie unter [Azure DNS-Aliaseinträge – Übersicht](dns-alias.md).