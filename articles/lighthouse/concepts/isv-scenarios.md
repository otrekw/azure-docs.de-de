---
title: Azure Lighthouse in ISV-Szenarien
description: Die Funktionen von Azure Lighthouse können von ISVs verwendet werden, um mit Angeboten flexibler auf Kundenwünsche zu reagieren.
ms.date: 12/18/2020
ms.topic: conceptual
ms.openlocfilehash: d6a12a51d360ad236563b871dbd94cc442ade434
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97696297"
---
# <a name="azure-lighthouse-in-isv-scenarios"></a>Azure Lighthouse in ISV-Szenarien

Ein häufiges Szenario für [Azure Lighthouse](../overview.md) ist ein Dienstanbieter, der Ressourcen in den Azure AD-Mandanten (Azure Active Directory) seiner Kunden verwaltet. Die Funktionen von Azure Lighthouse können auch von unabhängigen Software Anbietern (ISVs) verwendet werden, die Ihren Kunden SaaS-basierte Angebote machen. Azure Lighthouse ist besonders nützlich für ISVs, die verwaltete Dienste oder Support anbieten, für die Zugriff auf den Abonnementbereich erforderlich ist.

## <a name="managed-service-offers-in-azure-marketplace"></a>Angebote für verwaltete Dienste im Azure Marketplace

Als ISV haben Sie möglicherweise bereits Lösungen für den Azure Marketplace veröffentlicht. Wenn Sie Ihren Kunden verwaltete Dienste anbieten, können Sie diese in Form von verwalteten Dienstangeboten veröffentlichen. Durch diese Angebote lässt sich der Onboardingprozesses optimieren. Ihre Dienste lassen sich so besser für die erforderliche Kundenanzahl skalieren. Azure Lighthouse unterstützt eine große Bandbreite [Verwaltungsaufgaben und Szenarien](cross-tenant-management-experience.md#enhanced-services-and-scenarios), die Sie nutzen können, um Ihren Kunden einen Mehrwert zu bieten.

Weitere Informationen finden Sie unter [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md)

## <a name="using-azure-lighthouse-with-azure-managed-applications"></a>Verwendung von Azure Lighthouse mit verwalteten Azure-Anwendungen

[Verwaltete Azure-Anwendungen](../../azure-resource-manager/managed-applications/overview.md) sind eine weitere Möglichkeit für ISVs, Dienste für ihre Kunden bereitzustellen. Sie können Azure Lighthouse zusammen mit Ihren verwalteten Azure-Anwendungen verwenden, um erweiterte Szenarien zu ermöglichen.

Weitere Informationen finden Sie unter [Gemeinsame Verwendung von Azure Lighthouse und verwalteten Azure-Anwendungen](managed-applications.md)

## <a name="saas-based-multi-tenant-offerings"></a>SaaS-basierte Angebote mit mehreren Mandanten

In einem weiteren Szenario hostet ein ISV die Ressourcen in einem Abonnement in seinem eigenen Mandanten und verwendet Azure Lighthouse, um seinen Kunden Zugriff auf diese Ressourcen zu geben. Der Kunde kann sich dann bei seinem eigenen Mandanten anmelden und bei Bedarf auf diese Ressourcen zugreifen. ISVs behalten ihre IP-Adresse in ihrem eigenen Mandanten und können ihre eigenen Supportpläne verwenden, um Tickets für die in ihrem Mandanten gehostete Lösung zu nutzen, anstatt den Plan des Kunden zu verwenden. Da sich die Ressourcen im Mandanten des ISV befinden, können alle Aktionen direkt vom ISV ausgeführt werden, wie z. B. das Anmelden bei VMs, das Installieren von Apps und das Ausführen von Wartungsaufgaben.

In diesem Szenario wird im Grunde den Benutzern im Mandanten des Kunden der Zugriff als „verwaltender Mandant“ gewährt, auch wenn der Kunde die Ressourcen des ISV nicht verwaltet. Da sie direkt auf den Mandanten des ISV zugreifen, ist es wichtig, nur die erforderlichen Mindestberechtigungen zu erteilen, damit Kunden nicht versehentlich Änderungen an der Lösung oder an anderen ISV-Ressourcen vornehmen können.

Um diese Architektur zu ermöglichen, muss der ISV die Objekt-ID für eine Benutzergruppe im Azure AD-Mandanten des Kunden zusammen mit dessen Mandanten-ID abrufen. Der ISV erstellt dann eine ARM-Vorlage, in der dieser Benutzergruppe die entsprechenden Berechtigungen erteilt werden, und [stellt sie im Abonnement des ISV bereit](../how-to/onboard-customer.md), das die Ressourcen enthält, auf die der Kunde zugreift.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](cross-tenant-management-experience.md).
- Informationen zur [delegierten Azure-Ressourcenverwaltung](azure-delegated-resource-management.md)