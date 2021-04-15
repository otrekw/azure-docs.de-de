---
title: 'IPsec-/IKE-Richtlinie für S2S-VPN- oder VNET-zu-VNET-Verbindungen: Azure-Portal'
titleSuffix: Azure VPN Gateway
description: Konfigurieren Sie die IPsec-/IKE-Richtlinie für S2S- oder VNET-zu-VNET-Verbindungen mithilfe von Azure-VPN-Gateways über Azure Resource Manager und das Azure-Portal.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/18/2020
ms.author: yushwang
ms.openlocfilehash: 2b298185866d16da02fe8d3b3fdb41f0b0b1f726
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98878543"
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections-azure-portal"></a>Konfigurieren der IPsec/IKE-Richtlinie für S2S-VPN- oder VNet-zu-VNet-Verbindungen: Azure-Portal

In diesem Artikel werden die Schritte zum Konfigurieren der IPsec-/IKE-Richtlinie für Site-to-Site-VPN- oder VNET-zu-VNET-Verbindungen in VPN Gateway über das Azure-Portal beschrieben. Die folgenden Abschnitte erläutern das Erstellen und Konfigurieren einer IPsec-/IKE-Richtlinie und zum Anwenden auf eine neue oder vorhandene Verbindung.

## <a name="about-ipsec-and-ike-policy-parameters"></a><a name="about"></a>Informationen zu IPsec- und IKE-Richtlinienparametern

Der IPsec- und IKE-Protokollstandard unterstützt ein breites Spektrum von Kryptografiealgorithmen in verschiedenen Kombinationen. Informationen dazu, wie Sie damit erreichen können, dass die standortübergreifende und VNET-zu-VNET-Konnektivität Ihre Konformitäts- oder Sicherheitsanforderungen erfüllt, finden Sie unter [Kryptografische Anforderungen und Azure-VPN-Gateways](vpn-gateway-about-compliance-crypto.md).

Dieser Artikel enthält Anweisungen zum Erstellen und Konfigurieren einer IPsec-/IKE-Richtlinie und zu ihrem Anwenden auf eine neue oder vorhandene VPN Gateway-Verbindung.

### <a name="considerations"></a>Überlegungen

* Die IPsec-/IKE-Richtlinie kann nur für folgende Gateway-SKUs verwendet werden:
  * ***VpnGw1–5 und VpnGw1AZ–5AZ***
  * ***Standard** _ und _ *_HighPerformance_**
* Pro Verbindung kann jeweils nur ***eine*** Richtlinienkombination angegeben werden.
* Sie müssen alle Algorithmen und Parameter für IKE (Hauptmodus) und IPsec (Schnellmodus) angeben. Partielle Richtlinien sind nicht zulässig.
* Vergewissern Sie sich in den Spezifikationen Ihres VPN-Geräteanbieters, dass die Richtlinie von Ihren lokalen VPN-Geräten unterstützt wird. S2S- bzw. VNet-zu-VNet-Verbindungen können nicht hergestellt werden, wenn die Richtlinien inkompatibel sind.

## <a name="workflow"></a><a name ="workflow"></a>Workflow

In diesem Abschnitt wird der Workflow zum Erstellen und Aktualisieren der IPsec/IKE-Richtlinie für eine S2S-VPN- oder VNet-zu-VNet-Verbindung beschrieben:

1. Erstellen eines virtuellen Netzwerks und eines VPN-Gateways
2. Erstellen eines Gateways des lokalen Netzwerks für eine standortübergreifende Verbindung oder eines anderen virtuellen Netzwerks und Gateways für eine VNET-zu-VNET-Verbindung
3. Erstellen Sie eine Verbindung (IPsec oder VNET-to-VNET).
4. Konfigurieren/aktualisieren/entfernen Sie die IPsec-/IKE-Richtlinie für die Verbindungsressourcen.

Mithilfe der Anweisungen in diesem Artikel können Sie IPsec-/IKE-Richtlinien einrichten und konfigurieren, wie im folgenden Diagramm dargestellt:

:::image type="content" source="./media/ipsec-ike-policy-howto/policy-diagram.png" alt-text="Diagramm zur IPsec-/IKE-Richtlinie" border="false":::

## <a name="supported-cryptographic-algorithms--key-strengths"></a><a name ="params"></a>Unterstützte Kryptografiealgorithmen und Schlüsselstärken

### <a name="algorithms-and-keys"></a><a name ="table1"></a>Algorithmen und Schlüssel

Die folgende Tabelle gibt Aufschluss über die unterstützten Kryptografiealgorithmen und Schlüsselstärken, die von den Kunden konfiguriert werden können:

| **IPsec/IKE**    | **Optionen**    |
| ---              | ---            |
| IKE-Verschlüsselung   | AES256, AES192, AES128, DES3, DES                  |
| IKE-Integrität    | SHA384, SHA256, SHA1, MD5                          |
| DH-Gruppe         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, keine |
| IPsec-Verschlüsselung | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, keine    |
| IPsec-Integrität  | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5 |
| PFS-Gruppe        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, keine   |
| QM-SA-Gültigkeitsdauer   | (**Optional**: Wenn kein Wert angegeben wird, werden die Standardwerte verwendet)<br>Sekunden (ganze Zahl; **min. 300**/Standard: 27.000 Sekunden)<br>KB (ganze Zahl; **min. 1024**/Standard: 102.400.000 KB)    |
| Datenverkehrsselektor | UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)    |
| DPD-Timeout      | Sekunden (Integer: min. 9, max. 3.600, Standard: 45 s) |
|  |  |

#### <a name="important-requirements"></a>Wichtige Anforderungen

* Ihre lokale VPN-Gerätekonfiguration muss folgenden Algorithmus- und Parameterangaben für die Azure-IPsec-/IKE-Richtlinie entsprechen oder selbige enthalten:
  * IKE-Verschlüsselungsalgorithmus (Hauptmodus/Phase 1)
  * IKE-Integritätsalgorithmus (Hauptmodus/Phase 1)
  * DH-Gruppe (Hauptmodus/Phase 1)
  * IPsec-Verschlüsselungsalgorithmus (Schnellmodus/Phase 2)
  * IPsec-Integritätsalgorithmus (Schnellmodus/Phase 2)
  * PFS-Gruppe (Schnellmodus/Phase 2)> * Datenverkehrsselektor (bei Verwendung von UsePolicyBasedTrafficSelectors)
  * Bei der SA-Gültigkeitsdauer handelt es sich lediglich um eine lokale Angabe. Diese muss nicht übereinstimmen.

* Wenn GCMAES als IPsec-Verschlüsselungsalgorithmus verwendet wird, müssen Sie für die IPsec-Integrität denselben GCMAES-Algorithmus und dieselbe Schlüssellänge auswählen, z. B. „GCMAES128“ für beides.

* In der Tabelle [Algorithmen und Schlüssel](#table1) oben gilt:
  * IKE entspricht Hauptmodus oder Phase 1
  * IPsec entspricht Hauptmodus oder Phase 2.
  * Die DH-Gruppe gibt die im Hauptmodus oder in Phase 1 verwendete Diffie-Hellmen-Gruppe an.
  * Die PFS-Gruppe gibt die im Schnellmodus oder in Phase 2 verwendete Diffie-Hellmen-Gruppe an.

* Die SA-Gültigkeitsdauer des IKE-Hauptmodus ist für die Azure-VPN-Gateways auf 28.800 Sekunden festgelegt.

* Wenn **UsePolicyBasedTrafficSelectors** für eine Verbindung auf „$True“ festgelegt wird, wird das Azure-VPN-Gateway für eine Verbindung mit einer richtlinienbasierten lokalen VPN-Firewall konfiguriert. Wenn Sie „PolicyBasedTrafficSelectors“ aktivieren, müssen für Ihr VPN-Gerät die entsprechenden Datenverkehrsselektoren mit allen Präfixkombinationen zwischen Ihrem lokalen Netzwerk (lokalen Netzwerkgateway) und den Präfixen des virtuellen Azure-Netzwerks definiert sein (anstelle von Any-to-Any). Wenn die Präfixe Ihres lokalen Netzwerks also beispielsweise 10.1.0.0/16 und 10.2.0.0/16 lauten und für Ihr virtuelles Netzwerk die Präfixe 192.168.0.0/16 und 172.16.0.0/16 verwendet werden, müssen folgende Datenverkehrsselektoren angegeben werden:
  * 10.1.0.0/16 <====> 192.168.0.0/16
  * 10.1.0.0/16 <====> 172.16.0.0/16
  * 10.2.0.0/16 <====> 192.168.0.0/16
  * 10.2.0.0/16 <====> 172.16.0.0/16

   Weitere Informationen zu richtlinienbasierten Datenverkehrsselektoren finden Sie unter [Herstellen einer Verbindung zwischen Azure-VPN-Gateways und mehreren lokalen richtlinienbasierten VPN-Geräten mit PowerShell](vpn-gateway-connect-multiple-policybased-rm-ps.md).

* DPD-Timeout: Der Standardwert beträgt für Azure-VPN-Gateways 45 Sekunden. Wenn das Timeout auf kürzere Zeiträume festgelegt wird, erstellt IKE aggressiver neue Schlüssel, sodass die Verbindung in einigen Fällen getrennt erscheint. Dies ist möglicherweise nicht wünschenswert, wenn Ihre lokalen Standorte weiter von der Azure-Region entfernt sind, in der sich das VPN-Gateway befindet, oder wenn der Zustand der physischen Verbindung zu Paketverlusten führen könnte. Die allgemeine Empfehlung besteht darin, das Timeout auf einen Wert zwischen **30 und 45** Sekunden festzulegen.

### <a name="diffie-hellman-groups"></a>Diffie-Hellman-Gruppe

Die folgende Tabelle enthält die entsprechenden Diffie-Hellman-Gruppen, die von der benutzerdefinierten Richtlinie unterstützt werden:

| **Diffie-Hellman-Gruppe**  | **DHGroup**              | **PFSGroup** | **Schlüssellänge** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768-Bit-MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024-Bit-MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048-Bit-MODP  |
| 19                        | ECP256                   | ECP256       | 256-Bit-ECP    |
| 20                        | ECP384                   | ECP384       | 384-Bit-ECP    |
| 24                        | DHGroup24                | PFS24        | 2048-Bit-MODP  |

Ausführlichere Informationen finden Sie unter [RFC3526](https://tools.ietf.org/html/rfc3526) und [RFC5114](https://tools.ietf.org/html/rfc5114).

## <a name="s2s-vpn-with-ipsecike-policy"></a><a name ="S2S"></a>S2S-VPN mit IPsec-/IKE-Richtlinie

In diesem Abschnitt werden die Schritte zum Erstellen einer Site-to-Site-VPN-Verbindung mit einer IPsec-/IKE-Richtlinie beschrieben. Mit den folgenden Schritten wird die Verbindung erstellt, wie im folgenden Diagramm dargestellt:

:::image type="content" source="./media/ipsec-ike-policy-howto/site-to-site-diagram.png" alt-text="Site-to-Site-Richtlinie" border="false":::

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><a name="createvnet1"></a>Schritt 1: Erstellen des virtuellen Netzwerks, VPN-Gateways und Gateways des lokalen Netzwerks

Erstellen Sie die folgenden Ressourcen, wie in den folgenden Screenshots gezeigt. Die einzelnen Schritte finden Sie unter [Erstellen einer Site-to-Site-VPN-Verbindung](./tutorial-site-to-site-portal.md).

* **Virtuelles Netzwerk:**  TestVNet1

   :::image type="content" source="./media/ipsec-ike-policy-howto/testvnet-1.png" alt-text="VNET":::

* **VPN-Gateway:** VNet1GW

   :::image type="content" source="./media/ipsec-ike-policy-howto/vnet-1-gateway.png" alt-text="Gateway":::

* **Lokales Netzwerkgateway:** Site6

   :::image type="content" source="./media/ipsec-ike-policy-howto/lng-site-6.png" alt-text="Website":::

* **Connection:** VNet1 zu Site6

    :::image type="content" source="./media/ipsec-ike-policy-howto/connection-site-6.png" alt-text="Connection":::

### <a name="step-2---configure-ipsecike-policy-on-the-s2s-vpn-connection"></a><a name="s2sconnection"></a>Schritt 2: Konfigurieren der IPsec-/IKE-Richtlinie für die S2S-VPN-Verbindung

In diesem Abschnitt konfigurieren Sie eine IPsec-/IKE-Richtlinie mit den folgenden Algorithmen und Parametern:

* IKE:   AES256, SHA384, DHGroup24, DPD-Timeout: 45 Sekunden
* IPsec: AES256, SHA256, PFS: Kein, SA-Gültigkeitsdauer 30.000 Sekunden und 102.400.000 KB

1. Navigieren Sie im Azure-Portal zur Verbindungsressource **VNet1toSite6**. Wählen Sie die Seite **Konfiguration** und dann für die IPsec-/IKE-Richtlinie **Benutzerdefiniert** aus, um alle Konfigurationsoptionen anzuzeigen. Der nachstehende Screenshot zeigt die Konfiguration entsprechend der Liste:

    :::image type="content" source="./media/ipsec-ike-policy-howto/policy-site-6.png" alt-text="Site6":::

1. Wenn Sie GCMAES für IPsec verwenden, müssen für die IPsec-Verschlüsselung und -Integrität derselbe GCMAES-Algorithmus und dieselbe Schlüssellänge verwendet werden. Der folgende Screenshot gibt z. B. sowohl für IPsec-Verschlüsselung als auch für IPsec-Integrität „GCMAES128“ an:

   :::image type="content" source="./media/ipsec-ike-policy-howto/gcmaes.png" alt-text="GCMAES für IPsec":::

1. Optional können Sie **Aktivieren** für die Option **Use policy based traffic selectors** (Richtlinienbasierte Datenverkehrsselektoren verwenden) auswählen, um wie oben beschrieben dem Azure-VPN-Gateway das lokale Verbinden mit richtlinienbasierten VPN-Geräten zu ermöglichen.

   :::image type="content" source="./media/ipsec-ike-policy-howto/policy-based-selector.png" alt-text="Richtlinienbasierter Datenverkehrsselektor":::

1. Nachdem Sie alle Optionen ausgewählt haben, wählen Sie **Speichern** aus, um die Änderungen an der Verbindungsressource zu übernehmen. Die Richtlinie wird in ungefähr einer Minute erzwungen.

> [!IMPORTANT]
>
> * Nachdem für eine Verbindung eine IPsec/IKE-Richtlinie angegeben wurde, sendet bzw. akzeptiert das Azure-VPN-Gateway den IPsec/IKE-Vorschlag mit angegebenen Kryptografiealgorithmen und Schlüsselstärken nur für die jeweilige Verbindung. Stellen Sie sicher, dass Ihr lokales VPN-Gerät für die Verbindung die exakte Richtlinienkombination nutzt bzw. akzeptiert, da der S2S-VPN-Tunnel andernfalls nicht hergestellt wird.
>
> * Die Optionen **Policy-based traffic selector** (Richtlinienbasierter Datenverkehrsselektor) und **DPD timeout** können mit der Richtlinie **Standard** ohne die benutzerdefinierte IPsec-/IKE-Richtlinie angegeben werden, wie im Screenshot oben dargestellt.
>

## <a name="vnet-to-vnet-with-ipsecike-policy"></a><a name ="vnet2vnet"></a>VNET-zu-VNET mit IPsec-/IKE-Richtlinie

Die Schritte zum Erstellen einer VNET-zu-VNET-Verbindung mit einer IPsec-/IKE-Richtlinie ähneln denen für eine S2S-VPN-Verbindung.

:::image type="content" source="./media/ipsec-ike-policy-howto/vnet-policy.png" alt-text="Diagramm für VNET-zu-VNET-Richtlinie" border="false":::

1. Erstellen Sie die VNET-zu-VNET-Verbindung über die Schritte im Artikel [Erstellen einer VNET-zu-VNET-Verbindung](vpn-gateway-vnet-vnet-rm-ps.md).

2. Nach Abschluss der Schritte sehen Sie zwei VNET-zu-VNET-Verbindungen, wie im folgenden Screenshot der VNet2GW-Ressource dargestellt:

   :::image type="content" source="./media/ipsec-ike-policy-howto/vnet-vnet-connections.png" alt-text="VNET-to-VNET-Verbindungen":::

3. Navigieren Sie zu der Verbindungsressource, und wechseln Sie im Portal zur Seite **Konfiguration**. Wählen Sie **Benutzerdefiniert** für die **IPsec-/IKE-Richtlinie** aus, um die benutzerdefinierten Richtlinienoptionen anzuzeigen. Wählen Sie die Kryptografiealgorithmen mit den entsprechenden Schlüssellängen aus.

   Im Screenshot ist eine andere IPsec-/IKE-Richtlinie mit den folgenden Algorithmen und Parametern zu sehen:
   * IKE: AES128, SHA1, DHGroup14, DPD-Timeout: 45 Sekunden
   * IPsec: GCMAES128, GCMAES128, PFS14, SA-Gültigkeitsdauer 14.400 Sekunden und 102.400.000 KB

   :::image type="content" source="./media/ipsec-ike-policy-howto/vnet-vnet-policy.png" alt-text="Verbindungsrichtlinie":::

4. Wählen Sie **Speichern** aus, um die Richtlinienänderungen auf die Verbindungsressource anzuwenden.

5. Wenden Sie dieselbe Richtlinie auf die andere Verbindungsressource (VNet2toVNet1) an. Andernfalls kann keine Verbindung für den IPsec-/IKE-VPN-Tunnel hergestellt werden, da die Richtlinie nicht übereinstimmt.

   > [!IMPORTANT]
   > Nachdem für eine Verbindung eine IPsec/IKE-Richtlinie angegeben wurde, sendet bzw. akzeptiert das Azure-VPN-Gateway den IPsec/IKE-Vorschlag mit angegebenen Kryptografiealgorithmen und Schlüsselstärken nur für die jeweilige Verbindung. Stellen Sie sicher, dass die IPsec-Richtlinien für beide Verbindungen identisch sind, da die VNet-zu-VNet-Verbindung ansonsten nicht hergestellt wird.

6. Nachdem Sie diese Schritte ausgeführt haben, wird die Verbindung innerhalb weniger Minuten hergestellt, und Sie verfügen über die folgende Netzwerktopologie:

    :::image type="content" source="./media/ipsec-ike-policy-howto/policy-diagram.png" alt-text="Diagramm zur IPsec-/IKE-Richtlinie" border="false":::

## <a name="to-remove-custom-ipsecike-policy-from-a-connection"></a><a name ="deletepolicy"></a>So entfernen Sie eine benutzerdefinierte IPsec-/IKE-Richtlinie aus einer Verbindung

1. Um eine benutzerdefinierte Richtlinie aus einer Verbindung zu entfernen, navigieren Sie zur Verbindungsressource und dann zur Seite **Konfiguration**, um die aktuelle Richtlinie anzuzeigen.

2. Wählen Sie **Standard** als **IPsec-/IKE-Richtlinie** aus. Dadurch werden alle zuvor für die Verbindung angegebenen benutzerdefinierten Richtlinien entfernt und die IPsec-/IKE-Standardeinstellungen für diese Verbindung wieder hergestellt:

   :::image type="content" source="./media/ipsec-ike-policy-howto/delete-policy.png" alt-text="Richtlinie löschen":::

3. Wählen Sie **Speichern** aus, um die benutzerdefinierte Richtlinie zu entfernen und die IPsec-/IKE-Standardeinstellungen für die Verbindung wiederherzustellen.

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen zu richtlinienbasierten Datenverkehrsselektoren finden Sie unter [Herstellen einer Verbindung zwischen Azure-VPN-Gateways und mehreren lokalen richtlinienbasierten VPN-Geräten mit PowerShell](vpn-gateway-connect-multiple-policybased-rm-ps.md).