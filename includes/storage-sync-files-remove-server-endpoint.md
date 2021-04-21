---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 2f50a1d046b7b7d3fd7507ef87f8f29096a4faa4
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107774561"
---
Nein: Das Entfernen eines Serverendpunkts entspricht nicht dem Neustart eines Servers! Das Entfernen und erneute Erstellen des Serverendpunkts ist fast nie die optimale Lösung für das Beheben von Problemen mit der Synchronisierung, dem Cloudtiering oder anderen Aspekten der Azure-Dateisynchronisierung. Das Entfernen eines Serverendpunkts ist ein destruktiver Vorgang. Es kann zu Datenverlust führen, wenn mehrstufige Dateien außerhalb des Namespaces des Serverendpunkts vorhanden sind. Unter [Warum sind mehrstufige Dateien außerhalb des Serverendpunkt-Namespaces vorhanden?](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) finden Sie weitere Informationen. Oder es kann bei mehrstufigen Dateien innerhalb des Namespaces des Serverendpunkts zu nicht zugänglichen Dateien führen. Diese Probleme werden nicht dadurch gelöst, dass der Serverendpunkt neu erstellt wird. Auf Ihrem Serverendpunkt sind möglicherweise auch dann mehrstufige Dateien vorhanden, wenn Sie das Cloudtiering nie aktiviert haben. Aus diesem Grund wird empfohlen, den Serverendpunkt nur zu entfernen, wenn Sie die Azure-Dateisynchronisierung mit diesem spezifischen Ordner nicht mehr verwenden möchten oder von einem Microsoft-Supportmitarbeiter ausdrücklich dazu aufgefordert wurden. Weitere Informationen zum Entfernen von Serverendpunkten finden Sie unter [Entfernen eines Serverendpunkts](../articles/storage/file-sync/file-sync-server-endpoint.md#remove-a-server-endpoint).    
