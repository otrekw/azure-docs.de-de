---
title: Azure Key Vault-Sicherung | Microsoft-Dokumentation
description: Verwenden Sie dieses Dokument, um ein Geheimnis, ein Zertifikat oder einen Schlüssel zu sichern, das bzw. der in Azure Key Vault gespeichert ist.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: sudbalas
ms.openlocfilehash: 8a152e2771f0b207e81f42c6ecae2e4d14605051
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156355"
---
# <a name="azure-key-vault-backup"></a>Sicherung in Azure Key Vault

In diesem Dokument erfahren Sie, wie Sie eine Sicherung der einzelnen Geheimnisse, Schlüssel und Zertifikate durchführen, die in Ihrem Schlüsseltresor gespeichert sind. Diese Sicherung soll als Offlinekopie aller Geheimnisse in dem unwahrscheinlichen Fall dienen, dass Sie keinen Zugriff mehr auf Ihren Schlüsseltresor haben.

## <a name="overview"></a>Übersicht

Key Vault stellt automatisch mehrere Features zur Gewährleistung der Verfügbarkeit und zur Vermeidung von Datenverlusten zur Verfügung. Diese Sicherung sollte nur versucht werden, wenn eine wichtige geschäftliche Begründung dafür vorliegt, eine Sicherung Ihrer Geheimnisse beizubehalten. Das Sichern von Geheimnissen in Ihrem Schlüsseltresor kann zusätzliche betriebliche Herausforderungen mit sich bringen, etwa im Zusammenhang mit der Verwaltung von mehreren Protokoll-, Berechtigungs- und Sicherungssätzen beim Ablauf oder bei der Rotation von Geheimnissen.

Key Vault gewährleistet die Verfügbarkeit in Notfallszenarien und führt automatisch ein Failover von Anforderungen auf ein Regionspaar aus, ohne dass ein Eingreifen des Benutzers erforderlich ist. Weitere Informationen finden Sie unter folgendem Link: [Azure Key Vault – Leitfaden zur Notfallwiederherstellung](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)

Key Vault schützt durch den Schutz vor vorläufigem und endgültigem Löschen vor versehentlichem und böswilligem Löschen Ihrer Geheimnisse. Wenn Sie Ihre Geheimnisse vor versehentlichem oder böswilligem Löschen schützen möchten, konfigurieren Sie für Ihren Schlüsseltresor die Funktionen zum Schutz vor vorläufigem und endgültigem Löschen. Weitere Informationen finden Sie in folgendem Dokument: [Azure Key Vault-Wiederherstellung](https://docs.microsoft.com/azure/key-vault/general/overview-soft-delete)

## <a name="limitations"></a>Einschränkungen

Azure Key Vault unterstützt derzeit keine Methode zum Sichern eines gesamten Schlüsseltresors in einem einzigen Vorgang. Von Microsoft oder dem Azure Key Vault-Team werden Versuche, die in diesem Dokument aufgeführten Befehle zum Ausführen einer automatisierten Sicherung eines Schlüsseltresors zu verwenden, nicht unterstützt.

Wenn Sie die in diesem Dokument gezeigten Befehle zum Erstellen einer benutzerdefinierten Automatisierung verwenden, kann dies zu Fehlern führen.

* Die Sicherung von Geheimnissen kann zu Timeoutfehlern führen.
* Bei der Sicherung wird eine Point-in-Time-Momentaufnahme erstellt. Geheimnisse werden während einer Sicherung unter Umständen erneuert, was zu einem Konflikt mit den Verschlüsselungsschlüsseln führt.
* Wenn die Grenzwerte des Schlüsseltresors für Anforderungen pro Sekunde überschritten werden, wird der Schlüsseltresor gedrosselt. Dies führt zu einem Fehler bei der Sicherung.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Wenn Sie ein im Schlüsseltresor gespeichertes Objekt (Geheimnis, Schlüssel oder Zertifikat) sichern, wird das Objekt beim Sicherungsvorgang als verschlüsseltes Blob heruntergeladen. Dieses Blob kann außerhalb von Azure nicht entschlüsselt werden. Um verwendbare Daten aus diesem Blob zu erhalten, müssen Sie das Blob in einem Schlüsseltresor innerhalb desselben Azure-Abonnements und derselben Azure-Geografie wiederherstellen.
[Azure-Geografien](https://azure.microsoft.com/global-infrastructure/geographies/)

## <a name="prerequisites"></a>Voraussetzungen

* Mindestens Berechtigungen auf der Ebene „Mitwirkender“ für ein Azure-Abonnement
* Primärer Schlüsseltresor mit zu sichernden Geheimnissen
* Sekundärer Schlüsseltresor, in dem Geheimnisse wiederhergestellt werden

## <a name="back-up-and-restore-with-azure-portal"></a>Sichern und Wiederherstellen über das Azure-Portal

### <a name="back-up"></a>Sichern

1. Navigieren Sie zum Azure-Portal.
2. Wählen Sie Ihren Schlüsseltresor aus.
3. Navigieren Sie zu dem Objekt (Geheimnis, Schlüssel oder Zertifikat), das Sie sichern möchten.

    ![Image](../media/backup-1.png)

4. Wählen Sie das Objekt aus.
5. Auswählen von „Sicherung herunterladen“

    ![Image](../media/backup-2.png)
    
6. Klicken Sie auf die Schaltfläche „Herunterladen“.

    ![Image](../media/backup-3.png)
    
7. Speichern Sie das verschlüsselte Blob an einem sicheren Speicherort.

### <a name="restore"></a>Restore

1. Navigieren Sie zum Azure-Portal.
2. Wählen Sie Ihren Schlüsseltresor aus.
3. Navigieren Sie zu dem Typ des Objekts (Geheimnis, Schlüssel oder Zertifikat), das Sie wiederherstellen möchten.
4. Auswählen von „Sicherung wiederherstellen“

    ![Image](../media/backup-4.png)
    
5. Navigieren Sie zu dem Speicherort, an dem Sie das verschlüsselte Blob gespeichert haben.
6. Klicken Sie auf „OK“.

## <a name="back-up-and-restore-with-the-azure-cli"></a>Sichern und Wiederherstellen mit der Azure CLI

```azurecli
## Login To Azure
az login

## Set your Subscription
az account set --subscription {AZURE SUBSCRIPTION ID}

## Register Key Vault as a Provider
az provider register -n Microsoft.KeyVault

## Backup a Certificate in Key Vault
az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Key in Key Vault
az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Secret in Key Vault
az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Certificate in Key Vault
az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Key in Key Vault
az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Secret in Key Vault
az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

```

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie die Protokollierung und Überwachung für Ihre Azure Key Vault-Instanz. [Azure-Schlüsseltresor-Protokollierung](https://docs.microsoft.com/azure/key-vault/general/logging)
