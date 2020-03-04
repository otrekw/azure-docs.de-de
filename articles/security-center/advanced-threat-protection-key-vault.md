---
title: Einrichten von Advanced Threat Protection für Azure Key Vault
description: In diesem Artikel wird erläutert, wie Sie Advanced Threat Protection für Azure Key Vault in Azure Security Center einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 3ca3026a259e7f23bbfa01398a96cbdbfae1321c
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623984"
---
# <a name="threat-protection-for-azure-key-vault-preview"></a>Bedrohungsschutz für Azure Key Vault (Vorschauversion)

Advanced Threat Protection für Azure Key Vault bietet eine zusätzliche Ebene der Sicherheitsintelligenz. Dieses Tool erkennt potenziell schädliche Versuche, auf Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen. Mithilfe des nativen Advanced Threat Protection-Diensts in Azure Security Center können Sie Bedrohungen beheben, ohne ein Sicherheitsexperte zu sein und ohne weitere Sicherheitsüberwachungssysteme erlernen zu müssen.

Wenn Security Center anomale Aktivitäten erkennt, werden Warnungen angezeigt. Außerdem wird der Administrator des Abonnements per E-Mail über die Details der verdächtigen Aktivität informiert und erhält Empfehlungen, wie er die identifizierten Bedrohungen untersuchen und beheben kann.

## <a name="configuring-threat-protection-from-security-center"></a>Konfigurieren des Bedrohungsschutzes über Security Center

Standardmäßig ist Advanced Threat Protection für alle Ihre Key Vault-Konten aktiviert, wenn Sie den Standard-Tarif von Security Center abonnieren. Weitere Informationen finden Sie unter [Preise](security-center-pricing.md).

So aktivieren oder deaktivieren Sie den Schutz für ein bestimmtes Abonnement:

1. Wählen Sie im linken Bereich von Security Center **Preise und Einstellungen** aus.

1. Wählen Sie das Abonnement mit den Speicherkonten aus, für die Sie die Bedrohungserkennung aktivieren oder deaktivieren möchten.

1. Wählen Sie **Tarif**aus.

1. Suchen Sie in der Gruppe **Tarif nach Ressourcentyp auswählen** die Zeile **Key Vaults**, und wählen Sie **Aktiviert** oder **Deaktiviert** aus.

    [![Aktivieren oder Deaktivieren von Advanced Threat Protection für Key Vault in Azure Security Center](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)

1. Wählen Sie **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Advanced Threat Protection für Azure Key Vault aktivieren und deaktivieren. 

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Bedrohungsschutz in Azure Security Center:](threat-protection.md) In diesem Artikel werden die Quellen von Sicherheitswarnungen in Azure Security Center beschrieben.
- [Key Vault-Sicherheitswarnungen:](alerts-reference.md#alerts-azurekv) der Key Vault-Abschnitt der Referenztabelle für alle Azure Security Center-Warnungen