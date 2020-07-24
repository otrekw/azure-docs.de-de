---
title: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Automation
description: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Automation
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 07/09/2020
ms.subservice: ''
ms.openlocfilehash: a7ff659eb6fc204208c84146a2fc33c8278f7154
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207280"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-automation-preview"></a>Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Automation (Vorschauversion)

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem VNet und bindet den Automation-Dienst dadurch in Ihr VNet ein. Der Netzwerkdatenverkehr zwischen den Computern im VNet und dem Automation-Konto wird über das VNet und eine private Verbindung im Microsoft-Backbone-Netzwerk geleitet, sodass keine Offenlegung im öffentlichen Internet erfolgt.

Angenommen, Sie verfügen über ein VNet, in dem der ausgehende Internetzugriff deaktiviert ist. Sie möchten jedoch privat auf Ihr Automation-Konto zugreifen und Automation-Funktionen wie Webhooks, State Configuration und Runbookaufträge für Hybrid Runbook Worker verwenden. Außerdem möchten Sie, dass Benutzer nur über das VNet auf das Automation-Konto zugreifen können.  Diese Ziele werden durch die Bereitstellung eines privaten Endpunkts erreicht.

In diesem Artikel wird erläutert, wann und wie Sie einen privaten Endpunkt mit Ihrem Automation-Konto (Vorschauversion) einrichten.

![Konzeptionelle Übersicht von Private Link für Azure Automation](./media/private-link-security/private-endpoints-automation.png)

>[!NOTE]
> Die Unterstützung für Private Link mit Azure Automation (Vorschauversion) ist nur in Azure Commercial-Clouds und Clouds der US-Regierungsbehörden verfügbar.

## <a name="advantages"></a>Vorteile

Private Link bietet folgende Möglichkeiten:

- Herstellen einer privaten Verbindung mit Azure Automation ohne öffentlichen Netzwerkzugriff.
- Herstellen einer privaten Verbindung mit einem Azure Monitor Log Analytics-Arbeitsbereich ohne öffentlichen Netzwerkzugriff.

    >[!NOTE]
    >Dies ist erforderlich, wenn Ihr Automation-Konto mit einem Log Analytics-Arbeitsbereich verknüpft ist, um Auftragsdaten weiterzuleiten, und wenn Sie Funktionen wie Updateverwaltung, Änderungsnachverfolgung und Bestand, State Configuration oder „VMs außerhalb der Geschäftszeiten starten/beenden“ aktiviert haben. Weitere Informationen zu Private Link für Azure Monitor finden Sie unter [Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor](../../azure-monitor/platform/private-link-security.md).

- Sicherstellen, dass der Zugriff auf Ihre Automation-Daten nur über autorisierte private Netzwerke erfolgt.
- Verhindern von Datenexfiltration aus Ihren privaten Netzwerken durch Definieren Ihrer Azure Automation-Ressource, die eine Verbindung über Ihren privaten Endpunkt herstellt.
- Herstellen einer sicheren Verbindung zwischen Ihrem privaten lokalen Netzwerk und Azure Automation mittels ExpressRoute und Private Link.
- Kapseln des gesamten Datenverkehrs innerhalb des Microsoft Azure-Backbonenetzwerks.

Weitere Informationen finden Sie unter [Hauptvorteile von Private Link](../../private-link/private-link-overview.md#key-benefits).

## <a name="how-it-works"></a>Funktionsweise

Azure Automation Private Link stellt eine Verbindung zwischen einem oder mehreren privaten Endpunkten (und damit auch den virtuellen Netzwerke, in denen sich die Endpunkte befinden) und Ihrer Azure Automation-Ressource her. Diese Endpunkte sind Computer, die Webhooks verwenden, um Runbooks zu starten, Computer, die die „Hybrid Runbook Worker“-Rolle hosten, und DSC-Knoten.

Nachdem Sie private Endpunkte für Automation erstellt haben, wird jede der öffentlichen Automation-URLs, die Sie oder ein Computer direkt kontaktieren können, einem privaten Endpunkt in Ihrem VNet zugeordnet.

Im Rahmen der Vorschauversion kann ein Automation-Konto nicht auf Azure-Ressourcen zugreifen, die mithilfe eines privaten Endpunkts geschützt werden. Beispiele: Azure Key Vault, Azure SQL, Azure Storage-Konto usw.

### <a name="webhook-scenario"></a>Webhook-Szenario

Sie können Runbooks starten, indem Sie einen POST in der Webhook-URL ausführen. Die URL sieht beispielsweise wie folgt aus: `https://<automationAccountId>.webhooks.<region>.azure-automation.net/webhooks?token=gzGMz4SMpqNo8gidqPxAJ3E%3d`.

### <a name="state-configuration-agentsvc-scenario"></a>State Configuration-Szenario (agentsvc)

State Configuration bietet Ihnen den Azure-Dienst für die Konfigurationsverwaltung, mit dem Sie PowerShell DSC-Konfigurationen (Desired State Configuration) schreiben, verwalten und kompilieren können – für Knoten in der Cloud oder in einem lokalen Rechenzentrum.

Der Agent auf dem Computer wird beim DSC-Dienst registriert und verwendet dann den Dienstendpunkt zum Abrufen der DSC-Konfiguration. Der Agent-Dienstendpunkt sieht wie folgt aus: `https://<automationAccountId>.agentsvc.<region>.azure-automation.net`.

Die URL für einen öffentlichen und privaten Endpunkt wäre identisch, würde aber einer privaten IP-Adresse zugeordnet, wenn Private Link aktiviert ist.

## <a name="planning-based-on-your-network"></a>Planung basierend auf dem Netzwerk

Bevor Sie Ihre Automation-Kontoressource einrichten, überprüfen Sie Ihre Anforderungen an die Netzwerkisolation. Bewerten Sie den Zugriff Ihrer virtuellen Netzwerke auf das öffentliche Internet sowie die Zugriffsbeschränkungen für Ihr Automation-Konto (einschließlich des Einrichtens eines Private Link-Gruppenbereichs für Azure Monitor-Protokolle, falls diese in Ihr Automation-Konto integriert sind). Nehmen Sie außerdem eine Überprüfung der [DNS-Einträge](./automation-region-dns-records.md) des Automation-Diensts in Ihren Plan auf, um sicherzustellen, dass die unterstützten Features ohne Probleme funktionieren.

### <a name="connect-to-a-private-endpoint"></a>Herstellen einer Verbindung mit einem privaten Endpunkt

Erstellen Sie einen privaten Endpunkt, um eine Verbindung mit unserem Netzwerk herzustellen. Sie können ihn im [Private Link-Center des Azure-Portals](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints) erstellen. Nachdem Ihre Änderungen an „publicNetworkAccess“ und Private Link übernommen wurden, kann es bis zu 35 Minuten dauern, bis sie wirksam werden.

In diesem Abschnitt erstellen Sie einen privaten Endpunkt für Ihre Automation-Konto.

1. Wählen Sie oben links im Bildschirm **Ressource erstellen > Netzwerk > Private Link-Center (Vorschau)** aus.

2. Wählen Sie unter **Private Link-Center – Übersicht** bei der Option **Private Verbindung mit einem Dienst herstellen** den Befehl **Starten** aus.

3. Geben Sie unter **Virtuellen Computer erstellen – Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name | Geben Sie Ihren *PrivateEndpoint* ein. |
    | Region | Wählen Sie **YourRegion** aus. |
    |||

4. Klicken Sie auf **Weiter: Ressource** aus.

5. Geben Sie unter **Privaten Endpunkt erstellen – Ressource** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.Automation/automationAccounts** aus. |
    | Resource |Wählen Sie *myAutomationAccount* aus.|
    |Unterressource des Ziels |Wählen Sie je nach Ihrem Szenario *Webhook* oder *DSCAndHybridWorker* aus.|
    |||

6. Klicken Sie auf **Weiter: Konfiguration** aus.

7. Geben Sie unter **Privaten Endpunkt erstellen – Konfiguration** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |**NETZWERK**| |
    | Virtuelles Netzwerk| Wählen Sie *MyVirtualNetwork* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    |**PRIVATE DNS-INTEGRATION**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(New)privatelink.azure-automation.net* aus. |
    |||

8. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

9. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Wählen Sie im **Private Link-Center (Vorschau)** die Option **Private Endpunkte** aus, um Ihre Private Link-Ressource anzuzeigen.

![Automation-Ressource von Private Link](./media/private-link-security/private-link-automation-resource.png)

Wählen Sie die Ressource aus, um alle Details anzuzeigen. Hierdurch wird ein neuer privater Endpunkt für Ihr Automation-Konto erstellt und diesem eine private IP-Adresse aus Ihrem virtuellen Netzwerk zugewiesen. Der **Verbindungsstatus** lautet **Genehmigt**.

Ähnlich wird ein eindeutiger vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) für die State Configuration (agentsvc) und für die Hybrid Runbook Worker-Auftragslaufzeit (jrds) erstellt. Jedem davon wird von Ihrem VNet eine gesonderte IP-Adresse zugewiesen, und der **Verbindungsstatus** lautet **Genehmigt**.

Wenn der Dienstconsumer über RBAC-Berechtigungen für die Automation-Ressource verfügt, kann er die Automation-Genehmigungsmethode auswählen. In diesem Fall ist keine Aktion des Dienstanbieters erforderlich, wenn die Anforderung die Automation-Anbieterressource erreicht, und die Verbindung wird automatisch genehmigt.

## <a name="set-public-network-access-flags"></a>Einrichten von Netzwerkzugriffsflags

Sie können ein Automation-Konto so konfigurieren, dass es alle öffentlichen Konfigurationen verweigert und nur Verbindungen über private Endpunkte zulässt, um die Netzwerksicherheit weiter zu erhöhen. Wenn Sie den Zugriff auf das Automation-Konto so einschränken möchten, dass nur noch aus dem VNet darauf zugegriffen werden kann, und nicht über das öffentliche Internet, können Sie die Eigenschaft `publicNetworkAccess` auf `$true` festlegen.

Wenn die Einstellung **Zugriff auf öffentliches Netzwerk verweigern** auf `true` festgelegt ist, sind nur Verbindungen über private Endpunkte zulässig, und alle Verbindungen über öffentliche Endpunkte werden mit einer Fehlermeldung verweigert.

Das folgende PowerShell-Skript zeigt, wie Sie die **Public Network Access**-Eigenschaft auf der Ebene des Automation-Kontos mit `Get` und `Set` abrufen und festlegen können:

```powershell
$account = Get-AzResource -ResourceType Microsoft.Automation/automationAccounts -ResourceGroupName "<resourceGroupName>" -Name "<automationAccountName>" -ApiVersion "2020-01-13-preview"
$account.Properties | Add-Member -Name 'publicNetworkAccess' -Type NoteProperty -Value $true
$account | Set-AzResource -Force -ApiVersion "2020-01-13-preview"
```

## <a name="dns-configuration"></a>DNS-Konfiguration

Wenn Sie eine Verbindung mit einer Private Link-Ressource über einen voll qualifizierten Domänennamen (FQDN) als Teil der Verbindungszeichenfolge herstellen, ist es wichtig, Ihre DNS-Einstellungen so zu konfigurieren, dass sie in die zugewiesene private IP-Adresse aufgelöst werden. Bestehende Azure-Dienste verfügen möglicherweise bereits über eine DNS-Konfiguration, die beim Herstellen einer Verbindung über einen öffentlichen Endpunkt verwendet werden kann. Ihre DNS-Konfiguration sollte überprüft und aktualisiert werden, um eine Verbindung mithilfe Ihres privaten Endpunkts herzustellen.

Die Netzwerkschnittstelle, die dem privaten Endpunkt zugeordnet ist, enthält alle Informationen, die zur Konfiguration Ihres DNS erforderlich sind, einschließlich FQDN und privater IP-Adressen, die einer bestimmten Private Link-Ressource zugewiesen wurden.

Mit den folgenden Optionen können Sie Ihre DNS-Einstellungen für private Endpunkte konfigurieren:

* Verwenden Sie die Hostdatei (wird nur für Tests empfohlen). Sie können die Hostdatei auf einem virtuellen Computer verwenden, um die Verwendung des DNS als erste Möglichkeit für die Namensauflösung außer Kraft zu setzen.

* Verwenden Sie eine [private DNS-Zone](../../dns/private-dns-privatednszone.md). Sie können Private DNS-Zonen verwenden, um die DNS-Auflösung für einen bestimmten privaten Endpunkt außer Kraft zu setzen. Eine private DNS-Zone kann mit Ihrem virtuellen Netzwerk verknüpft werden, um bestimmte Domänen aufzulösen. Damit der Agent auf Ihrem virtuellen Computer über den privaten Endpunkt kommunizieren kann, erstellen Sie einen Private DNS-Eintrag als `privatelink.azure-automation.net`. Fügen Sie der IP-Adresse des privaten Endpunkts einen neuen DNS *A*-Eintragszuordnung hinzu.

* Verwenden Sie Ihre DNS-Weiterleitung (optional). Sie können Ihre DNS-Weiterleitung verwenden, um die DNS-Auflösung für eine bestimmte Private Link-Ressource zu überschreiben. Wenn Ihr [DNS-Server](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) in einem virtuellen Netzwerk gehostet wird, können Sie zum Verwenden einer privaten DNS-Zone eine DNS-Weiterleitungsregel erstellen, um die Konfiguration für alle Private Link-Ressourcen zu vereinfachen.

Weitere Informationen finden Sie unter [DNS-Konfiguration für private Azure-Endpunkte](../../private-link/private-endpoint-dns.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist ein privater Endpunkt in Azure?](../../private-link/private-endpoint-overview.md).