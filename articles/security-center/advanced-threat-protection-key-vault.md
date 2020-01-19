---
title: Einrichten von Advanced Threat Protection für Azure Key Vault | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Advanced Threat Protection für Azure Key Vault in Azure Security Center einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 2375d8ee92d9c04c287b7fca793fcdc236e1e8f4
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75720037"
---
# <a name="set-up-advanced-threat-protection-for-azure-key-vault-preview"></a>Einrichten von Advanced Threat Protection für Azure Key Vault (Vorschau)

Advanced Threat Protection für Azure Key Vault bietet eine zusätzliche Ebene der Sicherheitsintelligenz. Dieses Tool erkennt potenziell schädliche Versuche, auf Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen. Mithilfe des nativen Advanced Threat Protection-Diensts in Azure Security Center können Sie Bedrohungen beheben, ohne ein Sicherheitsexperte zu sein und ohne weitere Sicherheitsüberwachungssysteme erlernen zu müssen.

Wenn Security Center anomale Aktivitäten erkennt, werden Warnungen angezeigt. Außerdem wird der Administrator des Abonnements per E-Mail über die Details der verdächtigen Aktivität informiert und erhält Empfehlungen, wie er die identifizierten Bedrohungen untersuchen und beheben kann.

> [!NOTE]
> Advanced Threat Protection für Azure Key Vault ist zurzeit nur in Regionen von Nordamerika verfügbar.

## <a name="set-up-advanced-threat-protection-from-azure-security-center"></a>Einrichten von Advanced Threat Protection im Azure Security Center

Standardmäßig ist Advanced Threat Protection für alle Ihre Key Vault-Konten aktiviert, wenn Sie den Tarif „Standard“ von Security Center abonniert haben. Weitere Informationen finden Sie unter [Preise](security-center-pricing.md).

Um den Schutz für ein bestimmtes Abonnement zu aktivieren oder zu deaktivieren, gehen Sie folgendermaßen vor:

1. Wählen Sie im linken Bereich von Security Center **Preise und Einstellungen** aus.
1. Wählen Sie das Abonnement mit den Speicherkonten aus, für die Sie die Bedrohungserkennung aktivieren oder deaktivieren möchten.
1. Wählen Sie **Tarif**aus.
1. Suchen Sie in der Gruppe **Tarif nach Ressourcentyp auswählen** die Zeile **Key Vaults**, und wählen Sie **Aktiviert** oder **Deaktiviert** aus.

    [![Aktivieren oder Deaktivieren von Advanced Threat Protection für Key Vault in Azure Security Center](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)
1. Wählen Sie **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Advanced Threat Protection für Azure Key Vault aktivieren und deaktivieren. 

Weitere verwandte Informationen finden Sie im folgenden Artikel:

- [Bedrohungserkennung für die Azure-Dienstebene in Security Center](security-center-alerts-service-layer.md): In diesem Artikel werden die Warnungen im Zusammenhang mit Advanced Threat Protection für Azure Key Vault beschrieben.
