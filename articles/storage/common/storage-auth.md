---
title: Autorisieren von Datenvorgängen
titleSuffix: Azure Storage
description: Erhalten Sie Informationen über die verschiedenen Methoden zum Autorisieren des Zugriffs auf Azure Storage, z. B. Azure Active Directory, Autorisierung mit gemeinsam verwendetem Schlüssel oder Shared Access Signatures (SAS).
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: b0f2ad7566d0204871a9c6441315d6201662d92b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616288"
---
# <a name="authorizing-access-to-data-in-azure-storage"></a>Autorisierung des Zugriffs auf Daten in Azure Storage

Immer dann, wenn Sie auf Daten in Ihrem Speicherkonto zugreifen, sendet der Client eine Anforderung über HTTP/HTTPS an Azure Storage. Jede Anforderung an eine sichere Ressource muss autorisiert sein, damit der Dienst sicherstellt, dass der Client über die erforderlichen Berechtigungen zum Zugriff auf die Daten verfügt.

In der folgenden Tabelle werden die Optionen beschrieben, die in Azure Storage zum Autorisieren des Ressourcenzugriffs zur Verfügung stehen:

|  |Gemeinsam verwendeter Schlüssel (Speicherkontoschlüssel)  |Shared Access Signature (SAS)  |Azure Active Directory (Azure AD)  |Active Directory (Vorschau) |Anonymer öffentlicher Lesezugriff  |
|---------|---------|---------|---------|---------|---------|
|Azure-Blobs     |[Unterstützt](/rest/api/storageservices/authorize-with-shared-key/)         |[Unterstützt](storage-sas-overview.md)         |[Unterstützt](storage-auth-aad.md)         |Nicht unterstützt|[Unterstützt](../blobs/storage-manage-access-to-resources.md)         |
|Azure Files (SMB)     |[Unterstützt](/rest/api/storageservices/authorize-with-shared-key/)         |Nicht unterstützt         |[Unterstützt, aber nur mit AAD Domain Services](../files/storage-files-active-directory-overview.md)         |[Unterstützt, Anmeldeinformationen müssen mit Azure AD synchronisiert werden](../files/storage-files-active-directory-overview.md)|Nicht unterstützt         |
|Azure Files (REST)     |[Unterstützt](/rest/api/storageservices/authorize-with-shared-key/)         |[Unterstützt](storage-sas-overview.md)         |Nicht unterstützt         |Nicht unterstützt |Nicht unterstützt         |
|Azure-Warteschlangen     |[Unterstützt](/rest/api/storageservices/authorize-with-shared-key/)         |[Unterstützt](storage-sas-overview.md)         |[Unterstützt](storage-auth-aad.md)         |Nicht unterstützt | Nicht unterstützt         |
|Azure-Tabellen     |[Unterstützt](/rest/api/storageservices/authorize-with-shared-key/)         |[Unterstützt](storage-sas-overview.md)         |Nicht unterstützt         |Nicht unterstützt| Nicht unterstützt         |

Im Anschluss werden die einzelnen Autorisierungsoptionen kurz erläutert:

- **Integration von Azure Active Directory (Azure AD)** für Blobs und Warteschlangen. Azure AD bietet rollenbasierte Zugriffssteuerung (RBAC, Role-Based Access Control) für eine präzise Steuerung des Zugriffs eines Clients auf Ressourcen in einem Speicherkonto. Weitere Informationen zur Azure AD-Integration für Blobs und Warteschlangen finden Sie unter [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](storage-auth-aad.md).

- **Azure Active Directory Domain Services-Authentifizierung (Azure AD DS)** für Azure Files. Azure Files unterstützt die identitätsbasierte Autorisierung per Server Message Block (SMB) über Azure AD DS. Sie können die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für eine präzise Steuerung des Clientzugriffs auf Azure Files-Ressourcen in einem Speicherkonto verwenden. Weitere Informationen zur Azure Files-Authentifizierung mithilfe von Domänendiensten finden Sie in der [Übersicht](../files/storage-files-active-directory-overview.md).

- **AD-Authentifizierung (Active Directory)** für Azure Files. Azure Files unterstützt identitätsbasierte Autorisierung über SMB über AD. Ihr AD-Domänendienst kann auf lokalen Computern oder auf Azure-VMs gehostet werden. Der SMB-Zugriff auf Dateien wird mithilfe von AD-Anmeldeinformationen von in die Domäne eingebundenen Computern unterstützt, entweder lokal oder in Azure. Sie können RBAC für Zugriffssteuerung auf Freigabeebene und NTFS-DACLs für die Berechtigungserzwingung auf Verzeichnis-/Dateiebene verwenden. Weitere Informationen zur Azure Files-Authentifizierung mithilfe von Domänendiensten finden Sie in der [Übersicht](../files/storage-files-active-directory-overview.md).

- **Autorisierung mit gemeinsam verwendetem Schlüssel** für Blobs, Dateien, Warteschlangen und Tabellen. Ein Client mit gemeinsam verwendetem Schlüssel übergibt mit jeder Anforderung einen Header, der mit dem Speicherkonto-Zugriffsschlüssel signiert wird. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](/rest/api/storageservices/authorize-with-shared-key/).
- **Shared Access Signatures** für Blobs, Dateien, Warteschlangen und Tabellen. Shared Access Signatures (SAS) ermöglichen den begrenzten delegierten Zugriff auf Ressourcen in einem Speicherkonto. Einschränkungen des Zeitintervalls, für das die Signatur gültig ist, oder von Berechtigungen, die sie gewährt, bieten Flexibilität beim Verwalten des Zugriffs. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures (SAS)](storage-sas-overview.md).
- **Anonymer öffentlicher Lesezugriff** für Container und Blobs. Autorisierung ist nicht erforderlich. Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](../blobs/storage-manage-access-to-resources.md).  

Standardmäßig werden alle Ressourcen in Azure Storage gesichert und stehen nur dem Kontobesitzer zur Verfügung. Obwohl Sie jede der oben beschriebenen Autorisierungsstrategien verwenden können, um Clients Zugriff auf Ressourcen in Ihrem Speicherkonto zu gewähren, empfiehlt Microsoft im Sinne optimaler Sicherheit und Benutzerfreundlichkeit nach Möglichkeit die Verwendung von Azure AD.

## <a name="next-steps"></a>Nächste Schritte

- [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](storage-auth-aad.md)
- [Autorisieren mit einem gemeinsam verwendeten Schlüssel](/rest/api/storageservices/authorize-with-shared-key/)
- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](storage-sas-overview.md)
