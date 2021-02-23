---
title: Aktivieren des privaten Zugriffs mit Private Link (Vorschau) über das Azure-Portal
titleSuffix: Azure Digital Twins
description: In diesem Artikel erfahren Sie, wie Sie den privaten Zugriff für Azure Digital Twins-Lösungen mit Private Link über das Azure-Portal aktivieren.
author: baanders
ms.author: baanders
ms.date: 1/25/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 2545915edf9e39b63100a2bb16bd34fa6777675c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100473462"
---
# <a name="enable-private-access-with-private-link-preview-azure-portal"></a>Aktivieren des privaten Zugriffs mit Private Link (Vorschau): Azure-Portal

[!INCLUDE [digital-twins-private-link-selector.md](../../includes/digital-twins-private-link-selector.md)]

In diesem Artikel werden verschiedene Methoden zum [Aktivieren von Private Link mit einem privaten Endpunkt für eine Azure Digital Twins-Instanz](concepts-security.md#private-network-access-with-azure-private-link-preview) beschrieben (befindet sich derzeit in der Vorschauphase). Das Konfigurieren eines privaten Endpunkts für Ihre Azure Digital Twins-Instanz ermöglicht es Ihnen, Ihre Azure Digital Twins-Instanz zu schützen und eine öffentliche Offenlegung zu vermeiden. Außerdem wird auch die Datenexfiltration aus Ihrer [Azure Virtual Network-Instanz (virtuelles Netzwerk)](../virtual-network/virtual-networks-overview.md) verhindert.

In diesem Artikel wird die Vorgehensweise mit dem [**Azure-Portal**](https://portal.azure.com) beschrieben.

Die folgenden Schritte werden in diesem Artikel behandelt: 
1. Aktivieren Sie Private Link, und konfigurieren Sie einen privaten Endpunkt für eine Azure Digital Twins-Instanz.
1. Deaktivieren oder aktivieren Sie Zugriffsflags für das öffentliche Netzwerkzugriff, um den API-Zugriff ausschließlich auf Private Link-Verbindungen einzuschränken.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie einen privaten Endpunkt einrichten können, benötigen Sie eine [**Azure Virtual Network-Instanz (virtuelles Netzwerk)**](../virtual-network/virtual-networks-overview.md), in dem der Endpunkt bereitgestellt werden kann. Wenn Sie noch nicht über ein virtuelles Netzwerk verfügen, können Sie einen [Schnellstart](../virtual-network/quick-create-portal.md) für Azure Virtual Network durchlaufen, um eines einzurichten.

## <a name="add-a-private-endpoint-for-an-azure-digital-twins-instance"></a>Hinzufügen eines privaten Endpunkts für eine Azure Digital Twins-Instanz 

Wenn Sie das [Azure-Portal](https://portal.azure.com) verwenden, können Sie Private Link mit einem privaten Endpunkt für eine Azure Digital Twins-Instanz bei der Ersteinrichtung oder für eine bereits vorhandene Instanz zu einem späteren Zeitpunkt aktivieren. 

Beide dieser Erstellungsmethoden bieten Ihnen dieselben Konfigurationsoptionen und dasselbe Endergebnis für Ihre Instanz. In diesem Abschnitt werden beide Vorgehensweisen beschrieben.

>[!TIP]
> Sie können einen Private Link-Endpunkt auch über den Private Link-Dienst einrichten, anstatt über Ihre Azure Digital Twins-Instanz. Dies bietet Ihnen dieselben Konfigurationsoptionen und dasselbe Endergebnis.
>
> Weitere Informationen zum Einrichten von Private Link-Ressourcen finden Sie in der Private Link-Dokumentation zum [Azure-Portal](../private-link/create-private-endpoint-portal.md), zur [Azure CLI](../private-link/create-private-endpoint-cli.md), zu [ARM](../private-link/create-private-endpoint-template.md) oder [PowerShell](../private-link/create-private-endpoint-powershell.md).

### <a name="add-a-private-endpoint-during-instance-creation"></a>Hinzufügen eines privaten Endpunkts beim Erstellen der Instanz

In diesem Abschnitt aktivieren Sie Private Link mit einem privaten Endpunkt für eine Azure Digital Twins-Instanz, die derzeit erstellt wird. Dieser Abschnitt befasst sich mit dem Netzwerkschritt des Erstellungsprozesses. Eine umfassende exemplarische Vorgehensweise zum Erstellen einer Azure Digital Twins-Instanz finden Sie unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-portal.md) aus.

Die Private Link-Optionen finden Sie auf der Registerkarte **Netzwerk** des Instanzsetups.

Auf dieser Registerkarte können Sie private Endpunkte aktivieren, indem Sie die Option **Privater Endpunkt** als **Verbindungsmethode** auswählen.

Dadurch wird ein Abschnitt namens **Private Endpunktverbindungen** hinzugefügt, in dem Sie die Details Ihres privaten Endpunkts konfigurieren können. Klicken Sie auf **+ Hinzufügen**, um fortzufahren.

:::image type="content" source="media/how-to-enable-private-link/create-instance-networking-1.png" alt-text="Screenshot: Azure-Portal mit der Registerkarte „Netzwerk“ des Dialogfelds „Ressource erstellen“ für Azure Digital Twins, der Registerkartenname, die Option „Privater Endpunkt“ unter Verbindungsmethode und die Schaltfläche „+ Hinzufügen“ zum Erstellen einer neuen privaten Endpunktverbindung sind hervorgehoben" lightbox="media/how-to-enable-private-link/create-instance-networking-1.png":::

Daraufhin wird eine Seite geöffnet, auf der Sie die Details des neuen privaten Endpunkts eingeben können.

:::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-full.png" alt-text="Screenshot: Azure-Portal mit der Seite „Privaten Endpunkt erstellen“ mit den im Folgenden beschriebenen Feldern":::

1. Wählen Sie Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. Legen Sie denselben **Standort** fest, den Ihr virtuelles Netzwerk aufweist. Geben Sie einen **Namen** für den Endpunkt ein, und wählen Sie für **Target sub-resources** (Untergeordnete Zielressourcen) die Option *API* aus.

1. Wählen Sie als Nächstes das **virtuelle Netzwerk** und **Subnetz** aus, die Sie zum Bereitstellen des Endpunkts verwenden möchten.

1. Entscheiden Sie zuletzt, ob Sie die **Integration mit einer privaten DNS-Zone** verwenden möchten. Sie können die Standardoption **Ja** verwenden, oder dem Link im Portal verwenden, um [mehr über die private DNS-Integration zu erfahren](../private-link/private-endpoint-overview.md#dns-configuration).

Nachdem Sie die Konfigurationsoptionen ausgewählt haben, klicken Sie auf **OK**.

Dadurch kehren Sie zur Registerkarte **Netzwerk** des Setups der Azure Digital Twins-Instanz zurück, auf der Ihr neuer Endpunkt unter „Private Endpunktverbindungen“ angezeigt werden sollte.

:::image type="content" source="media/how-to-enable-private-link/create-instance-networking-2.png" alt-text="Screenshot: Azure-Portal mit der Registerkarte „Netzwerk“ des Dialogfelds „Ressource erstellen“ für Azure Digital Twins, die neue private Endpunktverbindung und die Navigationsschaltflächen (Überprüfen und erstellen, Zurück, Weiter: Erweitert) sind hervorgehoben" lightbox="media/how-to-enable-private-link/create-instance-networking-2.png":::

Anschließend können Sie die Navigationsschaltflächen unten verwenden, um mit der Einrichtung der Instanz fortzufahren.

### <a name="add-a-private-endpoint-to-an-existing-instance"></a>Hinzufügen eines privaten Endpunkts zu einer vorhandenen Instanz

In diesem Abschnitt aktivieren Sie Private Link mit einem privaten Endpunkt für eine Azure Digital Twins-Instanz, die bereits vorhanden ist.

1. Öffnen Sie zunächst das [Azure-Portal](https://portal.azure.com) in einem Browser. Rufen Sie Ihre Azure Digital Twins-Instanz auf, indem Sie mithilfe der Suchleiste des Portals nach ihrem Namen suchen.

1. Klicken Sie im linken Menü auf **Netzwerk (Vorschau)** .

1. Wechseln Sie zur Registerkarte **Privater Endpunktverbindungen**.

1. Klicken Sie auf **+ Privater Endpunkt**, um das Dialogfeld **Privaten Endpunkt erstellen** zu öffnen.

    :::image type="content" source="media/how-to-enable-private-link/add-endpoint-digital-twins.png" alt-text="Screenshot: Azure-Portal mit der Seite „Netzwerk (Vorschau)“ für eine Azure Digital Twins-Instanz mit hervorgehobener Registerkarte „Private Endpunktverbindungen“ und hervorgehobener Schaltfläche „+ Privater Endpunkt“" lightbox="media/how-to-enable-private-link/add-endpoint-digital-twins.png":::

1. Geben Sie auf der Registerkarte  **Grundlagen** das **Abonnement** und die **Ressourcengruppe** Ihres Projekts sowie einen **Namen** und eine **Region** für Ihren Endpunkt an. Die Region muss mit der Region des verwendeten virtuellen Netzwerks übereinstimmen.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-1.png" alt-text="Screenshot: Azure-Portal mit der Registerkarte „Grundlagen“ des Dialogfelds „Privaten Endpunkt erstellen“ und den oben beschriebenen Feldern":::

    Wenn Sie fertig sind, klicken Sie auf **Weiter: Ressource >** , um zur nächsten Registerkarte zu gelangen.

1. Geben Sie auf der Registerkarte **Ressource** die folgenden Informationen an: 
    * **Verbindungsmethode:** Klicken Sie auf **Connect to an Azure resource in my directory** (Verbindung mit einer Azure-Ressource in meinem Verzeichnis herstellen), um nach Ihrer Azure Digital Twins-Instanz zu suchen.
    * **Abonnement**: Geben Sie Ihr Abonnement an.
    * **Ressourcentyp:** Wählen Sie die Option **Microsoft.DigitalTwins/digitalTwinsInstances** aus.
    * **Ressource:** Wählen Sie den Namen Ihrer Azure Digital Twins-Instanz aus.
    * **Target sub-resource** (Untergeordnete Zielressourcen): Wählen Sie die Option **API** aus.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-2.png" alt-text="Screenshot: Azure-Portal mit der zweiten Registerkarte „Ressource“ des Dialogfelds „Privaten Endpunkt erstellen“ und den oben beschriebenen Feldern":::

    Wenn Sie fertig sind, klicken Sie auf **Weiter: Konfiguration >** , um zur nächsten Registerkarte zu gelangen.    

1. Geben Sie die folgenden Informationen auf der Registerkarte  **Konfiguration** an:
    * **Virtuelles Netzwerk:** Wählen Sie Ihr virtuelles Netzwerk aus.
    * **Subnetz**: Wählen Sie ein Subnetz Ihres virtuellen Netzwerks aus.
    * **Integration in eine private DNS-Zone**: Entscheiden Sie, ob Sie die **Integration mit einer privaten DNS-Zone** verwenden möchten. Sie können die Standardoption **Ja** verwenden, oder dem Link im Portal verwenden, um [mehr über die private DNS-Integration zu erfahren](../private-link/private-endpoint-overview.md#dns-configuration).
    Wenn Sie **Ja** auswählen möchten, können Sie die Standardoption beibehalten.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-3.png" alt-text="Screenshot: Azure-Portal mit der dritte Registerkarte „Konfiguration“ des Dialogfelds „Privaten Endpunkt erstellen“ und den oben beschriebenen Feldern":::

    Wenn Sie fertig sind, können Sie auf **Überprüfen und erstellen** klicken, um das Setup abzuschließen. 

1. Überprüfen Sie auf der Registerkarte **Überprüfen und erstellen** Ihre Angaben, und klicken Sie dann auf  **Erstellen**. 

Wenn die Bereitstellung des Endpunkts abgeschlossen ist, sollte dieser unter den privaten Endpunktverbindungen für Ihre Azure Digital Twins-Instanz angezeigt werden.

>[!TIP]
> Der Endpunkt kann auch über Private Link Center im Azure-Portal angezeigt werden.

## <a name="disable--enable-public-network-access-flags"></a>Deaktivieren und Aktivieren von Zugriffsflags für das öffentliche Netzwerk

Sie können Ihre Azure Digital Twins-Instanz so konfigurieren, dass alle öffentlichen Verbindungen verweigert und nur Verbindungen über private Endpunkte zugelassen werden, um die Netzwerksicherheit zu erhöhen. Diese Aktion erfolgt mithilfe eines **Zugriffsflags für das öffentliche Netzwerk**. 

Mithilfe dieser Richtlinie können Sie den API-Zugriff auf Private Link-Verbindungen einschränken. Wenn das Zugriffsflag für das öffentliche Netzwerk auf *deaktiviert* festgelegt wird, geben alle REST-API-Aufrufe der Azure Digital Twins-Instanzdatenebene über die öffentliche Cloud `403, Unauthorized` zurück. Wenn die Richtlinie auf *deaktiviert* festgelegt ist und eine Anforderung über einen privaten Endpunkt erfolgt, ist der API-Aufruf erfolgreich.

In diesem Artikel wird gezeigt, wie der Wert des Netzwerkflags über das [Azure-Portal](https://portal.azure.com) geändert wird. Anweisungen zur Durchführung dieses Verfahrens mithilfe der Azure CLI oder des ARMClient-Befehlstools finden Sie in der [CLI-Version](how-to-enable-private-link-cli.md) dieses Artikels.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Zum Deaktivieren oder Aktivieren des öffentlichen Netzwerkzugriffs über das [Azure-Portal](https://portal.azure.com) öffnen Sie das Portal, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz.

1. Klicken Sie im linken Menü auf **Netzwerk (Vorschau)** .

1. Legen Sie auf der Registerkarte **Öffentlicher Zugriff** für **Allow public network access to** (Öffentlichen Netzwerkzugriff zulassen auf) entweder **Deaktiviert** oder **Alle Netzwerke** fest.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-enable-private-link/network-flag-portal.png" alt-text="Screenshot: Azure-Portal mit der Seite „Netzwerk (Vorschau)“ für eine Azure Digital Twins-Instanz mit hervorgehobener Registerkarte „Öffentlicher Zugriff“ und er Option zum Zulassen des öffentlichen Netzwerkzugriffs" lightbox="media/how-to-enable-private-link/network-flag-portal.png":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

    Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr über Private Link für Azure: 
* [*Was ist der Azure Private Link-Dienst?*](../private-link/private-link-service-overview.md)