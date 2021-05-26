---
title: Verwaltete Identitäten mit Azure Video Analyzer
description: In diesem Thema wird erläutert, wie verwaltete Identitäten mit Azure Video Analyzer verwendet werden.
ms.service: azure-video-analyzer
ms.topic: how-to
ms.date: 05/04/2021
ms.openlocfilehash: 8d9558319168e2a94c81f764c9b19f70dc904f79
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385585"
---
# <a name="managed-identity"></a>Verwaltete Identität

Die Verwaltung von Geheimnissen und Anmeldeinformationen für eine sichere Kommunikation zwischen verschiedenen Diensten stellt für Entwickler eine häufige Herausforderung dar. Bei Azure machen verwaltete Identitäten die Verwaltung von Anmeldeinformationen für Entwickler überflüssig, indem sie eine Identität für die Azure-Ressource in Azure Active Directory (Azure AD) bereitstellen und diese verwenden, um Azure AD-Tokens zu erhalten.

Wenn Sie ein Azure Video Analyzer-Konto erstellen, müssen Sie es ein Azure-Speicherkonto zuordnen. Wenn Sie Video Analyzer verwenden, um das Live-Video von einer Kamera aufzuzeichnen, werden diese Daten als Blobs in einem Container im Speicherkonto gespeichert. Sie müssen eine verwaltete Identität verwenden, um dem Video Analyzer-Konto wie folgt den entsprechenden Zugriff auf das Speicherkonto zu gewähren.


## <a name="user-assigned-managed-identity-for-video-analyzer"></a>Vom Benutzer zugewiesene verwaltete Identität für Video Analyzer

1. Erstellen Sie eine [benutzerseitig zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)

1. Erstellen eines Azure-Speicherkontos

   [!INCLUDE [the video analyzer account and storage account must be in the same subscription and region](./includes/note-account-storage-same-subscription.md)]

1. Fügen Sie der verwalteten Identität die Rollen [Blob-Daten-Speicher-Contributor](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) und [Leser](../../role-based-access-control/built-in-roles.md#reader) für das obengenannte Speicherkonto hinzu.

1. Erstellen Sie das Video Analyzer-Konto, und geben Sie die vom Benutzer zugewiesene verwaltete Identität und das Speicherkonto als Werte für die relevanten Eigenschaften an.

Video Analyzer kann dann in Ihrem Namen anhand der verwalteten Identität auf das Speicherkonto zugreifen.

In [diesem](create-video-analyzer-account.md) Artikel finden Sie ein Beispiel der Verwendung des Azure-Portals, um die obengenannten Aufgaben zu erfüllen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen verwalteter Identitäten für Sie und Ihre Azure-Anwendungen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).
