---
title: Sicherheitsübersicht für Azure Data Share
description: Sicherheitsübersicht für Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 10f31b74b461941b15f13e45f90b5fbc408c90fe
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108412"
---
# <a name="security-overview-for-azure-data-share"></a>Sicherheitsübersicht für Azure Data Share

Dieser Artikel bietet eine Sicherheitsübersicht über den Azure Data Share-Dienst.

## <a name="security-overview"></a>Sicherheitsübersicht

Azure Data Share nutzt die zugrunde liegende Sicherheit, die von Azure angeboten wird, um ruhende Daten und Daten während der Übertragung zu schützen. Daten werden im ruhenden Zustand verschlüsselt, wenn dies vom zugrunde liegenden Datenspeicher unterstützt wird. Daten werden auch während der Übertragung verschlüsselt. Metadaten zu einer Datenfreigabe werden im ruhenden Zustand und während der Übertragung ebenfalls verschlüsselt. 

Zugriffssteuerungen können auf der Azure Data Share-Ressourcenebene festgelegt werden, um sicherzustellen, dass nur von berechtigten Personen darauf zugegriffen wird. 

Azure Data Share nutzt die verwaltete Identität (zuvor als MSI bezeichnet), um auf Datenspeicher zuzugreifen, die für die gemeinsame Nutzung von Daten verwendet werden. Zwischen einem Datenanbieter und einem Datenconsumer findet kein Austausch von Anmeldeinformationen statt. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). Weitere Informationen zu Rollen und Berechtigungen, die für die gemeinsame Nutzung von Daten erforderlich sind, finden Sie unter [Rollen und Anforderungen](concepts-roles-permissions.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).




