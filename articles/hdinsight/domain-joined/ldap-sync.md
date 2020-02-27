---
title: LDAP-Synchronisierung in Ranger und Apache Ambari in Azure HDInsight
description: Hier werden die LDAP-Synchronisierung in Ranger und Ambari behandelt und allgemeine Richtlinien bereitgestellt.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 99bd1ac156b12a5be7b8c5c17eb5b568b7070a25
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465667"
---
# <a name="ldap-sync-in-ranger-and-apache-ambari-in-azure-hdinsight"></a>LDAP-Synchronisierung in Ranger und Apache Ambari in Azure HDInsight

Cluster des HDInsight-Enterprise-Sicherheitspakets (ESP) verwenden Ranger für die Autorisierung. Mit Apache Ambari und Ranger werden Benutzer und Gruppen unabhängig voneinander synchronisiert, und beide funktionieren ein wenig anders. In diesem Artikel wird die LDAP-Synchronisierung in Ranger und Ambari behandelt.

## <a name="general-guidelines"></a>Allgemeine Richtlinien

* Stellen Sie Cluster immer mit Gruppen bereit.
* Anstatt Gruppenfilter in Ambari und Ranger zu ändern, versuchen Sie, diese in Azure AD zu verwalten und die erforderlichen Benutzer mithilfe geschachtelter Gruppen einzubinden.
* Sobald ein Benutzer synchronisiert ist, wird er auch dann nicht entfernt, wenn er nicht Bestandteil der Gruppen ist.
* Wenn Sie die LDAP-Filter direkt ändern müssen, verwenden Sie zuerst die Benutzeroberfläche, da Sie einige Überprüfungen enthält.

## <a name="users-are-synced-separately"></a>Benutzer werden getrennt synchronisiert

Ambari und Ranger nutzen die Benutzerdatenbank nicht gemeinsam, da Sie zwei unterschiedlichen Zwecken dienen. Wenn ein Benutzer die Ambari-Benutzeroberfläche verwenden muss, muss er mit Ambari synchronisiert werden. Ist der Benutzer nicht mit Ambari synchronisiert, wird er von der Ambari-Benutzeroberfläche/API abgelehnt, doch andere Teile des Systems funktionieren (diese werden von Ranger oder Resource Manager und nicht von Ambari geschützt). Wenn Sie den Benutzer in eine Ranger-Richtlinie einschließen möchten, synchronisieren Sie ihn mit Ranger.

Wenn ein sicherer Cluster bereitgestellt wird, werden Gruppenmitglieder transitiv (alle Untergruppen und deren Mitglieder) sowohl mit Ambari als auch mit Ranger synchronisiert. 

## <a name="ambari-user-sync-and-configuration"></a>Synchronisierung und Konfiguration von Ambari-Benutzern

Auf den Hauptknoten wird stündlich ein Cron-Auftrag (`/opt/startup_scripts/start_ambari_ldap_sync.py`) ausgeführt, um die Benutzersynchronisierung zu planen. Der Cron-Auftrag ruft die Ambari-REST-APIs zum Durchführen der Synchronisierung auf. Das Skript sendet eine Liste von Benutzern und Gruppen, die synchronisiert werden sollen (da die Benutzer möglicherweise nicht den angegebenen Gruppen angehören, werden beide einzeln angegeben). Ambari synchronisiert sAMAccountName als den Benutzernamen und alle Gruppenmitglieder transitiv.

Die Protokolle müssen sich in `/var/log/ambari-server/ambari-server.log` befinden. Weitere Informationen finden Sie im Artikel zum [Konfigurieren des Ambari-Protokolliergrads](https://docs.cloudera.com/HDPDocuments/Ambari-latest/administering-ambari/content/amb_configure_ambari_logging_level.html).

In Data Lake-Clustern werden mit dem Hook nach der Benutzererstellung die Basisordner für die synchronisierten Benutzer erstellt und diese werden als Besitzer der Basisordner festgelegt. Wenn der Benutzer nicht ordnungsgemäß mit Ambari synchronisiert wird, können beim Zugriff auf Staging- und andere temporäre Ordner Fehler auftreten.

### <a name="update-groups-to-be-synced-to-ambari"></a>Aktualisieren von Gruppen für die Synchronisierung mit Ambari

Wenn Sie Gruppenmitgliedschaften nicht in Azure AD verwalten können, haben Sie zwei Möglichkeiten:

* Führen Sie eine einmalige Synchronisierung aus, wie es im Artikel zum [Synchronisieren von LDAP-Benutzern und -Gruppen](https://docs.cloudera.com/HDPDocuments/HDP3/latest/ambari-authentication-ldap-ad/content/authe_ldapad_synchronizing_ldap_users_and_groups.html) genauer beschrieben ist. Sobald sich die Gruppenmitgliedschaft ändert, müssen Sie diese Synchronisierung erneut durchführen.

* Schreiben Sie einen Cron-Auftrag, und rufen Sie die [Ambari-API regelmäßig](https://community.cloudera.com/t5/Support-Questions/How-do-I-automate-the-Ambari-LDAP-sync/m-p/96634) mit den neuen Gruppen auf.

## <a name="ranger-user-sync-and-configuration"></a>Synchronisierung und Konfiguration von Ranger-Benutzern

Ranger verfügt über ein integriertes Synchronisierungsmodul, das stündlich zur Synchronisierung der Benutzer ausgeführt wird. Die Benutzerdatenbank wird nicht mit Ambari gemeinsam genutzt. HDInsight konfiguriert den Suchfilter so, dass der Administratorbenutzer, der Watchdog-Benutzer und die Mitglieder der Gruppe, die während der Clustererstellung angegeben wird, synchronisiert werden. Die Gruppenmitglieder werden transitiv synchronisiert:

* Deaktivieren Sie die inkrementelle Synchronisierung.
* Aktivieren Sie die Zuordnung zur Benutzergruppensynchronisierung.
* Geben Sie den Suchfilter an, um die transitiven Gruppenmitglieder einzubeziehen.
* Synchronisieren Sie sAMAccountName für Benutzer und das Namensattribut für Gruppen.

### <a name="group-or-incremental-sync"></a>Gruppen- oder inkrementelle Synchronisierung

Ranger unterstützt eine Gruppensynchronisierungsoption. Diese funktioniert aber als Schnittmenge mit dem Benutzerfilter und stellt keine Zusammenfassung von Gruppenmitgliedschaften und Benutzerfilter dar. Ein typischer Anwendungsfall für den Gruppensynchronisierungsfilter in Ranger ist Gruppenfilter: (dn=clusteradmingroup), Benutzerfilter: (city=seattle).

Die inkrementelle Synchronisierung funktioniert nur für die Benutzer, die bereits synchronisiert wurden (zum ersten Mal). Die inkrementelle Synchronisierung erfolgt nicht für neue Benutzer, die den Gruppen nach der ersten Synchronisierung hinzugefügt wurden.

### <a name="update-ranger-sync-filter"></a>Aktualisieren des Ranger-Synchronisierungsfilters

Der LDAP-Filter befindet sich auf der Ambari-Benutzeroberfläche im Abschnitt mit der Konfiguration der Ranger-Benutzersynchronisierung. Der vorhandene Filter hat die Form `(|(userPrincipalName=bob@contoso.com)(userPrincipalName=hdiwatchdog-core01@CONTOSO.ONMICROSOFT.COM)(memberOf:1.2.840.113556.1.4.1941:=CN=hadoopgroup,OU=AADDC Users,DC=contoso,DC=onmicrosoft,DC=com))`. Stellen Sie sicher, dass Sie ein Prädikat am Ende hinzufügen, und testen Sie den Filter mithilfe des Suchbefehls `net ads`, mit ldp.exe oder etwas Ähnlichem.

## <a name="ranger-user-sync-logs"></a>Protokolle der Ranger-Benutzersynchronisierung

Die Ranger-Benutzersynchronisierung kann über einen der Hauptknoten erfolgen. Die Protokolle befinden sich in `/var/log/ranger/usersync/usersync.log`. Führen Sie die folgenden Schritte aus, um den Ausführlichkeitsgrad der Protokolle zu erhöhen:

1. Melden Sie sich bei Ambari an.
1. Wechseln Sie zum Abschnitt mit der Ranger-Konfiguration.
1. Wechseln Sie zum erweiterten Abschnitt **usersync-log4j**.
1. Ändern Sie `log4j.rootLogger` in `DEBUG` (nach der Änderung sollte dies `log4j.rootLogger = DEBUG,logFile,FilterLog` ähneln).
1. Speichern Sie die Konfiguration, und starten Sie Ranger neu.

## <a name="next-steps"></a>Nächste Schritte

* [Authentifizierungsprobleme in Azure HDInsight](./domain-joined-authentication-issues.md)
* [Synchronisieren von Azure AD-Benutzern mit einem HDInsight-Cluster](../hdinsight-sync-aad-users-to-cluster.md)
