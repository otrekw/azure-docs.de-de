---
title: Anzeigen des Dienstprinzipals einer verwalteten Identität im Azure-Portal – Azure AD
description: Schrittanleitungen zur Anzeige des Dienstprinzipals einer verwalteten Identität im Azure-Portal.
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
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: c88f3cdc880888162301ace5c896161f669d2ede
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269198"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Anzeigen des Dienstprinzipals einer verwalteten Identität im Azure-Portal

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erhalten Sie Informationen zur Anzeige des Dienstprinzipals einer verwalteten Identität im Azure-Portal.

 > [!NOTE] 
 > Dienstprinzipale sind Unternehmensanwendungen. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter.
- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
- Aktivieren Sie eine [systemseitig zugewiesene Identität auf einem virtuellen Computer](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) oder in einer [Anwendung](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Anzeigen des Dienstprinzipals

Dieses Verfahren zeigt, wie der Dienstprinzipal einer VM mit aktivierter systemseitig zugewiesener Identität angezeigt wird. (Dieselben Schritte gelten für eine Anwendung.)

1. Klicken Sie auf **Azure Active Directory** und dann auf **Unternehmensanwendungen**.
2. Wählen Sie unter **Anwendungstyp** die Option **Alle Anwendungen** aus, und klicken Sie dann auf **Übernehmen**.
3. Geben Sie im Suchfilterfeld den Namen des virtuellen Computers oder der Anwendung mit aktivierter verwalteter Identität ein, oder wählen Sie den Namen in der angezeigten Liste aus.

   ![Anzeigen eines Dienstprinzipals für verwaltete Identitäten im Portal](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Nächste Schritte

[Verwaltete Identitäten für Azure-Ressourcen](./overview.md)