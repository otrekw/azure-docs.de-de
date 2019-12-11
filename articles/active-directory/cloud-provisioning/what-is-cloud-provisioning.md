---
title: Was ist die Azure AD Connect-Cloudbereitstellung? | Microsoft-Dokumentation
description: Hier wird die Azure AD Connect-Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e88f6f344c77109ee01cd6b0332138518f70270d
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795337"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Was ist die Azure AD Connect-Cloudbereitstellung?
Die Azure AD Connect-Cloudbereitstellung ist ein neuer Microsoft-Agent, der entwickelt wurde, um Ihre Hybrididentitätsziele für die Synchronisierung von Benutzern, Gruppen und Kontakten mit Azure AD zu erfüllen und zu erreichen.  Die Azure AD Connect-Cloudbereitstellung kann zusammen mit der Azure AD Connect-Synchronisierung verwendet werden und bietet die folgenden Vorteile:
    
- Unterstützung für die Synchronisierung mit einem Azure AD-Mandanten aus einer getrennten Active Directory-Gesamtstrukturumgebung mit mehreren Gesamtstrukturen: Zu den gängigen Szenarien gehören Fusionen und Übernahmen, bei denen die AD-Gesamtstrukturen des übernommenen Unternehmens von den AD-Gesamtstrukturen und Unternehmen der Muttergesellschaft isoliert sind, die in der Vergangenheit über mehrere AD-Gesamtstrukturen verfügten.
- Vereinfachte Installation mit einfachen Bereitstellungs-Agents: Die Agents fungieren als Brücke zwischen AD und Azure AD, wobei die gesamte Synchronisierungskonfiguration in der Cloud verwaltet wird. 
- Es können mehrere Bereitstellungs-Agents verwendet werden, um Bereitstellungen mit hoher Verfügbarkeit zu vereinfachen, was insbesondere für Organisationen wichtig ist, die die Kennworthashsynchronisierung zwischen AD und Azure AD verwenden müssen.


![Was ist Azure AD Connect?](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>Wie unterscheidet sich die Azure AD Connect-Cloudbereitstellung von der Azure AD Connect-Synchronisierung?
Bei der Azure AD Connect-Cloudbereitstellung wird die Bereitstellung über AD für Azure AD in Microsoft Online Services orchestriert. Eine Organisation muss nur in ihrer lokalen und IaaS-gehosteten Umgebung einen einfachen Agent bereitstellen, der als Brücke zwischen Azure AD und AD fungiert. Die Bereitstellungskonfiguration wird in Azure AD gespeichert und als Teil des Diensts verwaltet.

Die folgende Tabelle bietet einen Vergleich zwischen der Azure AD Connect-Synchronisierung und der Azure AD Connect-Cloudbereitstellung:

| Feature | Azure Active Directory Connect-Synchronisierung| Azure Active Directory Connect-Cloudbereitstellung |
|:--- |:---:|:---:|
|Herstellen einer Verbindung mit einer einzelnen lokalen AD-Gesamtstruktur|● |● |
| Herstellen einer Verbindung mit mehreren lokalen AD-Gesamtstrukturen |● |● |
| Herstellen einer Verbindung mit mehreren getrennten lokalen AD-Gesamtstrukturen | |● |
| Installationsmodell mit einfachen Agents | |● |
| Mehrere aktive Agents für Hochverfügbarkeit | |● |
| Herstellen einer Verbindung mit LDAP-Verzeichnissen|●| | 
| Unterstützung für Benutzerobjekte |● |● |
| Unterstützung für Gruppenobjekte |● |● |
| Unterstützung für Kontaktobjekte |● |● |
| Unterstützung für Geräteobjekte |● | |
| Zulassen grundlegender Anpassungen von Attributflüssen |● |● |
| Synchronisieren von benutzerdefinierten AD-Attributen (Verzeichniserweiterungen) |● | |
| Unterstützung der Kennworthashsynchronisierung |●|●|
| Unterstützung der Passthrough-Authentifizierung |●||
| Verbundunterstützung |●|●|
| Nahtloses einmaliges Anmelden|● |●|
| Unterstützt die Installation auf einem Domänencontroller |● |● |
| Unterstützung für Windows Server 2012 und Windows Server 2012 R2 |● |● |
| Filtern nach Domänen/Organisationseinheiten/Gruppen |● |● |
| Filterung nach den Attributwerten eines Objekts |● | |
| Zulassen eines minimalen Attributsatzes für die Synchronisierung (MinSync) |● |● |
| Zulassen der Entfernung von Attributen aus dem Attributfluss von AD nach Azure AD |● |● |
| Zulassen einer erweiterten Anpassung des Attributflusses |● | |
| Unterstützung des Rückschreibens (Kennwörter, Geräte, Gruppen) |● | |

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
