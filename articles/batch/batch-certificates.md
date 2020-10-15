---
title: Verwenden von Zertifikaten – Azure Batch
description: Verwenden von Zertifikaten zum Aktivieren der Authentifizierung von Anwendungen
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 5f7d547ad549b2747ae41182ee4058b001d9e78a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89146384"
---
# <a name="using-certificates-with-batch"></a>Verwenden von Zertifikaten mit Batch

Der Hauptgrund für die Verwendung von Zertifikaten mit Batch ist derzeit, dass Anwendungen in Pools ausgeführt werden, die bei einem Endpunkt authentifiziert werden müssen. 

Wenn Sie noch kein Zertifikat besitzen, können Sie mit dem Befehlszeilentool `makecert` ein selbstsigniertes Zertifikat generieren.

## <a name="upload-certificates-manually-through-the-azure-portal"></a>Manuelles Hochladen von Zertifikaten über das Azure-Portal

1. Wählen Sie in dem Batch-Konto, in das Sie ein Zertifikat hochladen möchten, **Zertifikate** und dann **Hinzufügen** aus. 

2. Laden Sie das Zertifikat mit der Erweiterung „.pfx“ oder „.cer“ hoch. 

Nachdem der Upload abgeschlossen ist, wird das Zertifikat der Liste der Zertifikate hinzugefügt, und Sie können den Fingerabdruck überprüfen.

Wenn Sie jetzt einen Batch-Pool erstellen, können Sie innerhalb des Pools zu „Zertifikate“ navigieren und das hochgeladene Zertifikat diesem Pool zuweisen.

## <a name="next-steps"></a>Nächste Schritte

Batch verfügt über die Zertifikat-API [AZ batch certificate create](/cli/azure/batch/certificate).

Informationen zur Verwendung von Key Vault finden Sie unter [Sicherer Zugriff auf Key Vault mit Batch](credential-access-key-vault.md).
