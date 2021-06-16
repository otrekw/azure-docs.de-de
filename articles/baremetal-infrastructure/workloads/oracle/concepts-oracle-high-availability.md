---
title: Hochverfügbarkeit und Notfallwiederherstellung für Oracle in der Bare-Metal-Infrastruktur
description: Hier finden Sie Informationen zur Hochverfügbarkeit und Notfallwiederherstellung für Oracle in der Azure-Bare-Metal-Infrastruktur.
ms.topic: how-to
ms.subservice: baremetal-oracle
ms.date: 04/15/2021
ms.openlocfilehash: 3b23774604b5126321d85892647e73118e08260f
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110575613"
---
# <a name="high-availability-and-disaster-recovery-for-oracle-on-baremetal"></a>Hochverfügbarkeit und Notfallwiederherstellung für Oracle in der Bare-Metal-Infrastruktur

In diesem Artikel werden die Grundlagen der Hochverfügbarkeit und Notfallwiederherstellung behandelt. Anschließend erfahren Sie, wie Sie Hochverfügbarkeit und Notfallwiederherstellung in einer Oracle-Umgebung in der Bare-Metal-Infrastruktur erreichen.

## <a name="high-availability-vs-disaster-recovery"></a>Gegenüberstellung von Hochverfügbarkeit und Notfallwiederherstellung

Hochverfügbarkeit und Notfallwiederherstellung decken verschiedene Arten von Ausfällen ab. Dazu werden verschiedene Features und Optionen von Oracle Database genutzt.

Hochverfügbarkeit ermöglicht es einem System, mehrere Ausfälle zu überstehen, ohne dass die Benutzerfreundlichkeit der Anwendung leidet. Ein hochverfügbares System zeichnet sich in der Regel durch Folgendes aus:

- Redundante Hardware ohne Single Point of Failure
- Automatische Wiederherstellung nach nicht kritischen Ausfällen (beispielsweise ausgefallene Laufwerke oder fehlerhafte Netzwerkkabel)
- Möglichkeit zur Durchführung von Hardware- und Softwareänderungen ohne spürbare Auswirkungen auf die Verarbeitung
- Erfüllung oder Übertreffung der Ziele für Recovery Time Objectives (RTOs) und Recovery Point Objectives (RPOs)

Das gängigste, für Hochverfügbarkeit genutzte Oracle-Feature ist [Oracle Real Application Clusters (RAC)](https://docs.oracle.com/en/database/oracle/oracle-database/19/racad/introduction-to-oracle-rac.html#GUID-5A1B02A2-A327-42DD-A1AD-20610B2A9D92).

Die Notfallwiederherstellung dient zum Schutz vor nicht behebbaren lokalisierten Ausfällen, die Ihre primäre Hochverfügbarkeitsstrategie beeinträchtigen würden. Im Oracle-Ökosystem wird sie mittels Datenbankreplikation bereitgestellt und auch als [Oracle Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/19/sbydb/preface.html#GUID-B6209E95-9DA8-4D37-9BAD-3F000C7E3590) bezeichnet.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Hochverfügbarkeitsfeatures für Oracle:

> [!div class="nextstepaction"]
> [Hochverfügbarkeitsfeatures für Oracle in der Azure-Bare-Metal-Infrastruktur](high-availability-features.md)
