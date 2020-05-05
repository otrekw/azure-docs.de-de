---
title: Erstellen einer Azure HPC Cache-Instanz
description: Hier erfahren Sie, wie Sie eine Azure HPC Cache-Instanz erstellen.
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 04/23/2020
ms.author: v-erkel
ms.openlocfilehash: 4ff31ca6a171beece1672802367f08768676efbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195008"
---
# <a name="create-an-azure-hpc-cache"></a>Erstellen einer Azure HPC Cache-Instanz

Erstellen Sie Ihren Cache mithilfe des Azure-Portals.

![Screenshot: Cacheübersicht im Azure-Portal mit der Schaltfläche „Erstellen“ im unteren Bereich](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definieren grundlegender Informationen

![Screenshot: Projektdetailseite im Azure-Portal](media/hpc-cache-create-basics.png)

Wählen Sie auf der Seite **Projektdetails** das Abonnement und die Ressourcengruppe zum Hosten des Caches aus. Achten Sie darauf, dass sich das Abonnement in der [Zugriffsliste](hpc-cache-prereqs.md#azure-subscription) befindet.

Legen Sie unter **Dienstdetails** den Cachenamen sowie folgende weitere Attribute fest:

* Standort: Wählen Sie eine der [unterstützten Regionen](hpc-cache-overview.md#region-availability) aus.
* Virtuelles Netzwerk: Sie können ein bereits vorhandenes virtuelles Netzwerk auswählen oder ein neues erstellen.
* Subnetz: Wählen Sie ein Subnetz mit mindestens 64 IP-Adressen (/24) aus, oder erstellen Sie es. Dieses Subnetz darf nur für diese Azure HPC Cache-Instanz verwendet werden.

## <a name="set-cache-capacity"></a>Festlegen der Cachekapazität
<!-- referenced from GUI - update aka.ms link if you change this header text -->

Legen Sie auf der Seite **Cache** die Kapazität Ihres Caches fest. Die hier festgelegten Werte bestimmen, wie viele Daten der Cache enthalten und wie schnell er Clientanforderungen bewältigen kann.

Die Kapazität wirkt sich auch auf die Kosten des Caches aus.

Legen Sie zum Auswählen der Kapazität die beiden folgenden Werte fest:

* Die maximale Datenübertragungsrate für den Cache (Durchsatz) in GB/Sekunde
* Die zugeordnete Speichermenge für zwischengespeicherte Daten in TB

Wählen Sie einen der verfügbaren Durchsatzwerte und eine der verfügbaren Cachespeichergrößen aus.

Beachten Sie, dass die tatsächliche Datenübertragungsrate von der Arbeitsauslastung, der Netzwerkgeschwindigkeit und der Art der Speicherziele abhängt. Die von Ihnen ausgewählten Werte bestimmen den maximalen Durchsatz für das gesamte Cachesystem, ein Teil davon wird jedoch für zusätzliche Aufgaben verwendet. Wenn ein Client beispielsweise eine Datei anfordert, die noch nicht im Cache gespeichert ist, oder wenn die Datei als veraltet markiert ist, nutzt der Cache einen Teil des Durchsatzes, um sie aus dem Back-End-Speicher abzurufen.

Mit Azure HPC Cache wird verwaltet, welche Dateien zwischengespeichert und vorab geladen werden, um die Cachetrefferraten zu maximieren. Cacheinhalte werden kontinuierlich bewertet, und seltener verwendete Dateien werden in den langfristigen Speicher verschoben. Wählen Sie eine Cachespeichergröße aus, die problemlos den aktiven Satz von Arbeitsdateien aufnehmen kann sowie Speicherplatz für Metadaten und andere Zusatzdaten bietet.

![Screenshot: Seite zum Festlegen der Cachegröße](media/hpc-cache-create-capacity.png)

## <a name="enable-azure-key-vault-encryption-optional"></a>Aktivieren der Azure Key Vault-Verschlüsselung (optional)

Wenn sich Ihr Cache in einer Region befindet, die kundenseitig verwaltete Verschlüsselungsschlüssel unterstützt, wird die Seite **Datenträgerverschlüsselungsschlüssel** zwischen den Registerkarten **Cache** und **Tags** angezeigt. Zum Zeitpunkt der Veröffentlichung wird diese Option in den Regionen „USA, Osten“, „USA, Süden-Mitte“ und „USA, Westen 2“ unterstützt.

Wenn Sie die für Ihren Cachespeicher verwendeten Verschlüsselungsschlüssel verwalten möchten, geben Sie die Azure Key Vault-Informationen auf der Seite **Datenträgerverschlüsselungsschlüssel** an. Der Schlüsseltresor muss sich in derselben Region und im selben Abonnement befinden wie der Cache.

Sie können diesen Abschnitt überspringen, wenn Sie keine kundenseitig verwalteten Schlüssel benötigen. Standardmäßig verschlüsselt Azure Daten mit von Microsoft verwalteten Schlüsseln. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung](../storage/common/storage-service-encryption.md).

> [!NOTE]
>
> * Nach dem Erstellen des Caches können Sie nicht mehr zwischen von Microsoft und von Kunden verwalteten Schlüsseln wechseln.
> * Nachdem der Cache erstellt wurde, müssen Sie ihn für den Zugriff auf den Schlüsseltresor autorisieren. Klicken Sie auf der Seite **Übersicht** des Caches auf die Schaltfläche **Verschlüsselung aktivieren**, um die Verschlüsselung zu aktivieren. Führen Sie diesen Schritt innerhalb von 90 Minuten nach dem Erstellen des Caches aus.
> * Cachedatenträger werden nach dieser Autorisierung erstellt. Dies bedeutet, dass die anfängliche Cacheerstellungszeit kurz ist, der Cache jedoch für mindestens zehn Minuten nach der Autorisierung des Zugriffs noch nicht verwendet werden kann.

Eine ausführliche Erläuterung des Verschlüsselungsverfahrens mit kundenseitig verwalteten Schlüsseln finden Sie unter [Verwenden von kundenseitig verwalteten Verschlüsselungsschlüsseln für Azure HPC Cache](customer-keys.md).

![Screenshot der Seite „Verschlüsselungsschlüssel“ mit ausgewählter Option „Vom Kunden verwaltet“ und erkennbaren Feldern des Schlüsseltresors](media/create-encryption.png)

Wählen Sie **Vom Kunden verwaltet** aus, um die Verschlüsselung mit kundenseitig verwalteten Schlüsseln auszuwählen. Die Felder für die Angabe des Schlüsseltresors werden angezeigt. Wählen Sie die zu verwendende Azure Key Vault-Instanz und dann den Schlüssel und die Version für diesen Cache aus. Bei dem Schlüssel muss es sich um einen 2.048-Bit-RSA-Schlüssel handeln. Auf dieser Seite können Sie einen Schlüsseltresor, einen Schlüssel oder eine Schlüsselversion neu erstellen.

Nach dem Erstellen des Caches müssen Sie ihn für die Verwendung des Key Vault-Diensts autorisieren. Weitere Informationen finden Sie unter [Autorisieren der Verschlüsselung mit Azure Key Vault aus dem Cache](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache).

## <a name="add-resource-tags-optional"></a>Hinzufügen von Ressourcentags (optional)

Auf der Seite **Tags** können Sie Ihrer Azure HPC Cache-Instanz [Ressourcentags](https://go.microsoft.com/fwlink/?linkid=873112) hinzufügen.

## <a name="finish-creating-the-cache"></a>Fertigstellen der Cacheerstellung

Nachdem Sie den neuen Cache konfiguriert haben, klicken Sie auf die Registerkarte **Überprüfen + erstellen**. Das Portal überprüft Ihre Auswahl und ermöglicht es Ihnen, die gewählten Optionen selbst noch einmal zu überprüfen. Ist alles korrekt, klicken Sie auf **Erstellen**.

Die Cacheerstellung dauert ungefähr zehn Minuten. Sie können den Status über den Benachrichtigungsbereich des Azure-Portals nachverfolgen.

![Screenshot der Cache-Erstellungsseiten „Bereitstellung wird ausgeführt“ und „Benachrichtigungen“ im Portal](media/hpc-cache-deploy-status.png)

Wenn die Erstellung abgeschlossen ist, wird eine Benachrichtigung mit einem Link zur neuen Azure HPC Cache-Instanz angezeigt, und der Cache wird in der Liste **Ressourcen** Ihres Abonnements angezeigt.

![Screenshot: Azure HPC Cache-Instanz im Azure-Portal](media/hpc-cache-new-overview.png)

> [!NOTE]
> Wenn für Ihren Cache kundenseitig verwaltete Verschlüsselungsschlüssel verwendet werden, wird der Cache möglicherweise in der Ressourcenliste aufgeführt, bevor der Bereitstellungsstatus in „Fertig“ geändert wird. Sobald der Status des Caches **Warten auf Schlüssel** lautet, können Sie ihn für die Verwendung des Schlüsseltresors [autorisieren](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache).

## <a name="next-steps"></a>Nächste Schritte

Nachdem der Cache in der Liste **Ressourcen** aufgeführt wird, können Sie mit dem nächsten Schritt fortfahren.

* [Definieren Sie Speicherziele](hpc-cache-add-storage.md), um dem Cache Zugriff auf Ihre Datenquellen zu gewähren.
* Wenn Sie kundenseitig verwaltete Verschlüsselungsschlüssel verwenden, müssen Sie auf der Übersichtsseite des Caches die [Azure Key Vault-Verschlüsselung autorisieren](customer-keys.md#3-authorize-azure-key-vault-encryption-from-the-cache), um das Cachesetup abzuschließen. Diesen Schritt müssen Sie ausführen, damit Sie anschließend Speicher hinzufügen können. Details finden Sie unter [Verwenden von kundenseitig verwalteten Verschlüsselungsschlüsseln](customer-keys.md).
