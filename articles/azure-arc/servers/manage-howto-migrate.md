---
title: Regionsübergreifendes Migrieren von Azure Arc-fähigen Servern
description: Erfahren Sie, wie Sie einen Azure Arc-fähigen Server aus einer Region in eine andere migrieren.
ms.date: 02/10/2021
ms.topic: conceptual
ms.openlocfilehash: 251a347205d93af715add52db293d8000438df44
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101650174"
---
# <a name="how-to-migrate-azure-arc-enabled-servers-across-regions"></a>Regionsübergreifendes Migrieren von Azure Arc-fähigen Servern

Es gibt Szenarien, in denen Sie Ihre vorhandenen Azure Arc-fähigen Server aus einer Region in eine andere verschieben möchten. Beispielsweise wenn Sie erkannt haben, dass der Computer in der falschen Region registriert wurde, um die Verwaltbarkeit zu verbessern oder ein Umzug aus Governancegründen.

Um einen Azure Arc-fähigen Server aus einer Azure-Region in eine andere zu migrieren, müssen Sie die VM-Erweiterungen deinstallieren, die Ressource in Azure löschen und sie in der anderen Region neu erstellen. Bevor Sie diese Schritte ausführen, sollten Sie den Computer überprüfen, um zu verifizieren, welche VM-Erweiterungen installiert sind.

> [!NOTE]
> Installierte Erweiterungen werden zwar weiterhin ausgeführt und setzen nach Abschluss dieses Verfahrens ihren normalen Betrieb fort, aber Sie können sie nicht mehr verwalten. Wenn Sie versuchen, die Erweiterungen auf dem Computer erneut bereitzustellen, tritt möglicherweise ein unvorhersehbares Verhalten auf.

## <a name="move-machine-to-other-region"></a>Verschieben eines Computers in eine andere Region

> [!NOTE]
> Bei diesem Vorgang kommt es während der Migration zu Ausfallzeiten.

1. Entfernen Sie mithilfe von [Azure CLI](manage-vm-extensions-cli.md#remove-an-installed-extension) oder [Azure PowerShell](manage-vm-extensions-powershell.md#remove-an-installed-extension) installierte VM-Erweiterungen aus dem [Azure-Portal](manage-vm-extensions-portal.md#uninstall-extension).

2. Verwenden Sie das **azcmagent**-Tool mit dem Parameter [Disconnect](manage-agent.md#disconnect), um den Computer von Azure Arc zu trennen und die Computerressource aus Azure zu löschen. Wenn Sie den Computer von Azure Arc-fähigen Servern trennen, wird der Connected Machine-Agent nicht entfernt, und Sie müssen den Agent nicht als Teil dieses Prozesses entfernen. Sie können dies manuell ausführen, während Sie interaktiv angemeldet sind, oder Sie können mithilfe desselben Dienstprinzipals automatisieren, den Sie für das Onboarding mehrerer Agents verwendet haben, oder mithilfe eines [Zugriffstokens](../../active-directory/develop/access-tokens.md) der Microsoft Identity-Plattform. Wenn Sie keinen Dienstprinzipal zum Registrieren des Computers bei Azure Arc-fähigen Servern verwendet haben, finden Sie im folgenden [Artikel](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) Informationen zum Erstellen eines Dienstprinzipals.

3. Registrieren Sie den Connected Machine-Agent bei Arc-fähigen Servern in der anderen Region erneut. Führen Sie das `azcmagent`-Tool mit dem Parameter [Connect](manage-agent.md#connect) aus, um diesen Schritt abzuschließen.

4. Stellen Sie die VM-Erweiterungen, die dem Computer ursprünglich bereitgestellt wurden, von Arc-fähigen Servern aus erneut bereit. Wenn Sie den „Azure Monitor für VMs (Insights)“-Agent oder den Log Analytics-Agent mithilfe einer Azure-Richtlinie bereitgestellt haben, werden die Agents nach dem nächsten [Auswertungszyklus](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) erneut bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Problembehandlung finden Sie im [Handbuch zur Problembehandlung des Connected Machine-Agents](troubleshoot-agent-onboard.md).

* Erfahren Sie, wie Sie Ihren Computer mithilfe von [Azure Policy](../../governance/policy/overview.md) verwalten, wie z. B. bei der VM-[Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md), dem Überprüfen, ob der Computer dem erwarteten Log Analytics-Arbeitsbereich Bericht erstattet, beim Aktivieren der Überwachung mit [Azure Monitor mit VMs](../../azure-monitor/vm/vminsights-enable-policy.md)-Richtlinien und vieles mehr.