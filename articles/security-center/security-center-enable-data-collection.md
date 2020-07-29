---
title: Datensammlung in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel wird die Installation eines Log Analytics-Agents und das Festlegen eines Log Analytics-Arbeitsbereichs beschrieben, in dem die gesammelten Daten gespeichert werden.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 04/27/2020
ms.author: memildin
ms.openlocfilehash: f7aca2820e599c4f3dad364f1ea14eadc634a548
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519713"
---
# <a name="data-collection-in-azure-security-center"></a>Datensammlung in Azure Security Center
Security Center sammelt Daten von Ihren virtuellen Azure-Computern (VMs), VM-Skalierungsgruppen, IaaS-Containern und Azure-fremden Computern (auch lokal), um sie auf Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mit dem Log Analytics-Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in den Arbeitsbereich. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen und angemeldeter Benutzer.

Die Datensammlung ist erforderlich, um einen Einblick in fehlende Updates, falsch konfigurierte Sicherheitseinstellungen des Betriebssystems, den Status des Endpunktschutzes sowie Integritäts- und Bedrohungsschutz bereitzustellen. 

In diesem Artikel wird die Installation eines Log Analytics-Agents und das Festlegen eines Log Analytics-Arbeitsbereichs beschrieben, in dem die gesammelten Daten gespeichert werden. Beide Vorgänge sind erforderlich, um die Datensammlung aktivieren. 

> [!NOTE]
> - Die Datensammlung ist nur für Computeressourcen (virtuelle Computer, VM-Skalierungsgruppen, IaaS-Container und Azure-fremde Computer) erforderlich. Sie können auch von Azure Security Center profitieren, wenn Sie keine Agents bereitstellen. Allerdings ist dann die Sicherheit eingeschränkt, und die oben aufgeführten Funktionen werden nicht unterstützt.  
> - Die Liste der unterstützten Plattformen finden Sie unter [Unterstützte Plattformen in Azure Security Center](security-center-os-coverage.md).
> - Ganz gleich, ob Sie einen neuen oder vorhandenen Arbeitsbereich verwenden, fallen für das Speichern von Daten in Log Analytics möglicherweise zusätzliche Gebühren für die Datenspeicherung an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="enable-automatic-provisioning-of-the-log-analytics-agent"></a>Aktivieren der automatischen Bereitstellung des Log Analytics-Agents <a name="auto-provision-mma"></a>

Zum Erfassen der Daten von den Computern muss der Log Analytics-Agent installiert sein. Die Installation des Agents kann automatisch (empfohlen) oder manuell erfolgen. Die automatische Bereitstellung ist standardmäßig deaktiviert.

Bei aktivierter automatischer Bereitstellung wird der Log Analytics-Agent von Security Center auf allen unterstützten und neu erstellten virtuellen Azure-Computern bereitgestellt. Die automatische Bereitstellung wird empfohlen, Sie können den Agent bei Bedarf jedoch auch manuell installieren (siehe [Manuelle Installation des Log Analytics-Agents](#manual-agent)).


So aktivieren Sie die automatische Bereitstellung des Log Analytics-Agents:
1. Wählen Sie im Portal im Security Center-Menü **Preise & Einstellungen** aus.
2. Wählen Sie das relevante Abonnement aus.

   ![Auswählen des Abonnements][7]

3. Klicken Sie auf **Datensammlung**.
4. Wählen Sie unter **Automatische Bereitstellung** die Option **Ein** aus, um die automatische Bereitstellung zu aktivieren.
5. Wählen Sie **Speichern** aus. Der Agent wird innerhalb von 15 Minuten auf allen VMs bereitgestellt. 

>[!TIP]
> Wenn ein Arbeitsbereich bereitgestellt werden muss, kann die Installation des Agents bis zu 25 Minuten dauern.

   ![Aktivieren der automatischen Bereitstellung][1]

>[!NOTE]
> - Anweisungen zum Bereitstellen einer bereits vorhandenen Installation finden Sie unter [Automatische Bereitstellung bei einer bereits vorhandenen Agent-Installation](#preexisting).
> - Anweisungen zur manuellen Bereitstellung finden Sie unter [Manuelle Installation der Log Analytics-Agent-Erweiterung](#manual-agent).
> - Anweisungen zum Deaktivieren der automatischen Bereitstellung, finden Sie unter [Deaktivieren der automatischen Bereitstellung](#offprovisioning).
> - Anweisungen für das Onboarding von Security Center mit PowerShell finden Sie unter [Automatisieren des Onboardings von Azure Security Center mit PowerShell](security-center-powershell-onboarding.md).
>

## <a name="workspace-configuration"></a>Arbeitsbereichskonfiguration
Von Security Center gesammelte Daten werden in Log Analytics-Arbeitsbereichen gespeichert. Ihre Daten können von virtuellen Azure-Computern gesammelt werden, die in von Security Center erstellten Arbeitsbereichen oder in einem vorhandenen Arbeitsbereich gespeichert sind, den Sie selbst erstellt haben. 

Die Konfiguration von Arbeitsbereichen wird abonnementbasiert festgelegt, und der gleiche Arbeitsbereich kann für mehrere Abonnements verwendet werden.

### <a name="using-a-workspace-created-by-security-center"></a>Verwenden eines von Security Center erstellten Arbeitsbereichs

Security Center kann automatisch einen Standardarbeitsbereich zum Speichern der Daten erstellen. 

Auswählen eines von Security Center erstellten Arbeitsbereichs:

1. Wählen Sie unter **Standardmäßige Arbeitsbereichskonfiguration** die Option „Von Security Center erstellte Arbeitsbereiche verwenden“ aus.
   ![Tarif auswählen][10] 

1. Klicken Sie auf **Speichern**.<br>
    Security Center erstellt eine neue Ressourcengruppe und einen Standardarbeitsbereich an diesem Standort und verbindet den Agent mit diesem Arbeitsbereich. Für den Arbeitsbereich und die Ressourcengruppe gilt folgende Namenskonvention:<br>
   **Arbeitsbereich: DefaultWorkspace-[Abonnement-ID]-[Region]<br> Ressourcengruppe: DefaultResourceGroup-[Region]**

   Wenn ein Abonnement VMs aus mehreren Geolocations enthält, erstellt Security Center mehrere Arbeitsbereiche. Mehrere Arbeitsbereiche werden erstellt, um Datenschutzregeln zu verwalten.
1. Security Center aktiviert gemäß dem für das Abonnement festgelegten Tarif automatisch eine Security Center-Lösung im Arbeitsbereich. 

> [!NOTE]
> Der Log Analytics-Tarif von Arbeitsbereichen, die vom Security Center erstellt wurden, hat keine Auswirkungen auf die Security Center-Abrechnung. Die Abrechnung von Security Center basiert immer auf Ihrer Security Center-Sicherheitsrichtlinie und den installierten Lösungen in einem Arbeitsbereich. Für den Free-Tarif aktiviert Security Center die Lösung *SecurityCenterFree* im Standardarbeitsbereich. Für den Standard-Tarif aktiviert Security Center die Lösung *Security* im Standardarbeitsbereich.
> Für das Speichern von Daten in Log Analytics fallen möglicherweise zusätzliche Gebühren für die Datenspeicherung an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).

Weitere Informationen zu vorhandenen Log Analytics-Konten finden Sie unter [Log Analytics-Bestandskunden](./faq-azure-monitor-logs.md).

### <a name="using-an-existing-workspace"></a>Verwenden eines vorhandenen Arbeitsbereichs

Wenn Sie bereits über einen vorhandenen Log Analytics-Arbeitsbereich verfügen, können Sie diesen verwenden.

Zum Verwenden Ihres vorhandenen Log Analytics-Arbeitsbereichs benötigen Sie Lese- und Schreibberechtigungen für den Arbeitsbereich.

> [!NOTE]
> Im vorhandenen Arbeitsbereich aktivierte Lösungen gelten für virtuelle Azure-Computer, die damit verbunden sind. Bei gebührenpflichtigen Lösungen kann dies mit zusätzlichen Kosten verbunden sein. Stellen Sie aus Datenschutzgründen sicher, dass sich der ausgewählte Arbeitsbereich in der richtigen geografischen Region befindet.
> Für das Speichern von Daten in Log Analytics fallen möglicherweise zusätzliche Gebühren für die Datenspeicherung an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).

So wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus:

1. Wählen Sie unter **Standardmäßige Arbeitsbereichskonfiguration** die Option **Use another workspace** (Anderen Arbeitsbereich verwenden) aus.

   ![Auswählen eines vorhandenen Arbeitsbereichs][2]

2. Wählen Sie im Pulldownmenü einen Arbeitsbereich zum Speichern der gesammelten Daten aus.

   > [!NOTE]
   > Im Pulldownmenü sind alle Arbeitsbereiche in allen Ihren Abonnements verfügbar. Weitere Informationen finden Sie unter [Abonnementübergreifende Arbeitsbereichsauswahl](security-center-enable-data-collection.md#cross-subscription-workspace-selection). Sie benötigen die Berechtigung zum Zugriff auf den Arbeitsbereich.
   >
   >

3. Wählen Sie **Speichern** aus.
4. Nach der Auswahl von **Speichern** werden Sie gefragt, ob Sie überwachte virtuelle Computer, die zuvor mit einem Standardarbeitsbereich verbunden waren, erneut konfigurieren möchten.

   - Klicken Sie auf **Nein**, wenn die neuen Arbeitsbereichseinstellungen nur auf neue virtuelle Computer angewendet werden sollen. Die neuen Arbeitsbereichseinstellungen gelten nur für neue Agent-Installationen (neu ermittelte virtuelle Computer, auf denen der Log Analytics-Agent nicht installiert ist).
   - Klicken Sie auf **Ja**, wenn die neuen Arbeitsbereichseinstellungen auf alle virtuellen Computer angewendet werden sollen. Darüber hinaus wird jeder virtuelle Computer, der mit einem von Security Center erstellten Arbeitsbereich verbunden ist, nun mit dem neuen Zielarbeitsbereich verbunden.

   > [!NOTE]
   > Wenn Sie auf „Ja“ klicken, dürfen die von Security Center erstellten Arbeitsbereiche erst gelöscht werden, wenn alle virtuellen Computer mit dem neuen Zielarbeitsbereich verbunden sind. Dieser Vorgang ist nicht erfolgreich, wenn ein Arbeitsbereich zu früh gelöscht wird.
   >
   >

   - Klicken Sie auf **Abbrechen**, um den Vorgang abzubrechen.

     ![Auswählen eines vorhandenen Arbeitsbereichs][3]

5. Wählen Sie den Tarif für den gewünschten Arbeitsbereich aus, den Sie für den Log Analytics-Agent festlegen möchten. <br>Zur Verwendung eines vorhandenen Arbeitsbereichs legen Sie den Tarif für den Arbeitsbereich fest. Dadurch wird im Arbeitsbereich eine Security Center-Lösung installiert, falls noch keine vorhanden ist.

    a.  Klicken Sie im Hauptmenü von „Security Center“ auf **Preise & Einstellungen**.
     
    b.  Wählen Sie den gewünschten Arbeitsbereich aus, in dem Sie den Agent verbinden möchten.
        ![Arbeitsbereich auswählen][7] c. Legen Sie den Tarif fest.
        ![Tarif auswählen][9]
   
   >[!NOTE]
   >Wenn für den Arbeitsbereich bereits eine **Security**- oder **SecurityCenterFree**-Lösung aktiviert ist, werden die Preise automatisch festgelegt. 

## <a name="cross-subscription-workspace-selection"></a>Abonnementübergreifende Arbeitsbereichsauswahl
Wenn Sie einen Arbeitsbereich zum Speichern Ihrer Daten auswählen, sind alle Arbeitsbereiche in allen Ihren Abonnements verfügbar. Über die abonnementübergreifende Arbeitsbereichsauswahl können Sie Daten von virtuellen Computern erfassen, die in verschiedenen Abonnements ausgeführt werden, und im Arbeitsbereich Ihrer Wahl speichern. Diese Auswahl wird empfohlen, wenn Sie einen zentralen Arbeitsbereich in Ihrer Organisation verwenden und diesen für die Sammlung von Sicherheitsdaten verwenden möchten. Weitere Informationen zum Verwalten von Arbeitsbereichen finden Sie unter [Verwalten von Arbeitsbereichen](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).


## <a name="data-collection-tier"></a>Datensammlungsebene
Das Auswählen einer Datensammlungsebene in Azure Security Center hat nur Auswirkung auf die Speicherung von Sicherheitsereignissen in Ihrem Log Analytics-Arbeitsbereich. Der Log Analytics-Agent sammelt und analysiert weiterhin die Sicherheitsereignisse, die für den Bedrohungsschutz von Azure Security Center erforderlich sind, unabhängig davon, auf welcher Ebene Sicherheitsereignisse ggf. in Ihrem Log Analytics-Arbeitsbereich gespeichert werden sollen. Wenn Sie Sicherheitsereignisse in Ihrem Arbeitsbereich speichern, können Sie diese Ereignisse in Ihrem Arbeitsbereich untersuchen, durchsuchen und überprüfen. 
> [!NOTE]
> Für das Speichern von Daten in Log Analytics fallen möglicherweise zusätzliche Gebühren für die Datenspeicherung an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/security-center/).
> 
> Sie können die passende Filterrichtlinie für Ihre Abonnements und Arbeitsbereiche auswählen. Dabei haben Sie die Wahl zwischen vier Gruppen von Ereignissen, die in Ihrem Arbeitsbereich gespeichert werden sollen: 

- **Keine:** deaktiviert die Speicherung von Sicherheitsereignissen. Dies ist die Standardeinstellung.
- **Minimal:** Eine kleinere Gruppe von Ereignissen für Kunden, die das Ereignisvolumen minimieren möchten.
- **Allgemein:** eine Gruppe von Ereignissen, die die Anforderungen der meisten Kunden erfüllt und einen vollständigen Überwachungspfad bietet.
- **Alle Ereignisse:** für Kunden, die alle Ereignisse speichern möchten.


> [!NOTE]
> Diese Sicherheitsereignisgruppen sind nur im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
Diese Gruppen wurden für typische Szenarien konzipiert. Überlegen Sie sich vor der Implementierung, welche Ihre Anforderungen erfüllt.
>
>

Bei der Bestimmung der Ereignisse für die Gruppen **Allgemein** und **Minimal** haben wir uns mit Kunden und Branchenstandards beschäftigt, um uns über die ungefilterte Häufigkeit der einzelnen Ereignisse und deren Verwendung zu informieren. Dabei haben wir uns an den folgenden Richtlinien orientiert:

- **Minimal:** Diese Gruppe deckt nur Ereignisse ab, die auf eine erfolgreiche Sicherheitsverletzung hindeuten, sowie wichtige Ereignisse, die nur sehr selten auftreten. So deckt diese Gruppe beispielsweise erfolgreiche und nicht erfolgreiche Benutzeranmeldungen (Ereignis-IDs 4624 und 4625), aber keine Abmeldungen ab. Diese sind zwar für die Überwachung wichtig, nicht aber für die Erkennung, und sie treten zudem relativ häufig auf. Der Großteil des Datenvolumens dieser Gruppe ist auf Anmeldeereignisse und auf das Prozesserstellungsereignis (Ereignis-ID 4688) zurückzuführen.
- **Allgemein:** Diese Gruppe bietet einen vollständigen Benutzerüberwachungspfad. So enthält die Gruppe beispielsweise sowohl Benutzeranmeldungen als auch Benutzerabmeldungen (Ereignis-ID 4634). Wir schließen Überwachungsaktionen wie Sicherheitsgruppenänderungen, wichtige Domänencontroller-Kerberos-Vorgänge und andere Ereignisse ein, die von Branchenorganisationen empfohlen werden.

Ereignisse mit sehr geringem Volumen wurden in die Gruppe „Allgemein“ aufgenommen, da das Hauptargument für ihre Verwendung (im Vergleich zu allen anderen Ereignissen) in der Verringerung des Volumens besteht (und nicht darin, bestimmte Ereignisse herauszufiltern).

Im Anschluss finden Sie eine vollständige Aufschlüsselung der Sicherheits- und AppLocker-Ereignis-IDs für die einzelnen Gruppen:

| Datenschicht | Indikatoren für gesammelte Ereignisse |
| --- | --- |
| Wenig | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Allgemein | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Wenn Sie ein Gruppenrichtlinienobjekt (Group Policy Object, GPO) verwenden, wird empfohlen, das Überwachungsrichtlinien-Prozesserstellungsereignis 4688 und das Feld *CommandLine* im Ereignis 4688 zu aktivieren. Weitere Informationen zum Prozesserstellungsereignis 4688 finden Sie in den [häufig gestellten Fragen](faq-data-collection-agents.md#what-happens-when-data-collection-is-enabled) zu Security Center. Weitere Informationen zu diesen Überwachungsrichtlinien finden Sie unter [Empfehlungen zur Überwachungsrichtlinie](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  Um die Datensammlung für [Adaptive Anwendungssteuerungen](security-center-adaptive-application.md) zu aktivieren, konfiguriert Security Center eine lokale AppLocker-Richtlinie im Überwachungsmodus, um alle Anwendungen zuzulassen. Damit generiert AppLocker Ereignisse, die dann von Security Center gesammelt und genutzt werden können. Es ist wichtig zu beachten, dass diese Richtlinie nicht auf Computern konfiguriert wird, auf denen bereits eine AppLocker-Richtlinie konfiguriert ist. 
> - Zum Erfassen von [Ereignis-ID 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156) der Windows-Filterplattform müssen Sie [Filterplattformverbindung überwachen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) aktivieren (Auditpol /set /subcategory:"Filtering Platform Connection" /Success:Enable).
>

So wählen Sie Ihre Filterrichtlinie:
1. Wählen Sie auf der Seite **Datensammlung** unter **Sicherheitsereignisse** Ihre Filterrichtlinie aus.
2. Wählen Sie **Speichern** aus.

   ![Auswählen der Filterrichtlinie][5]

### <a name="automatic-provisioning-in-cases-of-a-pre-existing-agent-installation"></a>Automatische Bereitstellung bei einer bereits vorhandenen Agent-Installation <a name="preexisting"></a> 

Die folgenden Anwendungsfälle geben an, wie die automatische Bereitstellung funktioniert, wenn bereits ein Agent oder eine Erweiterung installiert ist. 

- Der Log Analytics-Agent ist auf dem Computer installiert, jedoch nicht als Erweiterung (Direkt-Agent).<br>
Wenn der Log Analytics-Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, installiert Security Center die Log Analytics-Agent-Erweiterung und aktualisiert den Log Analytics-Agent ggf. auf die neueste Version.
Der installierte Agent erstellt seine Berichte weiterhin für die bereits konfigurierten Arbeitsbereiche und darüber hinaus für den in Security Center konfigurierten Arbeitsbereich (Multi-Homing wird auf Windows-Computern unterstützt).
Wenn der konfigurierte Arbeitsbereich ein Benutzerarbeitsbereich ist (nicht der Standardarbeitsbereich von Security Center), müssen Sie darin die Lösung „security“ oder „securityFree“ installieren, damit Security Center mit der Verarbeitung von Ereignissen von VMs und Computern, die ihre Berichte in diesem Arbeitsbereich erstellen, beginnen kann.<br>
<br>
Auf Linux-Computern wird das Multi-Homing von Agents noch nicht unterstützt, weshalb, wenn eine bestehende Agent-Installation erkannt wird, keine automatische Bereitstellung erfolgt und die Konfiguration des Computers nicht geändert wird.
<br>
Wenn für vorhandene Computer in Abonnements, die vor dem 17. März 2019 in Security Center integriert wurden, ein vorhandener Agent erkannt wurde, wird die Log Analytics-Agent-Erweiterung nicht installiert, und der Computer ist nicht betroffen. Für diese Computer wird die Empfehlung „Monitoring Agent-Integritätsprobleme auf Ihren Computern beheben“ angezeigt, damit Sie die Installationsprobleme des Agents auf diesen Computern beheben können.

  
- Der System Center Operations Manager-Agent ist auf dem Computer installiert.<br>
Security Center installiert die Log Analytics-Agent-Erweiterung parallel zum vorhandenen Operations Manager. Der vorhandene Operations Manager-Agent sendet weiterhin normal Berichte an den Operations Manager-Server. Der Operations Manager-Agent und der Log Analytics-Agent nutzen gemeinsame Laufzeitbibliotheken, die bei diesem Vorgang auf die neueste Version aktualisiert werden. Wenn die Operations Manager-Agentversion 2012 installiert ist, aktivieren Sie die automatische Bereitstellung **nicht**.<br>

- Eine VM-Erweiterung ist bereits vorhanden.<br>
    - Wenn der Monitoring Agent als Erweiterung installiert ist, erlaubt die Konfiguration der Erweiterung auch Berichte an nur einen einzelnen Arbeitsbereich. Bereits vorhandene Verbindungen mit Benutzerarbeitsbereichen werden von Security Center nicht überschrieben. Security Center speichert Sicherheitsdaten des virtuellen Computers im bereits verbundenen Arbeitsbereich, sofern darin die Lösung „security“ oder „securityFree“ installiert wurde. Security Center kann die Version der Erweiterung während dieses Vorgangs auf die neueste Version aktualisieren.  
    - Um festzustellen, an welchen Arbeitsbereich die vorhandene Erweiterung Daten sendet, führen Sie den Test zum [Überprüfen der Konnektivität mit Azure Security Center](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/) aus. Alternativ können Sie Log Analytics-Arbeitsbereiche öffnen, einen Arbeitsbereich und den virtuellen Computer auswählen und sich die Log Analytics-Agent-Verbindung ansehen. 
    - Wenn Sie über eine Umgebung verfügen, in der der Log Analytics-Agent auf Clientarbeitsstationen installiert ist und an einen vorhandenen Log Analytics-Arbeitsbereich berichtet, überprüfen Sie die Liste der [von Azure Security Center unterstützten Betriebssysteme](security-center-os-coverage.md), um sicherzustellen, dass Ihr Betriebssystem unterstützt wird. Weitere Informationen finden Sie unter [Log Analytics-Bestandskunden](./faq-azure-monitor-logs.md).
 
### <a name="turn-off-automatic-provisioning"></a>Deaktivieren der automatischen Bereitstellung <a name="offprovisioning"></a>
Sie können die automatische Bereitstellung in Ressourcen jederzeit deaktivieren, indem Sie diese Einstellung in der Sicherheitsrichtlinie deaktivieren. 


1. Kehren Sie zum Hauptmenü von Security Center zurück, und klicken Sie auf die Sicherheitsrichtlinie.
2. Klicken Sie in der Zeile des Abonnements, für das Sie die automatische Bereitstellung deaktivieren möchten, **Einstellungen bearbeiten>** aus.
3. Wählen Sie auf der Seite **Sicherheitsrichtlinie – Datensammlung** unter **Automatische Bereitstellung** die Option **Aus** aus.
4. Wählen Sie **Speichern** aus.

   ![Deaktivieren der automatischen Bereitstellung][6]

Wenn die automatische Bereitstellung deaktiviert ist, wird der Abschnitt mit der standardmäßigen Arbeitsbereichskonfiguration nicht angezeigt.

Wenn Sie die automatische Bereitstellung deaktivieren, nachdem sie zuerst aktiviert war, werden Agents nicht auf neuen virtuellen Computern bereitgestellt.

 
> [!NOTE]
>  Wenn Sie die automatische Bereitstellung deaktivieren, wird der Log Analytics-Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde. Informationen zum Entfernen der OMS-Erweiterungs finden Sie unter [Wie entferne ich durch Security Center installierte OMS-Erweiterungen?](faq-data-collection-agents.md#remove-oms).
>
    
## <a name="manual-agent-provisioning"></a>Manuelle Agent-Bereitstellung <a name="manual-agent"></a>
 
Es gibt mehrere Möglichkeiten, den Log Analytics-Agent manuell zu installieren. Achten Sie bei der manuellen Installation darauf, die automatische Bereitstellung zu deaktivieren.

### <a name="operations-management-suite-vm-extension-deployment"></a>Bereitstellung der Operations Management Suite-Erweiterung für virtuelle Computer 

Damit Security Center Sicherheitsdaten auf virtuellen Computern erfassen und Empfehlungen und Warnungen bereitstellen kann, können Sie den Log Analytics-Agent manuell installieren.

1. Deaktivieren Sie die automatische Bereitstellung.

1. Erstellen Sie optional einen Arbeitsbereich.

1. Legen Sie den Arbeitsbereich, für den Sie den Log Analytics-Agent installieren, auf den Standard-Tarif fest:

    1. Wählen Sie im Menü von Security Center **Preise und Einstellungen** aus.

    1. Legen Sie den Arbeitsbereich fest, für den Sie den Agent installieren. Stellen Sie sicher, dass sich der Arbeitsbereich im gleichen Abonnement befindet, das Sie in Security Center verwenden, und dass Sie über Lese-/Schreibberechtigungen für den Arbeitsbereich verfügen.

    1. Legen Sie den Standard-Tarif fest, und klicken Sie auf **Speichern**.

        ![Festlegen eines Arbeitsbereichs auf den Standard-Tarif](.\media\security-center-enable-data-collection\workspace-to-standard-tier.gif)

       >[!NOTE]
       >Wenn für den Arbeitsbereich bereits eine **Security**- oder **SecurityCenterFree**-Lösung aktiviert ist, werden die Preise automatisch festgelegt. 
   > 

1. Wenn Sie die Agents auf neuen virtuellen Computern mithilfe der Resource Manager-Vorlage bereitstellen möchten, installieren Sie den Log Analytics-Agent:

   a.  [Installieren des Log Analytics-Agents für Windows](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Installieren des Log Analytics-Agents für Linux](../virtual-machines/extensions/oms-linux.md)

1. Folgen Sie den Anweisungen in [Sammeln von Daten über virtuelle Azure-Computer](../azure-monitor/learn/quick-collect-azurevm.md), um die Erweiterungen auf vorhandenen virtuellen Computern bereitzustellen.

   > [!NOTE]
   > Der Abschnitt **Sammeln von Ereignis- und Leistungsdaten** ist optional.
   >

1. Wenn Sie PowerShell zum Bereitstellen der Erweiterung verwenden möchten, befolgen Sie die Anweisungen in der Dokumentation zu virtuellen Computern:

    - [Für Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#powershell-deployment)

    - [Für Linux-Computer](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#azure-cli-deployment)



> [!NOTE]
> Anweisungen für das Onboarding von Security Center mit PowerShell finden Sie unter [Automatisieren des Onboardings von Azure Security Center mit PowerShell](security-center-powershell-onboarding.md).

## <a name="troubleshooting"></a>Problembehandlung

-   Informationen zum Identifizieren von Problemen bei der automatischen Bereitstellung finden Sie unter [Integritätsprobleme des Überwachungs-Agents](security-center-troubleshooting-guide.md#mon-agent).

-  Informationen zum Identifizieren von Netzwerkanforderungen für den Überwachungs-Agent finden Sie unter [Beheben von Problemen mit den Netzwerkanforderungen für den Überwachungs-Agent](security-center-troubleshooting-guide.md#mon-network-req).
-   Informationen zum Identifizieren von Problemen beim manuellen Onboarding finden Sie unter [Behandeln von Problemen beim Operations Management Suite-Onboarding](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- So identifizieren Sie Probleme mit nicht überwachten virtuellen und physischen Computern

    Ein virtueller oder physischer Computer gilt als nicht von Security Center überwacht, wenn auf dem Computer die Log Analytics-Agent-Erweiterung nicht ausgeführt wird. Auf einem Computer kann bereits ein lokaler Agent installiert sein, z.B. der OMS-Direkt-Agent oder der System Center Operations Manager-Agent. Computer mit diesen Agents werden als nicht überwacht angesehen, da diese Agents in Security Center nicht vollständig unterstützt werden. Die Log Analytics-Agent-Erweiterung ist erforderlich, um von allen Security Center-Vorteilen vollständig profitieren zu können.

    Weitere Informationen zu den Gründen, aus denen Security Center VMs und Computer, die für die automatische Bereitstellung initialisiert wurden, nicht erfolgreich überwachen kann, finden Sie unter [Integritätsprobleme von Monitoring Agent](security-center-troubleshooting-guide.md#mon-agent).


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde die Funktionsweise der Datensammlung und der automatischen Bereitstellung in Security Center erläutert. Weitere Informationen zu Security Center finden Sie auf den folgenden Seiten:

* [Azure Security Center – Häufig gestellte Fragen](faq-general.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
