---
title: Bedrohungserkennung für virtuelle Computer und Server in Azure Security Center | Microsoft-Dokumentation
description: In diesem Thema werden die in Azure Security Center verfügbaren Warnungen für virtuelle Computer und Server vorgestellt.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: b101fd30ddbdbf0f8fdf6e02394cb10b9af5f4b0
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666362"
---
# <a name="threat-detection-for-vms-and-servers-in-azure-security-center"></a>Bedrohungserkennung für virtuelle Computer und Server in Azure Security Center

In diesem Thema werden die verschiedenen Arten von Erkennungsmethoden und Warnungen vorgestellt, die für virtuelle Computer und Server mit den folgenden Betriebssystemen verfügbar sind. Eine Liste mit den unterstützten Versionen finden Sie unter [Von Azure Security Center unterstützte Features und Plattformen](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Azure Security Center wird zur Überwachung und zum Schutz Ihrer Windows-basierten Computer in Azure-Dienste integriert. In Security Center werden die Warnungen und Behandlungsvorschläge aus allen diesen Diensten in einem benutzerfreundlichen Format präsentiert.

* **Microsoft Defender ATP** <a name="windows-atp"></a> – Security Center erweitert seine Plattformen für den Cloudworkloadschutz durch die Integration in Microsoft Defender Advanced Threat Protection (ATP). Dadurch stehen umfassende EDR-Funktionen (Endpoint Detection and Response; Endpunkterkennung und -reaktion) zur Verfügung.

    > [!NOTE]
    > Der Microsoft Defender ATP-Sensor wird automatisch auf Windows-Servern aktiviert, die Security Center verwenden.

    Wenn von Microsoft Defender ATP eine Bedrohung erkannt wird, wird eine Warnung ausgelöst. Die Warnung wird auf dem Security Center-Dashboard angezeigt. Über das Dashboard können Sie zur Microsoft Defender ATP-Konsole wechseln und eine detaillierte Untersuchung durchführen, um das Ausmaß des Angriffs zu ermitteln. Weitere Informationen zu Microsoft Defender ATP finden Sie unter [Integrieren von Servern in den Microsoft Defender ATP-Dienst](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

* **Absturzabbildanalyse** <a name="windows-dump"></a> – Beim Absturz von Software wird in einem Absturzabbild ein Teil des Arbeitsspeichers zum Zeitpunkt des Absturzes erfasst.

    Ein Absturz kann durch Schadsoftware verursacht worden sein oder Schadsoftware enthalten. Um einer Erkennung durch Sicherheitsprodukte zu entgehen, nutzen verschiedene Arten von Schadsoftware einen dateilosen Angriff. Dadurch werden Schreibvorgänge auf dem Datenträger sowie die Verschlüsselung von auf den Datenträger geschriebenen Softwarekomponenten vermieden. Diese Art von Angriff ist mit einem herkömmlichen datenträgerbasierten Ansatz nur schwer zu erkennen.

    Sie können diese Art von Angriff jedoch mithilfe einer Arbeitsspeicheranalyse erkennen. Durch die Analyse des Arbeitsspeichers im Absturzabbild kann Security Center Methoden erkennen, die der Angriff verwendet. Beispielsweise könnte der Angriff versuchen, Schwachstellen in der Software auszunutzen, auf vertrauliche Daten zuzugreifen und sich heimlich auf einem kompromittierten Computer einzunisten. Security Center erledigt diese Arbeit mit nur minimalen Leistungseinbußen bei den Hots.

    Eine Liste der Absturzabbildanalyse-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-crashdump).

* **Erkennung dateiloser Angriffe** <a name="windows-fileless"></a> – Dateilose Angriffe, die auf Ihre Endpunkte abzielen, sind gängig. Bei dateilosen Angriffen werden schädliche Nutzlasten in den Arbeitsspeicher injiziert, um einer Erkennung zu entgehen. Nutzlasten von Angreifern nisten sich im Arbeitsspeicher von kompromittierten Prozessen ein und führen ein breites Spektrum an schädlichen Aktivitäten aus.

    Die Erkennung dateiloser Angriffe erkennt dank automatisierter forensischer Techniken für den Arbeitsspeicher Toolkits, Techniken und Verhaltensweisen im Zusammenhang mit dateilosen Angriffen. Diese Lösung überprüft zur Laufzeit in regelmäßigen Abständen Ihren Computer und extrahiert Erkenntnisse direkt aus dem Arbeitsspeicher sicherheitskritischer Prozesse.

    Sie sucht nach Hinweisen auf Missbrauch, Codeinjektion oder die Ausführung schädlicher Nutzlasten. Die Erkennung dateiloser Angriffe generiert detaillierte Sicherheitswarnungen, um die Warnungsselektierung und Korrelation zu beschleunigen und Downstream-Reaktionszeiten zu verkürzen. Dieser Ansatz dient zur Ergänzung ereignisbasierter EDR-Lösungen und vergrößert das Erkennungsspektrum.

    Eine Liste der Warnungen bei Erkennung dateiloser Angriffe finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-filelessattackdetect).

> [!NOTE]
> Sie können Windows-Warnungen simulieren, indem Sie [dieses Sicherheitswarnungen](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046).

## Linux <a name="linux-machines"></a>

Security Center sammelt Überwachungsdatensätze von Linux-Computern mithilfe von **auditd** – einem der am häufigsten verwendeten Linux-Überwachungsframeworks. auditd befindet sich im Mainline-Kernel. 

* **Integration von Linux-AuditD-Warnungen und Microsoft Monitoring Agent (MMA)** <a name="linux-auditd"></a> – Beim auditd-System handelt es sich um ein Subsystem auf Kernelebene. Es dient zur Überwachung von Systemaufrufen, filtert diese Aufrufe nach einem bestimmten Regelsatz und schreibt entsprechende Meldungen in einen Socket. Security Center integriert Funktionen aus dem AuditD-Paket in Microsoft Monitoring Agent (MMA). Diese Integration ermöglicht das Sammeln von auditd-Ereignissen in allen unterstützten Linux-Distributionen ohne weitere Bedingungen.  

    auditd-Datensätze werden gesammelt, angereichert und mithilfe des Linux-MMA-Agents zu Ereignissen aggregiert. In Security Center werden ständig neue Analysen hinzugefügt, die Linux-Signale verwenden, um schädliches Verhalten auf cloudbasierten und lokalen Linux-Computern zu erkennen. Diese Analysen umfassen ähnlich wie bei Windows-Funktionen verdächtige Prozesse und Anmeldeversuche, das Laden von Kernmodulen sowie andere Aktivitäten, die darauf hindeuten, dass ein Computer angegriffen wird oder kompromittiert wurde.  

    Im Folgenden finden Sie mehrere Beispiele für Analysen, die verschiedene Phasen des Angriffslebenszyklus abdecken.

    Eine Liste der Linux-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-linux).

> [!NOTE]
> Sie können Linux-Warnungen simulieren, indem Sie das [ Azure Security Center-Playbook: Linux Detections (Linux-Erkennungsfunktionen)](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) herunterladen.

 
 ## <a name="next-steps"></a>Nächste Schritte

Beispiele und weitere Informationen zur Security Center-Erkennung finden Sie hier:

* [Leverage Azure Security Center to detect when compromised Linux machines attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Erkennen von Angriffen durch kompromittierte Linux-Computer mithilfe von Azure Security Center)
* [Azure Security Center can detect emerging vulnerabilities in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Erkennung neuer Sicherheitsrisiken in Linux durch Azure Security Center)
* [Leverage Azure Security Center to detect when compromised Linux machines attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Erkennen von Angriffen durch kompromittierte Linux-Computer mithilfe von Azure Security Center)
* [Azure Security Center can detect emerging vulnerabilities in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Erkennung neuer Sicherheitsrisiken in Linux durch Azure Security Center)