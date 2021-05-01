---
title: 'Beim Sqoop-Import-/Exportbefehl tritt bei einigen Benutzern in ESP-Clustern ein Fehler auf: Azure HDInsight'
description: 'Fehler beim Apache Sqoop-Import-/Exportbefehl mit der Fehlermeldung „Fehler beim Importieren: java.io.IOException: Der Besitz im Stagingverzeichnis /benutzer/ihr_benutzername/.staging entspricht nicht der Erwartung“ für einige Benutzer im Azure HDInsight-ESP-Cluster'
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/01/2021
ms.openlocfilehash: 79aae099e30bae7d5201fae3384f7b89b6c30bcf
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387165"
---
# <a name="scenario-sqoop-importexport-command-fails-for-usernames-greater-than-20-characters-in-azure-hdinsight-esp-clusters"></a>Szenario: Beim Sqoop-Import-/Exportbefehl tritt bei Benutzernamen mit mehr als 20 Zeichen in Azure HDInsight-ESP-Clustern ein Fehler auf.

In diesem Artikel werden ein bekanntes Problem und eine Problemumgehung bei der Verwendung von Clustern mit Unterstützung von Azure HDInsight ESP (Enterprise Security Pack) bei Verwendung des ADLS Gen2-Speicherkontos (ABFS) beschrieben.

## <a name="issue"></a>Problem

Beim Ausführen des Sqoop-Import-/Exportbefehls tritt bei einigen Benutzern folgender Fehler auf:

```
ERROR tool.ImportTool: Import failed: java.io.IOException:
The ownership on the staging directory /user/yourlongdomainuserna/.staging is not as expected. 
It is owned by yourlongdomainusername.
The directory must be owned by the submitter yourlongdomainuserna or yourlongdomainuserna@AADDS.CONTOSO.COM
```

Im obigen Beispiel ist in `/user/yourlongdomainuserna/.staging` zu erkennen, dass der Benutzername `yourlongdomainusername` auf 20 Zeichen gekürzt worden ist.

## <a name="cause"></a>Ursache

Die Länge des Benutzernamens überschreitet 20 Zeichen. 

Weitere Informationen finden Sie unter [Synchronisieren von Objekten und Anmeldeinformationen in einer verwalteten Azure Active Directory Domain Services-Domäne](../active-directory-domain-services/synchronization.md).

## <a name="workaround"></a>Problemumgehung

Verwenden Sie einen Benutzernamen mit höchstens 20 Zeichen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]
