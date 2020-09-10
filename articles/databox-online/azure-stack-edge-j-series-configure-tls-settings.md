---
title: Konfigurieren von TLS 1.2 auf Windows-Clients, die auf ein Azure Stack Edge-GPU-Gerät zugreifen
description: In diesem Artikel wird beschrieben, wie Sie TLS 1.2 auf Windows-Clients konfigurieren, die auf ein Azure Stack Edge-GPU-Gerät zugreifen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: bf6b591ef3158a5944b1ebeb37dd0ef5935f7215
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268501"
---
# <a name="configure-tls-12-on-windows-clients-accessing-azure-stack-edge-device"></a>Konfigurieren von TLS 1.2 auf Windows-Clients, die auf ein Azure Stack Edge-Gerät zugreifen

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Wenn Sie über einen Windows-Client auf Ihr Azure Stack Edge-Gerät zugreifen, muss auf dem Client TLS 1.2 konfiguriert werden. In diesem Artikel finden Sie Ressourcen und Richtlinien für die Konfiguration von TLS 1.2 auf Ihrem Windows-Client. 

Die hier aufgeführten Richtlinien basieren auf Tests, die auf einem Client mit Windows Server 2016 durchgeführt wurden.

## <a name="configure-tls-12-for-current-powershell-session"></a>Konfigurieren von TLS 1.2 für die aktuelle PowerShell-Sitzung

Führen Sie die folgenden Schritte aus, um TLS 1.2 auf Ihrem Client zu konfigurieren.

1. Führen Sie PowerShell als Administrator aus.
2. Geben Sie Folgendes ein, um TLS 1.2 für die aktuelle PowerShell-Sitzung festzulegen:
  
    ```azurepowershell
    $TLS12Protocol = [System.Net.SecurityProtocolType] 'Ssl3 , Tls12'
    [System.Net.ServicePointManager]::SecurityProtocol = $TLS12Protocol
    ```
## <a name="configure-tls-12-on-client"></a>Konfigurieren von TLS 1.2 auf einem Client

Befolgen Sie die Richtlinien in den folgenden Dokumenten, wenn Sie TLS 1.2 systemweit für Ihre Umgebung festlegen möchten:

- [Allgemein: So aktivieren Sie TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12)
- [Aktivieren von TLS 1.2 auf Clients](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-client)
- [Aktivieren von TLS 1.2 auf Standortservern und Remotestandortsystemen](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-server)
- [Protokolle in TLS/SSL (Schannel SSP)](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-ecc-curve-order)
- [Verschlüsselungssammlungen](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12): insbesondere [Konfigurieren der Reihenfolge von TLS-Verschlüsselungssammlungen](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order). Stellen Sie sicher, dass Sie Ihre aktuellen Verschlüsselungssammlungen aufführen und fehlende Sammlungen in der folgenden Liste voranstellen:

    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384

    Sie können diese Verschlüsselungssammlungen auch hinzufügen, indem Sie die Registrierungseinstellungen bearbeiten.

    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "Functions"  -PropertyType String -Value ("TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384")
    ```

- Festlegen von elliptischen Kurven

    Stellen Sie sicher, dass Sie Ihre aktuellen elliptischen Kurven aufführen und fehlende Kurven in der folgenden Liste voranstellen:

    - P-256 
    - P-384

    Sie können diese elliptischen Kurven auch hinzufügen, indem Sie die Registrierungseinstellungen bearbeiten.
    
    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "EccCurves" -PropertyType MultiString -Value @("NistP256", "NistP384")
    ```
    
    - [Legen Sie die Mindestgröße für den RSA-Schlüsselaustausch auf 2048 fest](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#keyexchangealgorithm---client-rsa-key-sizes).



## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Resource Manager](azure-stack-edge-j-series-connect-resource-manager.md)