---
title: Anzeigen des Dienstprinzipals einer verwalteten Identität mithilfe von PowerShell – Azure AD
description: Schrittanleitungen zur Anzeige des Dienstprinzipals einer verwalteten Identität über PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02750fc9f986b486deaf3c0d58ab538f9c634096
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608329"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Anzeigen des Dienstprinzipals einer verwalteten Identität über PowerShell

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erhalten Sie Informationen zur Anzeige des Dienstprinzipals einer verwalteten Identität über PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter.
- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
- Aktivieren Sie eine [systemseitig zugewiesene Identität auf einem virtuellen Computer](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) oder in einer [Anwendung](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity).
- Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="view-the-service-principal"></a>Anzeigen des Dienstprinzipals

Mit dem folgenden Befehl zeigen Sie den Dienstprinzipal einer VM oder einer Anwendung mit aktivierter systemseitig zugewiesener Identität an. Ersetzen Sie `<VM or application name>` durch Ihre eigenen Werte.

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Anzeigen von Azure AD-Dienstprinzipalen mithilfe von PowerShell finden Sie unter [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).


