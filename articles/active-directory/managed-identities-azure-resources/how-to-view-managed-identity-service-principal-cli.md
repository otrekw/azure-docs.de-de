---
title: Anzeigen des Dienstprinzipals einer verwalteten Identität – Azure CLI – Azure AD
description: Schrittanleitungen zur Anzeige des Dienstprinzipals einer verwalteten Identität über die Azure CLI.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/30/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1c0f167a36ceff6a9b62e2a3ccf24ca6e13e6294
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91611910"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Anzeigen des Dienstprinzipals einer verwalteten Identität über die Azure CLI

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erhalten Sie Informationen zur Anzeige des Dienstprinzipals einer verwalteten Identität über die Azure CLI.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter.
- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
- Aktivieren Sie eine [systemseitig zugewiesene Identität auf einem virtuellen Computer](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) oder in einer [Anwendung](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).
- Zum Ausführen der Beispielskripts haben Sie zwei Möglichkeiten:
    - Verwenden Sie die [Azure Cloud Shell](../../cloud-shell/overview.md), die Sie mithilfe der Schaltfläche **Testen** in der oberen rechten Ecke der Codeblöcke öffnen können.
    - Führen Sie Skripts lokal aus, indem Sie die neueste Version der [Azure CLI](/cli/azure/install-azure-cli) installieren, und melden Sie sich dann mit dem Befehl [az login](/cli/azure/reference-index#az-login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, in dem Sie Ressourcen erstellen möchten.   

## <a name="view-the-service-principal"></a>Anzeigen des Dienstprinzipals

Mit dem folgenden Befehl zeigen Sie den Dienstprinzipal einer VM oder einer Anwendung mit aktivierter verwalteter Identität an. Ersetzen Sie `<Azure resource name>` durch Ihre eigenen Werte.

```azurecli-interactive
az ad sp list --display-name <Azure resource name>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Azure AD-Dienstprinzipalen mit der Azure CLI finden Sie unter [az ad sp](/cli/azure/ad/sp).
