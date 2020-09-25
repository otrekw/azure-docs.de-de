---
title: 'Automatische Azure-Verwaltung für virtuelle Computer: Häufig gestellte Fragen'
description: Antworten auf häufig gestellte Fragen zur automatischen Azure-Verwaltung für virtuelle Computer.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: troubleshooting
ms.date: 09/04/2020
ms.author: deanwe
ms.openlocfilehash: ed97f7861f5dd959fd41ac22b4e497f492dbc3a3
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931677"
---
# <a name="frequently-asked-questions-for-azure-automanage-for-vms"></a>Häufig gestellte Fragen zur automatischen Azure-Verwaltung für VMs

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur [automatischen Azure-Verwaltung für virtuelle Computer](automanage-virtual-machines.md).

Suchen Sie in den Azure-Foren bei [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder [auf Twitter an @AzureSupport senden](https://twitter.com/AzureSupport). Sie können auch eine Azure-Supportanfrage senden. Um eine Supportanfrage zu senden, wählen Sie auf der [Azure-Supportseite](https://azure.microsoft.com/support/options/) die Option **Support erhalten** aus.


## <a name="azure-automanage-for-virtual-machines"></a>Automatische Azure-Verwaltung für virtuelle Computer

**Welche Voraussetzungen müssen erfüllt sein, um die automatische Azure-Verwaltung zu aktivieren?**

Die folgenden Voraussetzungen müssen für die Aktivierung der automatischen Azure-Verwaltung erfüllt sein:
- Nur Windows Server-VMs
- VMs müssen aktiv sein
- VMs müssen sich in einer unterstützten Region befinden
- Benutzer muss über die richtigen Berechtigungen verfügen
- Nur VMs, die keiner Skalierungsgruppe angehören
- VMs dürfen nicht mit einem Log Analytics-Arbeitsbereich in einem anderen Abonnement verknüpft werden

**Welche RBAC-Berechtigung ist erforderlich, um die automatische Azure-Verwaltung zu aktivieren?**

Benutzer müssen über die Rolle „Besitzer“ verfügen. Alternativ können Benutzer die Rolle „Mitwirkender“ zusammen mit der Rolle „Benutzerzugriffsadministrator“ aufweisen, um die automatische Verwaltung anzuwenden.


**Welche Regionen werden unterstützt?**

Es werden VMs in den folgenden Regionen unterstützt: Europa (Westen), USA (Osten), USA (Westen 2), Kanada (Mitte), USA (Westen-Mitte).


**Welche Funktionen automatisiert die automatische Azure-Verwaltung?**

Die automatische Azure-Verwaltung registriert, konfiguriert und überwacht während des gesamten Lebenszyklus der VM die [hier](virtual-machines-best-practices.md) aufgeführten Dienste.


**Kann ich Konfigurationen für die automatische Azure-Verwaltung anpassen?**

Kunden können die Einstellungen für bestimmte Dienste, z. B. die Datenaufbewahrung von Azure Backup, durch Konfigurationseinstellungen anpassen. Eine vollständige Liste der Einstellungen, die geändert werden können, finden Sie [hier](virtual-machines-best-practices.md) in der Dokumentation.


**Funktioniert die automatische Azure-Verwaltung sowohl mit Linux- als auch mit Windows-VMs?**

Derzeit unterstützt die automatische Azure-Verwaltung Azure-VMs für Windows Server.


**Kann ich die automatische Verwaltung selektiv nur auf eine Reihe von VMs anwenden?**

Die automatische Verwaltung kann durch einfaches Klicken und Zeigen auf ausgewählte neue und vorhandene VMs aktiviert werden. Die automatische Verwaltung kann auch jederzeit deaktiviert werden.


**Unterstützt die automatische Azure-Verwaltung VMs in einer VM-Skalierungsgruppe?**

Nein, die automatische Azure-Verwaltung unterstützt derzeit keine VMs in einer VM-Skalierungsgruppe.


**Wie viel kostet die automatische Azure-Verwaltung?**

Die automatische Azure-Verwaltung ist ohne zusätzliche Kosten in der öffentlichen Vorschauversion erhältlich. Angefügte Azure-Ressourcen, z. B. Azure Backup, werden Kosten verursachen.


**Kann ich die automatische Azure-Verwaltung über Azure Policy anwenden?**

Ja, wir haben eine integrierte Richtlinie erstellt, die die automatische Verwaltung auf alle VMs innerhalb des von Ihnen definierten Bereichs automatisch anwendet. Außerdem werden Sie das Konfigurationsprofil (DevTest oder Produktion) zusammen mit Ihrem Konto für die automatische Verwaltung angeben. Weitere Informationen über die Aktivierung der automatischen Verwaltung über Azure Policy finden Sie [hier](virtual-machines-policy-enable.md).


**Was ist ein Konto für die automatische Verwaltung?**

Das Konto für die automatische Verwaltung ist eine verwaltete Dienstidentität (Managed Service Identity, MSI), die den Sicherheitskontext oder die Identität bereitstellt, unter der die automatisierten Vorgänge erfolgen.


**Kann ich das Konfigurationsprofil meiner VM ändern?**

Zu diesem Zeitpunkt müssen Sie die automatische Verwaltung für diese VM deaktivieren und dann die automatische Verwaltung mit dem gewünschten Konfigurationsprofil und den gewünschten Einstellungen erneut aktivieren.


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