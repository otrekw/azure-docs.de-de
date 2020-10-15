---
title: Überwachen und Verwalten von Compliance mit Azure Policy
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit Azure Policy integrierte Richtlinien für Azure Machine Learning verwenden.
author: jhirono
ms.author: jhirono
ms.date: 09/15/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: e7eebb22efe32b290e078348337049c6c3e762db
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993292"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Überwachen und Verwalten von Azure Machine Learning mit Azure Policy

[Azure Policy](/azure/governance/policy) ist ein Governancetool, mit dem Sie sicherstellen können, dass Azure-Ressourcen mit Ihren Richtlinien konform sind. Mit Azure Machine Learning können Sie die folgenden Richtlinien zuweisen:

* **Kundenseitig verwalteter Schlüssel**: Überwachen oder erzwingen Sie, ob Arbeitsbereiche einen kundenseitig verwalteten Schlüssel verwenden müssen.
* **Private Link**: Überwachen Sie, ob Arbeitsbereiche mithilfe eines privaten Endpunkts mit einem virtuellen Netzwerk kommunizieren.

Richtlinien können in unterschiedlichen Bereichen festgelegt werden, z. B. auf Abonnement- oder Ressourcengruppenebene. Weitere Informationen finden Sie in der [Dokumentation zu Azure Policy](/azure/governance/policy/overview).

## <a name="built-in-policies"></a>Integrierte Richtlinien

Azure Machine Learning stellt eine Reihe von Richtlinien bereit, die Sie für gängige Szenarien mit Azure Machine Learning verwenden können. Sie können diese Richtliniendefinitionen Ihrem vorhandenen Abonnement zuweisen oder sie als Grundlage verwenden, um eigene benutzerdefinierte Definitionen zu erstellen. Eine vollständige Liste der integrierten Richtlinien für Azure Machine Learning finden Sie unter [Integrierte Richtlinien für Azure Machine Learning](/azure/governance/policy/samples/built-in-policies#machine-learning).

Um die integrierten Richtliniendefinitionen im Zusammenhang mit Azure Machine Learning anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu __Azure Policy__.
1. Wählen Sie __Definitionen__ aus.
1. Wählen Sie als __Typ__ die Option _Integriert_ und als __Kategorie__ die Option __Machine Learning__ aus.

Von hier aus können Sie Richtliniendefinitionen auswählen, um sie anzuzeigen. Beim Anzeigen einer Definition können Sie den Link __Zuweisen__ verwenden, um die Richtlinie einem bestimmten Bereich zuzuweisen und die Parameter für die Richtlinie zu konfigurieren. Weitere Informationen finden Sie unter [Zuweisen einer Richtlinie: Portal](/azure/governance/policy/assign-policy-portal).

Sie können Richtlinien auch mit [Azure PowerShell](/azure/governance/policy/assign-policy-powershell), der [Azure CLI](https://docs.microsoft.com/azure/governance/policy/assign-policy-azurecli) und [Vorlagen](/azure/governance/policy/assign-policy-template) zuweisen.

## <a name="workspaces-encryption-with-customer-managed-key"></a>Arbeitsbereichverschlüsselung mit kundenseitig verwaltetem Schlüssel

Steuert, ob Arbeitsbereiche mit einem kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) verschlüsselt werden sollen oder mithilfe eines von Microsoft verwalteten Schlüssels zum Verschlüsseln von Metriken und Metadaten. Weitere Informationen zur Verwendung von CMK finden Sie im Abschnitt [Azure Cosmos DB](concept-enterprise-security.md#azure-cosmos-db) des Artikels zur Unternehmenssicherheit.

Um diese Richtlinie zu konfigurieren, legen Sie den Effektparameter auf __audit__ (Überwachen) oder __Deny__ (Verweigern) fest. Wenn __audit__ festgelegt ist, können Sie Arbeitsbereiche ohne einen CMK erstellen, und im Aktivitätsprotokoll wird ein Warnungsereignis erstellt.

Wenn die Richtlinie auf __deny__ festgelegt ist, können Sie keinen Arbeitsbereich erstellen, es sei denn, es wird ein CMK angegeben. Der Versuch, einen Arbeitsbereich ohne einen CMK zu erstellen, führt zu einem Fehler, der `Resource 'clustername' was disallowed by policy` ähnelt und einen Fehler im Aktivitätsprotokoll generiert. Der Richtlinienbezeichner wird ebenfalls als Teil dieses Fehlers zurückgegeben.

## <a name="workspaces-should-use-private-link"></a>Arbeitsbereiche sollten Private Link verwenden

Steuert, ob Arbeitsbereiche Azure Private Link für die Kommunikation mit Azure Virtual Network verwenden sollen. Weitere Informationen zum Verwenden von Private Link finden Sie unter [Konfigurieren von Private Link für einen Arbeitsbereich](how-to-configure-private-link.md).

Um diese Richtlinie zu konfigurieren, legen Sie den Effektparameter auf __audit__ (Überwachen) fest. Wenn Sie einen Arbeitsbereich erstellen, ohne Private Link zu verwenden, wird im Aktivitätsprotokoll ein Warnereignis erstellt.

## <a name="next-steps"></a>Nächste Schritte

* [Dokumentation zu Azure Policy](/azure/governance/policy/overview)
* [Integrierte Richtlinien für Azure Machine Learning](policy-reference.md)
* [Arbeiten mit Sicherheitsrichtlinien mit Azure Security Center](/azure/security-center/tutorial-security-policy)