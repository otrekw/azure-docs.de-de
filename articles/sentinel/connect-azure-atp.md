---
title: Verbinden von Microsoft Defender for Identity-Daten (ehemals Azure ATP) mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Protokolle aus Microsoft Defender for Identity (ehemals Azure Advanced Threat Protection (ATP)) mit einem einzigen Klick an Azure Sentinel streamen können.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 1fe36dc7b3c04f033c1b693b657e07bcf42e3223
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101715002"
---
# <a name="connect-data-from-microsoft-defender-for-identity-formerly-azure-advanced-threat-protection"></a>Verbinden von Daten aus Microsoft Defender for Identity (ehemals Azure Advanced Threat Protection)

> [!IMPORTANT]
> Der Microsoft Defender for Identity-Datenconnector in Azure Sentinel befindet sich derzeit in der Public Preview.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel wird beschrieben, wie Sie Sicherheitswarnungen aus [Microsoft Defender for Identity](/azure-advanced-threat-protection/what-is-atp) zu Azure Sentinel streamen. 

Damit Integritätswarnungen zusätzlich zu Sicherheitswarnungen weitergeleitet werden, integrieren Sie Microsoft Defender for Identity mit einem Syslog-Server. Weitere Informationen finden Sie in der Dokumentation zu [Microsoft Defender for Identity](/defender-for-identity/setting-syslog). 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen.
- Sie müssen Vorschaukunde von Microsoft Defender for Identity sein und die Integration von Microsoft Defender for Identity mit Microsoft Cloud App Security aktivieren. Weitere Informationen finden Sie unter [Microsoft Defender for Identity-Integration](https://www.microsoft.com/microsoft-365/identity/advance-threat-protection).

## <a name="connect-to-microsoft-defender-for-identity"></a>Herstellen einer Verbindung mit Microsoft Defender for Identity

Stellen Sie sicher, dass die Vorschauversion von Microsoft Defender for Identity [in Ihrem Netzwerk aktiviert ist](/azure-advanced-threat-protection/install-atp-step1).
Wenn Microsoft Defender for Identity bereitgestellt wurde und Ihre Daten erfasst, können die verdächtigen Warnungen problemlos an Azure Sentinel gestreamt werden. Es kann bis zu 24 Stunden dauern, bis mit dem Streamen der Warnungen an Azure Sentinel begonnen wird.


1. Sie müssen zunächst die Integration von Microsoft Defender for Identity mit Microsoft Cloud App Security aktivieren, um eine Verbindung zwischen Microsoft Defender for Identity und Azure Sentinel herstellen zu können. Weitere Informationen hierzu finden Sie unter [Microsoft Defender for Identity-Integration](https://www.microsoft.com/microsoft-365/identity/advance-threat-protection).

1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Microsoft Defender for Identity (Preview)** (Microsoft Defender for Identity (Vorschau)).

1. Sie können auswählen, ob die Warnungen von Microsoft Defender for Identity automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

1. Klicken Sie auf **Verbinden**.

1. Suchen Sie nach **SecurityAlert**, um in Log Analytics das relevante Schema für die Microsoft Defender for Identity-Warnungen zu verwenden.

> [!NOTE]
> Wenn die Warnungen größer als 30 KB sind, zeigt Azure Sentinel das Feld für Entitäten in den Warnungen nicht mehr an.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Microsoft Defender for Identity mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).