---
title: include file
description: include file
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 02/03/2021
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: b0be074c995fcc62f63a4a2ebf4149d3040e7b6b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750650"
---
## <a name="best-practices"></a>Bewährte Methoden

Dieser Abschnitt enthält einige Richtlinien zu bewährten Methoden für das rekursive Festlegen von Zugriffssteuerungslisten. 

#### <a name="handling-runtime-errors"></a>Behandeln von Laufzeitfehlern

Ein Laufzeitfehler kann aus vielen Gründen auftreten (z. B. bei einem Ausfall oder einem Clientkonnektivitätsproblem). Wenn ein Laufzeitfehler auftritt, starten Sie den rekursiven ACL-Prozess neu. Zugriffssteuerungslisten können ohne negative Auswirkungen erneut auf Elemente angewandt werden. 

#### <a name="handling-permission-errors-403"></a>Behandeln von Berechtigungsfehlern (403)

Wenn beim Ausführen eines rekursiven ACL-Prozesses eine Zugriffssteuerungsausnahme auftritt, verfügt Ihr AD-[Sicherheitsprinzipal](../articles/role-based-access-control/overview.md#security-principal) möglicherweise nicht über ausreichende Berechtigungen zum Anwenden einer Zugriffssteuerungsliste auf eines der untergeordneten Elemente in der Verzeichnishierarchie. Wenn ein Berechtigungsfehler auftritt, wird der Prozess angehalten, und es wird ein Fortsetzungstoken bereitgestellt. Beheben Sie das Berechtigungsproblem, und verwenden Sie dann das Fortsetzungstoken, um das restliche Dataset zu verarbeiten. Die Verzeichnisse und Dateien, die bereits erfolgreich verarbeitet wurden, müssen nicht erneut verarbeitet werden. Sie können den rekursiven ACL-Prozess auch neu starten. Zugriffssteuerungslisten können ohne negative Auswirkungen erneut auf Elemente angewandt werden. 

#### <a name="credentials"></a>Anmeldeinformationen 

Es wird empfohlen, einen Azure AD-Sicherheitsprinzipal bereitzustellen, dem die Rolle [Speicherblob-Datenbesitzer](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner) im Bereich des Zielspeicherkontos oder -containers zugewiesen wurde. 

#### <a name="performance"></a>Leistung 

Um die Latenz zu verringern, empfiehlt es sich, den rekursiven ACL-Prozess auf einem virtuellen Azure-Computer (VM) auszuführen, der sich in derselben Region wie das Speicherkonto befindet. 

#### <a name="acl-limits"></a>Einschränkungen bei Zugriffssteuerungslisten

Die maximale Anzahl von Zugriffssteuerungslisten, die Sie auf ein Verzeichnis oder eine Datei anwenden können, beträgt 32 ACLs für den Zugriff und 32 Standard-ACLs. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).