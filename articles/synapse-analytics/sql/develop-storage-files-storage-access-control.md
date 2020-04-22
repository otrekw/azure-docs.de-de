---
title: Steuern des Speicherkontozugriffs für SQL On-Demand (Vorschau)
description: In diesem Artikel wird beschrieben, wie SQL On-Demand (Vorschau) auf Azure Storage zugreift und wie Sie den Speicherzugriff für SQL On-Demand in Azure Synapse Analytics steuern können.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0d2683091898e9c84457b3b538776f0e6b0469d4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420054"
---
# <a name="control-storage-account-access-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Steuern des Speicherkontozugriffs für SQL On-Demand (Vorschau) in Azure Synapse Analytics

Eine Abfrage von SQL On-Demand (Vorschau) liest Dateien direkt aus Azure Storage. Da das Speicherkonto ein Objekt außerhalb der SQL On-Demand-Ressource ist, sind geeignete Anmeldeinformationen erforderlich. Ein Benutzer benötigt die entsprechenden Berechtigungen, um diese Anmeldeinformationen zu verwenden. In diesem Artikel wird beschrieben, welche Arten von Anmeldeinformationen Sie verwenden können und wie die Suche nach Anmeldeinformationen für SQL- und Azure AD-Benutzer funktioniert.

## <a name="supported-storage-authorization-types"></a>Unterstützte Autorisierungstypen für den Speicherzugriff

Ein bei einer SQL On-Demand-Ressource angemeldeter Benutzer muss für den Zugriff auf und die Abfrage von Dateien in Azure Storage autorisiert sein. Drei Autorisierungstypen werden unterstützt:

- [Shared Access Signature (SAS)](#shared-access-signature)
- [Verwaltete Identität](#managed-identity)
- [Benutzeridentität](#user-identity)

> [!NOTE]
> [Azure AD-Pass-Through](#force-azure-ad-pass-through) ist das Standardverhalten, wenn Sie einen Arbeitsbereich erstellen. In diesem Fall müssen Sie nicht für jedes Speicherkonto, auf das über AD-Anmeldungen zugegriffen wird, eigene Anmeldeinformationen erstellen. Sie können [dieses Verhalten deaktivieren](#disable-forcing-azure-ad-pass-through).

In der folgenden Tabellen sehen Sie die verschiedenen Autorisierungstypen, die bereits jetzt oder in Kürze unterstützt werden.

| Autorisierungstyp                    | *SQL-Benutzer*    | *Azure AD-Benutzer*     |
| ------------------------------------- | ------------- | -----------    |
| [SAS](#shared-access-signature)       | Unterstützt     | Unterstützt      |
| [Verwaltete Identität](#managed-identity) | Nicht unterstützt | Nicht unterstützt  |
| [Benutzeridentität](#user-identity)       | Nicht unterstützt | Unterstützt      |

### <a name="shared-access-signature"></a>Shared Access Signature (SAS)

**Shared Access Signature (SAS)** bietet delegierten Zugriff auf Ressourcen in einem Speicherkonto. Mit einer SAS kann ein Kunde Clients Zugriff auf Ressourcen in einem Speicherkonto gewähren, ohne dafür Kontoschlüssel weitergeben zu müssen. Eine SAS bietet die Möglichkeit, präzise zu steuern, welche Art von Zugriff Sie Clients gewähren möchten, die über eine SAS verfügen. Dazu stehen Parameter wie Gültigkeitsintervall, erteilte Berechtigungen, zulässiger IP-Adressbereich und zulässiges Protokoll (HTTPS/HTTP) zur Verfügung.

Um ein SAS-Token abzurufen, navigieren Sie zu **Azure-Portal > Speicherkonto > Shared Access Signature > Berechtigungen konfigurieren > SAS und Verbindungszeichenfolge generieren**.

> [!IMPORTANT]
> Wenn ein SAS-Token generiert wird, enthält es am Anfang ein Fragezeichen („?“). Um das Token in SQL On-Demand zu verwenden, müssen Sie das Fragezeichen beim Erstellen der Anmeldeinformation entfernen. Beispiel:
>
> SAS-Token: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D

### <a name="user-identity"></a>Benutzeridentität

Die **Benutzeridentität** (auch „Pass-Through“ genannt) ist ein Autorisierungstyp, bei dem die Identität des bei SQL On-Demand angemeldeten Azure AD-Benutzers verwendet wird, um den Datenzugriff zu autorisieren. Vor dem Zugriff auf die Daten muss der Azure Storage-Administrator dem Azure AD-Benutzer die erforderlichen Berechtigungen erteilen. Wie Sie der Tabelle oben entnehmen können, wird diese Art der Autorisierung für den Typ „SQL-Benutzer“ nicht unterstützt.

> [!NOTE]
> Wenn Sie [Azure AD-Pass-Through](#force-azure-ad-pass-through) verwenden, müssen Sie nicht für jedes Speicherkonto, auf das über AD-Anmeldungen zugegriffen wird, eigene Anmeldeinformationen erstellen.

> [!IMPORTANT]
> Sie müssen über eine Rolle als Besitzer, Mitwirkender oder Leser für Storage-Blobdaten verfügen, um mithilfe Ihrer Identität auf die Daten zugreifen zu können.
> Auch wenn Sie Besitzer eines Speicherkontos sind, müssen Sie sich selbst zu einer der Rollen für den Zugriff auf Storage-Blobdaten hinzufügen.
>
> Weitere Informationen zur Zugriffssteuerung in Azure Data Lake Storage Gen2 finden Sie im Artikel [Zugriffssteuerung in Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-access-control.md).
>

### <a name="managed-identity"></a>Verwaltete Identität

Eine **verwaltete Identität** wird auch als MSI bezeichnet. Hierbei handelt es sich um ein Feature von Azure Active Directory (Azure AD), das Azure-Dienste für SQL On-Demand bereitstellt. Es stellt außerdem eine automatisch verwaltete Identität in Azure AD bereit. Diese Identität kann verwendet werden, um eine Anforderung für den Datenzugriff in Azure Storage zu autorisieren.

Vor dem Zugriff auf die Daten muss der Azure Storage-Administrator der verwalteten Identität Zugriffsberechtigungen für die Daten erteilen. Das Erteilen von Berechtigungen für eine verwaltete Identität erfolgt auf die gleiche Weise wie das Erteilen von Berechtigungen für jeden anderen Azure AD-Benutzer auch.

## <a name="create-credentials"></a>Erstellen von Anmeldeinformationen

Um ein Datei in Azure Storage abzufragen, benötigt Ihr SQL On-Demand-Endpunkt eine Anmeldeinformation auf Serverebene, die die Authentifizierungsinformationen enthält. Eine Anmeldeinformation wird durch Ausführen von [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) hinzugefügt. Sie müssen ein CREDENTIAL NAME-Argument angeben. Dieses muss entweder einem Teil des Pfads oder dem gesamten Pfad zu den Daten in Storage entsprechen (siehe unten).

> [!NOTE]
> Das Argument FOR CRYPTOGRAPHIC PROVIDER wird nicht unterstützt.

Bei allen unterstützten Autorisierungstypen können Anmeldeinformationen auf ein Konto, einen Container, ein beliebiges Verzeichnis (aber kein Stammverzeichnis) oder eine einzelne Datei zeigen.

CREDENTIAL NAME muss dem vollständigen Pfad zum Container, zum Ordner oder zur Datei im folgenden Format entsprechen: `<prefix>://<storage_account_path>/<storage_path>`.

| Externe Datenquelle       | Präfix | Speicherkontopfad                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Storage         | https  | <Speicherkonto>.blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <Speicherkonto>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <Speicherkonto>.dfs.core.windows.net              |

 <Speicherpfad> ist ein Pfad innerhalb Ihres Speichers, der auf den zu lesenden Ordner oder auf die zu lesende Datei zeigt.

> [!NOTE]
> Es gibt den speziellen CREDENTIAL NAME `UserIdentity`, der [Azure AD-Pass-Through erzwingt](#force-azure-ad-pass-through). Informieren Sie sich, wie sich dieses Argument beim Ausführen von Abfragen auf die [Suche nach Anmeldeinformationen](#credential-lookup) auswirkt.

Um einem Benutzer das Erstellen oder Verwerfen von Anmeldeinformationen zu ermöglichen, können Administratoren dem Benutzer optional die Berechtigung GRANT/DENY ALTER ANY CREDENTIAL erteilen:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

### <a name="supported-storages-and-authorization-types"></a>Unterstützte Speicher- und Autorisierungstypen

Sie können die folgenden Kombinationen aus Autorisierungstypen und Azure Storage-Typen verwenden:

|                     | Blob Storage   | ADLS Gen1        | ADLS Gen2     |
| ------------------- | ------------   | --------------   | -----------   |
| *SAS*               | Unterstützt      | Nicht unterstützt   | Unterstützt     |
| *Verwaltete Identität* | Nicht unterstützt  | Nicht unterstützt    | Nicht unterstützt |
| *Benutzeridentität*    | Unterstützt      | Unterstützt        | Unterstützt     |

### <a name="examples"></a>Beispiele

Je nach [Autorisierungstyp](#supported-storage-authorization-types) können Sie Anmeldeinformationen mit der folgenden T-SQL-Syntax erstellen.

**Shared Access Signature und Blob Storage**

Ersetzen Sie <*mystorageaccountname*> durch den tatsächlichen Namen Ihres Speicherkontos und <*mystorageaccountcontainername*> durch den tatsächlichen Namen des Containers:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

**Benutzeridentität und Azure Data Lake Storage Gen1**

Ersetzen Sie <*mystorageaccountname*> durch den tatsächlichen Namen Ihres Speicherkontos und <*mystorageaccountcontainername*> durch den tatsächlichen Namen des Containers:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.azuredatalakestore.net/webhdfs/v1/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

**Benutzeridentität und Azure Data Lake Storage Gen2**

Ersetzen Sie <*mystorageaccountname*> durch den tatsächlichen Namen Ihres Speicherkontos und <*mystorageaccountcontainername*> durch den tatsächlichen Namen des Containers:

```sql
CREATE CREDENTIAL [https://<mystorageaccountname>.dfs.core.windows.net/<mystorageaccountcontainername>]
WITH IDENTITY='User Identity';
GO
```

## <a name="force-azure-ad-pass-through"></a>Erzwingen von Azure AD-Pass-Through

Das Erzwingen von Azure AD-Pass-Through ist ein Standardverhalten, das durch den speziellen CREDENTIAL NAME `UserIdentity` erzielt wird. Dieser wird während der Bereitstellung eines Azure Synapse-Arbeitsbereichs automatisch erstellt. Er erzwingt die Verwendung eines Azure AD-Pass-Through-Vorgangs für jede Abfrage jeder Azure AD-Anmeldung, der auch dann ausgeführt wird, wenn andere Anmeldeinformationen vorhanden sind.

> [!NOTE]
> Azure AD-Pass-Through ist ein Standardverhalten. Sie müssen nicht für jedes Speicherkonto, auf das von AD-Anmeldungen zugegriffen wird, eigene Anmeldeinformationen erstellen.

Wenn Sie [die Erzwingung von Azure AD-Pass-Through für jede Abfrage deaktiviert haben](#disable-forcing-azure-ad-pass-through) und diese wieder aktivieren möchten, führen Sie Folgendes aus:

```sql
CREATE CREDENTIAL [UserIdentity]
WITH IDENTITY = 'User Identity';
```

Um die Erzwingung von Azure AD-Pass-Through für einen bestimmten Benutzer zu aktivieren, können Sie diesem Benutzer die Berechtigung REFERENCE in der Anmeldeinformation `UserIdentity` erteilen. Das folgende Beispiel aktiviert die Erzwingung von Azure AD-Pass-Through für den Benutzer „user_name“:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

Weitere Informationen dazu, wie SQL On-Demand nach Anmeldeinformationen sucht, finden Sie unter [Suche nach Anmeldeinformationen](#credential-lookup).

## <a name="disable-forcing-azure-ad-pass-through"></a>Deaktivieren der Erzwingung von Azure AD-Pass-Through

Sie können die [Erzwingung von Azure AD-Pass-Through für jede Abfrage](#force-azure-ad-pass-through) deaktivieren. Löschen Sie zu diesem Zweck die Anmeldeinformation `Userdentity` mit folgendem Befehl:

```sql
DROP CREDENTIAL [UserIdentity];
```

Informationen zum erneuten Aktivieren finden Sie im Abschnitt [Erzwingen von Azure AD-Pass-Through](#force-azure-ad-pass-through).

Um die Erzwingung von Azure AD-Pass-Through für einen bestimmten Benutzer zu deaktivieren, können Sie diesem Benutzer die Berechtigung REFERENCE in der Anmeldeinformation `UserIdentity` verweigern. Das folgende Beispiel deaktiviert die Erzwingung von Azure AD-Pass-Through für den Benutzer „user_name“:

```sql
DENY REFERENCES ON CREDENTIAL::[UserIdentity] TO USER [user_name];
```

Weitere Informationen dazu, wie SQL On-Demand nach Anmeldeinformationen sucht, finden Sie unter [Suche nach Anmeldeinformationen](#credential-lookup).

## <a name="grant-permissions-to-use-credential"></a>Erteilen von Berechtigungen zur Verwendung einer Anmeldeinformation

Um eine Anmeldeinformation verwenden zu können, muss ein Benutzer über die Berechtigung REFERENCES für diese Anmeldeinformation verfügen. Um dem Benutzer „specific_user“ die Berechtigung REFERENCES für die Anmeldeinformation „storage_credential“ zu erteilen, führen Sie folgenden Befehl aus:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Um reibungslose Azure AD-Pass-Through-Vorgänge sicherzustellen, verfügen alle Benutzer standardmäßig über die Berechtigung zur Verwendung der Anmeldeinformation `UserIdentity`. Dies wird durch automatische Ausführung der folgenden Anweisung während der Bereitstellung eines Azure Synapse-Arbeitsbereichs erreicht:

```sql
GRANT REFERENCES ON CREDENTIAL::[UserIdentity] TO [public];
```

## <a name="credential-lookup"></a>Suche nach Anmeldeinformationen

Beim Autorisieren von Abfragen wird die Suche nach Anmeldeinformationen verwendet, um auf ein Speicherkonto zuzugreifen. Dabei gelten folgende Regeln:

- Der Benutzer ist mit einer Azure AD-Anmeldung angemeldet.

  - Wenn die Anmeldeinformation „UserIdentity“ vorhanden ist und der Benutzer über REFERENCE-Berechtigungen dafür verfügt, wird Azure AD-Pass-Through verwendet. Andernfalls erfolgt eine [Suche nach Anmeldeinformationen anhand des Pfads](#lookup-credential-by-path).

- Der Benutzer ist mit einer SQL-Anmeldung angemeldet.

  - Es wird [anhand des Pfads nach Anmeldeinformationen gesucht](#lookup-credential-by-path).

### <a name="lookup-credential-by-path"></a>Suche nach Anmeldeinformationen anhand des Pfads

Wenn die Erzwingung von Azure AD-Pass-Through deaktiviert ist, basiert die Suche nach Anmeldeinformationen auf dem Speicherpfad (Tiefensuche) und der Existenz einer REFERENCES-Berechtigung für diese Anmeldeinformation. Wenn mehrere Anmeldeinformationen für den Zugriff auf ein und dieselbe Datei verwendet werden können, verwendet SQL On-Demand die spezifischste.  

Im Folgenden sehen Sie ein Beispiel für eine Abfrage im folgenden Dateipfad: *account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX.ext*

Die Suche nach Anmeldeinformationen erfolgt in dieser Reihenfolge:

```
account.dfs.core.windows.net/filesystem/folder1/.../folderN/fileX
account.dfs.core.windows.net/filesystem/folder1/.../folderN
account.dfs.core.windows.net/filesystem/folder1
account.dfs.core.windows.net/filesystem
account.dfs.core.windows.net
```

Wenn ein Benutzer keine REFERENCES-Berechtigung für Anmeldeinformation Nr. 5 besitzt, überprüft SQL On-Demand, ob der Benutzer über eine solche Berechtigung für eine Anmeldeinformation eine Ebene darüber verfügt. Dies wird so lange ausgeführt, bis SQL On-Demand die Anmeldeinformationen findet, für die der Benutzer eine REFERENCES-Berechtigung besitzt. Wenn keine solche Berechtigung gefunden wird, wird eine Fehlermeldung zurückgegeben.

### <a name="credential-and-path-level"></a>Anmeldeinformation und Pfadebene

Je nach gewünschter Pfadform gelten folgende Voraussetzungen für die Ausführung von Abfragen:

- Wenn die Abfrage für mehrere Dateien (Ordner, mit oder ohne Platzhalter) gilt, muss ein Benutzer Zugriff auf eine Anmeldeinformation mindestens auf Ebene des Stammverzeichnisses (Containerebene) besitzen. Diese Zugriffsebene ist erforderlich, weil das Auflisten von Dateien relativ zum Stammverzeichnis erfolgt (Einschränkungen von Azure Storage).
- Wenn eine Abfrage nur für eine einzelne Datei gilt, benötigt ein Benutzer Zugriff auf eine Anmeldeinformation auf einer beliebigen Ebene, da SQL On-Demand direkt, also ohne Auflisten von Ordnern, auf die Datei zugreift.

|                  | *Konto* | *Stammverzeichnis* | *Jedes andere Verzeichnis* | *File*        |
| ---------------- | --------- | ---------------- | --------------------- | ------------- |
| *Einzelne Datei*    | Unterstützt | Unterstützt        | Unterstützt             | Unterstützt     |
| *Mehrere Dateien* | Unterstützt | Unterstützt        | Nicht unterstützt         | Nicht unterstützt |

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie wichtige Informationen dazu, wie Sie verschiedene Arten von Ordnern und Dateien abfragen und Sichten erstellen und verwenden:

- [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md)
- [Abfragen von Ordnern und mehreren CSV-Dateien](query-folders-multiple-csv-files.md)
- [Abfragen bestimmter Dateien](query-specific-files.md)
- [Abfragen von Parquet-Dateien](query-parquet-files.md)
- [Erstellen und Verwenden von Sichten](create-use-views.md)
- [Abfragen von JSON-Dateien](query-json-files.md)
- [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md)
