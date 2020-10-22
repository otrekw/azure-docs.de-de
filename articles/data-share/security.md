---
title: Sicherheitsübersicht für Azure Data Share
description: Sicherheitsübersicht für Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 5d815c27ecc7825f0bc1e6772654b094a799b63d
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216554"
---
# <a name="security-overview-for-azure-data-share"></a>Sicherheitsübersicht für Azure Data Share

Dieser Artikel bietet eine Sicherheitsübersicht über den Azure Data Share-Dienst.

## <a name="security-overview"></a>Sicherheitsübersicht

Azure Data Share nutzt die zugrunde liegende Sicherheit, die von Azure angeboten wird, um ruhende Daten und Daten während der Übertragung zu schützen. Daten werden im ruhenden Zustand verschlüsselt, wenn dies vom zugrunde liegenden Datenspeicher unterstützt wird. Daten werden auch während der Übertragung verschlüsselt. Metadaten zu einer Datenfreigabe werden im ruhenden Zustand und während der Übertragung ebenfalls verschlüsselt. 

Zugriffssteuerungen können auf der Azure Data Share-Ressourcenebene festgelegt werden, um sicherzustellen, dass nur von berechtigten Personen darauf zugegriffen wird. 

Azure Data Share nutzt die verwaltete Identität (zuvor als MSI bezeichnet), um auf Datenspeicher zuzugreifen, die für die gemeinsame Nutzung von Daten verwendet werden. Zwischen einem Datenanbieter und einem Datenconsumer findet kein Austausch von Anmeldeinformationen statt. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). Weitere Informationen zu Rollen und Berechtigungen, die für die gemeinsame Nutzung von Daten erforderlich sind, finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).