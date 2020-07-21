---
title: Diagnose zur Problembehandlung der Konnektivität in der SSIS Integration Runtime
description: Dieser Artikel enthält Anleitungen zur Problembehandlung bei der Diagnose der Konnektivität in der SSIS Integration Runtime.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: meiyl
author: meiyl
ms.reviewer: sawinark
manager: yidetu
ms.date: 06/07/2020
ms.openlocfilehash: 8e520048a6067f134e847953f4f4aa0598d9926e
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86172455"
---
# <a name="troubleshoot-diagnose-connectivity-in-the-ssis-integration-runtime"></a>Problembehandlung bei Diagnose der Konnektivität in der SSIS Integration Runtime

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Wenn beim Ausführen von SSIS-Paketen in der SSIS Integration Runtime Konnektivitätsprobleme auftreten, insbesondere, wenn Ihre SSIS Integration Runtime mit dem virtuellen Azure-Netzwerk verbunden worden ist. Sie können versuchen, mithilfe dieses Features zur Verbindungsdiagnose auf der Seite zur Überwachung der SSIS Integration Runtime des Azure Data Factory-Portals Probleme selbst zu diagnostizieren. 

 ![Überwachungsseite – Diagnose der Konnektivität](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-diagnose-connectivity.png) ![Überwachungsseite – Testverbindung](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-test-connection.png)
 
In diesem Artikel werden die häufigsten Fehler behandelt, die Sie beim Testen der Verbindung in der SSIS Integration Runtime finden könnten. Beschrieben werden die möglichen Ursachen und Maßnahmen zur Behebung der Fehler. 

## <a name="common-errors-potential-causes-and-recommendation-solutions"></a>Häufige Fehler, potenzielle Ursachen und Lösungsempfehlungen

### <a name="error-code-invalidinput"></a>Fehlercode: InvalidInput.
* **Fehlermeldung**: Überprüfen Sie, ob die Eingabe korrekt ist.
* **Mögliche Ursachen:** Ihre Eingabe ist falsch.
* **Empfehlung:** Überprüfen Sie Ihre Eingabe.

### <a name="error-code-firewallornetworkissue"></a>Fehlercode: FirewallOrNetworkIssue.
* **Fehlermeldung**: Überprüfen Sie, ob dieser Port auf der Firewall/dem Server/der NSG geöffnet und das Netzwerk stabil ist.
* **Mögliche Ursachen:** 
  * Der Server öffnet diesen Port nicht.
  * Ihrer Netzwerksicherheitsgruppe wird der ausgehende Datenverkehr auf diesem Port verweigert.
  * Dieser Port wird von ihrer NVA/Azure-Firewall/lokalen Firewall nicht geöffnet.
* **Empfehlung:** 
  * Öffnen Sie diesen Port auf dem Server.
  * Aktualisieren Sie die Netzwerksicherheitsgruppe, sodass ausgehender Datenverkehr auf diesem Port zugelassen ist.
  * Öffnen Sie diesen Port auf der NVA/Azure-Firewall/lokalen Firewall.

### <a name="error-code-misconfigureddnssettings"></a>Fehlercode: MisconfiguredDnsSettings.
* **Fehlermeldung**: Wenn Sie Ihren eigenen DNS-Server in dem VNET verwenden, das mit Ihrer Azure-SSIS IR verknüpft ist, überprüfen Sie, ob er Ihren Hostnamen auflösen kann.
* **Mögliche Ursachen:** 
  *  Das Problem des benutzerdefinierten DNS
  *  Sie verwenden keinen vollständig qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) für Ihren privaten Hostnamen.
* **Empfehlung:** 
  *  Beheben Sie Ihr Problem mit dem benutzerdefinierten DNS, um sicherzustellen, dass er den Hostnamen auflösen kann.
  *  Verwenden Sie einen voll qualifizierten Domänennamen (Fully Qualified Domain Name, FQDN), z. B. <Ihr_privater_Server>.contoso.com statt <Ihr_privater_Server>, da die Azure-SSIS IR nicht automatisch Ihr eigenes DNS-Suffix anfügen wird.

### <a name="error-code-servernotallowremoteconenction"></a>Fehlercode: ServerNotAllowRemoteConenction.
* **Fehlermeldung**: Überprüfen Sie, ob der Server TCP-Remoteverbindungen über diesen Port zulässt.
* **Mögliche Ursachen:** 
  *  Ihre Serverfirewall lässt keine TCP-Remoteverbindungen zu.
  *  Ihr Server ist nicht online.
* **Empfehlung:** 
  *  Lassen Sie TCP-Remoteverbindungen über die Serverfirewall zu.
  *  Starten Sie den Server.
   
### <a name="error-code-misconfigurednsgsettings"></a>Fehlercode: MisconfiguredNsgSettings.
* **Fehlermeldung**: Stellen Sie sicher, dass die NSG Ihres VNET ausgehenden Datenverkehr über diesen Port zulässt. Wenn Sie Azure ExpressRoute und/oder eine UDR verwenden, überprüfen Sie, ob dieser Port auf Ihrer Firewall/Ihrem Server geöffnet ist.
* **Mögliche Ursachen:** 
  *  Ihrer Netzwerksicherheitsgruppe wird der ausgehende Datenverkehr auf diesem Port verweigert.
  *  Dieser Port wird von ihrer NVA/Azure-Firewall/lokalen Firewall nicht geöffnet.
* **Empfehlung:** 
  *  Aktualisieren Sie die Netzwerksicherheitsgruppe, sodass ausgehender Datenverkehr auf diesem Port zugelassen ist.
  *  Öffnen Sie diesen Port auf der NVA/Azure-Firewall/lokalen Firewall.

### <a name="error-code-genericissues"></a>Fehlercode: GenericIssues.
* **Fehlermeldung**: Fehler bei Testverbindung aufgrund generischer Probleme.
* **Mögliche Ursachen:** Bei der Testverbindung ist ein allgemeines vorübergehendes Problem aufgetreten.
* **Empfehlung:** Wiederholen Sie die Testverbindung später. Wenden Sie sich an das Azure Data Factory-Supportteam, wenn auch bei der Wiederholung Probleme auftreten.


### <a name="error-code-pspingexecutiontimeout"></a>Fehlercode: PSPingExecutionTimeout.
* **Fehlermeldung**: Timeout bei Testverbindung, versuchen Sie es später noch mal.
* **Mögliche Ursachen:** Timeout bei Testen der Konnektivität.
* **Empfehlung:** Wiederholen Sie die Testverbindung später. Wenden Sie sich an das Azure Data Factory-Supportteam, wenn auch bei der Wiederholung Probleme auftreten.

### <a name="error-code-networkinstable"></a>Fehlercode: NetworkInstable.
* **Fehlermeldung**: Die Testverbindung ist wg. Netzwerkinstabilität unregelmäßig.
* **Mögliche Ursachen:** Vorübergehendes Netzwerkproblem.
* **Empfehlung:** Überprüfen Sie, ob das Server- oder Firewallnetzwerk stabil ist.

## <a name="next-steps"></a>Nächste Schritte

- Stellen Sie Ihre Pakete bereit. Weitere Informationen finden Sie unter [Bereitstellen eines SSIS-Projekts mit SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms).
- Führen Sie Ihre Pakete aus. Weitere Informationen finden Sie unter [Ausführen von SSIS-Paketen in Azure mit SSMS](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms).
- Bestimmen Sie einen Zeitplan für Ihre Pakete. Weitere Informationen finden Sie unter [Planen der Ausführung eines SSIS-Pakets in Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15).

