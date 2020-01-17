---
title: Sicherheits- und Identitätsdienste von Azure Deutschland | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Vergleich der Sicherheits- und Identitätsdienste für Azure Deutschland.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: b91558446c8a51b0b08038134ff55432fc8f5a39
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75436646"
---
# <a name="azure-germany-security-and-identity-services"></a>Sicherheits- und Identitätsdienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

## <a name="key-vault"></a>Key Vault
Einzelheiten zum Azure Key Vault-Dienst und seiner Verwendung finden Sie in der [globalen Dokumentation zu Key Vault](../key-vault/index.yml).

Key Vault ist in Azure Deutschland allgemein verfügbar. Wie auch bei der globalen Azure-Umgebung gibt es keine Erweiterung. Key Vault ist daher nur über PowerShell und die CLI verfügbar.

## <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory stellt Identitäts- und Zugriffsfunktionen für Informationssysteme in Microsoft Azure bereit. Durch die Verwendung von Verzeichnisdiensten, Sicherheitsgruppen und Gruppenrichtlinien können Sie die Zugriffs- und Sicherheitsrichtlinien von Computern steuern, die Azure Active Directory verwenden. Mithilfe von Konten und Sicherheitsgruppen können Sie in Verbindung mit der rollenbasierten Zugriffssteuerung (RBAC) den Zugriff auf die Informationssysteme steuern. 

Azure Active Directory ist in Azure Deutschland allgemein verfügbar.

### <a name="variations"></a>Abweichungen

* Azure Active Directory in Azure Deutschland ist vollständig von Azure Active Directory in der globalen Azure-Umgebung getrennt. 
* Kunden können sich nicht mit einem Microsoft-Konto bei Azure Deutschland anmelden.
* Das Anmeldesuffix für Azure Deutschland lautet *onmicrosoft.de* (nicht *onmicrosoft.com* wie bei der globalen Azure-Umgebung).
* Kunden benötigen ein gesondertes Abonnement für Azure Deutschland.
* Kunden von Azure Deutschland können nicht auf Ressourcen zugreifen, die ein Abonnement oder eine Identität in der globalen Azure-Umgebung erfordern.
* Kunden der globalen Azure-Umgebung können nicht auf Ressourcen zugreifen, die ein Abonnement oder eine Identität in Azure Deutschland erfordern.
* Zusätzliche Domänen können nur in einer der Cloudumgebungen hinzugefügt bzw. überprüft werden.
 
> [!NOTE]
> Die Option zum Zuweisen von Rechten an Benutzer durch andere Mandanten, bei denen sich *beide Mandanten in Azure Deutschland befinden*, ist noch nicht verfügbar.


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.




