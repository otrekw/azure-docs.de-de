---
title: Hochverfügbarkeit – Azure-Dienst für dedizierte HSMs | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die grundlegenden Überlegungen zur Hochverfügbarkeit im Zusammenhang mit Azure Dedicated HSM. Dieser Artikel enthält ein Beispiel.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2021
ms.author: mbaldwin
ms.openlocfilehash: 85aa9d355b2440535ea13c8c15e95500bac22352
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98567258"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure-Dienst für dedizierte HSMs – Hochverfügbarkeit

Der Azure-Dienst für dedizierte HSMs wird durch die hoch verfügbaren Rechenzentren von Microsoft unterstützt. Alle hoch verfügbaren Rechenzentren sind jedoch anfällig für lokale Ausfälle und in extremen Fällen Ausfälle auf regionaler Ebene. Microsoft stellt HSM-Geräte in verschiedenen Rechenzentren innerhalb einer Region bereit, um sicherzustellen, dass bei der Bereitstellung mehrerer Geräte sich diese Geräte nicht auf einem gemeinsamen Rack befinden. Eine weitere Ebene der Hochverfügbarkeit kann durch die Rechenzentren in einer Region übergreifende Verbindung dieser HSMs mithilfe des Features „Thales-Hochverfügbarkeitsgruppe“ erreicht werden. Es ist auch möglich, Geräte Regionen übergreifend zu verbinden, um auf regionale Failover in einer Notfallwiederherstellung zu reagieren. Bei dieser Hochverfügbarkeitskonfiguration mit mehreren Ebenen erfolgt auf jeden Geräteausfall automatisch eine Reaktion, um die Ausführung der Anwendungen zu gewährleisten. Alle Rechenzentren verfügen auch vor Ort über Reservegeräte und -komponenten, sodass fehlerhaften Geräte rechtzeitig ersetzt werden können.

## <a name="high-availability-example"></a>Beispiel für Hochverfügbarkeit

Informationen zum Konfigurieren von HSM-Geräten für hohe Verfügbarkeit auf Softwareebene finden Sie im „Thales Luna 7-HSM-Administratorhandbuch“. Dieses Dokument ist auf der [Thales-HSM-Seite](https://thalesdocs.com/gphsm/Content/luna/network/luna_network_releases.htm) verfügbar.

Im folgenden Diagramm ist eine hoch verfügbare Architektur dargestellt. Sie verwendet mehrere Geräte in der Region und mehrere verbundene Geräte in einer separaten Region. Diese Architektur verwendet mindestens vier HSM-Geräte und virtuelle Netzwerkkomponenten.

![Hochverfügbarkeitsdiagramm](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, sich mit allen Schlüsselkonzepten des Diensts wie Hochverfügbarkeit und Sicherheit vor der Gerätebereitstellung und dem Entwurf oder der Bereitstellung von Anwendungen vertraut zu machen.
Weitere Themen auf Konzeptebene:

* [Bereitstellungsarchitektur](deployment-architecture.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)

Spezifische Details zum Konfigurieren von HSM-Geräten für hohe Verfügbarkeit finden Sie im Thales-Kundensupportportal für die Administratorhandbücher; siehe auch Abschnitt 6.
