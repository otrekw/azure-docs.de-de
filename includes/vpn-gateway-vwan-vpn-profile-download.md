---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/13/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1d14ee849c89e6c3807636d0a728157abd9de97a
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650651"
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
* OpenVPN (wenn Sie für das Gateway die OpenVPN- und Azure AD Authentifizierungseinstellungen aktiviert haben). Informationen zu VPN Gateway finden Sie unter [Erstellen eines Azure Active Directory-Mandanten](../articles/vpn-gateway/openvpn-azure-ad-tenant.md).) Informationen zu Virtual WAN finden Sie unter [Erstellen eines Azure Active Directory-Mandanten ](../articles/virtual-wan/openvpn-azure-ad-tenant.md).)

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
