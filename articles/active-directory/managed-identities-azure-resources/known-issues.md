---
title: Bekannte Probleme mit verwalteten Identitäten in Azure Active Directory
description: Bekannte Probleme mit verwalteten Identitäten für Azure-Ressourcen
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 04/08/2021
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 8d2b6323a15595e57e7e89c6a83f9f718422e1d7
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107226921"
---
# <a name="known-issues-with-managed-identities"></a>Bekannte Probleme mit verwalteten Identitäten

In diesem Artikel werden einige Probleme im Zusammenhang mit verwalteten Identitäten und deren Behebung erläutert. Häufig gestellte Fragen zu verwalteten Identitäten sind in unserem Artikel [Häufig gestellte Fragen](managed-identities-faq.md) dokumentiert.
## <a name="vm-fails-to-start-after-being-moved"></a>Der virtuelle Computer startet nicht, nachdem er verschoben wurde 

Wenn Sie einen virtuellen Computer im Ausführungszustand aus einer Ressourcengruppe oder einem Abonnement verschieben, wird er während des Verschiebens weiterhin ausgeführt. Wenn der virtuelle Computer nach dem Verschieben allerdings beendet und neu gestartet wird, schlägt der Start fehl. Die Ursache dieses Problems besteht darin, dass der virtuelle Computer den Verweis auf die verwalteten Identitäten für Azure-Ressourcen nicht aktualisiert und weiterhin auf die alte Ressourcengruppe verweist.

**Problemumgehung** 
 
Lösen Sie ein Update auf dem virtuellen Computer aus, damit die richtigen Werte für die verwalteten Identitäten für Azure-Ressourcen abgerufen werden können. Sie können eine VM-Eigenschaftsänderung vornehmen, um den Verweis auf die verwalteten Identitäten für Azure-Ressourcen zu aktualisieren. Beispielsweise können Sie mit dem folgenden Befehl einen neuen Tagwert auf dem virtuellen Computer festlegen:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Mit diesem Befehl wird das neue Tag „fixVM“ mit dem Wert 1 auf dem virtuellen Computer festgelegt. 
 
Durch das Festlegen dieser Eigenschaft wird der virtuelle Computer mit dem richtigen Ressourcen-URI der verwalteten Identitäten für Azure-Ressourcen aktualisiert. Anschließend sollte es möglich sein, den virtuellen Computer zu starten. 
 
Nachdem der virtuelle Computer gestartet wurde, kann das Tag mithilfe des folgenden Befehls entfernt werden:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="transferring-a-subscription-between-azure-ad-directories"></a>Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen

Verwaltete Identitäten werden nicht aktualisiert, wenn ein Abonnement in ein anderes Verzeichnis verschoben/übertragen wird. Infolgedessen tritt für alle vorhandenen system- oder benutzerseitig zugewiesenen verwalteten Identitäten ein Fehler auf. 

Problemumgehung für verwaltete Identitäten in einem Abonnement, die in ein anderes Verzeichnis verschoben wurden:

 - Für vom System zugewiesene verwaltete Identitäten: Deaktivieren Sie die Identitäten, und aktivieren Sie sie erneut. 
 - Für vom Benutzer zugewiesene verwaltete Identitäten: Löschen Sie die Identitäten, erstellen Sie sie neu, und fügen Sie sie erneut an die erforderlichen Ressourcen an (z. B. virtuelle Computer).

Weitere Informationen finden Sie unter [Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis](../../role-based-access-control/transfer-subscription.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im Artikel [Dienste mit Unterstützung verwalteter Identitäten](services-support-managed-identities.md), in dem diese Dienste aufgelistet sind, und im Artikel [Häufig gestellte Fragen](managed-identities-faq.md).
