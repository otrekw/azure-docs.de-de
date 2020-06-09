---
title: 'Schnellstart: Fivetran und Data Warehouse'
description: Erste Schritte mit Fivetran und einem Azure Synapse Analytics Data Warehouse.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 4cf6c82b330a2672fdde39c3acf156a74cb57e34
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659978"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>Schnellstart: Fivetran mit Data Warehouse 

Diese Schnellstartanleitung beschreibt, wie Sie einen neuen Fivetran-Benutzer für das Arbeiten mit einem Azure Synapse Analytics-Data Warehouse einrichten, das mit einem SQL-Pool bereitgestellt wird. Der Artikel geht davon aus, dass Sie über ein vorhandenes Data Warehouse verfügen.

## <a name="set-up-a-connection"></a>Herstellen einer Verbindung

1. Ermitteln Sie den vollqualifizierten Server- und Datenbanknamen, den Sie zum Herstellen einer Verbindung mit Ihrem Data Warehouse verwenden.
    
    Wenn Sie Hilfe beim Ermitteln dieser Informationen benötigen, finden Sie weitere Informationen unter [Herstellen einer Verbindung mit Ihrem Data Warehouse](../sql/connect-overview.md).

2. Wählen Sie im Setup-Assistenten aus, ob Sie Ihre Datenbank direkt oder über einen SSH-Tunnel verbinden möchten.

   Wenn Sie sich für eine direkte Verbindung mit Ihrer Datenbank entscheiden, müssen Sie eine Firewallregel erstellen, um den Zugriff zu ermöglichen. Dies ist die einfachste und sicherste Methode.

   Wenn Sie sich für eine Verbindung über einen SSH-Tunnel entscheiden, stellt Fivetran eine Verbindung mit einem separaten Server in Ihrem Netzwerk her. Der Server stellt einen SSH-Tunnel für Ihre Datenbank bereit. Sie müssen diese Methode verwenden, wenn sich Ihre Datenbank in einem nicht zugänglichen Subnetz in einem virtuellen Netzwerk befindet.

3. Fügen Sie der Firewall auf Serverebene die IP-Adresse **52.0.2.4** hinzu, um eingehende Verbindungen zu Ihrem Data Warehouse von Fivetran zuzulassen.

   Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](create-data-warehouse-portal.md#create-a-server-level-firewall-rule).

## <a name="set-up-user-credentials"></a>Einrichten der Anmeldeinformationen des Benutzers

1. Stellen Sie eine Verbindung mit Ihrem Data Warehouse her, indem Sie SQL Server Management Studio (SSMS) oder das von Ihnen bevorzugte Tool verwenden. Melden Sie sich als Serveradministrator an. Führen Sie dann die folgenden SQL-Befehle zum Erstellen eines Benutzers für Fivetran aus:

    - In der Masterdatenbank: 
    
      ```sql
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - In der Data Warehouse-Datenbank:

      ```sql
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. Erteilen Sie dem Fivetran-Benutzer die folgenden Berechtigungen für Ihr Data Warehouse:

    ```sql
    GRANT CONTROL to fivetran;
    ```

    Die Berechtigung CONTROL ist erforderlich, um datenbankweite Anmeldeinformationen zu erstellen, die verwendet werden, wenn ein Benutzer Dateien aus dem Azure Blob-Speicher mithilfe von PolyBase lädt.

3. Fügen Sie dem Fivetran-Benutzer eine geeignete Ressourcenklasse hinzu. Welche Ressourcenklasse Sie verwenden, hängt vom Speicher ab, der für die Erstellung eines Columnstore-Index benötigt wird. Beispielsweise erfordern Integrationen mit Produkten wie Marketo und Salesforce aufgrund der großen Anzahl von Spalten und des größeren Datenvolumens, das die Produkte verwenden, eine höhere Ressourcenklasse. Eine höhere Ressourcenklasse erfordert mehr Arbeitsspeicher zum Erstellen von Columnstore-Indizes.

    Es wird empfohlen, statische Ressourcenklassen zu verwenden. Sie können mit der `staticrc20`-Ressourcenklasse beginnen. Die `staticrc20`-Ressourcenklasse ordnet 200 MB für jeden Benutzer zu, und zwar unabhängig von der Leistungsstufe, die Sie verwenden. Wenn die Columnstore-Indizierung auf der Ebene der anfänglichen Ressourcenklasse fehlschlägt, erhöhen Sie die Ressourcenklasse.

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Weitere Informationen finden Sie in den Dokumenten zu [Speicher- und Parallelitätsgrenzwerten](memory-concurrency-limits.md) und [Ressourcenklassen](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory).


## <a name="connect-from-fivetran"></a>Herstellen einer Verbindung von Fivetran

Zum Herstellen einer Verbindung mit Ihrem Data Warehouse über Ihr Fivetran-Konto geben Sie die Anmeldeinformationen ein, mit denen Sie auf Ihr Data Warehouse zugreifen: 

* Host (Ihr Servername).
* Port.
* Datenbank.
* Benutzer (der Benutzername sollte **fivetran\@_Servername_** sein, wobei *Servername* Teil Ihres Azure-Host-URI ist: **_Server\_name_.database.windows.net**).
* Password:
