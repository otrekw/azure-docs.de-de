---
title: 'Behandeln häufiger Fehler: Azure Database for MySQL'
description: Hier erfahren Sie, wie Sie häufige Migrationsfehler für neue Benutzer des Azure Database for MySQL-Diensts behandeln.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: ebe9f936e3d0dfafec23842fcdbfd225995d546b
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719779"
---
# <a name="common-errors"></a>Häufige Fehler

Azure Database for MySQL ist ein vollständig verwalteter Dienst, der auf der Communityversion von MySQL basiert. Die MySQL-Umgebung in einer verwalteten Dienstumgebung kann sich von der MySQL-Ausführung in Ihrer eigenen Umgebung unterscheiden. Dieser Artikel enthält Informationen zu einigen der häufigen Fehler, die bei Benutzern auftreten können, die zu Azure Database for MySQL migrieren oder zum ersten Mal mit Azure Database for MySQL entwickeln.

## <a name="errors-due-to-lack-of-super-privilege-and-dba-role"></a>Fehler aufgrund fehlender SUPER-Berechtigung und DBA-Rolle

Die SUPER-Berechtigung und die DBA-Rolle werden für den Dienst nicht unterstützt. Folglich können allgemeine Fehler wie die folgenden auftreten:

#### <a name="error-1419-you-do-not-have-the-super-privilege-and-binary-logging-is-enabled-you-might-want-to-use-the-less-safe-log_bin_trust_function_creators-variable"></a>FEHLER 1419: Sie verfügen nicht über die SUPER-Berechtigung, und die binäre Protokollierung ist aktiviert (es *kann* ratsam sein, die weniger sichere Variable „log_bin_trust_function_creators“ zu verwenden).

Der obige Fehler kann auftreten, wenn Sie eine Funktion erstellen, etwas wie unten beschrieben auslösen oder ein Schema importieren. Die DDL-Anweisungen wie „CREATE FUNCTION“ oder „CREATE TRIGGER“ werden in das binäre Protokoll geschrieben, sodass sie vom sekundären Replikat ausgeführt werden können. Der SQL-Replikatthread verfügt über vollständige Berechtigungen, die ausgenutzt werden können, um Berechtigungen zu erhöhen. Um Server mit aktivierter binärer Protokollierung vor dieser Gefahr zu schützen, erfordert die MySQL-Engine, dass Ersteller gespeicherter Funktionen neben der üblichen erforderlichen Berechtigung „CREATE ROUTINE“ auch über die SUPER-Berechtigung verfügen. 

```sql
CREATE FUNCTION f1(i INT)
RETURNS INT
DETERMINISTIC
READS SQL DATA
BEGIN
  RETURN i;
END;
```

**Lösung:**  Legen Sie „log_bin_trust_function_creators“ auf dem Blatt [Serverparameter](howto-server-parameters.md) des Portals auf „1“ fest, führen Sie die DDL-Anweisungen aus, oder importieren Sie das Schema, um die gewünschten Objekte zu erstellen, und legen Sie den Parameter „log_bin_trust_function_creators“ nach der Erstellung wieder auf den vorherigen Wert fest.

#### <a name="error-1227-42000-at-line-101-access-denied-you-need-at-least-one-of-the-super-privileges-for-this-operation-operation-failed-with-exitcode-1"></a>FEHLER 1227 (42000) in Zeile 101: Zugriff verweigert. Für diesen Vorgang benötigen Sie (mindestens) eine SUPER-Berechtigung. Fehler für Vorgang mit Exitcode 1

Der obige Fehler kann auftreten, wenn Sie eine Dumpdatei importieren oder eine Prozedur erstellen, die Elemente vom Typ [DEFINER](https://dev.mysql.com/doc/refman/5.7/en/create-procedure.html) enthält. 

**Lösung:**  Zur Behebung dieses Fehlers kann der Administratorbenutzer durch Ausführen eines GRANT-Befehls Berechtigungen zum Erstellen oder Ausführen von Prozeduren erteilen, wie in den folgenden Beispielen gezeigt:

```sql
GRANT CREATE ROUTINE ON mydb.* TO 'someuser'@'somehost';
GRANT EXECUTE ON PROCEDURE mydb.myproc TO 'someuser'@'somehost';
```
Alternativ können Sie die DEFINER-Elemente durch den Namen des Administratorbenutzers ersetzen, der den Importvorgang ausführt, wie hier zu sehen:

```sql
DELIMITER;;
/*!50003 CREATE*/ /*!50017 DEFINER=`root`@`127.0.0.1`*/ /*!50003
DELIMITER;;

/* Modified to */

DELIMITER ;;
/*!50003 CREATE*/ /*!50017 DEFINER=`AdminUserName`@`ServerName`*/ /*!50003
DELIMITER ;
```

## <a name="next-steps"></a>Nächste Schritte
Sollten Sie nicht die gesuchte Antwort gefunden haben, haben Sie folgende Möglichkeiten:
- Stellen Sie Ihre Frage auf der [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-database-mysql.html) oder in [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
- Senden Sie eine E-Mail an das Azure Database for MySQL-Team: [@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für den technischen Support.
- Wenn Sie den Azure-Support kontaktieren möchten, [erstellen Sie ein Ticket im Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).
