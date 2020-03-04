---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ed061234da27716bfcdaf1cc065568908108f90a
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597807"
---
Alle in Azure Files gespeicherten Daten werden im Ruhezustand mithilfe der Azure-Speicherdienstverschlüsselung (Storage Service Encryption, SSE) verschlüsselt. Die Speicherdienstverschlüsselung funktioniert ähnlich wie BitLocker unter Windows: Daten werden unterhalb der Dateisystemebene verschlüsselt. Da Daten durch die Codierung auf dem Datenträger unterhalb des Dateisystems der Azure-Dateifreigabe verschlüsselt werden, benötigen Sie keinen Zugriff auf den zugrunde liegenden Schlüssel auf dem Client, um aus der Azure-Dateifreigabe zu lesen oder in diese zu schreiben.

Standardmäßig werden in Azure Files gespeicherte Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Bei von Microsoft verwalteten Schlüsseln ist Microsoft im Besitz der Schlüssel zum Verschlüsseln/Entschlüsseln der Daten und damit dafür verantwortlich, sie in regelmäßigen Abständen zu rotieren. Sie können auch Ihre eigenen Schlüssel selbst verwalten, sodass Sie den Rotationsvorgang steuern können. Wenn Sie Ihre Dateifreigaben mit vom Kunden verwalteten Schlüsseln verschlüsseln, ist Azure Files für den Zugriff auf Ihre Schlüssel autorisiert, um Lese- und Schreibanforderungen von Ihren Clients zu erfüllen. Mit vom Kunden verwalteten Schlüsseln können Sie diese Autorisierung jederzeit widerrufen. Dies bedeutet jedoch, dass die Azure-Dateifreigabe nicht mehr über SMB oder die FileREST-API zugänglich ist.

Azure Files verwendet dasselbe Verschlüsselungsschema wie die anderen Azure-Speicherdienste, z. B. Azure Blob Storage. Weitere Informationen zur Azure-Speicherdienstverschlüsselung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../articles/storage/common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).