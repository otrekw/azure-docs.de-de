---
title: Verwaltete Identitäten für Azure-Ressourcen
description: Eine Übersicht über verwaltete Identitäten für Azure-Ressourcen.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 10/06/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 728ca38cc3ef3bf989a75d757c69f7ca1993d82d
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91803114"
---
# <a name="what-are-managed-identities-for-azure-resources"></a>Was sind verwaltete Identitäten für Azure-Ressourcen?

Die Verwaltung von Geheimnissen und Anmeldeinformationen für eine sichere Kommunikation zwischen verschiedenen Diensten stellt für Entwickler eine häufige Herausforderung dar. In Azure brauchen Entwickler dank verwalteter Identitäten keine Anmeldeinformationen mehr zu verwalten. Für die Azure-Ressource in Azure AD wird eine Identität bereitgestellt, mit der Azure Active Directory (Azure AD)-Token abgerufen werden. Das erleichtert auch den Zugriff auf [Azure Key Vault](../../key-vault/general/overview.md). In diesem Schlüsseltresor können Entwickler Anmeldeinformationen auf sichere Art und Weise speichern. Verwaltete Identitäten für Azure-Ressourcen sorgen für eine Lösung des Problems, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereitgestellt wird.

Wofür kann eine verwaltete Identität verwendet werden?

   > [!VIDEO https://www.youtube.com/embed/5lqayO_oeEo]

Nachstehend sind einige Vorteile der Verwendung von verwalteten Identitäten beschrieben:

- Sie brauchen keine Anmeldeinformationen zu verwalten. Sie haben nicht einmal Zugriff auf die Anmeldeinformationen.
- Mit verwalteten Identitäten kann die Authentifizierung bei jedem Azure-Dienst erfolgen, der die Azure AD-Authentifizierung (einschließlich Azure Key Vault) unterstützt.
- Die Nutzung von verwalteten Identitäten verursacht keine zusätzlichen Kosten.

> [!NOTE]
> „Verwaltete Identitäten für Azure-Ressourcen“ ist der neue Name für den Dienst, der früher als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde.

## <a name="managed-identity-types"></a>Arten von verwalteten Identitäten

Es gibt zwei Arten von verwalteten Identitäten:

- **Systemseitig zugewiesen**: Bei einigen Azure-Diensten können Sie eine verwaltete Identität direkt in einer Dienstinstanz aktivieren. Wenn Sie eine systemseitig zugewiesene verwaltete Identität aktivieren, wird in Azure AD eine Identität erstellt, die an den Lebenszyklus der jeweiligen Dienstinstanz gebunden ist. Daher löscht Azure automatisch die Identität, wenn die Ressource gelöscht wird. Entwurfsbedingt kann nur diese Azure-Ressource diese Identität zum Anfordern von Token von Azure AD verwenden.
- **Benutzerseitig zugewiesen**: Sie können eine verwaltete Identität auch als eigenständige Azure-Ressource erstellen. Sie können eine [benutzerseitig zugewiesene verwaltete Identität erstellen](how-to-manage-ua-identity-portal.md) und diese einer oder mehreren Instanzen eines Azure-Diensts zuweisen. Bei benutzerseitig zugewiesenen verwalteten Identitäten wird die Identität getrennt von den Ressourcen verwaltet, für die sie verwendet wird. </br></br>

  > [!VIDEO https://www.youtube.com/embed/OzqpxeD3fG0]

Die nachstehende Tabelle verdeutlicht die Unterschiede zwischen den beiden Arten von verwalteten Identitäten.

|  Eigenschaft    | Systemseitig zugewiesene verwaltete Identität | Benutzerseitig zugewiesene verwaltete Identität |
|------|----------------------------------|--------------------------------|
| Erstellung |  Als Teil einer Azure-Ressource (etwa eines virtuellen Azure-Computers oder einer Azure App Service-Instanz) | Als eigenständige Azure-Ressource |
| Lebenszyklus | Gemeinsamer Lebenszyklus mit der Azure-Ressource, für die die verwaltete Identität erstellt wurde. <br/> Wenn die übergeordnete Ressource gelöscht wird, wird auch die verwaltete Identität gelöscht. | Unabhängiger Lebenszyklus. <br/> Muss explizit gelöscht werden. |
| Gemeinsame Nutzung durch mehrere Azure-Ressourcen | Keine gemeinsame Nutzung möglich. <br/> Kann nur einer einzelnen Azure-Ressource zugeordnet werden. | Gemeinsame Nutzung möglich. <br/> Die gleiche benutzerseitig zugewiesene verwaltete Identität kann mehreren Azure-Ressourcen zugeordnet werden. |
| Gängige Anwendungsfälle | Workloads innerhalb einer einzelnen Azure-Ressource. <br/> Workloads, für die unabhängige Identitäten erforderlich sind. <br/> Beispielsweise eine Anwendung, die auf einem einzelnen virtuellen Computer ausgeführt wird. | Workloads, die auf mehrere Ressourcen ausgeführt werden und sich eine einzelne Identität teilen können. <br/> Workloads, die im Rahmen eines Bereitstellungsablaufs vorab für eine sichere Ressource autorisiert werden müssen. <br/> Workloads, bei denen Ressourcen häufig recycelt werden, die Berechtigungen aber konsistent bleiben sollen. <br/> Beispielsweise eine Workload, bei der mehrere virtuelle Computer auf die gleiche Ressource zugreifen müssen. |

>[!IMPORTANT]
>Ungeachtet der ausgewählten Identitätsart ist eine verwaltete Identität ein Dienstprinzipal der besonderen Art, der nur zusammen mit Azure-Ressourcen verwendet werden kann. Wenn die verwaltete Identität gelöscht wird, wird automatisch auch der entsprechende Dienstprinzipal entfernt.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>Wie kann ich verwaltete Identitäten für Azure-Ressourcen verwenden?

![Einige Beispiele für die Verwendung von verwalteten Identitäten durch Entwickler für den Zugriff auf Ressourcen über deren Code ohne Verwaltung von Authentifizierungsinformationen](media/overview/azure-managed-identities-examples.png)

## <a name="what-azure-services-support-the-feature"></a>Welche Azure-Dienste unterstützen das Feature?<a name="which-azure-services-support-managed-identity"></a>

Verwaltete Identitäten für Azure-Ressourcen können zur Authentifizierung bei Diensten verwendet werden, die die Azure AD-Authentifizierung unterstützen. Eine Liste der Azure-Dienste, die die Funktion für verwaltete Identitäten für Azure-Ressourcen unterstützen, finden Sie unter [Services that support managed identities for Azure resources](./services-support-managed-identities.md) (Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen).

## <a name="next-steps"></a>Nächste Schritte

* [Use a Windows VM system-assigned managed identity to access Resource Manager](tutorial-windows-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Windows-VM für den Zugriff auf Resource Manager)
* [Use a Linux VM system-assigned managed identity to access Resource Manager](tutorial-linux-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Linux-VM für den Zugriff auf Resource Manager)
* [Verwenden verwalteter Identitäten für App Service und Azure Functions](../../app-service/overview-managed-identity.md)
* [Verwenden von verwalteten Identitäten mit Azure Container Instances](../../container-instances/container-instances-managed-identity.md)
* [Implementieren verwalteter Identitäten für Microsoft Azure-Ressourcen](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing).