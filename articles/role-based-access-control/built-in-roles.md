---
title: In Azure integrierte Rollen – Azure RBAC
description: In diesem Artikel werden die in Azure integrierten Rollen für die auf Azure-Rollen basierte Zugriffssteuerung (Azure RBAC) beschrieben. Es werden „Actions“, „NotActions“, „DataActions“ und „NotDataActions“ aufgelistet.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/04/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: 0a574ba281a037a06ddda1981ae6fa35b905bca1
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683665"
---
# <a name="azure-built-in-roles"></a>Integrierte Azure-Rollen

Die auf [Azure-Rollen basierte Zugriffssteuerung (Azure RBAC)](overview.md) verfügt über mehrere integrierte Azure-Rollen, die Sie Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten zuweisen können. Durch Rollenzuweisungen wird die Art und Weise gesteuert, wie Sie auf Azure-Ressourcen zugreifen. Wenn die integrierten Rollen den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen [benutzerdefinierten Azure-Rollen](custom-roles.md) erstellen.

Dieser Artikel enthält eine Liste der integrierten Azure-Rollen, die ständig weiterentwickelt werden. Verwenden Sie zum Abrufen der aktuellen Rollen [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) oder [az role definition list](/cli/azure/role/definition#az-role-definition-list). Eine Liste mit Administratorrollen für Azure Active Directory (Azure AD) finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="all"></a>All

Die folgende Tabelle enthält eine kurze Beschreibung und die eindeutige ID aller integrierten Rollen. Klicken Sie auf den Rollennamen, um die Liste der `Actions`, `NotActions`, `DataActions` und `NotDataActions` für jede Rolle anzuzeigen. Informationen zur Bedeutung dieser Aktionen und deren Anwendung auf die Verwaltung und Datenebenen finden Sie unter [Grundlegendes zu Azure-Rollendefinitionen](role-definitions.md).


> [!div class="mx-tableFixed"]
> | Integrierte Rolle | BESCHREIBUNG | id |
> | --- | --- | --- |
> | **Allgemein** |  |  |
> | [Mitwirkender](#contributor) | Ermöglicht Ihnen das Verwalten aller Optionen mit Ausnahme des Gewährens von Zugriff auf Ressourcen. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Besitzer](#owner) | Ermöglicht Ihnen das Verwalten aller Komponenten einschließlich des Zugriffs auf Ressourcen. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Leser](#reader) | Sie können alles anzeigen, aber keine Änderungen vornehmen. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [Benutzerzugriffsadministrator](#user-access-administrator) | Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Compute** |  |  |
> | [Mitwirkender von klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [VM-Administratoranmeldung](#virtual-machine-administrator-login) | Anzeigen von virtuellen Computern im Portal und Anmelden als Administrator | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Mitwirkender von virtuellen Computern](#virtual-machine-contributor) | Ermöglicht Ihnen das Verwalten virtueller Computer, aber weder den Zugriff darauf, noch auf deren verbundenen virtuellen Netzwerke oder Speicherkonten. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [VM-Benutzeranmeldung](#virtual-machine-user-login) | Anzeigen von virtuellen Computern im Portal und Anmelden als regulärer Benutzer. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Netzwerk** |  |  |
> | [Mitwirkender für den CDN-Endpunkt](#cdn-endpoint-contributor) | Diese Rolle kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [CDN-Endpunktleser](#cdn-endpoint-reader) | Diese Rolle kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [Mitwirkender für das CDN-Profil](#cdn-profile-contributor) | Diese Rolle kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [CDN-Profilleser](#cdn-profile-reader) | Diese Rolle kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [Mitwirkender von klassischem Netzwerk](#classic-network-contributor) | Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [DNS Zone Contributor](#dns-zone-contributor) | Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Mitwirkender von virtuellem Netzwerk](#network-contributor) | Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Traffic Manager-Mitwirkender](#traffic-manager-contributor) | Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Storage** |  |  |
> | [Avere-Mitwirkender](#avere-contributor) | Kann einen Avere vFXT-Cluster erstellen und verwalten. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Avere-Bediener](#avere-operator) | Wird vom Avere vFXT-Cluster zum Verwalten des Clusters verwendet | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Mitwirkender für Sicherungen](#backup-contributor) | Ermöglicht Ihnen die Verwaltung des Sicherungsdiensts. Sie können jedoch keine Tresore erstellen oder anderen Benutzern Zugriff gewähren. | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | [Sicherungsoperator](#backup-operator) | Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer. | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Sicherungsleser](#backup-reader) | Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen. | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Mitwirkender von klassischem Speicherkonto](#classic-storage-account-contributor) | Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“](#classic-storage-account-key-operator-service-role) | Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Data Box-Mitwirkender](#data-box-contributor) | Ermöglicht Ihnen das Verwalten aller Komponenten unter dem Data Box-Dienst, mit Ausnahme der Gewährung des Zugriffs für andere Benutzer. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Data Box-Leser](#data-box-reader) | Ermöglicht Ihnen das Verwalten des Data Box-Diensts, mit Ausnahme der Erstellung von Aufträgen oder der Bearbeitung von Auftragsdetails und der Gewährung des Zugriffs für andere Benutzer. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Data Lake Analytics-Entwickler](#data-lake-analytics-developer) | Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Lese- und Datenzugriff](#reader-and-data-access) | Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Mitwirkender von Speicherkonto](#storage-account-contributor) | Erlaubt die Verwaltung von Speicherkonten. Ermöglicht den Zugriff auf den Kontoschlüssel, der für den Datenzugriff über die Autorisierung mit einem gemeinsam verwendetem Schlüssel genutzt werden kann. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [Dienstrolle „Speicherkonto-Schlüsseloperator“](#storage-account-key-operator-service-role) | Ermöglicht das Auflisten und erneute Generieren von Zugriffsschlüsseln für Speicherkonten. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Mitwirkender an Speicherblobdaten](#storage-blob-data-contributor) | Lesen, Schreiben und Löschen von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Besitzer von Speicherblobdaten](#storage-blob-data-owner) | Bietet Vollzugriff auf Azure Storage-Blobcontainer und -daten, einschließlich POSIX-Zugriffssteuerung. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Leser von Speicherblobdaten](#storage-blob-data-reader) | Lesen und Auflisten von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Storage Blob-Delegator](#storage-blob-delegator) | Abrufen eines Benutzerdelegierungsschlüssels, mit dem dann eine SAS (Shared Access Signature) für einen Container oder Blob erstellt werden kann, die mit Azure AD-Anmeldeinformationen signiert ist. Weitere Informationen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas). | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Speicherdateidaten-SMB-Freigabemitwirkender](#storage-file-data-smb-share-contributor) | Ermöglicht den Lese-, Schreib- und Löschzugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Für diese Rolle gibt es keine integrierte Entsprechung auf Windows-Dateiservern. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Speicherdateidaten-SMB-Freigabemitwirkender mit erhöhten Rechten](#storage-file-data-smb-share-elevated-contributor) | Ermöglicht das Lesen, Schreiben, Löschen und Bearbeiten von Zugriffssteuerungslisten für Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Bearbeiten auf Windows-Dateiservern. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Speicherdateidaten-SMB-Freigabeleser](#storage-file-data-smb-share-reader) | Ermöglicht den Lesezugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Lesen auf Windows-Dateiservern. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Mitwirkender an Storage-Warteschlangendaten](#storage-queue-data-contributor) | Lesen, Schreiben und Löschen von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [Verarbeiter von Speicherwarteschlangen-Datennachrichten](#storage-queue-data-message-processor) | Einsehen, Abrufen und Löschen einer Nachricht aus einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [Absender der Speicherwarteschlangen-Datennachricht](#storage-queue-data-message-sender) | Hinzufügen von Nachrichten zu einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Storage-Warteschlangendatenleser](#storage-queue-data-reader) | Lesen und Auflisten von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Web** |  |  |
> | [Azure Maps-Datenleser](#azure-maps-data-reader) | Gewährt Lesezugriff auf kartenbezogene Daten von einem Azure Maps-Konto. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Mitwirkender von Suchdienst](#search-service-contributor) | Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [Mitwirkender von Webplan](#web-plan-contributor) | Ermöglicht Ihnen das Verwalten der Webpläne für Websites, nicht aber den Zugriff darauf. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Mitwirkender von Website](#website-contributor) | Ermöglicht Ihnen das Verwalten von Websites (nicht der Webpläne), nicht aber den Zugriff darauf. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Container** |  |  |
> | [AcrDelete](#acrdelete) | ACR-Löschvorgang | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | ACR-Imagesignaturgeber | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | ACR-Pullvorgang | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | ACR-Pushvorgang | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | ACR-Quarantänedatenleser | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | ACR-Quarantänedatenschreiber | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Administratorrolle für Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-admin-role) | Listet die Aktion für Anmeldeinformationen des Clusteradministrators auf. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Benutzerrolle für Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-user-role) | Listet die Aktion für Anmeldeinformationen des Clusterbenutzer auf. | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **Datenbanken** |  |  |
> | [Cosmos DB-Rolle „Kontoleser“](#cosmos-db-account-reader-role) | Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Cosmos DB-Operator](#cosmos-db-operator) | Ermöglicht das Verwalten von Azure Cosmos DB-Konten, aber nicht das Zugreifen auf deren Daten. Verhindert den Zugriff auf Kontoschlüssel und Verbindungszeichenfolgen. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Kann eine Wiederherstellungsanforderung für eine Cosmos DB-Datenbank oder einen Container für ein Konto übermitteln. | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor) | Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Mitwirkender von Redis-Cache](#redis-cache-contributor) | Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [Mitwirkender von SQL DB](#sql-db-contributor) | Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [Verwaltete SQL-Instanz: Mitwirkender](#sql-managed-instance-contributor) | Diese Rolle ermöglicht Ihnen das Verwalten verwalteter SQL-Instanzen und der erforderlichen Netzwerkkonfiguration, jedoch nicht das Erteilen des Zugriffs an andere. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [SQL-Sicherheits-Manager](#sql-security-manager) | Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [Mitwirkender von SQL Server](#sql-server-contributor) | Diese Rolle ermöglicht es Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Analyse** |  |  |
> | [Azure Event Hubs-Datenbesitzer](#azure-event-hubs-data-owner) | Ermöglicht den uneingeschränkten Zugriff auf die Azure Event Hubs-Ressourcen. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Azure Event Hubs-Datenempfänger](#azure-event-hubs-data-receiver) | Ermöglicht Empfängern den Zugriff auf die Azure Event Hubs-Ressourcen. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Azure Event Hubs-Datensender](#azure-event-hubs-data-sender) | Ermöglicht Absendern den Zugriff auf die Azure Event Hubs-Ressourcen. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Mitwirkender von Data Factory](#data-factory-contributor) | Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [Datenpurger](#data-purger) | Kann Analysedaten endgültig löschen. | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [HDInsight-Clusteroperator](#hdinsight-cluster-operator) | Ermöglicht Ihnen das Lesen und Ändern von HDInsight-Clusterkonfigurationen. | 61ed4efc-fab3-44fd-b111-e24485cc132a |
> | [Mitwirkender für die HDInsight-Domänendienste](#hdinsight-domain-services-contributor) | Ermöglicht Ihnen, Vorgänge im Zusammenhang mit Domänendiensten, die für das HDInsight Enterprise-Sicherheitspaket erforderlich sind, zu lesen, zu erstellen, zu ändern und zu löschen. | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Log Analytics-Mitwirkender](#log-analytics-contributor) | Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Log Analytics-Leser](#log-analytics-reader) | Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Blockchain** |  |  |
> | [Zugriff auf Blockchainmitgliedsknoten (Vorschauversion)](#blockchain-member-node-access-preview) | Ermöglicht den Zugriff auf Blockchainmitgliedsknoten. | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **KI und Machine Learning** |  |  |
> | [Mitwirkender für Cognitive Services](#cognitive-services-contributor) | Ermöglicht Ihnen das Erstellen, Lesen, Aktualisieren, Löschen und Verwalten von Cognitive Services-Schlüsseln. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [Cognitive Services-Datenleser (Vorschau)](#cognitive-services-data-reader-preview) | Ermöglicht das Lesen von Cognitive Services-Daten. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Cognitive Services-Benutzer](#cognitive-services-user) | Ermöglicht Ihnen das Lesen und Auflisten von Cognitive Services-Schlüsseln. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Mixed Reality** |  |  |
> | [Spatial Anchors-Kontomitwirkender](#spatial-anchors-account-contributor) | Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, nicht jedoch das Löschen von Ankern. | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Spatial Anchors-Kontobesitzer](#spatial-anchors-account-owner) | Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, einschließlich der Löschung von Ankern. | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Spatial Anchors-Kontoleser](#spatial-anchors-account-reader) | Ermöglicht Ihnen das Ermitteln und Lesen von Eigenschaften für Raumanker in Ihrem Dokument. | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Integration** |  |  |
> | [Mitwirkender des API-Verwaltungsdienstes](#api-management-service-contributor) | Kann Dienst und APIs verwalten. | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [Operatorrolle des API Management-Diensts](#api-management-service-operator-role) | Kann den Dienst, aber nicht die APIs verwalten. | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [Leserrolle des API Management-Diensts](#api-management-service-reader-role) | Schreibgeschützter Zugriff auf Dienst und APIs | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [App Configuration-Datenbesitzer](#app-configuration-data-owner) | Ermöglicht den Vollzugriff auf App Configuration-Daten. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [App Configuration-Datenleser](#app-configuration-data-reader) | Ermöglicht den Lesezugriff auf App Configuration-Daten. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Azure Service Bus-Datenbesitzer](#azure-service-bus-data-owner) | Ermöglicht den uneingeschränkten Zugriff auf die Azure Service Bus-Ressourcen. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Azure Service Bus-Datenempfänger](#azure-service-bus-data-receiver) | Ermöglicht Empfängern den Zugriff auf die Azure Service Bus-Ressourcen. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Azure Service Bus-Datensender](#azure-service-bus-data-sender) | Ermöglicht Absendern den Zugriff auf die Azure Service Bus-Ressourcen. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Besitzer der Azure Stack-Registrierung](#azure-stack-registration-owner) | Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [EventGrid EventSubscription-Mitwirkender](#eventgrid-eventsubscription-contributor) | Ermöglicht die Verwaltung von EventGrid-Ereignisabonnementvorgängen. | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [EventGrid EventSubscription-Leser](#eventgrid-eventsubscription-reader) | Ermöglicht das Lesen von EventGrid-Ereignisabonnements. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [Mitwirkender von Intelligent Systems-Konto](#intelligent-systems-account-contributor) | Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Logik-App-Mitwirkender](#logic-app-contributor) | Ermöglicht Ihnen die Verwaltung von Logik-Apps. Sie können aber nicht den App-Zugriff ändern. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Logik-App-Operator](#logic-app-operator) | Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. Sie können diese aber nicht bearbeiten oder aktualisieren. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Identität** |  |  |
> | [Mitwirkender für verwaltete Identität](#managed-identity-contributor) | Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen. | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Operator für verwaltete Identität](#managed-identity-operator) | Dem Benutzer zugewiesene Identität lesen und zuweisen. | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Security** |  |  |
> | [Azure Sentinel-Mitwirkender](#azure-sentinel-contributor) | Azure Sentinel-Mitwirkender | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Azure Sentinel-Leser](#azure-sentinel-reader) | Azure Sentinel-Leser | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Azure Sentinel-Antwortender](#azure-sentinel-responder) | Azure Sentinel-Antwortender | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Key Vault-Mitwirkender](#key-vault-contributor) | Ermöglicht Ihnen die Verwaltung von Schlüsseltresoren, aber nicht den Zugriff darauf. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Sicherheitsadministrator](#security-admin) | Anzeigen und Aktualisieren von Berechtigungen für Security Center. Gleiche Rechte wie der Sicherheitsleseberechtigte und kann darüber hinaus die Sicherheitsrichtlinie aktualisieren sowie Warnungen und Empfehlungen verwerfen. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Mitwirkender für Sicherheitsbewertungen](#security-assessment-contributor) | Ermöglicht das Pushen von Bewertungen an Security Center | 612c2aa1-cb24-443b-ac28-3ab7272de6f5 |
> | [Sicherheits-Manager (Legacy)](#security-manager-legacy) | Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Sicherheitsleseberechtigter](#security-reader) | Anzeigen von Berechtigungen für Security Center. Kann Empfehlungen, Warnungen, Sicherheitsrichtlinien und Sicherheitszustände anzeigen, jedoch keine Änderungen vornehmen. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [DevTest Labs-Benutzer](#devtest-labs-user) | Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [Lab-Ersteller](#lab-creator) | Ermöglicht Ihnen das Erstellen, Verwalten und Löschen verwalteter Labs unter Ihren Azure Lab-Konten. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Überwachen** |  |  |
> | [Mitwirkender der Application Insights-Komponente](#application-insights-component-contributor) | Kann Application Insights-Komponenten verwalten | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Application Insights-Momentaufnahmedebugger](#application-insights-snapshot-debugger) | Gibt dem Benutzer die Berechtigung zum Anzeigen und Herunterladen von Debugmomentaufnahmen, die mit dem Application Insights-Momentaufnahmedebugger erfasst wurden. Beachten Sie, dass diese Berechtigungen in der Rolle [Besitzer](#owner) oder [Mitwirkender](#contributor) nicht enthalten sind. Die Application Insights-Rolle „Momentaufnahmedebugger“ muss Benutzern direkt zugewiesen werden. Die Rolle wird nicht erkannt, wenn sie einer benutzerdefinierten Rolle hinzugefügt wird. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Überwachungsmitwirkender](#monitoring-contributor) | Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles). | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [Herausgeber von Überwachungsmetriken](#monitoring-metrics-publisher) | Ermöglicht die Veröffentlichung von Metriken für Azure-Ressourcen. | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [Überwachungsleser](#monitoring-reader) | Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles). | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Arbeitsmappenmitwirkender](#workbook-contributor) | Kann freigegebene Arbeitsmappen speichern. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [Arbeitsmappenleser](#workbook-reader) | Kann Arbeitsmappen lesen. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **Verwaltung und Governance** |  |  |
> | [Automation-Auftragsoperator](#automation-job-operator) | Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Operator für Automation](#automation-operator) | Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen. | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Automation-Runbookoperator](#automation-runbook-operator) | Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Onboarding von Azure Connected Machine](#azure-connected-machine-onboarding) | Kann Azure Connected Machine integrieren. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Ressourcenadministrator für Azure Connected Machine](#azure-connected-machine-resource-administrator) | Kann Azure Connected Machines lesen, schreiben, löschen und erneut integrieren. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Abrechnungsleser](#billing-reader) | Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht. | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Blueprint-Mitwirkender](#blueprint-contributor) | Kann Blaupausendefinitionen verwalten, aber nicht zuweisen. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [Blueprint-Operator](#blueprint-operator) | Kann vorhandene veröffentlichte Blaupausen zuweisen, aber keine neuen Blaupausen erstellen. Beachten Sie, dass dies nur funktioniert, wenn die Zuweisung mit einer vom Benutzer zugewiesenen verwalteten Identität erfolgt. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Mitwirkender für Cost Management](#cost-management-contributor) | Ermöglicht Ihnen das Anzeigen der Kosten und das Verwalten der Kostenkonfiguration (z. B. Budgets, Exporte). | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Cost Management-Leser](#cost-management-reader) | Ermöglicht Ihnen das Anzeigen der Kostendaten und -konfiguration (z. B. Budgets, Exporte). | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Hierarchieeinstellungsadministrator](#hierarchy-settings-administrator) | Ermöglicht Benutzern das Bearbeiten und Löschen von Hierarchieeinstellungen. | 350f8d15-c687-4448-8ae1-157740a3936d |
> | [Rolle „Mitwirkender für verwaltete Anwendungen“](#managed-application-contributor-role) | Ermöglicht das Erstellen von Ressourcen für verwaltete Anwendungen. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [Rolle „Bediener für verwaltete Anwendung“](#managed-application-operator-role) | Ermöglicht Ihnen das Lesen und Durchführen von Aktionen für Ressourcen der verwalteten Anwendung. | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Leser für verwaltete Anwendungen](#managed-applications-reader) | Ermöglicht Ihnen, Ressourcen in einer verwalteten App zu lesen und JIT-Zugriff anzufordern. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [Rolle „Registrierungszuweisung für verwaltete Dienste löschen“](#managed-services-registration-assignment-delete-role) | Mit der Rolle „Registrierungszuweisung für verwaltete Dienste löschen“ können Benutzer des verwaltenden Mandanten die ihrem Mandanten zugewiesene Registrierungszuweisung löschen. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [Verwaltungsgruppenmitwirkender](#management-group-contributor) | Rolle „Verwaltungsgruppenmitwirkender“ | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Verwaltungsgruppenleser](#management-group-reader) | Rolle „Verwaltungsgruppenleser“ | ac63b705-F282-497d-ac71-919bf39d939d |
> | [Mitwirkender von New Relic APM-Konto](#new-relic-apm-account-contributor) | Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [Policy Insights-Datenschreiber (Vorschau)](#policy-insights-data-writer-preview) | Ermöglicht Lesezugriff auf Ressourcenrichtlinien und Schreibzugriff auf Richtlinienereignisse für Ressourcenkomponenten. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [Mitwirkender bei Ressourcenrichtlinien](#resource-policy-contributor) | Benutzer mit Rechten zum Erstellen/Ändern der Ressourcenrichtlinie, zum Erstellen eines Supporttickets und zum Lesen von Ressourcen/der Hierarchie. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Site Recovery-Mitwirkender](#site-recovery-contributor) | Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung. | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Site Recovery-Operator](#site-recovery-operator) | Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge. | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Site Recovery-Leser](#site-recovery-reader) | Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge. | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Mitwirkender für Supportanfragen](#support-request-contributor) | Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen. | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Tagmitwirkender](#tag-contributor) | Hiermit können Tags für Entitäten verwaltet werden, ohne Zugriff auf die Entitäten selbst zu gewähren. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **Andere** |  |  |
> | [Mitwirkender von BizTalk](#biztalk-contributor) | Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Mitwirkender von Zeitplanungsauftragssammlung](#scheduler-job-collections-contributor) | Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |


## <a name="general"></a>Allgemein


### <a name="contributor"></a>Mitwirkender

Ermöglicht Ihnen das Verwalten aller Optionen mit Ausnahme des Gewährens von Zugriff auf Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Löschen von Rollen, Richtlinienzuweisungen, Richtliniendefinitionen und Richtliniensatzdefinitionen |
> | Microsoft.Authorization/*/Write | Erstellen von Rollen, Rollenzuweisungen, Richtlinienzuweisungen, Richtliniendefinitionen und Richtliniensatzdefinitionen |
> | Microsoft.Authorization/elevateAccess/Action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |
> | Microsoft.Blueprint/blueprintAssignments/write | Erstellt oder aktualisiert alle Blaupausenzuweisungen |
> | Microsoft.Blueprint/blueprintAssignments/delete | Löscht alle Blaupausenzuweisungen |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything except access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="owner"></a>Besitzer

Ermöglicht Ihnen das Verwalten aller Komponenten einschließlich des Zugriffs auf Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything, including access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader"></a>Leser

Sie können alles anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything, but not make any changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "permissions": [
    {
      "actions": [
        "*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="user-access-administrator"></a>Benutzerzugriffsadministrator

Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Authorization/* | Verwalten der Autorisierung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage user access to Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "User Access Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="compute"></a>Compute


### <a name="classic-virtual-machine-contributor"></a>Mitwirkender von klassischen virtuellen Computern

Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
> | Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Dient zum Verknüpfen einer reservierten IP-Adresse. |
> | Microsoft.ClassicNetwork/reservedIps/read | Ruft die reservierten IP-Adressen ab. |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Führt zum Beitritt zum virtuellen Netzwerk. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Dient zum Abrufen des virtuellen Netzwerks. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Gibt das Speicherkontoimage zurück. (Veraltet. Verwenden Sie „Microsoft.ClassicStorage/storageAccounts/vmImages“) |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "name": "d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/domainNames/*",
        "Microsoft.ClassicCompute/virtualMachines/*",
        "Microsoft.ClassicNetwork/networkSecurityGroups/join/action",
        "Microsoft.ClassicNetwork/reservedIps/link/action",
        "Microsoft.ClassicNetwork/reservedIps/read",
        "Microsoft.ClassicNetwork/virtualNetworks/join/action",
        "Microsoft.ClassicNetwork/virtualNetworks/read",
        "Microsoft.ClassicStorage/storageAccounts/disks/read",
        "Microsoft.ClassicStorage/storageAccounts/images/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-administrator-login"></a>VM-Administratoranmeldung

Anzeigen von virtuellen Computern im Portal und Anmelden als Administrator

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Hiermit melden Sie sich bei einem virtuellen Computer als normaler Benutzer an. |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Hiermit melden Sie sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten an. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as administrator",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "name": "1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.Compute/virtualMachines/loginAsAdmin/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Administrator Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-contributor"></a>Mitwirkender von virtuellen Computern

Ermöglicht Ihnen das Verwalten virtueller Computer, aber weder den Zugriff darauf, noch auf deren verbundenen virtuellen Netzwerke oder Speicherkonten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von Compute-Verfügbarkeitsgruppen |
> | Microsoft.Compute/locations/* | Erstellen und Verwalten von Compute-Speicherorten |
> | Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
> | Microsoft.Compute/virtualMachineScaleSets | Erstellen und Verwalten von Skalierungsgruppen für virtuelle Computer |
> | Microsoft.Compute/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | Microsoft.Compute/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | Microsoft.Compute/disks/delete | Löscht den Datenträger. |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für ein Application Gateway. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpft einen NAT-Pool für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine NAT-Regel für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkspeicherorten |
> | Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
> | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | Microsoft.Network/networkSecurityGroups/read | Ruft eine Netzwerksicherheitsgruppen-Definition ab. |
> | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. Nicht warnbar. |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/locations/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/virtualMachineScaleSets/*",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/delete",
        "Microsoft.DevTestLab/schedules/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/loadBalancers/probes/join/action",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/locations/*",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Network/networkSecurityGroups/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/write",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.SqlVirtualMachine/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-user-login"></a>VM-Benutzeranmeldung

Anzeigen von virtuellen Computern im Portal und Anmelden als regulärer Benutzer.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Hiermit melden Sie sich bei einem virtuellen Computer als normaler Benutzer an. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as a regular user.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb879df8-f326-4884-b1cf-06f3ad86be52",
  "name": "fb879df8-f326-4884-b1cf-06f3ad86be52",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine User Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="networking"></a>Netzwerk


### <a name="cdn-endpoint-contributor"></a>Mitwirkender für den CDN-Endpunkt

Diese Rolle kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "name": "426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-endpoint-reader"></a>CDN-Endpunktleser

Diese Rolle kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "name": "871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-contributor"></a>Mitwirkender für das CDN-Profil

Diese Rolle kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN profiles and their endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "name": "ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-reader"></a>CDN-Profilleser

Diese Rolle kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN profiles and their endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f96442b-4075-438f-813d-ad51ab4019af",
  "name": "8f96442b-4075-438f-813d-ad51ab4019af",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-network-contributor"></a>Mitwirkender von klassischem Netzwerk

Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerken |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "name": "b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicNetwork/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="dns-zone-contributor"></a>DNS Zone Contributor

Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/dnsZones/* | Erstellen und Verwalten von DNS-Zonen und -Einträgen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DNS zones and record sets in Azure DNS, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/befefa01-2a29-4197-83a8-272ff33ce314",
  "name": "befefa01-2a29-4197-83a8-272ff33ce314",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/dnsZones/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="network-contributor"></a>Mitwirkender von virtuellem Netzwerk

Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/* | Erstellen und Verwalten von Netzwerken |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7",
  "name": "4d97b98b-1d4f-4787-a291-c67834d212e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="traffic-manager-contributor"></a>Traffic Manager-Mitwirkender

Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Traffic Manager profiles, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "name": "a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/trafficManagerProfiles/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Traffic Manager Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="storage"></a>Storage


### <a name="avere-contributor"></a>Avere-Mitwirkender

Kann einen Avere vFXT-Cluster erstellen und verwalten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Compute/*/read |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft.Compute/disks/* |  |
> | Microsoft.Network/*/read |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/*/read |  |
> | Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Gibt das Ergebnis beim Löschen eines Blobs zurück. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Gibt ein Blob oder eine Liste von Blobs zurück. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Gibt das Ergebnis beim Schreiben eines Blobs zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create and manage an Avere vFXT cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "name": "4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/disks/*",
        "Microsoft.Network/*/read",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/*/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="avere-operator"></a>Avere-Bediener

Wird vom Avere vFXT-Cluster zum Verwalten des Clusters verwendet

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Compute/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Network/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.Network/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Microsoft.Network/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Gibt das Ergebnis beim Löschen eines Containers zurück. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermit wird eine Liste von Containern zurückgegeben. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Gibt das Ergebnis des PUT-Vorgangs für den Blobcontainer zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Gibt das Ergebnis beim Löschen eines Blobs zurück. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Gibt ein Blob oder eine Liste von Blobs zurück. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Gibt das Ergebnis beim Schreiben eines Blobs zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the Avere vFXT cluster to manage the cluster",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "name": "c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-contributor"></a>Mitwirkender für Sicherungen

Ermöglicht Ihnen die Verwaltung des Sicherungsdiensts. Sie können jedoch keine Tresore erstellen oder anderen Benutzern Zugriff gewähren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Verwalten der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Erstellen und Verwalten von Sicherungscontainern in Sicherungsfabrics des Recovery Services-Tresors |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Erstellen und Verwalten von Sicherungsrichtlinien |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Erstellen und Verwalten von gesicherten Elementen |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Erstellen und Verwalten von Containern mit Sicherungselementen |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Erstellen und Verwalten von Zertifikaten in Zusammenhang mit Sicherungen in einem Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | Microsoft.RecoveryServices/Vaults/usages/* | Erstellen und Verwalten der Nutzung des Recovery Services-Tresors |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Überprüft Vorgang für geschütztes Element. |
> | Microsoft.RecoveryServices/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Überprüft Features. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | Microsoft.RecoveryServices/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup service,but can't create vaults and give access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e467623-bb1f-42f4-a55d-6e525e11384b",
  "name": "5e467623-bb1f-42f4-a55d-6e525e11384b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupSecurityPIN/*",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/*",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/Vaults/usages/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-operator"></a>Sicherungsoperator

Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Führt eine Sicherung für geschützte Elemente aus. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Überprüft Vorgang für geschütztes Element. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Erstellt einen registrierten Container. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Führt die Abfrage für Workloads innerhalb eines Containers durch. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Ruft einen beabsichtigten Sicherungsschutz ab. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Überprüft Features. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | Microsoft.RecoveryServices/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup services, except removal of backup, vault creation and giving access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00c29273-979b-4161-815c-10b084fb9324",
  "name": "00c29273-979b-4161-815c-10b084fb9324",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/write",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/write",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-reader"></a>Sicherungsleser

Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Gibt die Konfiguration für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Ruft einen beabsichtigten Sicherungsschutz ab. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | Microsoft.RecoveryServices/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Überprüft Features. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view backup services, but can't make changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "name": "a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/read",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-contributor"></a>Mitwirkender von klassischem Speicherkonto

Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic storage accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "name": "86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-key-operator-service-role"></a>Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“

Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Classic Storage Account Key Operators are allowed to list and regenerate keys on Classic Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "name": "985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ClassicStorage/storageAccounts/listkeys/action",
        "Microsoft.ClassicStorage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-contributor"></a>Data Box-Mitwirkender

Ermöglicht Ihnen das Verwalten aller Komponenten unter dem Data Box-Dienst, mit Ausnahme der Gewährung des Zugriffs für andere Benutzer.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything under Data Box Service except giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/add466c9-e687-43fc-8d98-dfcf8d720be5",
  "name": "add466c9-e687-43fc-8d98-dfcf8d720be5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Databox/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-reader"></a>Data Box-Leser

Ermöglicht Ihnen das Verwalten des Data Box-Diensts, mit Ausnahme der Erstellung von Aufträgen oder der Bearbeitung von Auftragsdetails und der Gewährung des Zugriffs für andere Benutzer.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | Hiermit werden die unverschlüsselten Anmeldeinformationen für den Auftrag aufgelistet. |
> | Microsoft.Databox/locations/availableSkus/action | Mit dieser Methode wird die Liste der verfügbaren SKUs zurückgegeben. |
> | Microsoft.Databox/locations/validateInputs/action | Diese Methode führt alle Arten von Prüfungen aus. |
> | Microsoft.Databox/locations/regionConfiguration/action | Diese Methode gibt die Konfigurationen für die Region zurück. |
> | Microsoft.Databox/locations/validateAddress/action | Hiermit wird die Lieferadresse überprüft, und es werden – sofern vorhanden – alternative Adressen bereitgestellt. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Data Box Service except creating order or editing order details and giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "name": "028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Databox/*/read",
        "Microsoft.Databox/jobs/listsecrets/action",
        "Microsoft.Databox/jobs/listcredentials/action",
        "Microsoft.Databox/locations/availableSkus/action",
        "Microsoft.Databox/locations/validateInputs/action",
        "Microsoft.Databox/locations/regionConfiguration/action",
        "Microsoft.Databox/locations/validateAddress/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-lake-analytics-developer"></a>Data Lake Analytics-Entwickler

Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Löscht ein DataLakeAnalytics-Konto. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Dient zum Löschen einer Firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Erstellt oder aktualisiert eine Computerichtlinie. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Löscht eine Computerichtlinie. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you submit, monitor, and manage your own jobs but not create or delete Data Lake Analytics accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/47b7735b-770e-4598-a7da-8b91488b4c88",
  "name": "47b7735b-770e-4598-a7da-8b91488b4c88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BigAnalytics/accounts/*",
        "Microsoft.DataLakeAnalytics/accounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.BigAnalytics/accounts/Delete",
        "Microsoft.BigAnalytics/accounts/TakeOwnership/action",
        "Microsoft.BigAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action",
        "Microsoft.DataLakeAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Write",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Write",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Lake Analytics Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader-and-data-access"></a>Lese- und Datenzugriff

Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/ListAccountSas/action | Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything but will not let you delete or create a storage account or contained resource. It will also allow read/write access to all data contained in a storage account via access to storage account keys.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c12c1c16-33a1-487b-954d-41c89c60f349",
  "name": "c12c1c16-33a1-487b-954d-41c89c60f349",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/ListAccountSas/action",
        "Microsoft.Storage/storageAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader and Data Access",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-contributor"></a>Mitwirkender von Speicherkonto

Erlaubt die Verwaltung von Speicherkonten. Ermöglicht den Zugriff auf den Kontoschlüssel, der für den Datenzugriff über die Autorisierung mit einem gemeinsam verwendetem Schlüssel genutzt werden kann.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage storage accounts, including accessing storage account keys which provide full access to storage account data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "name": "17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-key-operator-service-role"></a>Dienstrolle „Speicherkonto-Schlüsseloperator“

Ermöglicht das Auflisten und erneute Generieren von Zugriffsschlüsseln für Speicherkonten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Storage Account Key Operators are allowed to list and regenerate keys on Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/81a9662b-bebf-436f-a333-f67b29880f12",
  "name": "81a9662b-bebf-436f-a333-f67b29880f12",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-contributor"></a>Mitwirkender an Storage-Blobdaten

Lesen, Schreiben und Löschen von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Löschen eines Containers. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Zurückgeben eines Containers oder einer Liste von Containern. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Ändern der Metadaten oder Eigenschaften eines Containers. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Löschen eines Blobs |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Zurückgeben eines Blob oder einer Liste von Blobs. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action | Verschiebt das Blob aus einem Pfad in einen anderen. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Schreiben in ein Blob. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "name": "ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-owner"></a>Besitzer von Speicherblobdaten

Bietet Vollzugriff auf Azure Storage-Blobcontainer und -daten, einschließlich POSIX-Zugriffssteuerung. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | Vollzugriffsberechtigungen für Container. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Vollzugriffsberechtigungen für Blobs. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "name": "b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/*",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-reader"></a>Leser von Speicherblobdaten

Lesen und Auflisten von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Zurückgeben eines Containers oder einer Liste von Containern. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Zurückgeben eines Blob oder einer Liste von Blobs. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-delegator"></a>Storage Blob-Delegator

Abrufen eines Benutzerdelegierungsschlüssels, mit dem dann eine SAS (Shared Access Signature) für einen Container oder Blob erstellt werden kann, die mit Azure AD-Anmeldeinformationen signiert ist. Weitere Informationen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for generation of a user delegation key which can be used to sign SAS tokens",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "name": "db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Delegator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-contributor"></a>Speicherdateidaten-SMB-Freigabemitwirkender

Ermöglicht den Lese-, Schreib- und Löschzugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Für diese Rolle gibt es keine integrierte Entsprechung auf Windows-Dateiservern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Gibt das Ergebnis des Schreibens einer Datei oder des Erstellens eines Ordners zurück. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Gibt das Ergebnis des Löschens einer Datei/eines Ordners zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "name": "0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-elevated-contributor"></a>Speicherdateidaten-SMB-Freigabemitwirkender mit erhöhten Rechten

Ermöglicht das Lesen, Schreiben, Löschen und Bearbeiten von Zugriffssteuerungslisten für Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Bearbeiten auf Windows-Dateiservern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Gibt das Ergebnis des Schreibens einer Datei oder des Erstellens eines Ordners zurück. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Gibt das Ergebnis des Löschens einer Datei/eines Ordners zurück. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Gibt das Ergebnis der Berechtigungsänderung für eine Datei/einen Ordner zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7264617-510b-434b-a828-9731dc254ea7",
  "name": "a7264617-510b-434b-a828-9731dc254ea7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Elevated Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-reader"></a>Speicherdateidaten-SMB-Freigabeleser

Ermöglicht den Lesezugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Lesen auf Windows-Dateiservern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure File Share over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aba4ae5f-2193-4029-9191-0cb91df5e314",
  "name": "aba4ae5f-2193-4029-9191-0cb91df5e314",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-contributor"></a>Mitwirkender an Storage-Warteschlangendaten

Lesen, Schreiben und Löschen von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Löschen einer Warteschlange. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Zurückgeben einer Warteschlange oder Liste mit Warteschlangen. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Ändern der Metadaten oder Eigenschaften einer Warteschlange. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Löschen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Einsehen oder Abrufen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Hinzufügen von Nachrichten zu einer Warteschlange. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "name": "974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-processor"></a>Verarbeiter von Speicherwarteschlangen-Datennachrichten

Einsehen, Abrufen und Löschen einer Nachricht aus einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Einsehen einer Nachricht. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Abrufen und Löschen einer Nachricht. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for peek, receive, and delete access to Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "name": "8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Processor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-sender"></a>Absender der Speicherwarteschlangen-Datennachricht

Hinzufügen von Nachrichten zu einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Hinzufügen von Nachrichten zu einer Warteschlange. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for sending of Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "name": "c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-reader"></a>Storage-Warteschlangendatenleser

Lesen und Auflisten von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Gibt eine Warteschlange oder eine Liste von Warteschlangen zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Einsehen oder Abrufen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/19e7f393-937e-4f77-808e-94535e297925",
  "name": "19e7f393-937e-4f77-808e-94535e297925",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="web"></a>Web


### <a name="azure-maps-data-reader"></a>Azure Maps-Datenleser

Gewährt Lesezugriff auf kartenbezogene Daten von einem Azure Maps-Konto.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Maps/accounts/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "name": "423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Mitwirkender von Suchdienst

Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Search services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "name": "7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Search/searchServices/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Search Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="web-plan-contributor"></a>Mitwirkender von Webplan

Ermöglicht Ihnen das Verwalten der Webpläne für Websites, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |
> | Microsoft.Web/hostingEnvironments/Join/Action | Tritt einer App Service-Umgebung bei. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the web plans for websites, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "name": "2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/serverFarms/*",
        "Microsoft.Web/hostingEnvironments/Join/Action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Web Plan Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="website-contributor"></a>Mitwirkender von Website

Ermöglicht Ihnen das Verwalten von Websites (nicht der Webpläne), nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Web/certificates/* | Erstellen und Verwalten von Websitezertifikaten |
> | Microsoft.Web/listSitesAssignedToHostName/read | Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Microsoft.Web/sites/* | Erstellen und Verwalten von Websites (die Erstellung von Websites erfordert auch Schreibberechtigungen für den zugehörigen App Service-Plan) |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage websites (not web plans), but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/de139f84-1756-47ae-9be6-808fbbe84772",
  "name": "de139f84-1756-47ae-9be6-808fbbe84772",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/certificates/*",
        "Microsoft.Web/listSitesAssignedToHostName/read",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Website Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="containers"></a>Container


### <a name="acrdelete"></a>AcrDelete

ACR-Löschvorgang

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/artifacts/delete | Löschen von Artefakten aus einer Containerregistrierung. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr delete",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/artifacts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrDelete",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrimagesigner"></a>AcrImageSigner

ACR-Imagesignaturgeber

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | Pushen/Pullen von Inhaltsvertrauen-Metadaten für eine Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr image signer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6cef56e8-d556-48e5-a04f-b8e64114680f",
  "name": "6cef56e8-d556-48e5-a04f-b8e64114680f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/sign/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrImageSigner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpull"></a>AcrPull

ACR-Pullvorgang

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Pullen oder Abrufen von Images aus einer Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr pull",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "name": "7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPull",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpush"></a>AcrPush

ACR-Pushvorgang

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Pullen oder Abrufen von Images aus einer Containerregistrierung |
> | Microsoft.ContainerRegistry/registries/push/write | Pushen oder Schreiben von Images in eine Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr push",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8311e382-0749-4cb8-b61a-304f252e45ec",
  "name": "8311e382-0749-4cb8-b61a-304f252e45ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read",
        "Microsoft.ContainerRegistry/registries/push/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPush",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinereader"></a>AcrQuarantineReader

ACR-Quarantänedatenleser

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/quarantine/read | Pullen oder Abrufen von Images in Quarantäne aus einer Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cdda3590-29a3-44f6-95f2-9f980659eb04",
  "name": "cdda3590-29a3-44f6-95f2-9f980659eb04",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineReader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinewriter"></a>AcrQuarantineWriter

ACR-Quarantänedatenschreiber

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerRegistry/registries/quarantine/read | Pullen oder Abrufen von Images in Quarantäne aus einer Containerregistrierung |
> | Microsoft.ContainerRegistry/registries/quarantine/write | Schreiben/Ändern des Quarantänezustands von unter Quarantäne gestellten Images |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data writer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "name": "c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read",
        "Microsoft.ContainerRegistry/registries/quarantine/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineWriter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Administratorrolle für Azure Kubernetes Service-Cluster

Listet die Aktion für Anmeldeinformationen des Clusteradministrators auf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Listet die clusterAdmin-Anmeldeinformationen eines verwalteten Clusters auf. |
> | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens mithilfe der Liste der Anmeldeinformationen ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster admin credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "name": "0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action",
        "Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster Admin Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-user-role"></a>Benutzerrolle für Azure Kubernetes Service-Cluster

Listet die Aktion für Anmeldeinformationen des Clusterbenutzer auf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Listet die clusterUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "name": "4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="databases"></a>Datenbanken


### <a name="cosmos-db-account-reader-role"></a>Cosmos DB-Rolle „Kontoleser“

Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.DocumentDB/*/read | Lesen einer beliebigen Sammlung |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | Microsoft.Insights/MetricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | Microsoft.Insights/Metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read Azure Cosmos DB Accounts data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "name": "fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDB/*/read",
        "Microsoft.DocumentDB/databaseAccounts/readonlykeys/action",
        "Microsoft.Insights/MetricDefinitions/read",
        "Microsoft.Insights/Metrics/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Account Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-operator"></a>Cosmos DB-Operator

Ermöglicht das Verwalten von Azure Cosmos DB-Konten, aber nicht das Zugreifen auf deren Daten. Verhindert den Zugriff auf Kontoschlüssel und Verbindungszeichenfolgen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.DocumentDb/databaseAccounts/* |  |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | **NotActions** |  |
> | Microsoft.DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Cosmos DB accounts, but not access data in them. Prevents access to account keys and connection strings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/230815da-be43-4aae-9cb4-875f7bd000aa",
  "name": "230815da-be43-4aae-9cb4-875f7bd000aa",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [
        "Microsoft.DocumentDB/databaseAccounts/readonlyKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/regenerateKey/*",
        "Microsoft.DocumentDB/databaseAccounts/listKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosbackupoperator"></a>CosmosBackupOperator

Kann eine Wiederherstellungsanforderung für eine Cosmos DB-Datenbank oder einen Container für ein Konto übermitteln.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | Sendet eine Anforderung zum Konfigurieren der Sicherung. |
> | Microsoft.DocumentDB/databaseAccounts/restore/action | Sendet eine Wiederherstellungsanforderung. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can submit restore request for a Cosmos DB database or a container for an account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "name": "db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/databaseAccounts/backup/action",
        "Microsoft.DocumentDB/databaseAccounts/restore/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosBackupOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="documentdb-account-contributor"></a>Mitwirkender von DocumentDB-Konto

Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.DocumentDb/databaseAccounts/* | Kann Azure Cosmos DB-Konten erstellen und verwalten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DocumentDB accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5bd9cd88-fe45-4216-938b-f97437e15450",
  "name": "5bd9cd88-fe45-4216-938b-f97437e15450",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DocumentDB Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="redis-cache-contributor"></a>Mitwirkender von Redis-Cache

Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Cache/redis/* | Erstellen und Verwalten von Redis-Caches |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Redis caches, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e0f68234-74aa-48ed-b826-c38b57376e17",
  "name": "e0f68234-74aa-48ed-b826-c38b57376e17",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cache/redis/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Redis Cache Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-db-contributor"></a>Mitwirkender von SQL DB

Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
> | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Insights/metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.Insights/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Überwachungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen bearbeiten |
> | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Verbindungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Datenmaskierungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen bearbeiten |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Sicherheitsmetriken bearbeiten |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL databases, but not access to them. Also, you can't manage their security-related policies or their parent SQL servers.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "name": "9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/databases/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL DB Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-managed-instance-contributor"></a>Verwaltete SQL-Instanz: Mitwirkender

Diese Rolle ermöglicht Ihnen das Verwalten verwalteter SQL-Instanzen und der erforderlichen Netzwerkkonfiguration, jedoch nicht das Erteilen des Zugriffs an andere.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Network/networkSecurityGroups/* |  |
> | Microsoft.Network/routeTables/* |  |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/managedInstances/* |  |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Network/virtualNetworks/subnets/* |  |
> | Microsoft.Network/virtualNetworks/* |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.Insights/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL Managed Instances and required network configuration, but can't give access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "name": "4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Network/networkSecurityGroups/*",
        "Microsoft.Network/routeTables/*",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/managedInstances/*",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/*",
        "Microsoft.Network/virtualNetworks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Managed Instance Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-security-manager"></a>SQL-Sicherheits-Manager

Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von SQL Server-Überwachungsrichtlinien |
> | Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server-Überwachungseinstellungen |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die erweiterte Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungseinstellungen von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von Verbindungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von Datenmaskierungsrichtlinien von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Dient zum Abrufen von Details zur erweiterten Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | Microsoft.Sql/servers/databases/read | Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab. |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | Ruft ein Datenbankschema ab. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Ruft eine Datenbankspalte ab. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Abrufen einer Datentabelle. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von Sicherheitsmetriken von SQL Server-Datenbanken |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the security-related policies of SQL servers and databases, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "name": "056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/transparentDataEncryption/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/read",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/read",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/transparentDataEncryption/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/firewallRules/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Security Manager",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-server-contributor"></a>Mitwirkender von SQL Server

Diese Rolle ermöglicht es Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL-Servern |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Insights/metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.Insights/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL Server-Überwachungsrichtlinien bearbeiten |
> | Microsoft.Sql/servers/auditingSettings/* | SQL Server-Überwachungseinstellungen bearbeiten |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Überwachungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Verbindungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Datenmaskierungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Sicherheitsmetriken von SQL Server-Datenbanken bearbeiten |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server bearbeiten |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL servers and databases, but not access to them, and not their security -related policies.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "name": "6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/*",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="analytics"></a>Analytics


### <a name="azure-event-hubs-data-owner"></a>Azure Event Hubs-Datenbesitzer

Ermöglicht den uneingeschränkten Zugriff auf die Azure Event Hubs-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.EventHub/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f526a384-b230-433a-b45c-95f59c4a2dec",
  "name": "f526a384-b230-433a-b45c-95f59c4a2dec",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-receiver"></a>Azure Event Hubs-Datenempfänger

Ermöglicht Empfängern den Zugriff auf die Azure Event Hubs-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.EventHub/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/receive/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows receive access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "name": "a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/consumergroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-sender"></a>Azure Event Hubs-Datensender

Ermöglicht Absendern den Zugriff auf die Azure Event Hubs-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.EventHub/*/eventhubs/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/send/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2b629674-e913-4c01-ae53-ef4638d8f975",
  "name": "2b629674-e913-4c01-ae53-ef4638d8f975",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-factory-contributor"></a>Mitwirkender von Data Factory

Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.DataFactory/dataFactories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | Microsoft.DataFactory/factories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.EventGrid/eventSubscriptions/write | Erstellt oder aktualisiert eventSubscription. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and manage data factories, as well as child resources within them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/673868aa-7521-48a0-acc6-0f60742d39f5",
  "name": "673868aa-7521-48a0-acc6-0f60742d39f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DataFactory/dataFactories/*",
        "Microsoft.DataFactory/factories/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.EventGrid/eventSubscriptions/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Factory Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-purger"></a>Datenpurger

Kann Analysedaten endgültig löschen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Daten werden aus Application Insights gelöscht |
> | Microsoft.OperationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.OperationalInsights/workspaces/purge/action | Löscht die angegebenen Daten aus dem Arbeitsbereich. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can purge analytics data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "name": "150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/components/*/read",
        "Microsoft.Insights/components/purge/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/purge/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Purger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-cluster-operator"></a>HDInsight-Clusteroperator

Ermöglicht Ihnen das Lesen und Ändern von HDInsight-Clusterkonfigurationen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.HDInsight/*/read |  |
> | Microsoft.HDInsight/clusters/getGatewaySettings/action | Ruft Gatewayeinstellungen für HDInsight-Cluster ab. |
> | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Aktualisiert Gatewayeinstellungen für HDInsight-Cluster. |
> | Microsoft.HDInsight/clusters/configurations/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and modify HDInsight cluster configurations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/61ed4efc-fab3-44fd-b111-e24485cc132a",
  "name": "61ed4efc-fab3-44fd-b111-e24485cc132a",
  "permissions": [
    {
      "actions": [
        "Microsoft.HDInsight/*/read",
        "Microsoft.HDInsight/clusters/getGatewaySettings/action",
        "Microsoft.HDInsight/clusters/updateGatewaySettings/action",
        "Microsoft.HDInsight/clusters/configurations/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Cluster Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-domain-services-contributor"></a>Mitwirkender für die HDInsight-Domänendienste

Ermöglicht Ihnen, Vorgänge im Zusammenhang mit Domänendiensten, die für das HDInsight Enterprise-Sicherheitspaket erforderlich sind, zu lesen, zu erstellen, zu ändern und zu löschen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.AAD/*/read |  |
> | Microsoft.AAD/domainServices/*/read |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can Read, Create, Modify and Delete Domain Services related operations needed for HDInsight Enterprise Security Package",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "name": "8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "permissions": [
    {
      "actions": [
        "Microsoft.AAD/*/read",
        "Microsoft.AAD/domainServices/*/read",
        "Microsoft.AAD/domainServices/oucontainer/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Domain Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-contributor"></a>Log Analytics-Mitwirkender

Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.HybridCompute/machines/extensions/write | Installiert oder aktualisiert eine Azure Arc-Erweiterung |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Contributor can read all monitoring data and edit monitoring settings. Editing monitoring settings includes adding the VM extension to VMs; reading storage account keys to be able to configure collection of logs from Azure Storage; creating and configuring Automation accounts; adding solutions; and configuring Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "name": "92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Automation/automationAccounts/*",
        "Microsoft.ClassicCompute/virtualMachines/extensions/*",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.Compute/virtualMachines/extensions/*",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/*",
        "Microsoft.OperationsManagement/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-reader"></a>Log Analytics-Leser

Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Reader can view and search all monitoring data as well as and view monitoring settings, including viewing the configuration of Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/73c42c96-874c-492b-b04d-ab87d138a893",
  "name": "73c42c96-874c-492b-b04d-ab87d138a893",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.OperationalInsights/workspaces/sharedKeys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="blockchain"></a>Blockchain


### <a name="blockchain-member-node-access-preview"></a>Zugriff auf Blockchainmitgliedsknoten (Vorschauversion)

Ermöglicht den Zugriff auf Blockchainmitgliedsknoten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Ruft die vorhandenen Transaktionsknoten eines Blockchainmitglieds ab oder listet sie auf |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Stellt eine Verbindung mit dem Transaktionsknoten eines Blockchainmitglieds her |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for access to Blockchain Member nodes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "name": "31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Blockchain Member Node Access (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="ai--machine-learning"></a>KI und Machine Learning


### <a name="cognitive-services-contributor"></a>Mitwirkender für Cognitive Services

Ermöglicht Ihnen das Erstellen, Lesen, Aktualisieren, Löschen und Verwalten von Cognitive Services-Schlüsseln.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Features/features/read | Ruft die Features eines Abonnements ab. |
> | Microsoft.Features/providers/features/read | Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.Insights/logDefinitions/read | Dient zum Lesen von Protokolldefinitionen. |
> | Microsoft.Insights/metricdefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | Microsoft.Insights/metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, read, update, delete and manage keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "name": "25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.CognitiveServices/*",
        "Microsoft.Features/features/read",
        "Microsoft.Features/providers/features/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-data-reader-preview"></a>Cognitive Services-Datenleser (Vorschau)

Ermöglicht das Lesen von Cognitive Services-Daten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read Cognitive Services data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "name": "b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-user"></a>Cognitive Services-Benutzer

Ermöglicht Ihnen das Lesen und Auflisten von Cognitive Services-Schlüsseln.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Dient zum Auflisten von Schlüsseln. |
> | Microsoft.Insights/alertRules/read | Liest eine klassische Metrikwarnung. |
> | Microsoft.Insights/diagnosticSettings/read | Liest eine Diagnoseeinstellung für eine Ressource. |
> | Microsoft.Insights/logDefinitions/read | Dient zum Lesen von Protokolldefinitionen. |
> | Microsoft.Insights/metricdefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | Microsoft.Insights/metrics/read | Dient zum Lesen von Metriken. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and list keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a97b65f3-24c7-4388-baec-2e87135dc908",
  "name": "a97b65f3-24c7-4388-baec-2e87135dc908",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.CognitiveServices/accounts/listkeys/action",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Insights/diagnosticSettings/read",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="mixed-reality"></a>Mixed Reality


### <a name="spatial-anchors-account-contributor"></a>Spatial Anchors-Kontomitwirkender

Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, nicht jedoch das Löschen von Ankern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Hiermit werden Raumanker erstellt. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Hiermit aktualisieren Sie die Eigenschaften von Raumankern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, but not delete them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "name": "8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-owner"></a>Spatial Anchors-Kontobesitzer

Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, einschließlich der Löschung von Ankern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Hiermit werden Raumanker erstellt. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Hiermit werden Raumanker gelöscht. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Hiermit aktualisieren Sie die Eigenschaften von Raumankern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, including deleting them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/70bbe301-9835-447d-afdd-19eb3167307c",
  "name": "70bbe301-9835-447d-afdd-19eb3167307c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/delete",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-reader"></a>Spatial Anchors-Kontoleser

Ermöglicht Ihnen das Ermitteln und Lesen von Eigenschaften für Raumanker in Ihrem Dokument.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you locate and read properties of spatial anchors in your account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "name": "5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="integration"></a>Integration


### <a name="api-management-service-contributor"></a>Mitwirkender des API-Verwaltungsdienstes

Kann Dienst und APIs verwalten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/* | Erstellen und Verwalten des API Management-Diensts |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service and the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c",
  "name": "312a565d-c81f-4fd8-895a-4e21e48d571c",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-operator-role"></a>Operatorrolle des API Management-Diensts

Kann den Dienst, aber nicht die APIs verwalten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | Microsoft.ApiManagement/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Microsoft.ApiManagement/service/delete | Dient zum Löschen einer API Management-Dienstinstanz. |
> | Microsoft.ApiManagement/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts. |
> | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Microsoft.ApiManagement/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | Microsoft.ApiManagement/service/updatecertificate/action | Dient zum Hochladen eines TLS/SSL-Zertifikats für einen API Management-Dienst |
> | Microsoft.ApiManagement/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
> | Microsoft.ApiManagement/service/write | Erstellen oder Aktualisieren einer API Management-Dienstinstanz |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Abrufen von Benutzern zugeordneten Schlüsseln |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service but not the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "name": "e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/backup/action",
        "Microsoft.ApiManagement/service/delete",
        "Microsoft.ApiManagement/service/managedeployments/action",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.ApiManagement/service/restore/action",
        "Microsoft.ApiManagement/service/updatecertificate/action",
        "Microsoft.ApiManagement/service/updatehostname/action",
        "Microsoft.ApiManagement/service/write",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-reader-role"></a>Leserrolle des API Management-Diensts

Schreibgeschützter Zugriff auf Dienst und APIs

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ApiManagement/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | Microsoft.ApiManagement/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Abrufen von Benutzern zugeordneten Schlüsseln |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to service and APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/71522526-b88f-4d52-b57f-d31fc3546d0d",
  "name": "71522526-b88f-4d52-b57f-d31fc3546d0d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-owner"></a>App Configuration-Datenbesitzer

Ermöglicht den Vollzugriff auf App Configuration-Daten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/read |  |
> | Microsoft.AppConfiguration/configurationStores/*/write |  |
> | Microsoft.AppConfiguration/configurationStores/*/delete |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows full access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "name": "5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read",
        "Microsoft.AppConfiguration/configurationStores/*/write",
        "Microsoft.AppConfiguration/configurationStores/*/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-reader"></a>App Configuration-Datenleser

Ermöglicht den Lesezugriff auf App Configuration-Daten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-4d78-a4de-a74fb236a071",
  "name": "516239f1-63e1-4d78-a4de-a74fb236a071",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-owner"></a>Azure Service Bus-Datenbesitzer

Ermöglicht den uneingeschränkten Zugriff auf die Azure Service Bus-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ServiceBus/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/090c5cfd-751d-490a-894a-3ce6f1109419",
  "name": "090c5cfd-751d-490a-894a-3ce6f1109419",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-receiver"></a>Azure Service Bus-Datenempfänger

Ermöglicht Empfängern den Zugriff auf die Azure Service Bus-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ServiceBus/*/queues/read |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/receive/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for receive access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "name": "4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-sender"></a>Azure Service Bus-Datensender

Ermöglicht Absendern den Zugriff auf die Azure Service Bus-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ServiceBus/*/queues/read |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/send/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "name": "69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-stack-registration-owner"></a>Besitzer der Azure Stack-Registrierung

Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.AzureStack/registrations/products/*/action |  |
> | Microsoft.AzureStack/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
> | Microsoft.AzureStack/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Stack registrations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "name": "6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureStack/registrations/products/*/action",
        "Microsoft.AzureStack/registrations/products/read",
        "Microsoft.AzureStack/registrations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Stack Registration Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription-Mitwirkender

Ermöglicht die Verwaltung von EventGrid-Ereignisabonnementvorgängen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Listet globale Ereignisabonnements nach Thematyp auf. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Listet regionale Ereignisabonnements auf. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Listet regionale Ereignisabonnements nach Thematyp auf. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid event subscription operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "name": "428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/*",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription-Leser

Ermöglicht das Lesen von EventGrid-Ereignisabonnements.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.EventGrid/eventSubscriptions/read | Liest eventSubscription. |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Listet globale Ereignisabonnements nach Thematyp auf. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Listet regionale Ereignisabonnements auf. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Listet regionale Ereignisabonnements nach Thematyp auf. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read EventGrid event subscriptions.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2414bbcf-6497-4faf-8c65-045460748405",
  "name": "2414bbcf-6497-4faf-8c65-045460748405",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="intelligent-systems-account-contributor"></a>Mitwirkender von Intelligent Systems-Konto

Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Intelligent Systems-Konten |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Intelligent Systems accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/03a6d094-3444-4b3d-88af-7477090a9e5e",
  "name": "03a6d094-3444-4b3d-88af-7477090a9e5e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.IntelligentSystems/accounts/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Intelligent Systems Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-contributor"></a>Mitwirkender für Logik-Apps

Ermöglicht Ihnen die Verwaltung von Logik-Apps. Sie können aber nicht den App-Zugriff ändern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | Microsoft.ClassicStorage/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/metricAlerts/* |  |
> | Microsoft.Insights/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.Insights/logdefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | Microsoft.Insights/metricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | Microsoft.Logic/* | Verwaltet Logic Apps-Ressourcen. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Web/connectionGateways/* | Erstellt und verwaltet ein Verbindungsgateway. |
> | Microsoft.Web/connections/* | Erstellt und verwaltet eine Verbindung. |
> | Microsoft.Web/customApis/* | Erstellt und verwaltet eine benutzerdefinierte API. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | Microsoft.Web/sites/functions/listSecrets/action | Auflisten von Funktionsgeheimnissen. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage logic app, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "name": "87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logdefinitions/*",
        "Microsoft.Insights/metricDefinitions/*",
        "Microsoft.Logic/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*",
        "Microsoft.Web/connections/*",
        "Microsoft.Web/customApis/*",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/functions/listSecrets/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-operator"></a>Logik-App-Operator

Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. Sie können diese aber nicht bearbeiten oder aktualisieren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/*/read | Lesen von Insights-Warnungsregeln |
> | Microsoft.Insights/metricAlerts/*/read |  |
> | Microsoft.Insights/diagnosticSettings/*/read | Ruft die Diagnoseeinstellungen für Logic Apps ab. |
> | Microsoft.Insights/metricDefinitions/*/read | Ruft die verfügbaren Metriken für Logic Apps ab. |
> | Microsoft.Logic/*/read | Liest Logic Apps-Ressourcen. |
> | Microsoft.Logic/workflows/disable/action | Deaktiviert den Workflow. |
> | Microsoft.Logic/workflows/enable/action | Aktiviert den Workflow. |
> | Microsoft.Logic/workflows/validate/action | Überprüft den Workflow. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Web/connectionGateways/*/read | Liest Verbindungsgateways. |
> | Microsoft.Web/connections/*/read | Liest Verbindungen. |
> | Microsoft.Web/customApis/*/read | Liest benutzerdefinierte API. |
> | Microsoft.Web/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read, enable and disable logic app.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "name": "515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*/read",
        "Microsoft.Insights/metricAlerts/*/read",
        "Microsoft.Insights/diagnosticSettings/*/read",
        "Microsoft.Insights/metricDefinitions/*/read",
        "Microsoft.Logic/*/read",
        "Microsoft.Logic/workflows/disable/action",
        "Microsoft.Logic/workflows/enable/action",
        "Microsoft.Logic/workflows/validate/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*/read",
        "Microsoft.Web/connections/*/read",
        "Microsoft.Web/customApis/*/read",
        "Microsoft.Web/serverFarms/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="identity"></a>Identity


### <a name="managed-identity-contributor"></a>Mitwirkender für verwaltete Identität

Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/read | Ruft eine vorhandene zugewiesene Benutzeridentität ab. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/write | Erstellt eine neue zugewiesene Benutzeridentität oder aktualisiert die Markierungen, die einer vorhandenen zugewiesenen Benutzeridentität zugeordnet sind. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Löscht eine vorhandene zugewiesene Benutzeridentität. |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "name": "e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/write",
        "Microsoft.ManagedIdentity/userAssignedIdentities/delete",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-identity-operator"></a>Operator für verwaltete Identität

Dem Benutzer zugewiesene Identität lesen und zuweisen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and Assign User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f1a07417-d97a-45cb-824c-7a7467783830",
  "name": "f1a07417-d97a-45cb-824c-7a7467783830",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="security"></a>Sicherheit


### <a name="azure-sentinel-contributor"></a>Azure Sentinel-Mitwirkender

Azure Sentinel-Mitwirkender

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.SecurityInsights/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Microsoft.OperationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* |  |
> | Microsoft.OperationsManagement/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | Microsoft.OperationalInsights/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Microsoft.Insights/workbooks/* |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ab8e14d6-4a74-4a29-9ba8-549422addade",
  "name": "ab8e14d6-4a74-4a29-9ba8-549422addade",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-reader"></a>Azure Sentinel-Leser

Azure Sentinel-Leser

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.SecurityInsights/*/read |  |
> | Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action | Überprüfen der Benutzerautorisierung und -lizenz |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Microsoft.OperationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.OperationalInsights/workspaces/LinkedServices/read | Ruft verknüpfte Dienste im angegebenen Arbeitsbereich ab. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | Microsoft.OperationsManagement/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | Microsoft.OperationalInsights/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Microsoft.Insights/workbooks/read | Lesen einer Arbeitsmappe |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "name": "8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/LinkedServices/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-responder"></a>Azure Sentinel-Antwortender

Azure Sentinel-Antwortender

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.SecurityInsights/*/read |  |
> | Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action | Überprüfen der Benutzerautorisierung und -lizenz |
> | Microsoft.SecurityInsights/cases/* |  |
> | Microsoft.SecurityInsights/incidents/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | Microsoft.OperationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | Microsoft.OperationsManagement/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | Microsoft.OperationalInsights/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | Microsoft.Insights/workbooks/read | Lesen einer Arbeitsmappe |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Responder",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "name": "3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/cases/*",
        "Microsoft.SecurityInsights/incidents/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Responder",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-contributor"></a>Key Vault-Mitwirkender

Ermöglicht Ihnen die Verwaltung von Schlüsseltresoren, aber nicht den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage key vaults, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f25e0fa2-a7c8-4377-a976-54943a77a395",
  "name": "f25e0fa2-a7c8-4377-a976-54943a77a395",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.KeyVault/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.KeyVault/locations/deletedVaults/purge/action",
        "Microsoft.KeyVault/hsmPools/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-admin"></a>Sicherheitsadministrator

Anzeigen und Aktualisieren von Berechtigungen für Security Center. Gleiche Rechte wie der Sicherheitsleseberechtigte und kann darüber hinaus die Sicherheitsrichtlinie aktualisieren sowie Warnungen und Empfehlungen verwerfen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Authorization/policyAssignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
> | Microsoft.Authorization/policyDefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
> | Microsoft.Authorization/policySetDefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Admin Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "name": "fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Authorization/policyAssignments/*",
        "Microsoft.Authorization/policyDefinitions/*",
        "Microsoft.Authorization/policySetDefinitions/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-assessment-contributor"></a>Mitwirkender für Sicherheitsbewertungen

Ermöglicht das Pushen von Bewertungen an Security Center

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Security/assessments/write | Erstellen oder Aktualisieren von Sicherheitsbewertungen für Ihr Abonnement |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you push assessments to Security Center",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "name": "612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Security/assessments/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Assessment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-manager-legacy"></a>Sicherheits-Manager (Legacy)

Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.ClassicCompute/*/read | Lesen von Konfigurationsinformationen zu klassischen virtuellen Computern |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Schreiben der Konfiguration für klassische virtuelle Computer |
> | Microsoft.ClassicNetwork/*/read | Lesen von Konfigurationsinformationen zu klassischem Netzwerk |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "This is a legacy role. Please use Security Administrator instead",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "name": "e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/*/read",
        "Microsoft.ClassicCompute/virtualMachines/*/write",
        "Microsoft.ClassicNetwork/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Manager (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-reader"></a>Sicherheitsleseberechtigter

Anzeigen von Berechtigungen für Security Center. Kann Empfehlungen, Warnungen, Sicherheitsrichtlinien und Sicherheitszustände anzeigen, jedoch keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.operationalInsights/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Security/*/read | Lesen von Sicherheitskomponenten und -richtlinien |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "name": "39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*/read",
        "Microsoft.Support/*",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="devops"></a>DevOps


### <a name="devtest-labs-user"></a>DevTest Labs-Benutzer

Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Compute/availabilitySets/read | Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe. |
> | Microsoft.Compute/virtualMachines/*/read | Lesen der Eigenschaften eines virtuellen Computers (VM-Größen, Laufzeitstatus, VM-Erweiterungen usw.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei. |
> | Microsoft.Compute/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | Microsoft.Compute/virtualMachines/restart/action | Startet den virtuellen Computer neu. |
> | Microsoft.Compute/virtualMachines/start/action | Startet den virtuellen Computer. |
> | Microsoft.DevTestLab/*/read | Lesen der Eigenschaften eines Labs |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Dient zum Erstellen von virtuellen Computern in einem Lab. |
> | Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action | Stellt sicher, dass der aktuelle Benutzer über ein gültiges Profil im Lab verfügt |
> | Microsoft.DevTestLab/labs/formulas/delete | Dient zum Löschen von Formeln. |
> | Microsoft.DevTestLab/labs/formulas/read | Dient zum Lesen von Formeln. |
> | Microsoft.DevTestLab/labs/formulas/write | Dient zum Hinzufügen oder Ändern von Formeln. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Wertet Labrichtlinien aus. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | Listet die anwendbaren Zeitpläne zum Starten/Beenden auf, sofern vorhanden. |
> | Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action | Ruft eine Zeichenfolge ab, die den Inhalt der RDP-Datei für den virtuellen Computer darstellt. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. Nicht warnbar. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpft eine NAT-Regel für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | Microsoft.Network/networkInterfaces/*/read | Lesen der Eigenschaften einer Netzwerkschnittstelle (z.B. alle Load Balancer, zu denen die Netzwerkschnittstelle gehört) |
> | Microsoft.Network/networkInterfaces/join/action | Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle. Nicht warnbar. |
> | Microsoft.Network/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | Microsoft.Network/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | Microsoft.Network/publicIPAddresses/*/read | Lesen der Eigenschaften einer öffentlichen IP-Adresse |
> | Microsoft.Network/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. Nicht warnbar. |
> | Microsoft.Network/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | Microsoft.Resources/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | Microsoft.Resources/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76283e04-6283-4c54-8f91-bcf1374a3c64",
  "name": "76283e04-6283-4c54-8f91-bcf1374a3c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.DevTestLab/*/read",
        "Microsoft.DevTestLab/labs/claimAnyVm/action",
        "Microsoft.DevTestLab/labs/createEnvironment/action",
        "Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action",
        "Microsoft.DevTestLab/labs/formulas/delete",
        "Microsoft.DevTestLab/labs/formulas/read",
        "Microsoft.DevTestLab/labs/formulas/write",
        "Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action",
        "Microsoft.DevTestLab/labs/virtualMachines/claim/action",
        "Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action",
        "Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/networkInterfaces/join/action",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/publicIPAddresses/*/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listKeys/action"
      ],
      "notActions": [
        "Microsoft.Compute/virtualMachines/vmSizes/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DevTest Labs User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="lab-creator"></a>Lab-Ersteller

Ermöglicht Ihnen das Erstellen, Verwalten und Löschen verwalteter Labs unter Ihren Azure Lab-Konten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | Microsoft.LabServices/labAccounts/sizes/GetRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Hiermit werden Informationen zur regionalen Verfügbarkeit jeder Größenkategorie abgerufen, die in einem Labkonto konfiguriert sind. |
> | Microsoft.LabServices/labAccounts/getPricingAndAvailability/action | Ruft die Preise und die Verfügbarkeit von Kombinationen aus Größen, Regionen und Betriebssystemen für das Labkonto ab. |
> | Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action | Ruft Kerneinschränkungen und Nutzungsdaten für dieses Abonnement ab |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, manage, delete your managed labs under your Azure Lab Accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "name": "b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.LabServices/labAccounts/*/read",
        "Microsoft.LabServices/labAccounts/createLab/action",
        "Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getPricingAndAvailability/action",
        "Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Lab Creator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="monitor"></a>Überwachen


### <a name="application-insights-component-contributor"></a>Mitwirkender der Application Insights-Komponente

Kann Application Insights-Komponenten verwalten

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten von klassischen Netzwerkregeln |
> | Microsoft.Insights/metricAlerts/* | Erstellen und Verwalten von neuen Netzwerkregeln |
> | Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | Microsoft.Insights/webtests/* | Erstellen und Verwalten von Insights-Webtests |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage Application Insights components",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e",
  "name": "ae349356-3a1b-4a5e-921d-050484c6347e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Component Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="application-insights-snapshot-debugger"></a>Application Insights-Momentaufnahmedebugger

Gibt dem Benutzer die Berechtigung zum Anzeigen und Herunterladen von Debugmomentaufnahmen, die mit dem Application Insights-Momentaufnahmedebugger erfasst wurden. Beachten Sie, dass diese Berechtigungen in der Rolle [Besitzer](#owner) oder [Mitwirkender](#contributor) nicht enthalten sind. Die Application Insights-Rolle „Momentaufnahmedebugger“ muss Benutzern direkt zugewiesen werden. Die Rolle wird nicht erkannt, wenn sie einer benutzerdefinierten Rolle hinzugefügt wird. 

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives user permission to use Application Insights Snapshot Debugger features",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "name": "08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Snapshot Debugger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-contributor"></a>Überwachungsmitwirkender

Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | Microsoft.Insights/DiagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | Microsoft.Insights/eventtypes/* | Auflisten von Aktivitätsprotokollereignissen (Verwaltungsereignissen) in einem Abonnement. Diese Berechtigung gilt sowohl für den programmgesteuerten als auch für den Portalzugriff auf das Aktivitätsprotokoll. |
> | Microsoft.Insights/LogDefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | Microsoft.Insights/Metrics/* | Lesen von Metriken für eine Ressource. |
> | Microsoft.Insights/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Erstellen und Verwalten von Insights-Webtests |
> | Microsoft.Insights/workbooks/* |  |
> | Microsoft.Insights/privateLinkScopes/* |  |
> | Microsoft.Insights/privateLinkScopeOperationStatuses/* |  |
> | Microsoft.OperationalInsights/workspaces/write | Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird. |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lesen/Schreiben/Löschen von Log Analytics-Lösungspaketen. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Lesen/Schreiben/Löschen von gespeicherten Log Analytics-Suchvorgängen. |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lesen/Schreiben/Löschen von Log Analytics-Speicherdetailinformationen. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | Microsoft.AlertsManagement/smartDetectorAlertRules/* |  |
> | Microsoft.AlertsManagement/actionRules/* |  |
> | Microsoft.AlertsManagement/smartGroups/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data and update monitoring settings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "name": "749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.AlertsManagement/alerts/*",
        "Microsoft.AlertsManagement/alertsSummary/*",
        "Microsoft.Insights/actiongroups/*",
        "Microsoft.Insights/activityLogAlerts/*",
        "Microsoft.Insights/AlertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/DiagnosticSettings/*",
        "Microsoft.Insights/eventtypes/*",
        "Microsoft.Insights/LogDefinitions/*",
        "Microsoft.Insights/metricalerts/*",
        "Microsoft.Insights/MetricDefinitions/*",
        "Microsoft.Insights/Metrics/*",
        "Microsoft.Insights/Register/Action",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/privateLinkScopes/*",
        "Microsoft.Insights/privateLinkScopeOperationStatuses/*",
        "Microsoft.OperationalInsights/workspaces/write",
        "Microsoft.OperationalInsights/workspaces/intelligencepacks/*",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action",
        "Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*",
        "Microsoft.Support/*",
        "Microsoft.WorkloadMonitor/monitors/*",
        "Microsoft.WorkloadMonitor/notificationSettings/*",
        "Microsoft.AlertsManagement/smartDetectorAlertRules/*",
        "Microsoft.AlertsManagement/actionRules/*",
        "Microsoft.AlertsManagement/smartGroups/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-metrics-publisher"></a>Herausgeber von Überwachungsmetriken

Ermöglicht die Veröffentlichung von Metriken für Azure-Ressourcen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Insights/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Hiermit werden Metriken geschrieben. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Enables publishing metrics against Azure resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c390055eb",
  "name": "3913510d-42f4-4e42-8a64-420c390055eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/Register/Action",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Insights/Metrics/Write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Metrics Publisher",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-reader"></a>Überwachungsleser

Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.OperationalInsights/workspaces/search/action | Führt eine Suchabfrage aus. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "name": "43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-contributor"></a>Arbeitsmappenmitwirkender

Kann freigegebene Arbeitsmappen speichern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Insights/workbooks/write | Erstellen oder Aktualisieren einer Arbeitsmappe |
> | Microsoft.Insights/workbooks/delete | Löschen einer Arbeitsmappe |
> | Microsoft.Insights/workbooks/read | Lesen einer Arbeitsmappe |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can save shared workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "name": "e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/workbooks/write",
        "Microsoft.Insights/workbooks/delete",
        "Microsoft.Insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-reader"></a>Arbeitsmappenleser

Kann Arbeitsmappen lesen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | microsoft.insights/workbooks/read | Lesen einer Arbeitsmappe |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "name": "b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "permissions": [
    {
      "actions": [
        "microsoft.insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="management--governance"></a>Verwaltung + Governance


### <a name="automation-job-operator"></a>Automation-Auftragsoperator

Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and Manage Jobs using Automation Runbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "name": "4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Job Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-operator"></a>Operator für Automation

Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | Microsoft.Automation/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | Microsoft.Automation/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Ruft einen Azure Automation-Auftragszeitplan ab. |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Erstellt einen Azure Automation-Auftragszeitplan. |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist. |
> | Microsoft.Automation/automationAccounts/read | Ruft ein Azure Automation-Konto ab. |
> | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Microsoft.Automation/automationAccounts/schedules/read | Ruft ein Azure Automation-Zeitplanasset ab. |
> | Microsoft.Automation/automationAccounts/schedules/write | Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Automation Operators are able to start, stop, suspend, and resume jobs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d3881f73-407a-4167-8283-e981cbba0404",
  "name": "d3881f73-407a-4167-8283-e981cbba0404",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobSchedules/read",
        "Microsoft.Automation/automationAccounts/jobSchedules/write",
        "Microsoft.Automation/automationAccounts/linkedWorkspace/read",
        "Microsoft.Automation/automationAccounts/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Automation/automationAccounts/schedules/read",
        "Microsoft.Automation/automationAccounts/schedules/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-runbook-operator"></a>Automation-Runbookoperator

Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Automation/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read Runbook properties - to be able to create Jobs of the runbook.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "name": "5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Runbook Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-onboarding"></a>Onboarding von Azure Connected Machine

Kann Azure Connected Machine integrieren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.HybridCompute/machines/read | Liest einen beliebigen Azure Arc-Computer. |
> | Microsoft.HybridCompute/machines/write | Schreibt auf einem Azure Arc-Computer |
> | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Abrufen der Zuweisung der Gastkonfiguration |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "name": "b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.GuestConfiguration/guestConfigurationAssignments/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-resource-administrator"></a>Ressourcenadministrator für Azure Connected Machine

Kann Azure Connected Machines lesen, schreiben, löschen und erneut integrieren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.HybridCompute/machines/read | Liest einen beliebigen Azure Arc-Computer. |
> | Microsoft.HybridCompute/machines/write | Schreibt auf einem Azure Arc-Computer |
> | Microsoft.HybridCompute/machines/delete | Löscht einen Azure Arc-Computer |
> | Microsoft.HybridCompute/machines/reconnect/action | Verbindet einen Azure Arc-Computer erneut |
> | Microsoft.HybridCompute/machines/extensions/write | Installiert oder aktualisiert eine Azure Arc-Erweiterung |
> | Microsoft.HybridCompute/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read, write, delete and re-onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cd570a14-e51a-42ad-bac8-bafd67325302",
  "name": "cd570a14-e51a-42ad-bac8-bafd67325302",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/machines/delete",
        "Microsoft.HybridCompute/machines/reconnect/action",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.HybridCompute/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Resource Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="billing-reader"></a>Abrechnungsleser

Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Billing/*/read | Lesen von Abrechnungsinformationen |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to billing data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "name": "fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Billing/*/read",
        "Microsoft.Commerce/*/read",
        "Microsoft.Consumption/*/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Billing Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-contributor"></a>Blueprint-Mitwirkender

Kann Blaupausendefinitionen verwalten, aber nicht zuweisen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Blueprint/blueprints/* | Erstellen und Verwalten von Blaupausendefinitionen oder -artefakten |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage blueprint definitions, but not assign them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/41077137-e803-4205-871c-5a86e6a753b4",
  "name": "41077137-e803-4205-871c-5a86e6a753b4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprints/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-operator"></a>Blueprint-Operator

Kann vorhandene veröffentlichte Blaupausen zuweisen, aber keine neuen Blaupausen erstellen. Beachten Sie, dass dies nur funktioniert, wenn die Zuweisung mit einer vom Benutzer zugewiesenen verwalteten Identität erfolgt.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Blueprint/blueprintAssignments/* | Erstellen und Verwalten von Blaupausenzuweisungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can assign existing published blueprints, but cannot create new blueprints. NOTE: this only works if the assignment is done with a user-assigned managed identity.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/437d2ced-4a38-4302-8479-ed2bcb43d090",
  "name": "437d2ced-4a38-4302-8479-ed2bcb43d090",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprintAssignments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-contributor"></a>Mitwirkender für Cost Management

Ermöglicht Ihnen das Anzeigen der Kosten und das Verwalten der Kostenkonfiguration (z. B. Budgets, Exporte).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Advisor/configurations/read | Konfigurationen abrufen |
> | Microsoft.Advisor/recommendations/read | Liest Empfehlungen. |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view costs and manage cost configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/434105ed-43f6-45c7-a02f-909b2ba83430",
  "name": "434105ed-43f6-45c7-a02f-909b2ba83430",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*",
        "Microsoft.CostManagement/*",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-reader"></a>Cost Management-Leser

Ermöglicht Ihnen das Anzeigen der Kostendaten und -konfiguration (z. B. Budgets, Exporte).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | Ruft die Abonnementliste ab. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Advisor/configurations/read | Konfigurationen abrufen |
> | Microsoft.Advisor/recommendations/read | Liest Empfehlungen. |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view cost data and configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/72fafb9e-0641-4937-9268-a91bfd8191a3",
  "name": "72fafb9e-0641-4937-9268-a91bfd8191a3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hierarchy-settings-administrator"></a>Hierarchieeinstellungsadministrator

Ermöglicht Benutzern das Bearbeiten und Löschen von Hierarchieeinstellungen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Management/managementGroups/settings/write | Erstellt oder aktualisiert Hierarchieeinstellungen einer Verwaltungsgruppe. |
> | Microsoft.Management/managementGroups/settings/delete | Löscht Hierarchieeinstellungen einer Verwaltungsgruppe. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows users to edit and delete Hierarchy Settings",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/350f8d15-c687-4448-8ae1-157740a3936d",
  "name": "350f8d15-c687-4448-8ae1-157740a3936d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/settings/write",
        "Microsoft.Management/managementGroups/settings/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Hierarchy Settings Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-contributor-role"></a>Rolle „Mitwirkender für verwaltete Anwendungen“

Ermöglicht das Erstellen von Ressourcen für verwaltete Anwendungen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Solutions/applications/* |  |
> | Microsoft.Solutions/register/action | Hiermit registrieren Sie sich bei Lösungen. |
> | Microsoft.Resources/subscriptions/resourceGroups/* |  |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for creating managed application resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/641177b8-a67a-45b9-a033-47bc880bb21e",
  "name": "641177b8-a67a-45b9-a033-47bc880bb21e",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/*",
        "Microsoft.Solutions/register/action",
        "Microsoft.Resources/subscriptions/resourceGroups/*",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-operator-role"></a>Rolle „Bediener für verwaltete Anwendung“

Ermöglicht Ihnen das Lesen und Durchführen von Aktionen für Ressourcen der verwalteten Anwendung.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Solutions/applications/read | Hiermit wird eine Liste mit Anwendungen abgerufen. |
> | Microsoft.Solutions/*/action |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and perform actions on Managed Application resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "name": "c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/read",
        "Microsoft.Solutions/*/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-applications-reader"></a>Leser für verwaltete Anwendungen

Ermöglicht Ihnen, Ressourcen in einer verwalteten App zu lesen und JIT-Zugriff anzufordern.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read resources in a managed app and request JIT access.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "name": "b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Solutions/jitRequests/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Applications Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-services-registration-assignment-delete-role"></a>Rolle „Registrierungszuweisung für verwaltete Dienste löschen“

Mit der Rolle „Registrierungszuweisung für verwaltete Dienste löschen“ können Benutzer des verwaltenden Mandanten die ihrem Mandanten zugewiesene Registrierungszuweisung löschen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.ManagedServices/registrationAssignments/read | Ruft eine Liste der Registrierungszuweisungen für verwaltete Dienste ab. |
> | Microsoft.ManagedServices/registrationAssignments/delete | Entfernt die Registrierungszuweisung für verwaltete Dienste. |
> | Microsoft.ManagedServices/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Managed Services Registration Assignment Delete Role allows the managing tenant users to delete the registration assignment assigned to their tenant.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/91c1777a-f3dc-4fae-b103-61d183457e46",
  "name": "91c1777a-f3dc-4fae-b103-61d183457e46",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedServices/registrationAssignments/read",
        "Microsoft.ManagedServices/registrationAssignments/delete",
        "Microsoft.ManagedServices/operationStatuses/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Services Registration assignment Delete Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-contributor"></a>Verwaltungsgruppenmitwirkender

Rolle „Verwaltungsgruppenmitwirkender“

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Management/managementGroups/delete | Löscht eine Verwaltungsgruppe. |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Hebt die Zuordnung des Abonnements mit der Verwaltungsgruppe auf. |
> | Microsoft.Management/managementGroups/subscriptions/write | Ordnet ein vorhandenes Abonnement der Verwaltungsgruppe zu. |
> | Microsoft.Management/managementGroups/write | Erstellt oder aktualisiert eine Verwaltungsgruppe. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Contributor Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "name": "5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/delete",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/delete",
        "Microsoft.Management/managementGroups/subscriptions/write",
        "Microsoft.Management/managementGroups/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-reader"></a>Verwaltungsgruppenleser

Rolle „Verwaltungsgruppenleser“

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Management/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ac63b705-f282-497d-ac71-919bf39d939d",
  "name": "ac63b705-f282-497d-ac71-919bf39d939d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="new-relic-apm-account-contributor"></a>Mitwirkender von New Relic APM-Konto

Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage New Relic Application Performance Management accounts and applications, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d28c62d-5b37-4476-8438-e587778df237",
  "name": "5d28c62d-5b37-4476-8438-e587778df237",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "NewRelic.APM/accounts/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "New Relic APM Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="policy-insights-data-writer-preview"></a>Policy Insights-Datenschreiber (Vorschau)

Ermöglicht Lesezugriff auf Ressourcenrichtlinien und Schreibzugriff auf Richtlinienereignisse für Ressourcenkomponenten.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/policyassignments/read | Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung. |
> | Microsoft.Authorization/policydefinitions/read | Dient zum Abrufen von Informationen zu einer Richtliniendefinition. |
> | Microsoft.Authorization/policysetdefinitions/read | Ruft Informationen zu einer Richtliniengruppendefinition ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.PolicyInsights/checkDataPolicyCompliance/action | Überprüft den Compliancestatus einer angegebenen Komponente anhand von Datenrichtlinien. |
> | Microsoft.PolicyInsights/policyEvents/logDataEvents/action | Protokollieren der Ressourcenkomponenten-Richtlinienereignisse. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to resource policies and write access to resource component policy events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "name": "66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/policyassignments/read",
        "Microsoft.Authorization/policydefinitions/read",
        "Microsoft.Authorization/policysetdefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.PolicyInsights/checkDataPolicyCompliance/action",
        "Microsoft.PolicyInsights/policyEvents/logDataEvents/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Policy Insights Data Writer (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="resource-policy-contributor"></a>Ressourcenrichtlinienmitwirkender

Benutzer mit Rechten zum Erstellen/Ändern der Ressourcenrichtlinie, zum Erstellen eines Supporttickets und zum Lesen von Ressourcen/der Hierarchie.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | Microsoft.Authorization/policyassignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
> | Microsoft.Authorization/policydefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
> | Microsoft.Authorization/policysetdefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Users with rights to create/modify resource policy, create support ticket and read resources/hierarchy.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/36243c78-bf99-498c-9df9-86d9f8d28608",
  "name": "36243c78-bf99-498c-9df9-86d9f8d28608",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/policyassignments/*",
        "Microsoft.Authorization/policydefinitions/*",
        "Microsoft.Authorization/policysetdefinitions/*",
        "Microsoft.PolicyInsights/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Resource Policy Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-contributor"></a>Site Recovery-Mitwirkender

Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Erstellen oder Aktualisieren von Einstellungen für Replikationswarnungen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Erstellen und Verwalten von Replikationsfabrics |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Erstellen und Verwalten von Replikationsrichtlinien |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Erstellen und Verwalten von Wiederherstellungsplänen |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Erstellen und Verwalten der Speicherkonfiguration des Recovery Services-Tresors |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.RecoveryServices/vaults/replicationOperationStatus/read | Liest alle Status des Tresorreplikationsvorgangs |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Site Recovery service except vault creation and role assignment",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "name": "6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/*",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/*",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/*",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/*",
        "Microsoft.RecoveryServices/Vaults/storageConfig/*",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/vaults/replicationOperationStatus/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-operator"></a>Site Recovery-Operator

Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Network/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Prüft die Konsistenz des Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Dient zum Neuzuordnen des Gateways. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Verlängert das Zertifikat für Fabrics. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Dient zum erneuten Schützen geschützter Elemente. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Dient zum Wechseln von Schutzcontainern. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Dient zum Aktualisieren des Anbieters. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Wiederherstellungsplan für Failovercommit |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Wiederherstellungsplan für geplantes Failover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Wiederherstellungsplan für erneutes Schützen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Wiederherstellungsplan für Testfailover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Wiederherstellungsplan für Testfailoverbereinigung |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Wiederherstellungsplan für Failover |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Storage/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you failover and failback but not perform other Site Recovery management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494ae006-db33-4328-bf46-533a6560a3ca",
  "name": "494ae006-db33-4328-bf46-533a6560a3ca",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-reader"></a>Site Recovery-Leser

Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Dient zum Lesen beliebiger Aufträge |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view Site Recovery status but not perform other management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "name": "dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/read",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="support-request-contributor"></a>Mitwirkender für Supportanfragen

Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create and manage Support requests",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "name": "cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Support Request Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="tag-contributor"></a>Tagmitwirkender

Hiermit können Tags für Entitäten verwaltet werden, ohne Zugriff auf die Entitäten selbst zu gewähren.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | Microsoft.Resources/subscriptions/resources/read | Ruft die Ressourcen eines Abonnements ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | Microsoft.Resources/tags/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage tags on entities, without providing access to the entities themselves.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "name": "4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
        "Microsoft.Resources/subscriptions/resources/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/tags/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Tag Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="other"></a>Andere


### <a name="biztalk-contributor"></a>Mitwirkender von BizTalk

Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage BizTalk services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "name": "5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BizTalkServices/BizTalk/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "BizTalk Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="scheduler-job-collections-contributor"></a>Mitwirkender von Zeitplanungsauftragssammlung

Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Aktionen** |  |
> | Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.Insights/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | Microsoft.Resources/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Auftragssammlungen |
> | Microsoft.Support/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Scheduler job collections, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "name": "188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Scheduler/jobcollections/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Scheduler Job Collections Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Zuordnung von Ressourcenanbieter zu Dienst](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Benutzerdefinierte Azure-Rollen](custom-roles.md)
- [Berechtigungen in Azure Security Center](../security-center/security-center-permissions.md)
