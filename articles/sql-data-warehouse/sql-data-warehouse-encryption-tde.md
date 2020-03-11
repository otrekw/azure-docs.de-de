---
title: Transparent Data Encryption (Portal)
description: Transparent Data Encryption (TDE) in Azure Synapse Analytics
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: c121f9c16895a749922525d1ba85ee2c2e60cfb0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195855"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Erste Schritte mit Transparent Data Encryption (TDE)
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ sein, um Transparent Data Encryption (TDE) zu aktivieren.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung
Führen Sie zum Aktivieren von TDE die folgenden Schritte aus:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com)
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen** .
3. Wählen Sie die Option **Transparten Data Encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Ein**. ![][2]
5. Wählen Sie **Speichern** aus. 
   ![][3]  

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung
Führen Sie zum Deaktivieren von TDE die folgenden Schritte aus:

1. Öffnen Sie die Datenbank im [Azure-Portal](https://portal.azure.com)
2. Klicken Sie im Datenbank-Blatt auf die Schaltfläche **Einstellungen** .
3. Wählen Sie die Option **Transparten Data Encryption** aus. ![][1]
4. Wählen Sie die Einstellung **Aus**. ![][4]
5. Wählen Sie **Speichern** aus. 
   ![][5]  

## <a name="encryption-dmvs"></a>Verschlüsselung-DMVs
Die Verschlüsselung kann mit folgenden DMVs überprüft werden:

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
