---
title: Verwenden und Verwalten einer Azure App Service-Umgebung
description: Informationen zum Erstellen, Veröffentlichen und Skalieren von Apps in einer Azure App Service-Umgebung. In diesem Dokument finden Sie eine Beschreibung der allgemeinen Aufgaben.
author: ccompy
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.topic: article
ms.date: 01/01/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7be1676c8949cd30d5e1fe93a73afd75a5a9b67f
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565652"
---
# <a name="use-an-app-service-environment"></a>Verwenden einer App Service-Umgebung #

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz im virtuellen Azure-Netzwerk des Kunden. Eine ASE besteht aus den folgenden Komponenten:

- **Front-Ends**: Auf den Front-Ends endet HTTP/HTTPS in einer App Service-Umgebung.
- **Worker**: Bei Workern handelt es sich um die Ressourcen, die Ihre Apps hosten.
- **Datenbank**: Die Datenbank enthält Informationen, durch die die Umgebung festgelegt ist.
- **Storage**: Der Speicher wird zum Hosten der vom Kunden veröffentlichten Apps verwendet.

Sie können eine ASE mit einer externen oder internen virtuellen IP (VIP) für den App-Zugriff bereitstellen. Die Bereitstellung mit einer externen VIP wird im Allgemeinen als externe ASE bezeichnet. Die interne Version wird als ILB-ASE bezeichnet, da ein interner Load Balancer (ILB) verwendet wird. Weitere Informationen zur ILB-ASE finden Sie unter [Erstellen und Verwenden einer ILB-ASE][MakeILBASE].

## <a name="create-an-app-in-an-ase"></a>Erstellen einer App in einer ASE ##

Um eine App in einer ASE zu erstellen, verwenden Sie den gleichen Prozess wie bei der normalen Erstellung, jedoch mit einigen kleinen Unterschieden. Beim Erstellen eines neuen App Service-Plans (ASP) gilt Folgendes:

- Anstatt einen geografischen Standort auszuwählen, an dem Sie Ihre App bereitstellen, wählen Sie eine ASE als Ihren Standort aus.
- Alle in einer ASE erstellten App Service-Pläne können nur den Tarif „Isoliert“ aufweisen.

Wenn Sie über keine ASE verfügen, können Sie eine erstellen. Folgen Sie hierzu den Anweisungen in [Erstellen einer App Service-Umgebung][MakeExternalASE].

So erstellen Sie eine App in einer ASE

1. Wählen Sie **Ressource erstellen** > **Web + Mobil** > **Web-Apps** aus.

2. Geben Sie einen Namen für die App ein. Wenn Sie bereits einen App Service-Plan in einer ASE ausgewählt haben, entspricht der Domänenname der App dem Domänenname der ASE.

    ![Namensauswahl für Apps][1]

1. Wählen Sie ein Abonnement aus.

1. Geben Sie einen Namen für eine neue Ressourcengruppe ein, oder wählen Sie **Vorhandene verwenden** aus, und wählen Sie in der Dropdownliste eine Ressourcengruppe aus.

1. Wählen Sie Ihr Betriebssystem aus. 

1. Wählen Sie einen in Ihrer ASE vorhandenen App Service-Plan aus, oder erstellen Sie mit den folgenden Schritten einen neuen:

    a. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressource erstellen > Web-App** aus.

    b. Wählen Sie das Abonnement aus.
    
    c. Wählen Sie die Ressourcengruppe aus bzw. erstellen Sie sie.
    
    d. Geben Sie den Namen Ihrer Web-App an.
    
    e. Wählen Sie „Code“ oder „DockerContainer“ aus.
    
    f. Wählen Sie „Runtimestapel“ aus.
    
    g. Wählen Sie „Linux“ oder „Windows“ aus. 
    
    h. Wählen Sie Ihre ASE in der Dropdownliste **Region** aus. 
    
    i. Wählen Sie einen neuen App Service-Plan aus, oder erstellen Sie ihn. Wählen Sie beim Erstellen eines neuen App Service-Plans die entsprechende SKU-Größe **Isoliert** aus.
    
    ![Isolierte Tarife][2]

    > [!NOTE]
    > Linux-Apps und Windows-Apps können sich nicht im selben App Service-Plan befinden, jedoch in derselben App Service-Umgebung. 
    >

2. Wählen Sie **Bewerten + erstellen** und dann **Erstellen** aus, wenn die Angaben stimmen.

## <a name="how-scale-works"></a>Skalieren ##

Jede App Service-App wird in einem App Service-Plan ausgeführt. App Service-Umgebungen enthalten App Service-Pläne, und App Service-Pläne enthalten Apps. Wenn Sie eine App skalieren, skalieren Sie den App Service-Plan und damit alle Apps in diesem Plan.

Wenn Sie einen App Service-Plan skalieren, wird die erforderliche Infrastruktur automatisch hinzugefügt. Bei der Skalierung von Vorgängen kommt es zu einer Zeitverzögerung, während die Infrastruktur hinzugefügt wird. Wenn Sie mehrere Skalierungsvorgänge nacheinander durchführen, wird die erste Skalierungsanforderung für die Infrastruktur verarbeitet, und die anderen werden in die Warteschlange eingereiht. Nachdem der erste Skalierungsvorgang abgeschlossen ist, werden die anderen Infrastrukturanforderungen zusammen ausgeführt. Nach dem Hinzufügen der Infrastruktur werden die App Service-Pläne entsprechend zugewiesen. Auch die Erstellung eines neuen App Service-Plans selbst ist ein Skalierungsvorgang, weil zusätzliche Hardware angefordert wird. 

Bei einer mehrinstanzenfähigen App Service-Instanz erfolgt die Skalierung unmittelbar, da sofort ein Ressourcenpool für die Unterstützung verfügbar ist. In einer ASE gibt es keinen solchen Puffer. Die Ressourcen werden nach Bedarf zugeordnet.

In einer ASE können Sie einen App Service-Plan auf bis zu 100 Instanzen zentral hochskalieren. Eine ASE kann übergreifend für alle App Service-Pläne, die sich in der ASE befinden, eine Gesamtmenge von bis zu 201 Instanzen enthalten. 

## <a name="ip-addresses"></a>IP-Adressen ##

In App Service gibt es die Möglichkeit, eine dedizierte IP-Adresse an eine App zu vergeben. Die Funktion zum Zuordnen einer dedizierten IP-Adresse zu einer App ist nach dem Konfigurieren von IP-basiertem SSL verfügbar, wie unter [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure App Service][ConfigureSSL] beschrieben. In einer ILB-ASE können Sie keine zusätzlichen IP-Adressen hinzufügen, die für IP-basiertes SSL verwendet werden sollen.

Bei einer externen ASE können Sie IP-basiertes SSL für Ihre App genauso wie für eine mehrinstanzenfähige App Service-Instanz konfigurieren. In der ASE ist immer eine freie Adresse vorhanden (bis zu 30 IP-Adressen). Bei jeder Verwendung einer IP-Adresse wird eine weitere hinzugefügt, sodass immer eine sofort verfügbare Adresse zur Verfügung steht. Bei der Vergabe einer weiteren IP-Adresse ist eine zeitliche Verzögerung erforderlich. Daher können IP-Adressen nicht in schneller Abfolge hinzugefügt werden.

## <a name="front-end-scaling"></a>Front-End-Skalierung ##

Beim horizontalen Hochskalieren Ihrer App Service-Pläne werden die Worker zur Unterstützung der App Service-Pläne automatisch hinzugefügt. Jede ASE wird mit zwei Front-Ends erstellt. Die Front-Ends werden mit einer Rate von einem Front-End pro 15 App Service-Planinstanzen automatisch horizontal hochskaliert. Wenn Sie drei App Service-Pläne mit jeweils fünf Instanzen verwenden, verfügen Sie insgesamt über 15 Instanzen und drei Front-Ends. Wenn Sie auf insgesamt 30 Instanzen skalieren, haben Sie vier Front-Ends usw. 

Die standardmäßig zugeordnete Anzahl von Front-Ends ist gut für mittlere Auslastungen geeignet. Sie können das Verhältnis auf bis zu 1 Front-End für je 5 Instanzen verringern. Darüber hinaus können Sie auch die Größe der Front-Ends ändern. Standardmäßig wird „Einzelkern“ verwendet. Sie können die Größe der Front-Ends im Portal in eine Option mit zwei oder vier Kernen ändern. Für die Änderung des Verhältnisses oder der Größen von Front-Ends fällt eine Gebühr an. Weitere Informationen finden Sie unter [Azure App Service – Preise][Pricing]. Wenn Sie die Auslastungskapazität Ihrer ASE verbessern möchten, erzielen Sie ein besseres Ergebnis, indem Sie zuerst auf Front-Ends mit zwei Kernen skalieren, bevor Sie das Skalierungsverhältnis anpassen. Das Ändern der Kerngröße Ihrer Front-Ends führt zu einem Upgrade Ihrer ASE und sollte außerhalb der regulären Geschäftszeiten durchgeführt werden.

Front-End-Ressourcen sind die HTTP/HTTPS-Endpunkte der ASE. Bei der Front-End-Standardkonfiguration beträgt der Speicherverbrauch pro Front-End konstant etwa 60 %. Der Hauptgrund für die Skalierung Ihrer Front-Ends ist die CPU-Auslastung, die vorrangig auf HTTPS-Datenverkehr basiert.

## <a name="app-access"></a>App-Zugriff ##

In einer externen ASE lautet das Domänensuffix für die App-Erstellung *.&lt;asename&gt;.p.azurewebsites.net*. Wenn Ihre ASE den Namen _external-ase_ hat und Sie in dieser ASE eine App mit dem Namen _contoso_ hosten, erreichen Sie sie über die folgenden URLs:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

Weitere Informationen zum Erstellen einer externen ASE finden Sie unter [Erstellen einer App Service-Umgebung][MakeExternalASE].

In einer ILB-ASE lautet das Domänensuffix für die App-Erstellung *.&lt;asename&gt;.appserviceenvironment.net*. Wenn Ihre ASE den Namen _ilb-ase_ hat und Sie in dieser ASE eine App mit dem Namen _contoso_ hosten, erreichen Sie sie über die folgenden URLs:

- contoso.ilb-ase.appserviceenvironment.net
- contoso.scm.ilb-ase.appserviceenvironment.net

Weitere Informationen zum Erstellen einer ILB-ASE finden Sie unter [Erstellen und Verwenden einer ILB-ASE][MakeILBASE]. 

Die scm-URL wird verwendet, um auf die Kudu-Konsole zuzugreifen oder Ihre App per Web Deploy zu veröffentlichen. Informationen zur Kudu-Konsole finden Sie in [Kudu-Konsole für Azure App Service][Kudu]. Die Kudu-Konsole bietet eine Webbenutzeroberfläche für das Debuggen, Hochladen und Bearbeiten von Dateien und vieles mehr.

## <a name="publishing"></a>Veröffentlichung ##

Wie bei dem mehrinstanzenfähigen App Service können Sie auch folgendermaßen in einer ASE veröffentlichen:

- Webbereitstellung
- FTP
- Continuous Integration
- Drag & Drop in der Kudu-Konsole
- Eine IDE wie Visual Studio, Eclipse oder IntelliJ IDEA

Mit einer externen ASE weisen alle diese Veröffentlichungsoptionen das gleiche Verhalten auf. Weitere Informationen finden Sie unter [Bereitstellen in Azure App Service][AppDeploy]. 

Der wesentliche Unterschied bei der Veröffentlichung betrifft die ILB-ASE. In einer ILB-ASE sind die Veröffentlichungsendpunkte nur über die ILB verfügbar. Die ILB liegt in einer privaten IP-Adresse im ASE-Subnetz im virtuellen Netzwerk. Wenn Sie keinen Netzwerkzugriff auf die ILB haben, können Sie in der betreffenden ASE keine Apps veröffentlichen. Wie in [Erstellen und Verwenden einer ILB-ASE][MakeILBASE] erwähnt, müssen Sie für die Apps im System ein DNS konfigurieren. Dazu gehört der SCM-Endpunkt. Wenn sie nicht ordnungsgemäß definiert sind, ist keine Veröffentlichung möglich. Darüber hinaus müssen Ihre IDEs über Netzwerkzugriff auf den ILB verfügen, um direkt an ihn veröffentlichen zu können.

Internetbasierte CI-Systeme wie GitHub oder Azure DevOps können nicht standardmäßig mit einer ILB-ASE verwendet werden, da der Veröffentlichungsendpunkt nicht über das Internet erreichbar ist. Sie können die Veröffentlichung in einer ILB-ASE über Azure DevOps ermöglichen, indem Sie in dem VNET, das die ILB-ASE enthält, einen selbstgehosteten Release-Agent installieren. Alternativ können Sie auch ein CI-System wie Dropbox verwenden, das ein Pullmodell verwendet.

Die Veröffentlichungsendpunkte für die Apps in einer ILB-ASE verwenden die Domäne, mit der die ILB-ASE erstellt wurde. Dies wird im Veröffentlichungsprofil und auf dem Portalblatt der App angezeigt (in **Übersicht** > **Zusammenfassung** sowie in den **Eigenschaften**). 

## <a name="storage"></a>Storage

Eine ASE verfügt für alle Apps der ASE über Speicher mit einer Größe von 1 TB. Ein App Service-Plan mit der SKU „Isoliert“ verfügt standardmäßig über ein Limit von 250 GB. Bei fünf oder mehr App Service-Plänen müssen Sie darauf achten, dass Sie das Limit von 1 TB für die ASE nicht überschreiten. Falls Sie für einen App Service-Plan mehr als 250 GB benötigen, können Sie sich an den Support wenden, um das Limit für den App Service-Plan auf maximal 1 TB erhöhen zu lassen. Nachdem das Limit für den Plan angepasst wurde, gilt weiterhin die gemeinsame Obergrenze von 1 TB für alle App Service-Pläne der ASE. 

## <a name="logging"></a>Protokollierung ##

Sie können Ihre ASE in Azure Monitor integrieren, um Protokolle zur ASE an Storage, Event Hub oder Log Analytics zu senden. Die derzeit protokollierten Elemente sind:

| Situation | Meldung |
|---------|----------|
| ASE ist fehlerhaft | Die angegebene ASE ist aufgrund einer ungültigen VNET-Konfiguration fehlerhaft. Die ASE wird angehalten, falls der Fehlerzustand weiter besteht. Stellen Sie sicher, dass die folgenden Richtlinien befolgt werden: https://docs.microsoft.com/azure/app-service/environment/network-info. |
| Speicherplatz im ASE-Subnetz ist nahezu erschöpft | Die angegebene ASE befindet sich in einem Subnetz, dessen Speicherplatz nahezu erschöpft ist. Es sind {0} verbleibende Adressen vorhanden. Nachdem diese Adressen ausgeschöpft wurden, kann die ASE nicht mehr skaliert werden.  |
| ASE nähert sich dem Limit für die Höchstmenge der Instanzen | Für die angegebene ASE wird in Kürze das Limit für die Höchstmenge der Instanzen erreicht. Derzeit sind {0} App Service-Planinstanzen bei einer maximalen Anzahl von 201 Instanzen vorhanden. |
| ASE kann eine Abhängigkeit nicht erreichen | Die angegebene ASE kann {0} nicht erreichen.  Stellen Sie sicher, dass die folgenden Richtlinien befolgt werden: https://docs.microsoft.com/azure/app-service/environment/network-info. |
| ASE wurde angehalten | Die angegebene ASE wurde angehalten. Der Grund für das Anhalten der ASE kann ein Kontoausfall oder eine ungültige Konfiguration des virtuellen Netzwerks sein. Beseitigen Sie die Grundursache, und setzen Sie die Ausführung der ASE fort, damit die Bereitstellung von Datenverkehr erfolgen kann. |
| ASE-Upgrade wurde gestartet | Ein Plattformupgrade für die angegebene ASE wurde gestartet. Es ist mit Verzögerungen bei den Skalierungsvorgängen zu rechnen. |
| ASE-Upgrade wurde abgeschlossen | Ein Plattformupgrade für die angegebene ASE wurde abgeschlossen. |
| Skalierungsvorgänge wurden gestartet | Für einen App Service-Plan ({0}) wurde die Skalierung gestartet. Gewünschter Status: {1} I{2}-Worker 
| Skalierungsvorgänge wurden abgeschlossen | Die Skalierung eines App Service-Plans ({0}) wurde abgeschlossen. Aktueller Status: {1} I{2}-Worker |
| Für Skalierungsvorgänge sind Fehler aufgetreten | Bei der Skalierung eines App Service-Plans ({0}) ist ein Fehler aufgetreten. Aktueller Status: {1} I{2}-Worker |

Aktivieren Sie die Protokollierung für Ihre ASE wie folgt: 

1. Navigieren Sie im Portal zu den Diagnoseeinstellungen.  
1. Wählen Sie „Diagnoseeinstellung hinzufügen“ aus.
1. Geben Sie einen Namen für die Protokollintegration an.
1. Überprüfen und konfigurieren Sie die gewünschten Protokollziele. 
1. Überprüfen Sie „AppServiceEnvironmentPlatformLogs“.

![Einstellungen für ASE-Diagnoseprotokollierung][4]

Bei der Integration mit Log Analytics können Sie die Protokolle anzeigen, indem Sie im ASE-Portal die Option „Protokolle“ auswählen und eine Abfrage für „AppServiceEnvironmentPlatformLogs“ erstellen. 

## <a name="upgrade-preference"></a>Upgradeeinstellung ##

Bei Verwendung von mehreren ASEs kann es sein, dass einige ASEs vor anderen ASEs aktualisiert werden sollen. Im Resource Manager-Objekt „HostingEnvironment“ der ASE können Sie einen Wert für „UpgradePreference“ festlegen. Die Einstellung „upgradePreference“ kann per Vorlage, ARMClient oder https://resources.azure.com konfiguriert werden.  Die drei Auswahlmöglichkeiten für Werte sind:

* None: Dies ist die Standardeinstellung. Sie bedeutet, dass Azure keinen bestimmten Batch für das Upgrade Ihrer ASE verwendet.
* Early: Dies bedeutet, dass Ihre ASE in der ersten Hälfte der App Service-Upgrades aktualisiert wird.
* Late: Dies bedeutet, dass Ihre ASE in der zweiten Hälfte der App Service-Upgrades aktualisiert wird.

Bei Verwendung von https://resources.azure.com können Sie den Wert für „upgradePreferences“ wie folgt festlegen:

1. Navigieren Sie zu „resources.azure.com“, und melden Sie sich mit Ihrem Azure-Konto an.
1. Navigieren Sie zu „subscriptions\/\[Abonnementname\]\/resourceGroups\/\[Ressourcengruppenname\]\/providers\/Microsoft.Web\/hostingEnvironments\/\[ASE-Name\].
1. Wählen Sie oben „Lesen/Schreiben“ aus.
1. Wählen Sie „Bearbeiten“ aus.
1. Ändern Sie den Wert für „upgradePreference“ in eine der drei Optionen.
1. Wählen Sie „Patch“ aus.

![Ressourcenanzeige auf „azure.com“][5]

Die Verwendung des Features „upgradePreferences“ ergibt am meisten Sinn, wenn Sie mehrere ASEs nutzen, da Ihre als „Early“ gekennzeichneten ASEs vor den mit „Late“ gekennzeichneten ASEs aktualisiert werden. Bei Verwendung von mehreren ASEs sollten Sie für Ihre Entwicklungs-/Test-ASEs die Option „Early“ und für Ihre Produktions-ASEs die Option „Late“ auswählen.

## <a name="pricing"></a>Preise ##

Die Preis-SKU **Isoliert** sollte nur für eine ASE verwendet werden. Alle App Service-Pläne, die in einer ASE gehostet werden, befinden sich in der Preis-SKU „Isoliert“. Die Raten für isolierte App Service-Pläne können für verschiedene Regionen unterschiedlich sein. 

Zusätzlich zum Preis Ihrer App Service-Pläne fällt eine Pauschalgebühr für die eigentliche ASE an. Die Pauschalgebühr ist von der Größe Ihrer ASE unabhängig. Sie fällt für die ASE-Infrastruktur mit der Standardskalierungsrate mit einem zusätzlichen Front-End für jeweils 15 Instanzen des App Service-Plans an.  

Wenn die Standardskalierung von einem Front-End für jeweils 15 Instanzen des App Service-Plans nicht schnell genug ist, können Sie das Verhältnis, nach dem Front-Ends hinzugefügt werden, oder die Größe der Front-Ends anpassen.  Wenn Sie das Verhältnis oder die Größe anpassen, bezahlen Sie für die Front-End-Kerne, die standardmäßig nicht hinzugefügt werden.  

Wenn Sie das Skalierungsverhältnis beispielsweise in den Wert 10 ändern, wird für je zehn Instanzen in Ihren App Service-Plänen ein Front-End hinzugefügt. Die Pauschalgebühr deckt ein Skalierungsverhältnis von einem Front-End pro 15 Instanzen ab. Mit einem Skalierungsverhältnis von „10“ bezahlen Sie für das dritte Front-End, das für die 10 Instanzen des App Service-Plans hinzugefügt wird, eine Gebühr. Sie müssen nicht dafür bezahlen, wenn Sie 15 Instanzen erreichen, da es in diesem Fall automatisch hinzugefügt wurde.

Wenn Sie die Größe der Front-Ends in 2 Kerne ändern, aber das Verhältnis nicht anpassen, bezahlen Sie für die zusätzlichen Kerne.  Eine ASE wird mit zwei Front-Ends erstellt. Sie würden also selbst unterhalb des Schwellenwerts für die automatische Skalierung zwei zusätzliche Kerne bezahlen, wenn Sie die Größe auf 2-Kern-Front-Ends erhöhen.

Weitere Informationen finden Sie unter [Azure App Service – Preise][Pricing].

## <a name="delete-an-ase"></a>Löschen einer ASE ##

So löschen Sie eine ASE 

1. Verwenden Sie **Löschen** im oberen Bereich des Blatts **App Service-Umgebung**. 

1. Geben Sie den Namen Ihrer ASE ein, um zu bestätigen, dass Sie sie löschen möchten. Beim Löschen einer ASE wird auch ihr gesamter Inhalt gelöscht. 

    ![Löschen einer ASE][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png
[4]: ./media/using_an_app_service_environment/usingase-logsetup.png
[4]: ./media/using_an_app_service_environment/usingase-logs.png
[5]: ./media/using_an_app_service_environment/usingase-upgradepref.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
