---
title: API-Voraussetzungen – Azure Marketplace
description: Voraussetzungen für das Verwenden der Cloud-Partnerportal-APIs.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 09/23/2020
ms.openlocfilehash: 374cc3398d1037b19ceddcbdaafd333f643e44ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91261157"
---
# <a name="api-prerequisites"></a>API-Voraussetzungen

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. Verwenden Sie CPP-APIs nur für vorhandene Produkte, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Sie benötigen zwei programmgesteuerte Ressourcen für die Verwendung der Cloud-Partnerportal-APIs: einen Dienstprinzipal und ein Azure AD-Zugriffstoken (Azure Active Directory).

## <a name="create-service-principal-in-azure-active-directory-tenant"></a>Erstellen eines Dienstprinzipals im Azure Active Directory-Mandanten

Zunächst müssen Sie einen Dienstprinzipal in Ihrem Azure AD-Mandanten erstellen. Diesem Mandanten wird ein eigener Satz von Berechtigungen im Cloud-Partnerportal zugewiesen. In Ihrem Code werden APIs mit diesem Mandanten statt mit Ihren persönlichen Anmeldeinformationen aufgerufen. Eine ausführliche Erläuterung zum Erstellen eines Dienstprinzipals finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="add-service-principal-to-your-account"></a>Hinzufügen des Dienstprinzipals zu Ihrem Konto

Nachdem Sie den Dienstprinzipal in Ihrem Mandanten erstellt haben, können Sie ihn als Benutzer zu Ihrem Partner Center-Portalkonto hinzufügen. Genau wie ein Benutzer kann der Dienstprinzipal ein Besitzer oder ein Mitwirkender im Portal sein. Ausführliche Informationen finden Sie unter **Nächste Schritte**.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Informationen unter [Verwalten von Azure AD-Anwendungen](partner-center-portal/manage-account.md#manage-azure-ad-applications).
