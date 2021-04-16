---
title: Rollen und Anforderungen für Azure Data Share
description: Hier erfahren Sie mehr über die erforderlichen Berechtigungen zum Freigeben und Empfangen von Daten mithilfe von Azure Data Share.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 03/24/2021
ms.openlocfilehash: a832c8956f7a3d4f8669209d7ed311e7555e1e75
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105644244"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Rollen und Anforderungen für Azure Data Share 

In diesem Artikel werden Rollen und Berechtigungen beschrieben, die zum Freigeben und Empfangen von Daten mit dem Azure Data Share-Dienst erforderlich sind. 

## <a name="roles-and-requirements"></a>Rollen und Anforderungen

Mit dem Azure Data Share-Dienst können Sie Daten freigeben, ohne Anmeldeinformationen zwischen Datenanbieter und Consumer auszutauschen. Für die Snapshot-basierte Freigabe verwendet der Azure Data Share-Dienst Managed Identities (früher bekannt als MSIs), um sich beim Azure-Datenspeicher zu authentifizieren. Der verwalteten Identität der Azure Data Share-Ressource muss der Zugriff auf den Azure-Datenspeicher gewährt werden, um Daten lesen oder schreiben zu können.

Zum Freigeben oder Empfangen von Daten aus einem Azure-Datenspeicher benötigt der Benutzer mindestens die folgenden Berechtigungen. 

* Berechtigung zum Schreiben in den Azure-Datenspeicher. Diese Berechtigung ist normalerweise in der Rolle **Mitwirkender** enthalten.

Für die Speicher- und Data Lake-Snapshot-basierte Freigabe benötigen Sie außerdem die Berechtigung zum Erstellen von Rollenzuweisungen im Azure-Datenspeicher. Die Berechtigung zum Erstellen von Rollenzuweisungen ist normalerweise in der Rolle **Besitzer**, der Rolle „Benutzerzugriffsadministrator“ oder einer benutzerdefinierten Rolle mit zugewiesener Berechtigung *Microsoft.Authorization/role assignments/write* enthalten. Diese Berechtigung ist nicht erforderlich, wenn der verwalteten Identität der Data Share-Ressource bereits Zugriff auf den Azure-Datenspeicher gewährt wurde. Im Folgenden finden Sie eine Zusammenfassung der Rollen, die der verwalteten Identität der Data Share-Ressource zugewiesen werden:

|**Datenspeichertyp**|**Quelldatenspeicher des Datenanbieters**|**Zieldatenspeicher des Datenconsumers**|
|---|---|---|
|Azure Blob Storage| Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
|Azure Data Lake Gen1 | Besitzer | Nicht unterstützt
|Azure Data Lake Gen2 | Leser von Speicherblobdaten | Mitwirkender an Storage-Blobdaten
|

Bei SQL-Snapshot-basierter Freigabe muss ein SQL-Benutzer über einen externen Anbieter in Azure SQL-Datenbank erstellt werden, und zwar mit demselben Namen wie die Azure Data Share-Ressource. Zum Erstellen dieses Benutzers ist eine Azure Active Directory-Administratorberechtigung erforderlich. Nachstehend finden Sie eine Zusammenfassung der für den SQL-Benutzer erforderlichen Berechtigung.

|**SQL-Datenbanktyp**|**SQL-Benutzerberechtigung des Datenanbieters**|**SQL-Benutzerberechtigung des Datenconsumers**|
|---|---|---|
|Azure SQL-Datenbank | db_datareader | db_datareader, db_datawriter, db_ddladmin
|Azure Synapse Analytics | db_datareader | db_datareader, db_datawriter, db_ddladmin
|

### <a name="data-provider"></a>Datenanbieter
Für die auf Speicher- und Data Lake-Snapshots basierende Freigabe muss der verwalteten Identität des Anbieters der Datenfreigabe-Ressource Zugriff auf den Azure-Quelldatenspeicher gewährt werden, um einen Datensatz in Azure Data Share hinzuzufügen. Bei einem Speicherkonto beispielsweise wird der verwalteten Identität der Data Share-Ressource die Rolle *Leser von Speicherblobdaten* zugewiesen. Dies geschieht automatisch durch den Azure Data Share-Dienst, wenn der Benutzer ein Dataset über das Azure-Portal hinzufügt und über die entsprechende Berechtigung verfügt. So ist der Benutzer beispielsweise Besitzer des Azure-Datenspeichers oder Mitglied einer benutzerdefinierten Rolle, der die Berechtigung *Microsoft.Authorization/role assignments/write* zugewiesen wurde. 

Alternativ kann der Benutzer veranlassen, dass der Besitzer des Azure-Datenspeichers die verwaltete Identität der Data Share-Ressource seinem Datenspeicher manuell hinzufügt. Diese Aktion muss nur einmal pro Data Share-Ressource ausgeführt werden. Wenn Sie eine Rollenzuweisung für die verwaltete Identität der Data Share-Ressource manuell erstellen möchten, führen Sie die folgenden Schritte aus:  

1. Navigieren Sie zum Azure-Datenspeicher.
1. Wählen Sie **Access Control (IAM)** aus.
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie unter *Rolle* die gewünschte Rolle in der vorstehenden Rollenzuweisungstabelle aus (z. B. für das Speicherkonto die Rolle *Leser von Speicherblobdaten*).
1. Geben Sie unter *Auswählen* den Namen Ihrer Azure Data Share-Ressource ein.
1. Klicken Sie auf *Speichern*.

Weitere Informationen zur Rollenzuweisung finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md). Wenn Sie Daten mithilfe von REST-APIs freigeben, können Sie die Rollenzuweisung mithilfe der API erstellen. Informationen dazu finden Sie unter [Zuweisen von Azure-Rollen mithilfe der REST-API](../role-based-access-control/role-assignments-rest.md). 

Bei SQL-Snapshot-basierten Teilen muss ein SQL-Benutzer über einen externen Anbieter in SQL-Datenbank erstellt werden, und zwar mit demselben Namen wie die Azure Data Share-Ressource, wenn mithilfe von Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank hergestellt wird. Diesem Benutzer muss die Berechtigung *db_datareader* erteilt werden. Ein Beispielskript sowie weitere Voraussetzungen für die SQL-basierte Freigabe finden Sie im Tutorial [Freigeben und Empfangen von Daten aus Azure SQL-Datenbank und Azure Synapse Analytics](how-to-share-from-sql.md). 

### <a name="data-consumer"></a>Datenconsumer
Um Daten im Speicherkonto zu empfangen, muss der verwalteten Identität der Ressource zur gemeinsamen Nutzung von Verbraucherdaten Zugriff auf das Zielspeicherkonto gewährt werden. Der verwalteten Identität der Datenfreigabe Ressource muss die Rolle *Mitwirkender von Speicher-BLOBs* erteilt werden. Dies geschieht automatisch durch den Azure Data Share-Dienst, wenn der Benutzer einen Zielspeicherkonto über das Azure-Portal angibt und über die entsprechende Berechtigung verfügt. So ist der Benutzer beispielsweise Besitzer des Azure-Datenspeichers oder Mitglied einer benutzerdefinierten Rolle, der die Berechtigung *Microsoft.Authorization/role assignments/write* zugewiesen wurde. 

Alternativ kann der Benutzer den Eigentümer des Speicherkontos dazu veranlassen, die verwaltete Identität der Datenfreigabe-Ressource manuell zum Speicherkonto hinzuzufügen. Diese Aktion muss nur einmal pro Data Share-Ressource ausgeführt werden. Wenn Sie eine Rollenzuweisung für die verwaltete Identität der Data Share-Ressource manuell erstellen möchten, führen Sie die folgenden Schritte aus: 

1. Navigieren Sie zum Azure-Datenspeicher.
1. Wählen Sie **Access Control (IAM)** aus.
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie unter *Rolle* die gewünschte Rolle in der vorstehenden Rollenzuweisungstabelle aus (z. B. für das Speicherkonto die Rolle *Leser von Speicherblobdaten*).
1. Geben Sie unter *Auswählen* den Namen Ihrer Azure Data Share-Ressource ein.
1. Klicken Sie auf *Speichern*.

Weitere Informationen zur Rollenzuweisung finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md). Wenn Sie Daten mithilfe von REST-APIs erhalten, können Sie die Rollenzuweisung mithilfe der API erstellen. Informationen dazu finden Sie unter [Zuweisen von Azure-Rollen mithilfe der REST-API](../role-based-access-control/role-assignments-rest.md). 

Bei SQL-basierten Zielen muss ein SQL-Benutzer über einen externen Anbieter in SQL-Datenbank erstellt werden, und zwar mit demselben Namen wie die Azure Data Share-Ressource, wenn mithilfe von Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank hergestellt wird. Diesem Benutzer muss die Berechtigung *db_datareader, db_datawriter, db_ddladmin* erteilt werden. Ein Beispielskript sowie weitere Voraussetzungen für die SQL-basierte Freigabe finden Sie im Tutorial [Freigeben und Empfangen von Daten aus Azure SQL-Datenbank und Azure Synapse Analytics](how-to-share-from-sql.md). 

## <a name="resource-provider-registration"></a>Ressourcenanbieterregistrierung 

Sie müssen den Microsoft.DataShare-Ressourcenanbieter möglicherweise in den folgenden Szenarien in Ihrem Azure-Abonnement manuell registrieren: 

* Erstmaliges Anzeigen einer Azure Data Share-Einladung in Ihrem Azure-Mandanten
* Freigeben von Daten aus einem Azure-Datenspeicher in einem anderen Azure-Abonnement Ihrer Azure Data Share-Ressource
* Empfangen von Daten in einem Azure-Datenspeicher in einem anderen Azure-Abonnement Ihrer Azure Data Share-Ressource

Führen Sie diese Schritte aus, um den Microsoft.DataShare-Ressourcenanbieter in Ihrem Azure-Abonnement zu registrieren. Sie benötigen Zugriff als *Mitwirkender* auf das Azure-Abonnement, um einen Ressourcenanbieter zu registrieren.

1. Navigieren Sie im Azure-Portal zu **Abonnements**.
1. Wählen Sie das Abonnement aus, das Sie für Azure Data Share verwenden.
1. Klicken Sie auf **Ressourcenanbieter**.
1. Suchen Sie nach „Microsoft.DataShare“.
1. Klicken Sie auf **Registrieren**.
 
Weitere Informationen zu Ressourcenanbietern finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Rollen in Azure finden Sie unter [Grundlegendes zu Azure-Rollendefinitionen](../role-based-access-control/role-definitions.md).