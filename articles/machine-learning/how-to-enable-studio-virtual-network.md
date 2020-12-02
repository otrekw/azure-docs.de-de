---
title: Aktivieren von Azure Machine Learning Studio in einem virtuellen Netzwerk
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Azure Machine Learning Studio konfigurieren, um auf Daten zuzugreifen, die in einem virtuellen Netzwerk gespeichert sind.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 10/21/2020
ms.custom: contperfq4, tracking-python
ms.openlocfilehash: df4d777ad78240b3ca84c51152b37861c4ccc486
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94960001"
---
# <a name="use-azure-machine-learning-studio-in-an-azure-virtual-network"></a>Verwenden von Azure Machine Learning Studio in einem virtuellen Netzwerk

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning Studio in einem virtuellen Netzwerk verwenden. Folgendes wird vermittelt:

> [!div class="checklist"]
> - Zugreifen auf Studio von einer Ressource innerhalb eines virtuellen Netzwerks aus
> - Konfigurieren Sie private Endpunkte für Speicherkonten.
> - Erteilen der Studio-Berechtigung, auf Daten zuzugreifen, die in einem virtuellen Netzwerk gespeichert sind
> - Erfahren Sie, wie sich das Studio auf die Speichersicherheit auswirkt.

Der Artikel ist Teil 5 einer fünfteiligen Artikelreihe, in der Sie schrittweise durch die Absicherung eines Azure Machine Learning-Workflows geführt werden. Es wird dringend empfohlen, zunächst [Teil 1: Virtuelle Netzwerke im Überblick](how-to-network-security-overview.md) zu lesen, um die Gesamtarchitektur besser zu verstehen. 

Sehen Sie sich auch die anderen Artikel in dieser Reihe an:

[1. Übersicht zu VNETs](how-to-network-security-overview.md) > [2. Schützen des Arbeitsbereichs](how-to-secure-workspace-vnet.md) > [3. Schützen der Trainingsumgebung](how-to-secure-training-vnet.md) > [4. Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md) > **5. Verwenden von Studio in einem virtuellen Netzwerk**


> [!IMPORTANT]
> Wenn sich Ihr Arbeitsbereich in einer __Sovereign Cloud__ befindet, z. B. Azure Government oder Azure China 21Vianet, wird von integrierten Notebooks die Verwendung von Speicher, der sich in einem virtuellen Netzwerk befindet, _nicht_ unterstützt. Stattdessen können Sie Jupyter-Notebooks aus einer Compute-Instanz verwenden. Weitere Informationen finden Sie im Abschnitt [Zugreifen auf Daten auf einem Compute-Instanz-Notebook](how-to-secure-training-vnet.md#access-data-in-a-compute-instance-notebook).


## <a name="prerequisites"></a>Voraussetzungen

+ In der [Übersicht über die Netzwerksicherheit](how-to-network-security-overview.md) finden Sie Informationen zu gängigen Szenarien im Zusammenhang mit virtuellen Netzwerken sowie zur Gesamtarchitektur virtueller Netzwerke.

+ Ein bereits vorhandenes virtuelles Netzwerk und Subnetz, das verwendet werden kann

+ Ein vorhandener [Azure Machine Learning-Arbeitsbereich mit aktiviertem Private Link](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint).

+ Ein vorhandenes [Azure Storage-Konto](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints), das Ihrem virtuellen Netzwerk hinzugefügt wurde.

## <a name="access-the-studio-from-a-resource-inside-the-vnet"></a>Zugriff auf Studio von einer Ressource innerhalb des virtuellen Netzwerks aus

Wenn Sie über eine Ressource innerhalb eines virtuellen Netzwerks (z. B. eine Computeinstanz oder eine VM) auf das Studio zugreifen, müssen Sie aus dem virtuellen Netzwerk an das Studio ausgehenden Datenverkehr zulassen. 

Wenn Sie z. B. ausgehenden Datenverkehr mit Netzwerksicherheitsgruppen (NSG) einschränken, fügen Sie einem __Diensttag__-Ziel von __AzureFrontDoor.Frontend__ eine Regel hinzu.

## <a name="access-data-using-the-studio"></a>Zugreifen auf Daten mithilfe von Studio

Nachdem Sie Ihrem virtuellen Netzwerk mit einem [Dienstendpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints) oder [privaten Endpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints) ein Azure-Speicherkonto hinzugefügt haben, müssen Sie Ihr Speicherkonto für die Verwendung der [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) konfigurieren, um dem Studio Zugriff auf Ihre Daten zu gewähren.

Wenn Sie die verwaltete Identität nicht aktivieren, erhalten Sie den Fehler `Error: Unable to profile this dataset. This might be because your data is stored behind a virtual network or your data does not support profile.` Außerdem werden die folgenden Vorgänge deaktiviert:

* Vorschau der Daten im Studio.
* Visualisieren von Daten im Designer.
* Senden eines AutoML-Experiments.
* Starten eines Beschriftungsprojekts.

Das Studio unterstützt das Lesen von Daten aus den folgenden Datenspeichertypen in einem virtuellen Netzwerk:

* Azure Blob
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL-Datenbank

### <a name="grant-workspace-managed-identity-__reader__-access-to-storage-private-link"></a>Gewähren von __Lesezugriff__ auf die private Speicherverbindung für die vom Arbeitsbereich verwaltete Identität

Dieser Schritt ist nur erforderlich, wenn Sie Ihrem virtuellen Netzwerk das Azure-Speicherkonto mit einem [privaten Endpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints) hinzugefügt haben. Weitere Informationen finden Sie unter der integrierten Rolle [Leser](../role-based-access-control/built-in-roles.md#reader).

### <a name="configure-datastores-to-use-workspace-managed-identity"></a>Konfigurieren von Datenspeichern für die Verwendung der vom Arbeitsbereich verwalteten Identität

Azure Machine Learning verwendet [Datenspeicher](concept-data.md#datastores), um eine Verbindung mit Speicherkonten herzustellen. Führen Sie die folgenden Schritte aus, um die Verwendung der verwalteten Identität in Ihren Datenspeichern zu konfigurieren. 

1. Wählen Sie __Datastores__ (Datenspeicher) im Studio aus.

1. Um einen neuen Datenspeicher zu erstellen, wählen Sie __+ New datastore__ (+ Neuer Datenspeicher) aus.

    Wenn Sie einen vorhandenen Datenspeicher aktualisieren möchten, wählen Sie diesen aus, und wählen Sie __Anmeldeinformationen aktualisieren__.

1. Wählen Sie in den Datenspeichereinstellungen __Ja__ für __Allow Azure Machine Learning service to access the storage using workspace managed identity__ (Azure Machine Learning Service den Zugriff auf den Speicher über die vom Arbeitsbereich verwaltete Identität erlauben).


Mit diesen Schritten wird die vom Arbeitsbereich verwaltete Identität mithilfe der ressourcenbasierten Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC) dem Speicherdienst als __Leser__ hinzugefügt. Mit __Reader__-Zugriff kann der Arbeitsbereich Firewalleinstellungen abrufen und sicherstellen, dass die Daten das virtuelle Netzwerk nicht verlassen.

> [!NOTE]
> Diese Änderungen können bis zu 10 Minuten dauern.

## <a name="technical-notes-for-managed-identity"></a>Technische Hinweise zur verwalteten Identität

Die Verwendung der verwalteten Identität für den Zugriff auf Speicherdienste hat Auswirkungen auf bestimmte Sicherheitsaspekte. In diesem Abschnitt werden die Änderungen für die einzelnen Speicherkontotypen beschrieben.

> [!IMPORTANT]
> Diese Überlegungen gelten nur für den __Typ des Speicherkontos__, auf das Sie zugreifen.

### <a name="azure-blob-storage"></a>Azure Blob Storage

Für __Azure Blob Storage__ wird die vom Arbeitsbereich verwaltete Identität auch als [Blob Data Reader](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) (Blobdatenleser) hinzugefügt, sodass Daten aus Blobspeicher gelesen werden können.

### <a name="azure-data-lake-storage-gen2-access-control"></a>Zugriffssteuerung von Azure Data Lake Storage Gen2

Sie können den Datenzugriff innerhalb eines virtuellen Netzwerks mit Azure RBAC- und POSIX-Zugriffssteuerungslisten (Access Control Lists, ACLs) steuern.

Fügen Sie die vom Arbeitsbereich verwaltete Identität der Rolle [Blobdatenleser](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) hinzu, um Azure RBAC zu verwenden. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung (RBAC)](../storage/blobs/data-lake-storage-access-control-model.md#role-based-access-control).

Um ACLs zu verwenden, kann der verwalteten Identität des Arbeitsbereichs wie jedem anderen Sicherheitsprinzip Zugriff gewährt werden. Weitere Informationen finden Sie unter [Zugriffssteuerungslisten für Dateien und Verzeichnisse](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="azure-data-lake-storage-gen1-access-control"></a>Zugriffssteuerung von Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 unterstützt nur Zugriffssteuerungslisten im POSIX-Format. Sie können der vom Arbeitsbereich verwalteten Identität wie jedem anderen Sicherheitsprinzip Zugriff auf Ressourcen zuweisen. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

### <a name="azure-sql-database-contained-user"></a>Eigenständiger Benutzer in Azure SQL-Datenbank

Für den Zugriff mithilfe der verwalteten Identität auf Daten, die in einer Azure SQL-Datenbank-Instanz gespeichert sind, müssen Sie einen eigenständigen SQL-Benutzer erstellen, der der verwalteten Identität zugeordnet ist. Weitere Informationen zum Erstellen eines Benutzers von einem externen Anbieter finden Sie unter [Erstellen eigenständiger Benutzer mit Zuordnung zu Azure AD-Identitäten](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities).

Nachdem Sie einen eigenständigen SQL-Benutzer erstellt haben, erteilen Sie mithilfe des [GRANT T-SQL-Befehls](/sql/t-sql/statements/grant-object-permissions-transact-sql) Berechtigungen.

### <a name="azure-machine-learning-designer-default-datastore"></a>Standarddatenspeicher für Azure Machine Learning-Designer

Der Designer verwendet als Ausgabespeicher standardmäßig das mit Ihrem Arbeitsbereich verknüpfte Speicherkonto. Sie können jedoch einen beliebigen Datenspeicher, auf den Sie Zugriff haben, als Ausgabespeicher angeben. Wenn Sie in Ihrer Umgebung virtuelle Netzwerke verwenden, können Sie mit diesen Steuerelementen sicherstellen, dass Ihre Daten sicher und zugänglich bleiben.

So legen Sie einen neuen Standardspeicher für eine Pipeline fest

1. Wählen Sie in einem Pipeline-Entwurf das **Zahnradsymbol für Einstellungen** neben dem Pipeline-Titel aus.
1. Wählen Sie **Standarddatenspeicher auswählen** aus.
1. Geben Sie einen neuen Datenspeicher an.

Sie können den Standarddatenspeicher auch auf Modulbasis überschreiben. Dadurch können Sie den Speicherort für jedes einzelne Modul steuern.

1. Wählen Sie das Modul aus, für das Sie den Ausgabespeicher angeben möchten.
1. Erweitern Sie den Abschnitt **Ausgabeeinstellungen**.
1. Wählen Sie **Standardausgabeeinstellungen außer Kraft setzen** aus.
1. Wählen Sie **Ausgabeeinstellungen festlegen** aus.
1. Geben Sie einen neuen Datenspeicher an.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist ein optionaler Teil einer vierteiligen Serie zu virtuellen Netzwerken. Weitere Informationen zum Schützen eines virtuellen Netzwerks finden Sie in den verbleibenden Artikeln:

* [Teil 1: Virtuelle Netzwerke im Überblick](how-to-network-security-overview.md)
* [Teil 2: Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md)
* [Teil 3: Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
* [Teil 4: Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md)