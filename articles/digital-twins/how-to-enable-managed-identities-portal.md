---
title: 'Aktivieren einer verwalteten Identität für die Weiterleitung von Ereignissen (Vorschau): Portal'
titleSuffix: Azure Digital Twins
description: In diesem Artikel erfahren Sie, wie Sie eine systemseitig zugewiesene Identität für Azure Digital Twins aktivieren und zum Weiterleiten von Ereignissen mithilfe des Portals verwenden.
author: baanders
ms.author: baanders
ms.date: 1/21/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1211cd306f10fb349bd42568697443ff103a171c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703087"
---
# <a name="enable-a-managed-identity-for-routing-azure-digital-twins-events-preview-azure-portal"></a>Aktivieren einer verwalteten Identität für die Weiterleitung von Azure Digital Twins-Ereignissen (Vorschau): Azure-Portal

[!INCLUDE [digital-twins-managed-service-identity-selector.md](../../includes/digital-twins-managed-service-identity-selector.md)]

In diesem Artikel wird beschrieben, wie Sie eine [systemseitig zugewiesene Identität für eine Azure Digital Twins-Instanz](concepts-security.md#managed-identity-for-accessing-other-resources-preview) (derzeit in der Vorschauphase) aktivieren und die Identität zum Weiterleiten von Ereignissen an unterstützte Ziele wie [Event Hub](../event-hubs/event-hubs-about.md), [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) und [Azure Storage Container](../storage/blobs/storage-blobs-introduction.md) verwenden.

In diesem Artikel wird die Vorgehensweise mit dem [**Azure-Portal**](https://portal.azure.com) beschrieben.

Die folgenden Schritte werden in diesem Artikel behandelt: 

1. Erstellen Sie eine Azure Digital Twins-Instanz mit einer systemseitig zugewiesenen Identität, oder aktivieren Sie eine systemseitig zugewiesene Identität für eine vorhandene Azure Digital Twins-Instanz. 
1. Fügen Sie entsprechende Rollen zur Identität hinzu. Beispielsweise können Sie der Identität die Rolle **Azure Event Hub Data Sender** (Azure Event Hub-Datensender), wenn Sie einen Event Hub-Endpunkt verwenden, oder die Rolle **Azure Service Bus-Datensender** zuweisen, wenn Sie einen Service Bus-Endpunkt verwenden.
1. Erstellen Sie einen Endpunkt in Azure Digital Twins, der systemseitig zugewiesene Identitäten für die Authentifizierung verwenden kann.

## <a name="enable-system-managed-identities-for-an-instance"></a>Aktivieren von systemseitig verwalteten Identitäten für eine Instanz 

Wenn Sie eine systemseitig zugewiesene Identität für Ihre Azure Digital Twins-Instanz aktivieren, erstellt Azure automatisch eine Identität in [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) für diese. Diese Identität kann dann für die Authentifizierung bei Azure Digital Twins-Endpunkten für die Ereignisweiterleitung verwendet werden.

Sie können systemseitig verwaltete Identitäten für eine Azure Digital Twins-Instanz **als Teil der ersten Einrichtung der Instanz** oder **später für eine bereits vorhandene Instanz** aktivieren.

Beide dieser Erstellungsmethoden bieten Ihnen dieselben Konfigurationsoptionen und dasselbe Endergebnis für Ihre Instanz. In diesem Abschnitt werden beide Vorgehensweisen beschrieben.

### <a name="add-a-system-managed-identity-during-instance-creation"></a>Hinzufügen einer systemseitig verwalteten Identität während der Instanzerstellung

In diesem Abschnitt erfahren Sie, wie Sie eine systemseitig verwaltete Identität für eine Azure Digital Twins-Instanz aktivieren, die derzeit erstellt wird. Dieser Abschnitt befasst sich mit dem Schritt des Erstellungsprozesses für die verwaltete Identität. Eine umfassende exemplarische Vorgehensweise zum Erstellen einer Azure Digital Twins-Instanz finden Sie unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-portal.md) aus.

Die Option für systemseitig verwaltete Identitäten befindet sich auf der Registerkarte **Erweitert** des Instanzsetups.

Wählen Sie auf dieser Registerkarte die Option **Ein** für **Systemseitig verwaltete Identität** aus, um dieses Feature zu aktivieren.

:::image type="content" source="media/how-to-enable-managed-identities/create-instance-advanced.png" alt-text="Screenshot: Azure-Portal mit der Registerkarte „Erweitert“ des Dialogfelds „Ressource erstellen“ für Azure Digital Twins, der Registerkartenname, die Option „Ein“ für systemseitig verwaltete Identitäten und die Navigationsschaltflächen (Überprüfen und Erstellen, Zurück, Weiter: Erweitert) sind hervorgehoben":::

Anschließend können Sie die Navigationsschaltflächen unten verwenden, um mit der Einrichtung der Instanz fortzufahren.

### <a name="add-a-system-managed-identity-to-an-existing-instance"></a>Hinzufügen einer systemseitig verwalteten Identität zu einer vorhandenen Instanz

In diesem Abschnitt fügen Sie eine systemseitig verwaltete Identität zu einer bereits vorhandenen Azure Digital Twins-Instanz hinzu.

1. Öffnen Sie zunächst das [Azure-Portal](https://portal.azure.com) in einem Browser.

1. Suchen Sie über die Suchleiste des Portals nach dem Namen Ihrer Instanz, und wählen Sie diesen aus, um die Details anzuzeigen.

1. Klicken Sie im linken Menü auf **Identität (Vorschau)** .

1. Aktivieren Sie auf dieser Seite die Option **Ein**, um das Feature zu aktivieren.

1. Klicken Sie auf **Speichern** und zum Bestätigen auf **Ja**.

    :::image type="content" source="media/how-to-enable-managed-identities/identity-digital-twins.png" alt-text="Screenshot: Azure-Portal mit der Seite „Identität (Vorschau)“ für eine Azure Digital Twins-Instanz mit dem Seitennamen im Azure Digital Twins-Instanzmenü, der Option „Ein“ als Status, der Schaltfläche „Speichern“ und der Bestätigungsschaltfläche „Ja“ hervorgehoben":::

Nachdem die Änderungen gespeichert wurden, werden auf dieser Seite weitere Felder für die **Objekt-ID** und **Berechtigungen** der neuen Identität angezeigt.

Sie können die **Objekt-ID** bei Bedarf hier kopieren und die Schaltfläche **Berechtigungen** verwenden, um die Azure-Rollen anzuzeigen, die der Identität zugewiesen sind. Im nächsten Abschnitt richten Sie einige Rollen ein.

## <a name="assign-azure-roles-to-the-identity"></a>Zuweisen von Azure-Rollen zur Identität 

Nachdem Sie eine systemseitig zugewiesene Identität für Ihre Azure Digital Twins-Instanz erstellt haben, müssen Sie dieser die entsprechenden Rollen für die Authentifizierung bei verschiedenen Arten von [Endpunkten](concepts-route-events.md) zum Weiterleiten von Ereignissen an unterstützte Ziele zuweisen. In diesem Abschnitt werden die Rollenoptionen und das Zuweisen dieser zur systemseitig zugewiesenen Identität beschrieben.

>[!NOTE]
> Hierbei handelt es sich um einen wichtigen Schritt. Wenn Sie diesen Schritt nicht durchführen, kann die Identität nicht auf Ihre Endpunkte zugreifen und Ereignisse werden nicht übermittelt.

### <a name="supported-destinations-and-azure-roles"></a>Unterstützte Ziele und Azure-Rollen 

Im Folgenden finden Sie die mindestens erforderlichen Rollen, die eine Identität je nach Art von Ziel für den Zugriff auf einen Endpunkt benötigt. Rollen mit höheren Berechtigungen (beispielsweise Datenbesitzerrollen) funktionieren ebenfalls.

| Destination | Azure-Rolle |
| --- | --- |
| Azure Event Hubs | Azure Event Hubs-Datensender |
| Azure-Servicebus | Azure Service Bus-Datensender |
| Azure-Speichercontainer | Mitwirkender an Storage-Blobdaten |

Weitere Informationen über Endpunkte, Routen und die verschiedenen unterstützten Ziele für das Routing in Azure Digital Twins finden Sie unter [*Konzepte: Ereignisrouten*](concepts-route-events.md).

### <a name="assign-the-role"></a>Zuweisen der Rolle

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Öffnen Sie zum Zuweisen einer Rolle zur Identität zunächst das [Azure-Portal](https://portal.azure.com).

1. Navigieren Sie zu Ihrer Endpunktressource (Event Hub-Instanz, Service Bus-Instanz oder Speichercontainer), indem Sie ihren Namen über die Suchleiste des Portals suchen. 
1. Klicken Sie im linken Menü auf **Zugriffssteuerung (IAM)** .
1. Klicken Sie auf **+ Hinzufügen**, um eine neue Rollenzuweisung hinzuzufügen.

    :::image type="content" source="media/how-to-enable-managed-identities/add-role-assignment-1.png" alt-text="Screenshot: Azure-Portal mit der Seite „Zugriffssteuerung (IAM)“ für eine Event Hub-Instanz und hervorgehobener Schaltfläche „+ Hinzufügen“" lightbox="media/how-to-enable-managed-identities/add-role-assignment-1.png":::

1. Geben Sie auf der daraufhin angezeigten Seite **Rollenzuweisung hinzufügen** die folgenden Werte ein:
    * **Rolle**: Wählen Sie die gewünschte Rolle im Dropdownmenü aus.
    * **Zugriff zuweisen zu**: Klicken Sie unter **Systemseitig zugewiesene verwaltete Identität** auf **Digital Twins**.
    * **Abonnement**: Wählen Sie Ihr Abonnement aus. Dadurch werden alle verwalteten Identitäten in Azure Digital Twins im ausgewählten Abonnement angezeigt.
    * **Select**: Wählen Sie hier die verwaltete Identität Ihrer Azure Digital Twins-Instanz aus, der die Rolle zugewiesen wird. Der Name der verwalteten Identität stimmt mit dem Name der Instanz überein, suchen Sie also nach dem Namen Ihrer Azure Digital Twins-Instanz. Wenn Sie das Suchergebnis auswählen, wird die Identität für die Instanz im Abschnitt **Ausgewählte Mitglieder** unten auf der Seite angezeigt.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-enable-managed-identities/add-role-assignment-2.png" alt-text="Eingeben der aufgelisteten Felder im Dialogfeld „Rollenzuweisung hinzufügen“":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

Wenn Sie fertig sind, klicken Sie auf **Speichern**.

## <a name="create-an-endpoint-with-identity-based-authentication"></a>Erstellen eines Endpunkts mit identitätsbasierter Authentifizierung

Nachdem Sie eine systemseitig verwaltete Identität für Ihre Azure Digital Twins-Instanz eingerichtet und dieser die entsprechenden Rollen zugewiesen haben, können Sie Azure Digital Twins-[Endpunkte](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins) erstellen, die die Identität für die Authentifizierung verwenden können. Diese Option ist nur für Event Hub- und Service Bus-Endpunkte verfügbar (sie wird nicht für Event Grid unterstützt).

>[!NOTE]
> Sie können einen Endpunkt nicht bearbeiten, der bereits mit einer schlüsselbasierten Identität erstellt wurde, um zur identitätsbasierten Authentifizierung zu wechseln. Sie müssen den Authentifizierungstyp auswählen, wenn der Endpunkt erstellt wird.

Befolgen Sie die [Anweisungen zum Erstellen eines Azure Digital Twins-Endpunkts](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins).

Wenn Sie zum Schritt zum Eingeben der erforderlichen Informationen zum Endpunkttyp gelangen, wählen Sie den Authentifizierungstyp **Identity-based** (identitätsbasiert) aus.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-hub-authentication.png" alt-text="Screenshot: Erstellen eines Endpunkts vom Typ „Event Hub“" lightbox="media/how-to-manage-routes-portal/create-endpoint-event-hub-authentication.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Schließen Sie das Setup Ihres Endpunkts ab, und klicken Sie auf **Speichern**.

## <a name="considerations-for-disabling-system-managed-identities"></a>Überlegungen zum Deaktivieren von systemseitig verwalteten Identitäten

Da Identitäten separat von den Endpunkten verwaltet werden, die sie verwenden, ist es wichtig, dass die möglichen Auswirkungen von Änderungen an der Identität oder ihrer Rollen auf die Endpunkte Ihrer Azure Digital Twins-Instanz berücksichtigt werden. Wenn die Identität deaktiviert ist oder eine notwendige Rolle für einen Endpunkt entfernt wird, kann der Endpunkt unzugänglich und der Ablauf der Ereignisse unterbrochen werden.

Sie müssen den Endpunkt löschen und mit einem anderen Authentifizierungstyp [neu erstellen](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins), um einen Endpunkt weiterhin zu verwenden, der mit einer verwalteten Identität eingerichtet wurde, die deaktiviert wurde. Es kann bis zu eine Stunde dauern, bis Ereignisse nach dieser Änderung wieder an den Endpunkt übermittelt werden.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie mehr über verwaltete Identitäten in Azure AD: 
* [*Verwaltete Identitäten für Azure-Ressourcen*](../active-directory/managed-identities-azure-resources/overview.md)