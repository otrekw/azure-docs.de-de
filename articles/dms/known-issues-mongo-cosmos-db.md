---
title: 'Bekannte Probleme: Migrieren von MongoDB zu Azure Cosmos DB'
titleSuffix: Azure Database Migration Service
description: Hier erfahren Sie mehr über bekannte Probleme und Migrationseinschränkungen beim Migrieren von MongoDB zu Azure Cosmos DB mithilfe von Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: troubleshooting
ms.date: 02/27/2020
ms.openlocfilehash: be6d9d3b8a20e11c874234baae26629ce0187e4f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91291809"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Bekannte Probleme/Migrationseinschränkungen beim Migrieren von MongoDB zur Azure Cosmos DB-API für MongoDB

In den folgenden Abschnitten werden bekannte Probleme und Einschränkungen in Bezug auf Migrationen von MongoDB zur Cosmos DB-API für MongoDB beschrieben.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>Die Migration kann aufgrund der Verwendung des falschen SSL-Zertifikats nicht durchgeführt werden

* **Symptom**: Dieses Problem wird offensichtlich, wenn ein Benutzer keine Verbindung mit dem MongoDB-Quellserver herstellen kann. Obwohl alle Firewallports geöffnet sind, kann der Benutzer weiterhin keine Verbindung herstellen.

| Ursache         | Lösung |
| ------------- | ------------- |
| Bei Verwendung eines selbstsignierten Zertifikats in Azure Database Migration Service kann die Migration aufgrund des falschen SSL-Zertifikats nicht durchgeführt werden. Die Fehlermeldung enthält möglicherweise den Text „Das Remotezertifikat ist laut Validierungsverfahren ungültig“. | Verwenden Sie ein Originalzertifikat der Zertifizierungsstelle.  Selbstsignierte Zertifikate werden im Allgemeinen nur bei internen Tests verwendet. Wenn Sie ein Originalzertifikat einer Zertifizierungsstelle installieren, können Sie SSL in Azure Database Migration Service ohne Probleme verwenden (Verbindungen mit Cosmos DB verwenden SSL über Mongo-API).<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>Die Liste der in DMS zuzuordnenden Datenbanken kann nicht abgerufen werden

* **Symptom**: Auf dem Blatt **Datenbankeinstellung** kann die Datenbankliste nicht angezeigt werden, wenn auf dem Blatt **Quelle auswählen** der Modus **Daten aus Azure Storage** verwendet wird.

| Ursache         | Lösung |
| ------------- | ------------- |
| In der Verbindungszeichenfolge für das Speicherkonto fehlen die SAS-Informationen, daher ist die Authentifizierung nicht möglich. | Erstellen Sie die SAS für den Blobcontainer in Storage-Explorer, und verwenden Sie die URL mit den SAS-Informationen des Containers als Quelldetail-Verbindungszeichenfolge.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Verwenden einer nicht unterstützten Version der Datenbank

* **Symptom**: Die Migration kann nicht durchgeführt werden.

| Ursache         | Lösung |
| ------------- | ------------- |
| Sie versuchen, eine nicht unterstützte Version von MongoDB zu Azure Cosmos DB zu migrieren. | Neue veröffentlichte Versionen von MongoDB werden getestet, um die Kompatibilität mit Azure Database Migration Service sicherzustellen, und der Dienst wird regelmäßig aktualisiert, um die neueste(n) Version(en) zu akzeptieren. Wenn die Migration sofort durchgeführt werden muss, können Sie zur Problemumgehung die Datenbanken/Sammlungen in Azure Storage exportieren und die Quelle auf das resultierende Speicherabbild verweisen. Erstellen Sie die SAS für den Blobcontainer in Storage-Explorer, und verwenden Sie dann die URL mit den SAS-Informationen des Containers als Quelldetail-Verbindungszeichenfolge.<br><br> |

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich das Tutorial [Onlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB mit DMS](tutorial-mongodb-cosmos-db-online.md) an.
* Sehen Sie sich das Tutorial [Offlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB mit DMS](tutorial-mongodb-cosmos-db.md) an.