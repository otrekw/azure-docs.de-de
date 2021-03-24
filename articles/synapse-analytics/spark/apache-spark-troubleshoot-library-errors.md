---
title: Beheben von Fehlern bei der Bibliotheksinstallation
description: Dieses Tutorial enthält eine Übersicht über die Problembehandlung für Fehler bei der Bibliotheksinstallation.
services: synapse-analytics
author: midesa
ms.author: midesa
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: conceptual
ms.date: 01/04/2021
ms.openlocfilehash: 57e9d0c584600a8fac90499d72cfac1620052603
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101694919"
---
# <a name="troubleshoot-library-installation-errors"></a>Beheben von Fehlern bei der Bibliotheksinstallation 
Wenn Sie Code eines Drittanbieters oder lokal erstellten Code für Ihre Anwendungen verfügbar machen möchten, können Sie in einem Ihrer serverlosen Apache Spark-Pools eine Bibliothek installieren. Die in der Datei „requirements.txt“ aufgeführten Pakete werden zum Zeitpunkt des Poolstarts von PyPi heruntergeladen. Diese Anforderungsdatei wird jedes Mal verwendet, wenn eine Spark-Instanz aus diesem Spark-Pool erstellt wird. Nachdem eine Bibliothek für einen Spark-Pool installiert wurde, ist sie in allen Sitzungen verfügbar, die denselben Pool verwenden. 

In einigen Fällen wird eine Bibliothek möglicherweise nicht in Ihrem Apache Spark-Pool angezeigt. Dieser Fall tritt häufig auf, wenn ein Fehler in der bereitgestellten Datei „requirements.txt“ oder in den angegebenen Bibliotheken vorliegt. Wenn beim Installationsvorgang der Bibliothek ein Fehler auftritt, wird der Apache Spark-Pool auf die in der Synapse-Basisruntime angegebenen Bibliotheken zurückgesetzt.

In diesem Dokument finden Sie Informationen zu häufig auftretenden Problemen und Hilfe beim Debuggen von Fehlern bei der Bibliotheksinstallation.

## <a name="force-update-your-apache-spark-pool"></a>Erzwingen eines Updates Ihres Apache Spark-Pools
Wenn Sie die Bibliotheken in Ihrem Apache Spark-Pool aktualisieren, werden diese Änderungen nach dem Neustart des Pools übernommen. Wenn Sie über aktive Aufträge verfügen, werden diese weiterhin in der ursprünglichen Version des Spark-Pools ausgeführt.

Sie können die Anwendung der Änderungen erzwingen, indem Sie die Option **Neue Einstellungen erzwingen** auswählen. Mit dieser Einstellung werden alle aktuellen Sitzungen für den ausgewählten Spark-Pool beendet. Nachdem die Sitzungen beendet wurden, müssen Sie warten, bis der Pool neu gestartet wird. 

![Hinzufügen von Python-Bibliotheken](./media/apache-spark-azure-portal-add-libraries/update-libraries.png "Hinzufügen von Python-Bibliotheken")

## <a name="track-installation-progress"></a>Nachverfolgen des Installationsstatus
Bei jeder Aktualisierung eines Pools mit einem neuen Satz von Bibliotheken wird ein vom System reservierter Spark-Auftrag gestartet. Mit diesem Spark-Auftrag können Sie den Status der Bibliotheksinstallation überwachen. Wenn bei der Installation aufgrund von Bibliothekskonflikten oder anderen Problemen ein Fehler auftritt, wird der Spark-Pool auf den vorherigen oder den Standardzustand zurückgesetzt. 

Darüber hinaus können Benutzer auch die Installationsprotokolle überprüfen, um Abhängigkeitskonflikte zu identifizieren oder zu ermitteln, welche Bibliotheken während des Poolupdates installiert wurden.

So zeigen Sie diese Protokolle an
1. Navigieren Sie auf der Registerkarte **Überwachen** zur Liste der Spark-Anwendungen. 
2. Wählen Sie den Spark-Systemanwendungsauftrag aus, der Ihrem Poolupdate entspricht. Diese Systemaufträge werden unter dem Titel *SystemReservedJob-LibraryManagement* ausgeführt.
   ![Screenshot mit vom System reservierten Bibliotheksauftrag.](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job.png "Anzeigen eines Systembibliotheksauftrags")
3. Wechseln Sie zur Anzeige der Protokolle **driver** und **stdout**. 
4. Innerhalb der Ergebnisse werden die Protokolle angezeigt, die sich auf die Installation Ihrer Pakete beziehen.
    ![Screenshot mit Ergebnissen des vom System reservierten Bibliotheksauftrags](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job-results.png "Anzeigen des Status eines Systembibliotheksauftrags")

## <a name="validate-your-permissions"></a>Überprüfen von Berechtigungen
Um Bibliotheken zu installieren und zu aktualisieren, müssen Sie über die Berechtigung **Mitwirkender an Storage-Blobdaten** oder **Besitzer von Speicherblobdaten** für das primäre Azure Data Lake Storage Gen2-Speicherkonto verfügen, das mit dem Azure Synapse Analytics-Arbeitsbereich verknüpft ist.

Um zu überprüfen, ob Sie über diese Berechtigungen verfügen, können Sie folgenden Code ausführen:

```python
from pyspark.sql.types import StructType,StructField, StringType, IntegerType
data2 = [("James","Smith","Joe","4355","M",3000),
    ("Michael","Rose","Edward","40288","F",4000)
  ]

schema = StructType([ \
    StructField("firstname",StringType(),True), \
    StructField("middlename",StringType(),True), \
    StructField("lastname",StringType(),True), \
    StructField("id", StringType(), True), \
    StructField("gender", StringType(), True), \
    StructField("salary", IntegerType(), True) \
  ])
 
df = spark.createDataFrame(data=data2,schema=schema)

df.write.csv("abfss://<<ENTER NAME OF FILE SYSTEM>>@<<ENTER NAME OF PRIMARY STORAGE ACCOUNT>>.dfs.core.windows.net/validate_permissions.csv")

```
Wenn Sie einen Fehler erhalten, fehlen Ihnen wahrscheinlich die erforderlichen Berechtigungen. Informationen dazu, wie Sie die erforderlichen Berechtigungen erhalten, finden Sie in diesem Dokument: [Zuweisen der Berechtigungen „Mitwirkender an Storage-Blobdaten“ oder „Besitzer von Speicherblobdaten“](../../storage/common/storage-auth-aad-rbac-portal.md#assign-an-azure-built-in-role).

Wenn Sie eine Pipeline ausführen, muss die Arbeitsbereichs-MSI auch über eine der Berechtigungen „Besitzer von Speicherblobdaten“ oder „Mitwirkender an Storage-Blobdaten“ verfügen. Informationen dazu, wie Sie Ihrer Arbeitsbereichsidentität diese Berechtigung zuweisen, finden Sie unter: [Erteilen von Berechtigungen für die verwaltete Identität eines Arbeitsbereichs](../security/how-to-grant-workspace-managed-identity-permissions.md).

## <a name="check-the-environment-configuration-file"></a>Überprüfen der Umgebungskonfigurationsdatei
Eine Umgebungskonfigurationsdatei kann für ein Upgrade der Conda-Umgebung verwendet werden. Die zulässigen Dateiformate für die Verwaltung von Python-Pools sind [hier](./apache-spark-manage-python-packages.md) aufgeführt.

Beachten Sie dabei folgende Einschränkungen:
   -  Der Inhalt der Anforderungsdatei darf keine zusätzlichen leeren Zeilen oder Leerzeichen enthalten. 
   -  Die [Synapse-Runtime](apache-spark-version-support.md) enthält eine Reihe von Bibliotheken, die in jedem serverlosen Apache Spark-Pool vorinstalliert sind. Pakete, die in der Basisruntime vorinstalliert sind, können nicht entfernt oder deinstalliert werden.
   -  Das Ändern der Version von PySpark, Python, Scala/Java, .NET oder Spark wird nicht unterstützt.
   -  Sitzungsbezogene Python-Bibliotheken akzeptieren nur Dateien mit der Erweiterung „.yml“.

## <a name="validate-wheel-files"></a>Überprüfen von Wheel-Dateien
Die serverlosen Synapse-Apache Spark-Pools basieren auf einer Linux-Distribution. Wenn Sie die Wheel-Dateien direkt von PyPi herunterladen und installieren, müssen Sie darauf eine Version auswählen, die unter Linux erstellt wurde und unter der gleichen Python-Version wie der Spark-Pool ausgeführt wird.

>[!IMPORTANT]
>Benutzerdefinierte Pakete können zwischen Sitzungen hinzugefügt oder geändert werden. Sie müssen jedoch warten, bis der Pool und die Sitzung neu gestartet werden, damit das aktualisierte Paket angezeigt wird.

## <a name="check-for-dependency-conflicts"></a>Überprüfen auf Abhängigkeitskonflikte
 Ganz allgemein kann das Auflösen von Python-Abhängigkeiten schwierig sein. Um das Debuggen lokaler Abhängigkeitskonflikte zu unterstützen, können Sie Ihre eigene virtuelle Umgebung basierend auf der Synapse-Runtime erstellen und darin Ihre Änderungen überprüfen.

So stellen Sie die Umgebung nach und überprüfen Ihre Updates
 1. [Laden Sie die Vorlage herunter](https://github.com/Azure-Samples/Synapse/blob/main/Spark/Python/base_environment.yml), um die Synapse-Runtime lokal neu zu erstellen. Möglicherweise gibt es geringfügige Unterschiede zwischen der Vorlage und der tatsächlichen Synapse-Umgebung.
   
 2. Erstellen Sie mithilfe der [folgenden Anweisungen](https://docs.conda.io/projects/conda/latest/user-guide/tasks/manage-environments.html) eine virtuelle Umgebung. In dieser Umgebung können Sie eine isolierte Python-Installation mit der angegebenen Liste von Bibliotheken erstellen. 
    
    ```
    conda myenv create -f environment.yml
    conda activate myenv
    ```
   
 3. Mit ``pip install -r <provide your req.txt file>`` aktualisieren Sie die virtuelle Umgebung mit den angegebenen Paketen. Wenn bei der Installation ein Fehler auftritt, liegt möglicherweise ein Konflikt zwischen den in der Synapse-Basisruntime vorinstallierten Komponenten und den Angaben in der Anforderungsdatei vor. Diese Abhängigkeitskonflikte müssen aufgelöst werden, damit Sie die aktualisierten Bibliotheken in Ihrem serverlosen Apache Spark-Pool anwenden können.

>[!IMPORTANT]
>Probleme können auftreten, wenn Sie pip und conda zusammen verwenden. Beim Kombinieren von pip und conda sollten Sie diese [empfohlenen bewährten Methoden](https://docs.conda.io/projects/conda/latest/user-guide/tasks/manage-environments.html#using-pip-in-an-environment) befolgen.

## <a name="next-steps"></a>Nächste Schritte
- Anzeigen der Standardbibliotheken: [Versionsunterstützung für Apache Spark](apache-spark-version-support.md)