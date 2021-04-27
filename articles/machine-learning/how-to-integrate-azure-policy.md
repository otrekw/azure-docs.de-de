---
title: Überwachen und Verwalten der Richtlinienkonformität
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe von Azure Policy integrierte Richtlinien für Azure Machine Learning erstellen können, um sicherzustellen, dass Ihre Arbeitsbereiche Ihren Anforderungen entsprechen.
author: aashishb
ms.author: aashishb
ms.date: 03/25/2021
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: f708e2181511da97ecffcd6f1636a2b232b4fbc6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105544365"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Überwachen und Verwalten von Azure Machine Learning mit Azure Policy

[Azure Policy](../governance/policy/index.yml) ist ein Governancetool, mit dem Sie sicherstellen können, dass Azure-Ressourcen mit Ihren Richtlinien konform sind. Mit Azure Machine Learning können Sie die folgenden Richtlinien zuweisen:

| Richtlinie | BESCHREIBUNG |
| ----- | ----- |
| **Kundenverwalteter Schlüssel** | Überwachen oder erzwingen Sie, ob Arbeitsbereiche einen kundenseitig verwalteten Schlüssel verwenden müssen. |
| **Privater Link** | Überprüfen oder erzwingen Sie, ob Arbeitsbereiche einen privaten Endpunkt zur Kommunikation mit einem virtuellen Netzwerk verwenden. |
| **Privater Endpunkt** | Konfigurieren Sie das Azure Virtual Network-Subnetz, in dem der private Endpunkt erstellt werden soll. |
| **Private DNS-Zone** | Konfigurieren Sie die private DNS-Zone, die für die private Verbindung verwendet werden soll. |
| **Benutzerseitig zugewiesene verwaltete Identität** | Überwachen oder erzwingen Sie, ob Arbeitsbereiche eine vom Benutzer zugewiesene verwaltete Identität verwenden. |

Richtlinien können in unterschiedlichen Bereichen festgelegt werden, z. B. auf Abonnement- oder Ressourcengruppenebene. Weitere Informationen finden Sie in der [Dokumentation zu Azure Policy](../governance/policy/overview.md).

## <a name="built-in-policies"></a>Integrierte Richtlinien

Azure Machine Learning stellt eine Reihe von Richtlinien bereit, die Sie für gängige Szenarien mit Azure Machine Learning verwenden können. Sie können diese Richtliniendefinitionen Ihrem vorhandenen Abonnement zuweisen oder sie als Grundlage verwenden, um eigene benutzerdefinierte Definitionen zu erstellen. Eine vollständige Liste der integrierten Richtlinien für Azure Machine Learning finden Sie unter [Integrierte Richtlinien für Azure Machine Learning](../governance/policy/samples/built-in-policies.md#machine-learning).

Um die integrierten Richtliniendefinitionen im Zusammenhang mit Azure Machine Learning anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu __Azure Policy__.
1. Wählen Sie __Definitionen__ aus.
1. Wählen Sie als __Typ__ die Option _Integriert_ und als __Kategorie__ die Option __Machine Learning__ aus.

Von hier aus können Sie Richtliniendefinitionen auswählen, um sie anzuzeigen. Beim Anzeigen einer Definition können Sie den Link __Zuweisen__ verwenden, um die Richtlinie einem bestimmten Bereich zuzuweisen und die Parameter für die Richtlinie zu konfigurieren. Weitere Informationen finden Sie unter [Zuweisen einer Richtlinie: Portal](../governance/policy/assign-policy-portal.md).

Sie können Richtlinien auch mit [Azure PowerShell](../governance/policy/assign-policy-powershell.md), der [Azure CLI](../governance/policy/assign-policy-azurecli.md) und [Vorlagen](../governance/policy/assign-policy-template.md) zuweisen.

## <a name="workspace-encryption-with-customer-managed-key"></a>Arbeitsbereichverschlüsselung mit kundenseitig verwaltetem Schlüssel

Hiermit wird gesteuert, ob Arbeitsbereiche mit einem kundenseitig verwalteten Schlüssel oder Metriken und Metadaten mithilfe eines von Microsoft verwalteten Schlüssels verschlüsselt werden sollen. Weitere Informationen zur Verwenden on kundenseitig verwalteten Schlüsseln finden Sie im Abschnitt [Azure Cosmos DB](concept-data-encryption.md#azure-cosmos-db) des Artikels zur Datenverschlüsselung.

Um diese Richtlinie zu konfigurieren, legen Sie den Effektparameter auf __audit__ (Überwachen) oder __Deny__ (Verweigern) fest. Bei Festlegung auf __audit__ können Sie einen Arbeitsbereich ohne einen kundenseitig verwalteten Schlüssel erstellen, und im Aktivitätsprotokoll wird ein Warnungsereignis erstellt.

Wenn die Richtlinie auf __deny__ festgelegt ist, können Sie keinen Arbeitsbereich erstellen, es sei denn, es wird ein kundenseitig verwalteter Schlüssel angegeben. Der Versuch, einen Arbeitsbereich ohne einen kundenseitig verwalteten Schlüssel zu erstellen, führt zu einem Fehler, der `Resource 'clustername' was disallowed by policy` ähnelt und einen Fehler im Aktivitätsprotokoll generiert. Der Richtlinienbezeichner wird ebenfalls als Teil dieses Fehlers zurückgegeben.

## <a name="workspace-should-use-private-link"></a>Arbeitsbereiche sollen Private Link verwenden

Hiermit wird gesteuert, ob ein Arbeitsbereich über Azure Private Link mit Azure Virtual Network kommunizieren soll. Weitere Informationen zum Verwenden von Private Link finden Sie unter [Konfigurieren von Private Link für einen Arbeitsbereich](how-to-configure-private-link.md).

Um diese Richtlinie zu konfigurieren, legen Sie den Effektparameter auf __audit__ (Überwachen) oder __Deny__ (Verweigern) fest. Bei Festlegung auf __audit__ können Sie einen Arbeitsbereich ohne Private Link erstellen, und im Aktivitätsprotokoll wird ein Warnungsereignis erstellt.

Bei Festlegung der Richtlinie auf __deny__ können Sie nur dann einen Arbeitsbereich erstellen, wenn Private Link verwendet wird. Der Versuch, einen Arbeitsbereich ohne Private Link zu erstellen, führt zu einem Fehler. Der Fehler wird auch im Aktivitätsprotokoll protokolliert. Der Richtlinienbezeichner wird als Teil dieses Fehlers zurückgegeben.

## <a name="workspace-should-use-private-endpoint"></a>Arbeitsbereiche sollen private Endpunkte verwenden

Hiermit wird ein Arbeitsbereich so konfiguriert, dass innerhalb des angegebenen Azure Virtual Network-Subnetzes ein privater Endpunkt erstellt wird.

Legen Sie zur Konfiguration dieser Richtlinie den Parameter „effect“ auf __DeployIfNotExists__ fest. Stellen Sie die __privateEndpointSubnetID__ auf die Azure Ressourcen-Manager-ID des Subnetzes ein.
## <a name="workspace-should-use-private-dns-zones"></a>Arbeitsbereiche sollen private DNS-Zonen verwenden

Hiermit wird ein Arbeitsbereich so konfiguriert, dass eine private DNS-Zone verwendet und dabei die DNS-Standardauflösung für einen privaten Endpunkt überschrieben wird.

Legen Sie zur Konfiguration dieser Richtlinie den Parameter „effect“ auf __DeployIfNotExists__ fest. Geben Sie für __privateDnsZoneId__ die Azure Resource Manager-ID der privaten DNS-Zone an, die verwendet werden soll. 

## <a name="workspace-should-use-user-assigned-managed-identity"></a>Der Arbeitsbereich sollte eine vom Benutzer zugewiesene verwaltete Identität verwenden

Steuert, ob ein Arbeitsbereich mit einer vom System zugewiesenen verwalteten Identität (Standard) oder einer vom Benutzer zugewiesenen verwalteten Identität erstellt wird. Die verwaltete Identität für den Arbeitsbereich wird für den Zugriff auf zugehörige Ressourcen wie Azure Storage, Azure Container Registry, Azure Key Vault und Azure Anwendung Insights verwendet. Weitere Informationen finden Sie unter [Verwenden verwalteter Identitäten mit Azure Machine Learning](how-to-use-managed-identities.md).

Um diese Richtlinie zu konfigurieren, setzen Sie den Effekt-Parameter auf __prüfen__, __ablehnen__, oder __deaktiviert__ ein. Wenn diese Option auf __Audit__ gesetzt ist, können Sie einen Arbeitsbereich erstellen, ohne eine dem Benutzer zugewiesene verwaltete Identität anzugeben. Es wird eine vom System zugewiesene Identität verwendet und ein Warnereignis im Aktivitätsprotokoll erstellt.

Wenn die Richtlinie auf __Verweigern__ eingestellt ist, können Sie keinen Arbeitsbereich erstellen, es sei denn, Sie geben während des Erstellungsprozesses eine dem Benutzer zugewiesene Identität an. Der Versuch, einen Arbeitsbereich zu erstellen, ohne eine dem Benutzer zugewiesene Identität anzugeben, führt zu einem Fehler. Der Fehler wird auch in das Aktivitätsprotokoll aufgenommen. Der Richtlinienbezeichner wird als Teil dieses Fehlers zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

* [Dokumentation zu Azure Policy](../governance/policy/overview.md)
* [Integrierte Richtlinien für Azure Machine Learning](policy-reference.md)
* [Arbeiten mit Sicherheitsrichtlinien mit Azure Security Center](../security-center/tutorial-security-policy.md)