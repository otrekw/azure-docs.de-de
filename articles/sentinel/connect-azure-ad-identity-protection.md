---
title: Verknüpfen von Azure AD Identity Protection-Daten mit Azure Sentinel
description: Erfahren Sie, wie Sie Protokolle und Warnungen aus Azure AD Identity Protection an Azure Sentinel streamen, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern.
author: yelevin
manager: rkarlin
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 06/24/2020
ms.author: yelevin
ms.openlocfilehash: f420e80f10072c440f5401c042d6370a061b1a7f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98632240"
---
# <a name="connect-data-from-azure-active-directory-azure-ad-identity-protection"></a>Verbinden von Daten aus Azure AD Identity Protection

Sie können Protokolle aus [Azure AD Identity Protection](../active-directory/identity-protection/overview-identity-protection.md) an Azure Sentinel streamen, sodass Warnungen an Azure Sentinel gestreamt werden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Azure Active Directory Identity Protection bietet eine konsolidierte Ansicht der gefährdeten Benutzer, der Risikoerkennungen und der Sicherheitsrisiken sowie die Möglichkeit, Risiken umgehend zu beseitigen und Richtlinien zur automatischen Behandlung zukünftiger Ereignisse festzulegen. Der Dienst basiert auf der umfangreichen Erfahrung von Microsoft in Bezug auf den Schutz von Kundenidentitäten und erzielt bei über 13 Milliarden Anmeldeereignissen pro Tag eine extrem hohe Genauigkeit. 

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen über ein [Azure AD Premium P2-Abonnement](https://azure.microsoft.com/pricing/details/active-directory/) verfügen.
- Sie müssen über einen Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen verfügen.

## <a name="connect-to-azure-ad-identity-protection"></a>Herstellen einer Verbindung mit Azure AD Identity Protection

Wenn Sie über ein Azure AD Premium P2-Abonnement verfügen, ist Azure AD Identity Protection enthalten. Wenn [Richtlinien aktiviert sind](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md) und Warnungen generieren, können die Warnungsdaten ganz einfach an Azure Sentinel gestreamt werden.

1. Klicken Sie in Azure Sentinel auf **Data connectors** (Datenconnectors) und anschließend auf die Kachel **Azure AD Identity Protection**.

1. Klicken Sie auf **Verbinden**, um das Streamen von Azure AD Identity Protection-Ereignissen an Azure Sentinel zu starten.

1. Suchen Sie nach **SecurityAlert**, um in der Protokollanalyse das relevante Schema für die Azure AD Identity Protection-Warnungen zu verwenden.

Wenn Sie den Connector testen möchten, können Sie [Erkennungen simulieren](../active-directory/identity-protection/howto-identity-protection-simulate-risk.md), um Beispielwarnungen zu generieren, die an Azure Sentinel gestreamt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument erfahren Sie, wie Sie Azure AD Identity Protection mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
