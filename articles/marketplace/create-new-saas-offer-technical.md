---
title: Hinzufügen von technischen Details für Ihr SaaS-Angebot im Microsoft Partner Center
description: Erfahren Sie, wie Sie technische Details für Ihr SaaS (Software-as-a-Service)-Angebot für den kommerziellen Microsoft-Marketplace bereitstellen.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 09/02/2020
ms.openlocfilehash: 78a81a2d392363ac45979fd28138f838653a2358
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89380463"
---
# <a name="how-to-add-technical-details-for-your-saas-offer"></a>Hinzufügen von technischen Details für Ihr SaaS-Angebot

In diesem Artikel wird beschrieben, wie technische Details eingegeben werden, die es ermöglichen, eine Verbindung zwischen dem kommerziellen Microsoft-Marketplace und Ihrer Lösung herzustellen. Durch diese Verbindung können wir Ihr Angebot für den Kunden bereitstellen, wenn er es kaufen und verwalten möchte. Weitere Informationen zu diesen Einstellungen finden Sie unter [Technische Informationen](plan-saas-offer.md#technical-information).

> [!NOTE]
> Wenn Sie Transaktionen unabhängig verarbeiten möchten, wird diese Option nicht angezeigt. In diesem Fall können Sie mit [Vermarkten Ihres SaaS-Angebots](create-new-saas-offer-marketing.md) fortfahren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf der Registerkarte **Technische Konfiguration** definieren Sie die technischen Details, die vom kommerziellen Marketplace für die Kommunikation mit Ihrer SaaS-Anwendung oder -Lösung verwendet werden. 

- **URL der Angebotsseite** (erforderlich): Definieren Sie die URL der SaaS-Website (z. B. `https://contoso.com/signup`), zu der Kunden gelangen, nachdem sie Ihr Angebot im kommerziellen Marketplace erworben und den Konfigurationsvorgang vom neu erstellten SaaS-Abonnement aus gestartet haben.

  > [!IMPORTANT]
  > Ihre Angebotsseite sollte rund um die Uhr aktiv sein. Dies ist die einzige Möglichkeit, Sie über neue Käufe Ihrer SaaS-Angebote im kommerziellen Marketplace oder über Konfigurationsanforderungen eines aktiven Abonnements für ein Angebot zu benachrichtigen.

- **Verbindungswebhook** (erforderlich): Für alle asynchronen Ereignisse, die Microsoft an Sie senden muss (etwa, wenn das SaaS-Abonnement gekündigt wurde), muss eine Verbindungswebhook-URL angegeben werden. Diese URL wird aufgerufen, um Sie über das Ereignis zu informieren.

  > [!IMPORTANT]
  > Ihr Webhook sollte rund um die Uhr aktiv sein, da dies die einzige Möglichkeit ist, Sie über Aktualisierungen der SaaS-Abonnements Ihrer Kunden zu benachrichtigen, die über den kommerziellen Marketplace erworben wurden.

- **Azure Active Directory-Mandanten-ID** (erforderlich): Um die Mandanten-ID für Ihre Azure Active Directory-App (Azure AD) zu ermitteln, wechseln Sie in Azure Active Directory zum Blatt [App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade). Wählen Sie in der Spalte **Anzeigename** die App aus. Suchen Sie dann nach der aufgeführten Nummer für **Verzeichnis-ID (Mandant)** (z. B. `50c464d3-4930-494c-963c-1e951d15360e`).

- **Azure Active Directory-Anwendungs-ID** (erforderlich): Um die [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) zu ermitteln, wechseln Sie in Azure Active Directory zum Blatt [App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade). Wählen Sie in der Spalte **Anzeigename** die App aus. Suchen Sie dann nach der aufgeführten Nummer für die Anwendungs-ID (Client) wie z. B. `50c464d3-4930-494c-963c-1e951d15360e`.

Wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte fortfahren: Planübersicht

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen von Plänen für Ihr SaaS-Angebot](create-new-saas-offer-plans.md)
