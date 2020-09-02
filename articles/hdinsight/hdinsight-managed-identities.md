---
title: Verwaltete Identitäten in Azure HDInsight
description: Bietet eine Übersicht über die Implementierung von verwalteten Identitäten in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: 07a8c26f7fc314680c51270ebafe03d4e3a84757
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88749854"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Verwaltete Identitäten in Azure HDInsight

Eine verwaltete Identität ist eine in Azure Active Directory (Azure AD) registrierte Identität, deren Anmeldeinformationen von Azure verwaltet werden. Bei Verwendung von verwalteten Identitäten müssen Sie in Azure AD keine Dienstprinzipale registrieren. Sie müssen auch keine Anmeldeinformationen, z. B. Zertifikate, verwalten.

Verwaltete Identitäten werden in Azure HDInsight bei Bedarf zum Zugreifen auf Azure AD Domain Services oder auf Dateien in Azure Data Lake Storage Gen2 verwendet.

Es gibt zwei Arten von verwalteten Identitäten: benutzerseitig und systemseitig zugewiesene Identitäten. Azure HDInsight unterstützt nur benutzerseitig zugewiesene verwaltete Identitäten. HDInsight unterstützt keine systemseitig zugewiesenen verwalteten Identitäten. Eine benutzerseitig zugewiesene verwaltete Identität wird als eigenständige Azure-Ressource erstellt, die Sie dann einer oder mehreren Azure-Dienstinstanzen zuweisen können. Im Gegensatz dazu wird eine systemseitig zugewiesene verwaltete Identität in Azure AD erstellt und dann direkt und automatisch für eine bestimmte Azure-Dienstinstanz aktiviert. Die Lebensdauer dieser systemseitig zugewiesenen verwalteten Identität ist dann an die Lebensdauer der Dienstinstanz gebunden, für die sie aktiviert wurde.

## <a name="hdinsight-managed-identity-implementation"></a>Implementierung verwalteter HDInsight-Identitäten

In Azure HDInsight können verwaltete Identitäten nur vom HDInsight-Dienst für interne Komponenten verwendet werden. Es wird derzeit keine Methode unterstützt, mit der Sie Zugriffstoken anhand der auf HDInsight-Clusterknoten installierten verwalteten Identitäten zum Zugreifen auf externe Dienste generieren können. Für einige Azure-Dienste wie Compute-VMs werden verwaltete Identitäten mit einem Endpunkt implementiert, den Sie zum Beziehen von Zugriffstoken verwenden können. Dieser Endpunkt ist zurzeit nicht in HDInsight-Knoten verfügbar.

Wenn Sie für Ihre Anwendungen ein Bootstrap durchführen müssen, um zu vermeiden, dass Geheimnisse/Kennwörter in Analyseaufträgen (z. B. SCALA-Aufträgen) platziert werden, können Sie Ihre eigenen Zertifikate mithilfe von Skriptaktionen auf die Clusterknoten verteilen und dann mithilfe dieser Zertifikate ein Zugriffstoken abrufen (etwa zum Zugreifen auf Azure Key Vault).

## <a name="create-a-managed-identity"></a>Erstellen einer verwalteten Identität

Verwaltete Identitäten können mit einer der folgenden Methoden erstellt werden:

* [Azure portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure-Befehlszeilenschnittstelle](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

Die verbleibenden Schritte zum Konfigurieren der verwalteten Identität hängen von dem Szenario ab, in dem sie verwendet werden soll.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Szenarien für verwaltete Identitäten in Azure HDInsight

Verwaltete Identitäten werden in Azure HDInsight in verschiedenen Szenarien verwendet. In den verwandten Dokumenten finden Sie ausführliche Anweisungen zur Einrichtung und Konfiguration:

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity)
* [Enterprise-Sicherheitspaket](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln](disk-encryption.md)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>Was geschieht, wenn ich die verwaltete Identität nach der Clustererstellung lösche?

In Ihrem Cluster treten Probleme auf, wenn die verwaltete Identität benötigt wird. Es gibt derzeit keine Möglichkeit, eine verwaltete Identität nach der Clustererstellung zu aktualisieren oder zu ändern. Sie sollten daher unbedingt sicherstellen, dass die verwaltete Identität während der Clusterlaufzeit nicht gelöscht wird. Alternativ können Sie den Cluster neu erstellen und eine neue verwaltete Identität zuweisen.

## <a name="next-steps"></a>Nächste Schritte

* [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md)
