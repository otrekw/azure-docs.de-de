---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c61378510fbfc8bdc13f35ba1063a0d9316d88e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066210"
---
## <a name="1-download-the-file"></a>1. Herunterladen der Datei

Führen Sie die folgenden Befehle aus: Kopieren Sie die Ergebnis-URL in Ihren Browser, um die ZIP-Datei für das Profil herunterzuladen.

```azurepowershell-interactive
$profile = New-AzVpnClientConfiguration -ResourceGroupName AADAuth -Name AADauthGW -AuthenticationMethod "EapTls"
   
$PROFILE.VpnProfileSASUrl
```

## <a name="2-extract-the-zip-file"></a>2. Extrahieren der ZIP-Datei

Extrahieren Sie die ZIP-Datei. Die Datei enthält die folgenden Ordner:

* AzureVPN
* Allgemein
* OpenVPN (wenn Sie für das Gateway die OpenVPN- und Azure AD Authentifizierungseinstellungen aktiviert haben. Informationen zu VPN Gateway finden Sie unter [Erstellen eines Azure Active Directory-Mandanten](../articles/vpn-gateway/openvpn-azure-ad-tenant.md).) Informationen zu Virtual WAN finden Sie unter [Erstellen eines Azure Active Directory-Mandanten ](../articles/virtual-wan/openvpn-azure-ad-tenant.md).)

## <a name="3-retrieve-information"></a>3. Abrufen von Informationen

Navigieren Sie im Ordner **AzureVPN** zur Datei ***azurevpnconfig.xml***, und öffnen Sie sie mit Editor. Notieren Sie sich den Text zwischen den folgenden Tags.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Profildetails

Wenn Sie eine Verbindung hinzufügen, verwenden Sie die im vorherigen Schritt auf der Seite mit den Profildetails gesammelten Informationen. Die Felder entsprechen den folgenden Informationen:

   * **Audience:** Die Empfängerressource, für die das Token vorgesehen ist
   * **Issuer:** Der Azure AD-Mandant und der Sicherheitstokendienst (STS), der das Token ausgegeben hat
   * **Tenant:** Ein unveränderlicher, eindeutiger Bezeichner des Verzeichnismandanten, der das Token ausgestellt hat
   * **FQDN:** Der vollqualifizierte Domänenname (FQDN) in Azure VPN Gateway
   * **ServerSecret:** Der vorinstallierte Schlüssel des VPN-Gateways

## <a name="folder-contents"></a>Ordnerinhalt

* Der Ordner **generic** enthält das öffentliche Serverzertifikat und die Datei „VpnSettings.xml“. Die Datei „VpnSettings.xml“ enthält Informationen, die zum Konfigurieren eines generischen Clients benötigt werden.

* Die heruntergeladene ZIP-Datei kann auch noch die Ordner **WindowsAmd64** und **WindowsX86** enthalten. Diese Ordner enthalten das Installationsprogramm für SSTP und IKEv2 für Windows-Clients. Sie benötigen Administratorrechte auf dem Client, um diese zu installieren.