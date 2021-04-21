---
title: Informationen zur Zugriffssteuerung für Azure Key Vault-Zertifikate
description: Übersicht über die Zugriffssteuerung für Azure Key Vault-Zertifikate
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 10/12/2020
ms.author: sebansal
ms.openlocfilehash: 54874f30384d7f4827b13a597a469bfc67bc8fd2
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107752478"
---
# <a name="certificate-access-control"></a>Zertifikatzugriffssteuerung

 Die Zugriffssteuerung für Zertifikate wird von Key Vault verwaltet und wird von der Key Vault-Instanz bereitgestellt, die diese Zertifikate enthält. Die Zugriffssteuerungsrichtlinie für Zertifikate unterscheidet sich von der Zugriffssteuerungsrichtlinie für Schlüssel und Geheimnisse im selben Key Vault. Benutzer können einen oder mehrere Tresore zum Speichern von Zertifikaten erstellen und müssen für eine dem Szenario entsprechende Segmentierung und Verwaltung von Zertifikaten sorgen.  

 Die folgenden Berechtigungen können pro Prinzipal im Zugriffssteuerungseintrag für Geheimnisse in einem Schlüsseltresor verwendet werden und spiegeln die für ein Geheimnisobjekt zulässigen Vorgänge präzise wider:  

- Berechtigungen für Zertifikatverwaltungsvorgänge
  - **get**: Abrufen der aktuellen oder einer beliebigen anderen Version eines Zertifikats
  - **list**: Auflisten der aktuellen Zertifikate oder von Versionen eines Zertifikats  
  - **update**: Aktualisieren eines Zertifikats
  - **create**: Erstellen eines Key Vault-Zertifikats
  - **import**: Importieren von Zertifikatmaterial in ein Key Vault-Zertifikat
  - **delete**: Löschen eines Zertifikats, seiner Richtlinie und aller seiner Versionen  
  - **recover**: Wiederherstellen eines gelöschten Zertifikats
  - **backup**: Sichern eines Zertifikats in einem Schlüsseltresor
  - **restore**: Wiederherstellen eines gesicherten Zertifikats in einem Schlüsseltresor
  - **managecontacts**: Verwalten von Key Vault-Zertifikatkontakten  
  - **manageissuers**: Verwalten von Key Vault-Zertifikatautoritäten/-Zertifikatausstellern
  - **getissuers**: Abrufen der Autoritäten/Aussteller eines Zertifikats
  - **listissuers**: Auflisten der Autoritäten/Aussteller eines Zertifikats  
  - **setissuers**: Erstellen oder Aktualisieren der Autoritäten/Aussteller eines Key Vault-Zertifikats  
  - **deleteissuers**: Löschen der Autoritäten/Aussteller eines Key Vault-Zertifikats  
 
- Berechtigungen für privilegierte Vorgänge
  - **purge**: Bereinigen (dauerhaftes Löschen) eines gelöschten Zertifikats

Weitere Informationen finden Sie unter den [Zertifikatvorgängen in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault). Informationen zum Einrichten von Berechtigungen finden Sie unter [Tresore: Aktualisieren der Zugriffsrichtlinie](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="troubleshoot"></a>Problembehandlung
Unter Umständen wird ein Fehler aufgrund einer fehlenden Zugriffsrichtlinie angezeigt. Beispiel: ```Error type : Access denied or user is unauthorized to create certificate``` (Fehlertyp: Zugriff verweigert, oder der Benutzer ist nicht autorisiert, das Zertifikat zu erstellen). Zum Beheben dieses Fehlers müssen Sie die Berechtigung für Zertifikate bzw. für die Erstellung hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu Key Vault](../general/overview.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../general/about-keys-secrets-certificates.md)
- [Authentifizierung, Anforderungen und Antworten](../general/authentication-requests-and-responses.md)
- [Entwicklerhandbuch für Key Vault](../general/developers-guide.md)
