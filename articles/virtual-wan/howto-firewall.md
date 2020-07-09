---
title: Installieren einer Azure Firewall-Instanz in einem Virtual WAN-Hub
titleSuffix: Azure Virtual WAN
description: Schritte zum Konfigurieren von Azure Firewall in einem Virtual WAN-Hub
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 05/21/2020
ms.author: cherylmc
ms.openlocfilehash: f20ed76a72eecce59a7b8795a42b033230a2f7e0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753684"
---
# <a name="configure-azure-firewall-in-a-virtual-wan-hub"></a>Konfigurieren von Azure Firewall in einem Virtual WAN-Hub

Ein **geschützter Hub** ist ein Virtual WAN-Hub mit Azure Firewall. Dieser Artikel führt Sie durch die Schritte zum Konvertieren eines Virtual WAN-Hubs in einen geschützten Hub, indem Sie Azure Firewall direkt über die Azure Virtual WAN-Portalseiten installieren.

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie bereits ein virtuelles WAN mit einem oder mehreren Hubs bereitgestellt haben.

Führen Sie die Schritte in den folgenden Artikeln aus, um ein neues virtuelles WAN und einen neuen Hub zu erstellen:

* [Erstellen eines virtuellen WAN](virtual-wan-site-to-site-portal.md#openvwan)
* [Erstellen eines Hubs](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-virtual-hubs"></a>Anzeigen von virtuellen Hubs

Auf der Seite **Übersicht** für Ihr virtuelles WAN wird eine Liste der virtuellen Hubs und der geschützten Hubs angezeigt. Die folgende Abbildung zeigt ein virtuelles WAN ohne geschützte Hubs.

[ ![Übersicht](./media/howto-firewall/overview.png)](./media/howto-firewall/overview.png#lightbox)

## <a name="convert-to-secured-hub"></a>Konvertieren in einen geschützten Hub

1. Wählen Sie auf der Seite **Übersicht** für Ihr virtuelles WAN den Hub aus, den Sie in einen geschützten Hub konvertieren möchten. Auf der Seite des virtuellen Hubs werden zwei Optionen zum Bereitstellen von Azure Firewall in diesem Hub angezeigt. Wählen Sie eine der beiden Optionen aus.

   [ ![Sicherheit](./media/howto-firewall/security.png)](./media/howto-firewall/security.png#lightbox)

1. Nachdem Sie eine der Optionen ausgewählt haben, wird die Seite **In geschützten Hub konvertieren** angezeigt. Wählen Sie einen Hub zum Konvertieren aus, und wählen Sie dann unten auf der Seite **Weiter: Azure Firewall** aus.

   [ ![Auswählen des Hubs](./media/howto-firewall/select-hub.png)](./media/howto-firewall/select-hub.png#lightbox)
1. Nachdem Sie den Workflow abgeschlossen haben, wählen Sie **Bestätigen** aus.

   [ ![Bestätigen](./media/howto-firewall/confirm.png)](./media/howto-firewall/confirm.png#lightbox)

1. Nachdem der Hub in einen geschützten Hub konvertiert wurde, können Sie ihn auf der Seite **Übersicht** des virtuellen WAN anzeigen.

   [ ![Anzeigen eines geschützten Hubs](./media/howto-firewall/secured-hub.png)](./media/howto-firewall/secured-hub.png#lightbox)

## <a name="view-hub-resources"></a>Anzeigen von Hubressourcen

Wählen Sie auf der Seite **Übersicht** des virtuellen WAN den geschützten Hub aus. Auf der Seite des virtuellen Hubs können Sie alle zugehörigen Ressourcen anzeigen, einschließlich Azure Firewall.

[ ![Anzeigen von Hubressourcen](./media/howto-firewall/view-resources.png)](./media/howto-firewall/view-resources.png#lightbox)

Wählen Sie zum Anzeigen der Einstellungen für Azure Firewall auf dem geschützten Hub unter **Sicherheit** die Option **Einstellungen für geschützte virtuelle Hubs** aus.
[ ![Anzeigen der Hubeinstellungen](./media/howto-firewall/hub-settings.png)](./media/howto-firewall/hub-settings.png#lightbox)

## <a name="configure-additional-settings"></a>Konfigurieren zusätzlicher Einstellungen

Um zusätzliche Azure Firewall-Einstellungen für den virtuellen Hub zu konfigurieren, wählen Sie den Link zu **Azure Firewall Manager** aus. Informationen zu Firewall-Richtlinien finden Sie unter [Azure Firewall Manager](../firewall-manager/secure-cloud-network.md#create-a-firewall-policy-and-secure-your-hub).

[ ![Zusätzliche Einstellungen](./media/howto-firewall/additional-settings.png)](./media/howto-firewall/additional-settings.png#lightbox)

Wenn Sie zur Seite **Übersicht** für den Hub zurückkehren möchten, können Sie zurücknavigieren, indem Sie auf den Pfad klicken, wie in der nachstehenden Abbildung dargestellt.

[ ![Zurückkehren zur Übersicht](./media/howto-firewall/arrow.png)](./media/howto-firewall/arrow.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
