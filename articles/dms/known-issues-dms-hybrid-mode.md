---
title: Bekannte Probleme/Migrationseinschränkungen beim Verwenden des Hybridmodus
description: Erfahren Sie mehr über bekannte Probleme/MIgrationseinschränkungen beim Verwenden des Azure Database Migration Service im Hybridmodus.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/19/2019
ms.openlocfilehash: 60d1fc46ada70dc67c161f048a0206e7081ba591
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475948"
---
# <a name="known-issuesmigration-limitations-with-using-hybrid-mode"></a>Bekannte Probleme/Migrationseinschränkungen beim Verwenden des Hybridmodus

In den folgenden Abschnitten werden bekannte Probleme und Einschränkungen beschrieben, die mit der Verwendung des Azure Database Migration Service im Hybridmodus einhergehen.

## <a name="installer-fails-to-authenticate"></a>Fehler beim Authentifizieren des Installationsprogramms

Nach dem Hochladen des Zertifikats in die AdApp tritt eine Verzögerung von bis zu zwei Minuten auf, bevor die Authentifizierung bei Azure erfolgen kann. Das Installationsprogramm versucht zwar erst mit einer gewissen Verzögerung, die Authentifizierung zu wiederholen, die Verzögerung der Weiterleitung kann aber größer als das Wiederholungsintervall sein, und in dem Fall wird eine **FailedToGetAccessTokenException**-Meldung angezeigt. Wenn das Zertifikat in die richtige AdApp hochgeladen und in „dmssettings.json“ die richtige AppID angegeben wurde, versuchen Sie erneut, den Installationsbefehl auszuführen.

## <a name="service-offline-after-successful-installation"></a>Dienst nach erfolgreicher Installation „offline“

Wenn der Dienst nach erfolgreichem Abschluss des Installationsvorgangs als offline angezeigt wird, versuchen Sie es mit den folgenden Schritten.

1. Navigieren Sie im Azure-Portal in Ihrer Instanz des Azure Database Migration Service zur Registerkarte mit den **Hybrid**-Einstellungen, und überprüfen Sie, ob der Worker registriert ist, indem Sie das Raster der registrierten Worker überprüfen.

    Der Status dieses Workers sollte **Online** sein, bei einem Problem wird er jedoch möglicherweise als **Offline** angezeigt.

2. Überprüfen Sie auf dem Computer des Workers den Status des Diensts, indem Sie den folgenden PowerShell-Befehl ausführen:

    ```
    Get-Service Scenario*
    ```

    Dieser Befehl gibt den Status des Windows-Diensts zurück, der den Worker ausführt. Es sollte nur ein einzelnes Ergebnis vorhanden sein. Wenn der Worker beendet wurde, können Sie versuchen, ihn mithilfe des folgenden PowerShell-Befehls neu zu starten:

    ```
    Start-Service Scenario*
    ```

    Darüber hinaus können Sie den Dienst auf der Benutzeroberfläche der Windows-Dienste überprüfen.

3. Wenn der Windows-Dienst zwischen „Wird ausgeführt“ und „Beendet“ wechselt, ist beim Starten des Workers ein Problem aufgetreten. Überprüfen Sie die Protokolle des Azure Database Migration Service-Hybridworkers, um das Problem zu bestimmen.

    - Protokolle des Installationsprozesses sind im „Protokollordner“ innerhalb des Ordners gespeichert, aus dem die Programmdatei des Installationsprogramms ausgeführt wurde.
    - Protokolle des Azure Database Migration Service-Hybridworkers sind im Ordner **WorkerLogs** gespeichert, in dem Ordner, in dem der Worker installiert ist. Der Standardspeicherort für die Protokolldateien des Hybridworkers ist **C:\Programme\DatabaseMigrationServiceHybrid\WorkerLogs**.

## <a name="using-your-own-signed-certificate"></a>Verwenden Ihres eigenen signierten Zertifikats

Bei dem von der Aktion GenerateCert generierten Zertifikat handelt es sich um ein selbstsigniertes Zertifikat, das möglicherweise im Rahmen ihrer internen Sicherheitsrichtlinien nicht zulässig ist. Anstatt dieses Zertifikat zu verwenden, können Sie Ihr eigenes Zertifikat angeben und den Fingerabdruck in „dmssettings.json“ bereitstellen. Dieses Zertifikat muss in Ihre AdApp hochgeladen und auf dem Computer installiert werden, auf dem Sie den Hybridworker des Azure Database Migration Service installieren. Installieren Sie dieses Zertifikat anschließend mit dem privaten Schlüssel im Zertifikatspeicher des lokalen Computers.

## <a name="running-the-worker-service-as-a-low-privilege-account"></a>Ausführen des Workerdiensts als Konto mit geringen Rechten

Standardmäßig wird der Azure Database Migration Service-Hybridworkerdienst als lokales Systemkonto ausgeführt. Sie können das für diesen Dienst verwendete Konto ändern, sofern das verwendete Konto über Netzwerkberechtigungen verfügt. Verwenden Sie die folgende Vorgehensweise, um das „Ausführen als“-Konto des Diensts zu ändern.

1. Beenden Sie den Dienst, entweder über Windows-Dienste oder durch Verwenden des Befehls „Stop-Service“ in PowerShell.

2. Aktualisieren Sie den Dienst so, dass ein anderes Anmeldekonto verwendet wird.

3. Erteilen Sie dem neuen Konto in certmgr für Zertifikate des lokalen Computers die Berechtigungen des privaten Schlüssels für die Zertifikate **DMS Hybrid App Key** und **DMS Scenario Engine Key Pair**.

    a. Öffnen Sie certmgr, um die folgenden Schlüssel anzuzeigen:

    - DMS Hybrid App Key
    - DMS Hybrid Worker Setup Key
    - DMS Scenario Engine Key Pair

    b. Klicken Sie mit der rechten Maustaste auf den Eintrag **DMS Hybrid App Key**, zeigen Sie auf **Alle Aufgaben**, und wählen Sie dann **Private Schlüssel verwalten** aus.

    c. Wählen Sie auf der Registerkarte **Sicherheit** **Hinzufügen** aus, und geben Sie dann den Namen des Kontos ein.

    d. Verwenden Sie die gleichen Schritte, um dem Zertifikat **DMS Scenario Engine Key Pair** die Berechtigungen des privaten Schlüssels für das Konto zu erteilen.

## <a name="unregistering-the-worker-manually"></a>Manuelles Aufheben der Registrierung des Workers

Wenn Sie keinen Zugriff mehr auf den Workercomputer haben, können Sie die Registrierung des Workers aufheben und Ihre Instanz von Azure Database Migration Service erneut verwenden, indem Sie die folgenden Schritte ausführen:

1. Wechseln Sie im Azure-Portal zu Ihrer Instanz des Azure Database Migration Service, und navigieren Sie dann zur **Hybrid**-Einstellungsseite.

   Ihr Workereintrag wird mit dem Status **Offline** in der Liste angezeigt.

2. Wählen Sie ganz rechts in der Auflistung des Workereintrags die Auslassungspunkte und anschließend **Registrierung aufheben** aus.

## <a name="addressing-issues-for-specific-migration-scenarios"></a>Behandeln von Problemen für bestimmte Migrationsszenarien

In den Abschnitten unten werden szenariospezifische Probleme im Zusammenhang mit der Verwendung des Azure Database Migration Service-Hybridmodus zum Durchführen einer Onlinemigration beschrieben.

### <a name="online-migrations-to-azure-sql-database-managed-instance"></a>Onlinemigration zur einer verwalteten Microsoft Azure SQL-Datenbank-Instanz

**Hohe CPU-Auslastung**

**Problem:** Bei Onlinemigrationen zu einer verwalteten SQL-Datenbank-Instanz wird auf dem Computer, auf dem der Hybridworker ausgeführt wird, eine hohe CPU-Auslastung angezeigt, wenn zu viele Sicherungen vorhanden sind oder die Sicherungen zu groß sind.

**Lösung**: Um dieses Problem zu beheben, verwenden Sie komprimierte Sicherungen, teilen Sie die Migration so, dass sie mehrere Freigaben verwendet, oder skalieren Sie den Computer, auf dem der Hybridworker ausgeführt wird, zentral hoch.
