---
title: Verwenden von Azure Security Center zum Härten Ihrer Docker-Hosts und Schützen der Container
description: Schützen Ihrer Docker-Hosts und Sicherstellen, dass sie mit dem CIS-Docker-Benchmark konform sind
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: b30e08a2739000d2a7ec14a95742f2654e1d2ea1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98916233"
---
# <a name="harden-your-docker-hosts"></a>Härten Ihrer Docker-Hosts

Azure Security Center identifiziert nicht verwaltete Container, die auf IaaS-Linux-VMs oder anderen Linux-Computern gehostet werden, auf denen Docker-Container ausgeführt werden. Security Center bewertet kontinuierlich die Konfigurationen dieser Container. Anschließend werden sie mit dem [Docker-Benchmark von Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/docker/) verglichen.

Security Center umfasst den gesamten Regelsatz des CIS-Docker-Benchmark und benachrichtigt Sie, sobald Ihre Container eine der Steuerungen nicht erfüllen. Werden Fehlkonfigurationen gefunden, generiert Security Center Sicherheitsempfehlungen. Verwenden Sie die Seite **Empfehlungen** in Security Center, um Empfehlungen anzuzeigen und Probleme zu beheben.

Wenn Sicherheitsrisiken gefunden wurden, werden diese in einer einzigen Empfehlung gruppiert.

>[!NOTE]
> Diese CIS-Benchmarkprüfungen können nicht auf von AKS oder Databricks verwalteten virtuellen Computern ausgeführt werden.

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|Erfordert [Azure Defender für Server](defender-for-servers-introduction.md)|
|Erforderliche Rollen und Berechtigungen:|**Leser** in dem Arbeitsbereich, mit dem der Host verbunden wird|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||

## <a name="identify-and-remediate-security-vulnerabilities-in-your-docker-configuration"></a>Identifizieren und Beheben von Sicherheitsrisiken in der Docker-Konfiguration

1. Öffnen Sie über das Menü von Security Center die Seite **Empfehlungen**.

1. Filtern Sie nach der Empfehlung **Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden**, und wählen Sie diese Empfehlung aus.

    Auf der Empfehlungsseite werden die betroffenen Ressourcen (Docker-Hosts) angezeigt. 

    :::image type="content" source="./media/monitor-container-security/docker-host-vulnerabilities-found.png" alt-text="Empfehlung zum Beseitigen von Sicherheitsrisiken in Containersicherheitskonfigurationen":::

1. Um die CIS-Steuerelemente für einen bestimmten Host, bei dem ein Fehler aufgetreten ist, anzuzeigen und zu beheben, wählen Sie den zu untersuchenden Host aus. 

    > [!TIP]
    > Wenn Sie auf der Seite für den Ressourcenbestand begonnen haben und diese Empfehlung von dort aus erreicht haben, wählen Sie die Schaltfläche **Aktion ausführen** auf der Empfehlungsseite aus.
    >
    > :::image type="content" source="./media/monitor-container-security/host-security-take-action-button.png" alt-text="Schaltfläche „Aktion ausführen“ zum Starten von Log Analytics":::

    Log Analytics wird geöffnet und zeigt einen benutzerdefinierten Vorgang, der direkt ausgeführt werden kann. Die standardmäßige benutzerdefinierte Abfrage enthält eine Liste aller fehlerhaften Regeln, die bewertet wurden, sowie Anleitungen zum Beheben der Probleme.

    :::image type="content" source="./media/monitor-container-security/docker-host-vulnerabilities-in-query.png" alt-text="Log Analytics-Seite mit der Abfrage, mit der alle fehlerhaften CIS-Steuerelemente angezeigt werden":::

1. Optimieren Sie die Abfrageparameter bei Bedarf.

1. Wenn Sie sicher sind, dass der Befehl für Ihren Host geeignet und bereit ist, wählen Sie **Ausführen** aus.


## <a name="next-steps"></a>Nächste Schritte

Die Docker-Härtung ist nur ein Aspekt der Containersicherheitsfeatures von Security Center. 

Erfahren Sie mehr über [Containersicherheit in Security Center](container-security.md).