---
title: Festlegen des Bereichs für die Ermittlung von Servern auf VMware mit Azure Migrate
description: In diesem Artikel wird beschrieben, wie Sie den Suchbereich für die Bewertung und Migration von auf VMware gehosteten Servern mithilfe von Azure Migrate festlegen.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: how-to
ms.date: 03/13/2021
ms.openlocfilehash: 29ac42da6560a717f12cd256fdd71282e0bd313f
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104775356"
---
# <a name="set-discovery-scope-for-servers-in-vmware-environment"></a>Festlegen des Suchbereichs für Server in der VMware-Umgebung

In diesem Artikel wird beschrieben, wie Sie den Suchbereich für Server in der VMware-Umgebung in folgenden Szenarien einschränken:

- Ermitteln von Servern mit der [Azure Migrate-Appliance](migrate-appliance-architecture.md), wenn Sie das Tool „Azure Migrate: Ermittlung und Bewertung“ verwenden.
- Ermitteln von Servern mit der [Azure Migrate-Appliance](migrate-appliance-architecture.md), wenn Sie das Tool „Azure Migrate: Servermigration“ verwenden, um die Migration von Servern aus der VMware-Umgebung zu Azure ohne Agent durchzuführen.

Wenn Sie das Gerät einrichten, stellt es eine Verbindung mit vCenter Server her und startet die Ermittlung. Bevor Sie die Appliance mit vCenter Server verbinden, können Sie den Suchbereich auf bestimmte Rechenzentren oder Cluster, einen Clusterordner, Hosts, einen Hostordner oder einzelne Server in vCenter Server beschränken. Um den Bereich festzulegen, weisen Sie dem Konto, das die Appliance für den Zugriff auf vCenter Server verwendet, Berechtigungen zu.

## <a name="before-you-start"></a>Vorbereitung

Wenn Sie kein vCenter-Benutzerkonto eingerichtet haben, das Azure Migrate für die Ermittlung verwendet, tun Sie dies jetzt für die [Bewertung](./tutorial-discover-vmware.md#prepare-vmware) oder [Migration ohne Agent](./migrate-support-matrix-vmware-migration.md#agentless-migration).


## <a name="assign-permissions-and-roles"></a>Zuweisen von Berechtigungen und Rollen

Sie können Berechtigungen für VMware-Bestandsobjekte über eine der beiden folgenden Methoden zuweisen:

- Weisen Sie dem von der Appliance verwendeten Konto eine Rolle mit den erforderlichen Berechtigungen für die Objekte zu, die Sie in den Suchbereich einbeziehen möchten.
- Alternativ weisen Sie dem Konto die Rolle auf Rechenzentrumsebene zu, und geben Sie diese an die untergeordneten Objekte weiter. Weisen Sie dem Konto dann eine Rolle vom Typ **Kein Zugriff** für jedes Objekt zu, das nicht in den Bereich einbezogen werden soll. Diese Vorgehensweise wird jedoch nicht empfohlen. Sie ist mühsam und birgt das Risiko einer versehentlichen Preisgabe von Zugangsdaten, da jedem neuen untergeordneten Objekt automatisch die vom übergeordneten Objekt vererbten Zugangsrechte erteilt werden.

Sie können den Bereich der Bestandsermittlung nicht auf der Ebene der vCenter-Serverordner festlegen. Wenn Sie den Bereich für die Ermittlung auf Server in einem Serverordner festlegen müssen, erstellen Sie einen Benutzer, und gewähren Sie diesem Zugriff auf jeden einzelnen erforderlichen Server. Host- und Clusterordner werden unterstützt.


### <a name="assign-a-role-for-assessment"></a>Zuweisen einer Rolle für die Bewertung

1. Wenden Sie in dem für die Ermittlung verwendeten vCenter-Konto der Appliance die Rolle **Schreibgeschützt** für alle übergeordneten Objekte an, die Server hosten, die Sie ermitteln und bewerten möchten (Host, Cluster, Hostordner, Clusterordner, bis hin zum Rechenzentrum).
2. Geben Sie diese Berechtigungen an die untergeordneten Objekte in der Hierarchie weiter.

    ![Zuweisen von Berechtigungen](./media/tutorial-assess-vmware/assign-perms.png)

### <a name="assign-a-role-for-agentless-migration"></a>Zuweisen einer Rolle für Migration ohne Agent

1. Wenden Sie in dem für die Migration verwendeten vCenter-Konto der Appliance eine benutzerdefinierte Rolle mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) auf alle übergeordneten Objekte an, die Server hosten, die Sie ermitteln und migrieren möchten.
2. Sie können der Rolle einen Namen geben, der leichter zu identifizieren ist. Beispielsweise <em>Azure_Migrate</em>.

## <a name="work-around-for-server-folder-restriction"></a>Umgehen der Einschränkung für Serverordner

Derzeit können Sie mit dem Tool „Azure Migrate: Ermittlung und Bewertung“ keine Server ermitteln, wenn der Zugriff auf Ebene der vCenter-Serverordner gewährt wird. Wenn Sie die Ermittlung und Bewertung nach Serverordnern eingrenzen möchten, verwenden Sie diese Problemumgehung.

1. Weisen Sie reine Leseberechtigungen für alle Server in den Serverordnern zu, auf denen die Ermittlung und die Bewertung basieren sollen.
2. Gewähren Sie allen übergeordneten Objekten, die die Server hosten (Host, Cluster, Hostordner, Clusterordner, bis hin zum Rechenzentrum), schreibgeschützten Zugriff. Die Berechtigungen müssen nicht an alle untergeordneten Objekte weitergegeben werden.
3. Um die Anmeldeinformationen für die Ermittlung zu verwenden, wählen Sie das Rechenzentrum als **Sammlungsbereich** aus.


Durch die Einrichtung der rollenbasierten Zugriffssteuerung wird sichergestellt, dass das entsprechende vCenter-Benutzerkonto nur auf mandantenspezifische Server Zugriff hat.


## <a name="next-steps"></a>Nächste Schritte

[Einrichten der Appliance](how-to-set-up-appliance-vmware.md)