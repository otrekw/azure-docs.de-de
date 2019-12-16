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
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: f189abdfda62e306f520ebec22bded84ab069e5d
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74890410"
---
# <a name="azure-germany-security-and-identity-services"></a>Sicherheits- und Identitätsdienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/de-de/microsoft-cloud-2019-rechenzentren-deutschland/) haben wir keine neuen Kunden mehr akzeptiert und stellen keine neuen Funktionen und Services an den ursprünglichen Standorten von Microsoft Cloud Deutschland mehr bereit.
>
> Basierend auf die Entwicklung der Kundenbedürfnisse konzentriert sich unsere Cloudstrategie für Deutschland auf die Bereitstellung der [neuen Cloudregionen in Deutschland](https://news.microsoft.com/de-de/microsoft-eroeffnet-neue-cloud-rechenzentrumsregionen-in-deutschland/), die zu unserem globalen Cloudangebot passen.
>
> Starten Sie Ihre [Migration](https://docs.microsoft.com/de-de/azure/germany/germany-migration-main) noch heute und nutzen Sie die Vorteile der umfangreichen Funktionalität, Sicherheit auf Unternehmensebene und zahlreichen verfügbaren Funktionen, die in unseren neuen Rechenzentrumsregionen in Deutschland verfügbar sind.

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




