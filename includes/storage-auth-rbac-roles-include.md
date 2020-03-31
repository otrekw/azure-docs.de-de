---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/17/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4b10955a1d3b85acbcae109836bebc03ec04c72c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279283"
---
Azure stellt die folgenden integrierten Rollen für die rollenbasierte Zugriffssteuerung zum Autorisieren des Zugriffs auf Blob- und Warteschlangendaten mit Azure AD und OAuth bereit:

- [Besitzer von Speicherblobdaten](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Dient zum Festlegen des Besitzes sowie zum Verwalten der POSIX-Zugriffssteuerung für Azure Data Lake Storage Gen2. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Mitwirkender an Speicherblobdaten](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Dient zum Gewähren von Lese-/Schreib-/Löschberechtigungen für Blobspeicherressourcen.
- [Leser von Speicherblobdaten](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): Dient zum Gewähren von Leseberechtigungen für Blobspeicherressourcen.
- [Mitwirkender an Storage-Warteschlangendaten](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Dient zum Gewähren von Lese-/Schreib-/Löschberechtigungen für Azure-Warteschlangen.
- [Storage-Warteschlangendatenleser](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Dient zum Gewähren von Leseberechtigungen für Azure-Warteschlangen.
- [Verarbeiter von Speicherwarteschlangen-Datennachrichten](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Dient zum Gewähren von Berechtigungen zum Einsehen, Abrufen und Löschen für Nachrichten in Azure Storage-Warteschlangen.
- [Absender der Speicherwarteschlangen-Datennachricht](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Dient zum Gewähren von Berechtigungen zum Hinzufügen für Nachrichten in Azure Storage-Warteschlangen.

> [!NOTE]
> Die Verteilung von RBAC-Rollenzuweisungen kann bis zu fünf Minuten dauern.
>
> Nur Rollen, die explizit für den Datenzugriff definiert sind, ermöglichen einem Sicherheitsprinzipal den Zugriff auf Blob- oder Warteschlangendaten. Rollen wie **Besitzer**, **Mitwirkender** und **Speicherkontomitwirkender** gestatten einem Sicherheitsprinzipal die Verwaltung eines Speicherkontos, gewähren aber keinen Zugriff auf die Blob- oder Warteschlangendaten in diesem Konto.
>
> Der Zugriff auf Blob- oder Warteschlangendaten im Azure-Portal kann entweder über Ihr Azure AD-Konto oder mithilfe des Speicherkonto-Zugriffsschlüssels autorisiert werden. Weitere Informationen finden Sie unter [Zugreifen auf Blob- oder Warteschlangendaten über das Azure-Portal](../articles/storage/common/storage-access-blobs-queues-portal.md).
