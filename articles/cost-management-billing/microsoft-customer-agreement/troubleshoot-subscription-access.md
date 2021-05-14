---
title: 'Problembehandlung beim Abonnementzugriff nach dem Unterzeichnen einer Microsoft-Kundenvereinbarung: Azure'
description: Dieser Artikel hilft Ihnen bei der Problembehandlung beim Abonnementzugriff, nachdem Sie eine neue Microsoft-Kundenvereinbarung unterzeichnet haben.
author: bandersmsft
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 04/07/2021
ms.author: banders
ms.reviewer: baolcsva
ms.openlocfilehash: 887c3018c7791fe3e827a7cb974a237cdf2358dd
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129673"
---
# <a name="troubleshoot-subscription-access-after-you-sign-a-microsoft-customer-agreement"></a>Problembehandlung beim Abonnementzugriff nach dem Unterzeichnen einer Microsoft-Kundenvereinbarung

Dieser Artikel hilft Ihnen bei der Problembehandlung beim Abonnementzugriff, nachdem Sie eine neue Microsoft-Kundenvereinbarung unterzeichnet haben. Verwenden Sie die folgenden Informationen, um allgemeine Probleme zu beheben.

## <a name="troubleshoot-subscription-access"></a>Problembehandlung beim Abonnementzugriff

Stellen Sie sicher, dass als Abrechnungskontotyp „Microsoft-Kundenvereinbarung“ festgelegt ist. Der Abrechnungskontotyp wird im Azure-Portal unter **Cost Management + Billing** angezeigt. Weitere Informationen finden Sie unter [Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung](../understand/mca-understand-your-usage.md#check-access-to-a-microsoft-customer-agreement).

## <a name="troubleshoot-viewing-your-billing-account"></a>Problembehandlung für das Anzeigen Ihres Abrechnungskontos

Wenn Sie Probleme beim Anzeigen Ihres Abrechnungskontos haben und über mehrere Mandanten verfügen, versuchen Sie, die Verzeichnisse im Azure-Portal zu wechseln.

1. Wählen Sie oben rechts im Azure-Portal Ihr Azure-Konto aus.
1. Klicken Sie dann auf Verzeichnis wechseln.  
    :::image type="content" source="./media/troubleshoot-subscription-access/switch-directory.png" alt-text="Screenshot: Option „Verzeichnis wechseln“" lightbox="./media/troubleshoot-subscription-access/switch-directory.png" :::
1. Wählen Sie im Fenster „Verzeichnis + Abonnement“ das andere Verzeichnis aus, um zu diesem Verzeichnis zu wechseln.  
    :::image type="content" source="./media/troubleshoot-subscription-access/select-directory.png" alt-text="Screenshot: Auswählen eines anderen Verzeichnisses" lightbox="./media/troubleshoot-subscription-access/select-directory.png" :::

## <a name="troubleshoot-account-access"></a>Problembehandlung beim Kontozugriff

Sie können über mehrere Konten bei Microsoft verfügen, die dieselbe E-Mail-Adresse verwenden. Möglicherweise verfügen Sie über ein persönliches Konto und ein Geschäftskonto. Die Microsoft-Kundenvereinbarung verwendet Ihr Geschäfts-, Schul- oder Unikonto. Weitere Informationen finden Sie unter [Welches Konto möchten Sie verwenden?](https://support.microsoft.com/office/which-account-do-you-want-to-use-2b5bbd7a-7df6-4283-beff-8015e28eb7b9).

- Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an.  
    :::image type="content" source="./media/troubleshoot-subscription-access/two-accounts.png" alt-text="Screenshot: Auswahl des Geschäfts-, Schul- oder Unikontos" lightbox="./media/troubleshoot-subscription-access/two-accounts.png" :::

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Dokumentation zur Microsoft-Kundenvereinbarung](./index.yml).