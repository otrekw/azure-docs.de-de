---
title: Umgehen von Speicherfirewalleinstellungen
description: Eine Firewalleinstellung für ein Speicherkontonetzwerk kann zu Fehlern beim Erstellen eines Azure-Blobspeicherziels in Azure HPC Cache führen. Dieser Artikel bietet eine Problemumgehung für die Einschränkung, bis eine Softwarekorrektur vorhanden ist.
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/7/2019
ms.author: rohogue
ms.openlocfilehash: 6643662d498db8cbcffcb120a9ceabc46cfc04cb
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74174183"
---
# <a name="work-around-blob-storage-account-firewall-settings"></a>Umgehen der Firewalleinstellungen für Blobspeicherkonten

Eine bestimmte Einstellung, die in Firewalls für Speicherkonten verwendet wird, kann dazu führen, dass bei der Erstellung Ihres Blobspeicherziels ein Fehler auftritt. Das Azure HPC Cache-Team arbeitet an einer Softwarekorrektur für dieses Problem, aber Sie können es umgehen, indem Sie den Anweisungen in diesem Artikel folgen.

Die Firewalleinstellung, die den Zugriff nur aus „bestimmten Netzwerken“ gestattet, kann verhindern, dass der Cache ein Blobspeicherziel erstellt. Diese Konfiguration befindet sich auf der Einstellungsseite **Firewalls und virtuelle Netzwerke** des Speicherkontos.

Das Problem ist, dass der Cachedienst ein virtuelles Netzwerk mit versteckten Diensten verwendet, das von Kundenumgebungen getrennt ist. Es ist nicht möglich, dieses Netzwerk explizit für den Zugriff auf Ihr Speicherkonto zu autorisieren.

Wenn Sie ein Blobspeicherziel erstellen, verwendet der Cachedienst dieses Netzwerk zur Überprüfung, ob der Container leer ist. Wenn die Firewall den Zugriff aus dem versteckten Netzwerk nicht zulässt, schlägt die Prüfung fehl, und bei der Erstellung des Speicherziels tritt ein Fehler auf.

Um das Problem zu umgehen, ändern Sie vorübergehend Ihre Firewalleinstellungen, während Sie das Speicherziel erstellen:

1. Wechseln Sie zur Seite **Firewalls und virtuelle Netzwerke** des Speicherkontos, und ändern Sie die Einstellung „Zugriff erlauben von“ in **Alle Netzwerke**.
1. Erstellen Sie das Blobspeicherziel in Ihrem Azure HPC Cache.
1. Nachdem das Speicherziel erfolgreich erstellt wurde, ändern Sie die Firewalleinstellung des Kontos wieder zurück in **Ausgewählte Netzwerke**.

Azure HPC Cache verwendet nicht das virtuelle Dienstnetzwerk, um auf das fertige Speicherziel zuzugreifen.

Wenn Sie Hilfe bei dieser Problemumgehung benötigen, [wenden Sie sich an Microsoft Service and Support](hpc-cache-support-ticket.md).
