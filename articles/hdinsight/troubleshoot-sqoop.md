---
title: 'Beim Sqoop-Import-/Exportbefehl tritt bei einigen Benutzern in ESP-Clustern ein Fehler auf: Azure HDInsight'
description: 'Fehler beim Apache Sqoop-Import-/Exportbefehl mit der Fehlermeldung „Fehler beim Importieren: java.io.IOException: Der Besitz im Stagingverzeichnis /benutzer/ihr_benutzername/.staging entspricht nicht der Erwartung“ für einige Benutzer im Azure HDInsight-ESP-Cluster'
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/01/2021
ms.openlocfilehash: 2eba44d735b1b8ea9a88164ede0ca4860efcce8a
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112283931"
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

[!INCLUDE [troubleshooting next steps](includes/hdinsight-troubleshooting-next-steps.md)]
