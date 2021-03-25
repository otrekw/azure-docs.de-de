---
title: 'Automatische Azure-Verwaltung für virtuelle Computer: Häufig gestellte Fragen'
description: Antworten auf häufig gestellte Fragen zur automatischen Azure-Verwaltung für virtuelle Computer.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: troubleshooting
ms.date: 02/22/2021
ms.author: deanwe
ms.openlocfilehash: 0b4e116210cf68dc672122ad4ddc98f85067f3b8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101688007"
---
# <a name="frequently-asked-questions-for-azure-automanage-for-vms"></a>Häufig gestellte Fragen zur automatischen Azure-Verwaltung für VMs

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur [automatischen Azure-Verwaltung für virtuelle Computer](automanage-virtual-machines.md).

Suchen Sie in den Azure-Foren bei [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder [auf Twitter an @AzureSupport senden](https://twitter.com/AzureSupport). Sie können auch eine Azure-Supportanfrage senden. Um eine Supportanfrage zu senden, wählen Sie auf der [Azure-Supportseite](https://azure.microsoft.com/support/options/) die Option **Support erhalten** aus.


## <a name="azure-automanage-for-virtual-machines"></a>Automatische Azure-Verwaltung für virtuelle Computer

**Welche Voraussetzungen müssen erfüllt sein, um die automatische Azure-Verwaltung zu aktivieren?**

Die folgenden Voraussetzungen müssen für die Aktivierung der automatischen Azure-Verwaltung erfüllt sein:
- Unterstützte [Windows Server-Versionen](automanage-windows-server.md#supported-windows-server-versions) und [Linux-Distributionen](automanage-linux.md#supported-linux-distributions-and-versions)
- VMs müssen sich in einer unterstützten Region befinden
- Benutzer muss über die richtigen Berechtigungen verfügen
- Nur VMs, die keiner Skalierungsgruppe angehören
- Automatische Verwaltung unterstützt zurzeit keine Sandboxabonnements.

**Welche Azure RBAC-Berechtigung ist erforderlich, um die automatische Azure-Verwaltung zu aktivieren?**

Wenn Sie Automanage auf einem virtuellen Computer mit einem vorhandenen Automanage-Konto aktivieren, benötigen Sie die Rolle „Mitwirkender“ für die Ressourcengruppe, in der sich der virtuelle Computer befindet.

Wenn Sie beim Aktivieren ein neues Automanage-Konto verwenden, müssen Sie entweder über die Rolle „Besitzer“ oder über die Rollen „Mitwirkender“ und „Benutzerzugriffsadministrator“ für das Abonnement verfügen.


**Welche Regionen werden unterstützt?**

Die vollständige Liste der unterstützten Regionen finden Sie [hier](./automanage-virtual-machines.md#supported-regions).


**Welche Funktionen automatisiert die automatische Azure-Verwaltung?**

Die automatische Azure-Verwaltung registriert, konfiguriert und überwacht während des gesamten Lebenszyklus der VM die [hier](automanage-virtual-machines.md) aufgeführten Dienste.

**Funktioniert Azure Automanage mit Azure Arc-fähigen VMs?**

Automanage unterstützt zurzeit keine Arc-fähigen VMs.

**Kann ich Konfigurationen für die automatische Azure-Verwaltung anpassen?**

Kunden können die Einstellungen für bestimmte Dienste, z. B. die Datenaufbewahrung von Azure Backup, durch Konfigurationseinstellungen anpassen. Eine vollständige Liste der Einstellungen, die geändert werden können, finden Sie [hier](automanage-virtual-machines.md#customizing-an-environment-using-preferences) in der Dokumentation.


**Funktioniert die automatische Azure-Verwaltung sowohl mit Linux- als auch mit Windows-VMs?**

Ja, schauen Sie sich die unterstützten [Windows Server-Versionen](automanage-windows-server.md#supported-windows-server-versions) und [Linux-Distributionen](automanage-linux.md#supported-linux-distributions-and-versions) an.


**Kann ich die automatische Verwaltung selektiv nur auf eine Reihe von VMs anwenden?**

Die automatische Verwaltung kann durch einfaches Klicken und Zeigen auf ausgewählte neue und vorhandene VMs aktiviert werden. Die automatische Verwaltung kann auch jederzeit deaktiviert werden.


**Unterstützt die automatische Azure-Verwaltung VMs in einer VM-Skalierungsgruppe?**

Nein, die automatische Azure-Verwaltung unterstützt derzeit keine VMs in einer VM-Skalierungsgruppe.


**Wie viel kostet die automatische Azure-Verwaltung?**

Die automatische Azure-Verwaltung ist ohne zusätzliche Kosten in der öffentlichen Vorschauversion erhältlich. Angefügte Azure-Ressourcen, z. B. Azure Backup, werden Kosten verursachen.


**Kann ich die automatische Azure-Verwaltung über Azure Policy anwenden?**

Ja, wir haben eine integrierte Richtlinie erstellt, die die automatische Verwaltung auf alle VMs innerhalb des von Ihnen definierten Bereichs automatisch anwendet. Außerdem geben Sie die Umgebungskonfiguration (Dev/Test oder Produktion) zusammen mit Ihrem Automanage-Konto an. Weitere Informationen über die Aktivierung der automatischen Verwaltung über Azure Policy finden Sie [hier](virtual-machines-policy-enable.md).


**Was ist ein Konto für die automatische Verwaltung?**

Das Konto für die automatische Verwaltung ist eine verwaltete Dienstidentität (Managed Service Identity, MSI), die den Sicherheitskontext oder die Identität bereitstellt, unter der die automatisierten Vorgänge erfolgen.


**Wirkt sich die Aktivierung von Automanage auf zusätzliche VMs außer den von mir ausgewählten VMs aus?**

Wenn Ihre VM mit einem vorhandenen Log Analytics-Arbeitsbereich verknüpft ist, verwenden wir diesen Arbeitsbereich, um diese Lösungen anzuwenden: Änderungsnachverfolgung, Bestand und Updateverwaltung. Auf allen mit diesem Arbeitsbereich verbundenen VMs werden diese Lösungen aktiviert.


**Kann ich die Umgebung meiner VM ändern?**

Sie müssen derzeit Automanage für diese VM deaktivieren und dann mit der gewünschten Umgebung und den gewünschten Voreinstellungen erneut aktivieren.


**Wenn meine VM bereits für einen Dienst wie die Updateverwaltung konfiguriert ist, wird sie von der automatischen Verwaltung neu konfiguriert?**
Nein, die automatische Verwaltung wird sie nicht erneut konfigurieren. Wir werden damit beginnen, die mit diesem Dienst verbundenen Ressourcen auf Datendrifts zu überwachen.


**Warum weist meine VM im Portal für die automatische Verwaltung den Status „Fehler“ auf?**

Wenn der Status *Fehler* angezeigt wird, können Sie die Bereitstellung über die Ressourcengruppe beheben, in der sich Ihre VM befindet. Wechseln Sie zu **Ressourcengruppen**, wählen Sie Ihre Ressourcengruppe aus, klicken Sie auf **Bereitstellungen**, und beachten Sie dort den Status *Fehler* zusammen mit Fehlerdetails.

**Wie erhalte ich Unterstützung bei der Problembehandlung für die automatische Verwaltung?**

Sie können ein [Ticket für den technischen Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) einreichen. Für die Option **Dienst** suchen und wählen Sie *Automatische Verwaltung* unter dem Abschnitt *Überwachung und Verwaltung* aus.


## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, die automatische Verwaltung für virtuelle Computer im Azure-Portal zu aktivieren.

> [!div class="nextstepaction"]
> [Aktivieren der automatischen Verwaltung für virtuelle Computer im Azure-Portal](quick-create-virtual-machines-portal.md)