---
title: 'Remotearbeit mit P2S: Azure VPN Gateway'
description: Auf dieser Seite wird beschrieben, wie Sie VPN Gateway nutzen können, um Remotearbeit aufgrund der COVID-19-Pandemie zu ermöglichen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/03/2020
ms.author: alzam
ms.openlocfilehash: a70f7ccd1c3ca078bab0ff473283b505c0ded316
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441560"
---
# <a name="remote-work-using-azure-vpn-gateway-point-to-site"></a>Remotearbeit mit Azure VPN Gateway Point-to-Site

>[!NOTE]
>In diesem Artikel wird beschrieben, wie Sie Azure VPN Gateway, Azure, das Microsoft-Netzwerk und das Azure-Partnerökosystem nutzen können, um remote zu arbeiten und Netzwerkprobleme zu beheben, die Sie aufgrund der COVID-19-Krise bewältigen müssen.
>

In diesem Artikel werden die Optionen beschrieben, die Organisationen während der COVID-19-Epidemie zur Verfügung stehen, um den Remotezugriff für ihre Benutzer einzurichten oder um ihre vorhandenen Lösungen mit zusätzlicher Kapazität zu ergänzen.

Die Azure Point-to-Site-Lösung ist cloudbasiert und kann in kurzer Zeit bereitgestellt werden, um den höheren Bedarf von Benutzern zu unterstützen, die zu Hause arbeiten. Sie kann einfach zentral hochskaliert sowie ebenso einfach und schnell deaktiviert werden, wenn die größere Kapazität nicht mehr benötigt wird.

## <a name="about-point-to-site-vpn"></a><a name="p2s"></a>Informationen zu Point-to-Site-VPN

Mit einer P2S-VPN-Gatewayverbindung (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Diese Lösung ist nützlich für Telearbeiter, die an einem Remotestandort (beispielsweise zu Hause oder in einer Konferenz) eine Verbindung mit Azure-VNETs oder lokalen Rechenzentren herstellen möchten. In diesem Artikel wird basierend auf verschiedenen Szenarien beschrieben, wie Sie es Benutzern ermöglichen können, remote zu arbeiten.

In der folgenden Tabelle sind die Clientbetriebssysteme und Authentifizierungsoptionen aufgeführt, die zur Verfügung stehen. Es ist hilfreich, die Authentifizierungsmethode abhängig vom Clientbetriebssystem auszuwählen, das bereits verwendet wird. Wählen Sie beispielsweise OpenVPN mit zertifikatbasierter Authentifizierung aus, wenn Sie über einen Mix von Clientbetriebssystemen verfügen, die eine Verbindung herstellen müssen. Beachten Sie außerdem, dass das Point-to-Site-VPN nur für routenbasierte VPN-Gateways unterstützt wird.

![point-to-site](./media/working-remotely-support/ostable.png "OS")

## <a name="scenario-1---users-need-access-to-resources-in-azure-only"></a><a name="scenario1"></a>Szenario 1 – Benutzer benötigen nur Zugriff auf Ressourcen in Azure

In diesem Szenario müssen die Remotebenutzer nur auf Ressourcen in Azure zugreifen können.

![point-to-site](./media/working-remotely-support/scenario1.png "Szenario 1")

Um Benutzern die sichere Verbindung mit Azure-Ressourcen zu ermöglichen, sind die folgenden übergeordneten Schritte erforderlich:

1. Erstellen eines Gateways für das virtuelle Netzwerk (sofern nicht vorhanden)
2. Konfigurieren einer Point-to-Site-VPN-Verbindung im Gateway

   * Folgen Sie für die Zertifikatauthentifizierung [diesem Link](vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw).
   * Folgen Sie für OpenVPN [diesem Link](vpn-gateway-howto-openvpn.md).
   * Folgen Sie für die Azure AD-Authentifizierung [diesem Link](openvpn-azure-ad-tenant.md).
   * Informationen zur Problembehandlung bei Point-to-Site-Verbindungen finden Sie unter [diesem Link](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
3. Herunterladen und Verteilen der VPN-Clientkonfiguration
4. Verteilen der Zertifikate (bei Auswahl der Zertifikatauthentifizierung) an die Clients
5. Herstellen der Azure-VPN-Verbindung

## <a name="scenario-2---users-need-access-to-resources-in-azure-andor-on-prem-resources"></a><a name="scenario2"></a>Szenario 2 – Benutzer benötigen Zugriff auf Ressourcen in Azure und/oder lokale Ressourcen

In diesem Szenario müssen die Remotebenutzer auf Ressourcen in Azure und in lokalen Rechenzentren zugreifen können.

![point-to-site](./media/working-remotely-support/scenario2.png "Szenario 2")

Um Benutzern die sichere Verbindung mit Azure-Ressourcen zu ermöglichen, sind die folgenden übergeordneten Schritte erforderlich:

1. Erstellen eines Gateways für das virtuelle Netzwerk (sofern nicht vorhanden)
2. Konfigurieren einer Point-to-Site-VPN-Verbindung im Gateway (siehe [Szenario 1](#scenario1))
3. Konfigurieren eines Site-to-Site-Tunnels im Azure-Gateway für virtuelle Netzwerke mit aktiviertem BGP
4. Konfigurieren eines lokalen Geräts für die Verbindung mit dem Azure-Gateway für virtuelle Netzwerke
5. Herunterladen des Point-to-Site-Profils vom Azure-Portal und Verteilen an Clients

Informationen zum Einrichten eines Site-to-Site-VPN-Tunnels finden Sie unter [diesem Link](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="faq-for-native-azure-certificate-authentication"></a><a name="faqcert"></a>Häufig gestellte Fragen zur nativen Azure-Zertifikatauthentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faq-for-radius-authentication"></a><a name="faqradius"></a>Häufig gestellte Fragen zur RADIUS-Authentifizierung

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der Azure AD-Authentifizierung](openvpn-azure-ad-tenant.md)

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der RADIUS-Authentifizierung](point-to-site-how-to-radius-ps.md)

* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der nativen Azure-Zertifikatauthentifizierung](vpn-gateway-howto-point-to-site-rm-ps.md)

**„OpenVPN“ ist eine Marke von OpenVPN Inc.**