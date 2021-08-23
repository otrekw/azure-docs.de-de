---
title: Anleitung für die Migration von JBoss EAP zu Azure-VMs und -VM-Skalierungsgruppen
description: Dieser Leitfaden enthält Informationen zum Migrieren Ihrer Java-Unternehmensanwendungen von einem anderen Anwendungsserver zu JBoss EAP und von herkömmlichen lokalen Servern zu RHEL-VMs und -VM-Skalierungsgruppen in Azure.
author: theresa-nguyen
ms.author: bicnguy
ms.topic: article
ms.service: virtual-machines
ms.subservice: redhat
ms.assetid: 9b37b2c4-5927-4271-85c7-19adf33d838b
ms.date: 06/08/2021
ms.openlocfilehash: deded6927998daa5956cff2f63a0a53d3b973b6d
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111855097"
---
# <a name="how-to-migrate-java-applications-to-jboss-eap-on-azure-vms-and-virtual-machine-scale-sets"></a>Migrieren von Java-Anwendungen zu JBoss EAP in Azure-VMs und -VM-Skalierungsgruppen

Dieser Leitfaden enthält Informationen zur Migration Ihrer Java-Unternehmensanwendungen auf [Red Hat JBoss Enterprise Application Platform (EAP)](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) von einem herkömmlichen lokalen Server zu virtuellen Azure-Computern (VMs) und VM-Skalierungsgruppen unter Red Hat Enterprise Linux (RHEL), wenn Ihre Cloudstrategie eine Lift & Shift-Migration von Java-Anwendungen im Ist-Zustand vorsieht. Wenn Sie jedoch die „Lift & Optimize“-Methode vorziehen, können Sie alternativ Ihre containerisierten Anwendungen mithilfe von JBoss EAP-Images aus dem Red Hat-Katalog zu [Azure Red Hat OpenShift (ARO)](https://azure.microsoft.com/services/openshift/) migrieren oder Ihren Java Anwendungscode in einer Azure App Service-Instanz direkt in JBoss EAP ablegen.

## <a name="best-practice-starting-with-azure-marketplace-offers-and-quickstarts"></a>Bewährte Methode für den Einstieg in Azure Marketplace-Angebote und Schnellstarts

Red Hat und Microsoft haben sich zusammengetan, um [Azure Marketplace-Angebote für JBoss EAP](https://aka.ms/AMP-JBoss-EAP) zu unterbreiten, die einen soliden Ausgangspunkt für die Migration zu Azure darstellen. In der Dokumentation zur finden Sie eine Liste mit Angeboten und Plänen. Wählen Sie ein Angebot aus, das für Ihre vorhandene Bereitstellung am besten geeignet ist. Lesen Sie den Artikel [Bewährte Methoden für JBoss EAP in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/jboss-eap-azure-best-practices/).

Wenn keines der vorhandenen Angebote ein guter Ausgangspunkt ist, können Sie die Bereitstellung mithilfe von Azure-VMs und anderen verfügbaren Ressourcen manuell reproduzieren. Weitere Informationen finden Sie unter [Was ist IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)

### <a name="azure-marketplace-offers"></a>Azure Marketplace-Angebote

Red Hat hat in Zusammenarbeit mit Microsoft die folgenden Angebote im Azure Marketplace veröffentlicht. Sie können auf diese Angebote über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/) oder das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) zugreifen. Weitere Einzelheiten erfahren Sie im Artikel [Bereitstellen von Red Hat JBoss Enterprise Platform (EAP) für Azure-VMs und -VM-Skalierungsgruppen mithilfe des Azure Marketplace-Angebots](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/jboss-eap-azure-marketplace/).

Dieses Marketplace-Angebot umfasst verschiedene Kombinationen von JBoss EAP- und RHEL-Versionen mit flexiblen Modellen für das Abonnement von Support. JBoss EAP ist mit „Bring-Your-Own-Subscription (BYOS)“ verfügbar, aber für RHEL können Sie zwischen BYOS und nutzungsbasierter Zahlung wählen. Das Azure Marketplace-Angebot umfasst Planoptionen für JBoss EAP unter RHEL als eigenständige bzw. gruppierte VMs oder gruppierte VM-Skalierungsgruppen. Es folgen die 6 Pläne:

- JBoss EAP 7.3 unter RHEL 8.3 (eigenständige VM, nutzungsbasierte Zahlung)
- JBoss EAP 7.3 unter RHEL 8.3 (eigenständige VM, BYOS)
- JBoss EAP 7.3 unter RHEL 8.3 (gruppierte VMs, nutzungsbasierte Zahlung)
- JBoss EAP 7.3 unter RHEL 8.3 (gruppierte VMs, BYOS)
- JBoss EAP 7.3 unter RHEL 8.3 (gruppierte VM-Skalierungsgruppen, nutzungsbasierte Zahlung)
- JBoss EAP 7.3 unter RHEL 8.3 (gruppierte VM-Skalierungsgruppen, BYOS)

### <a name="azure-quickstart-templates"></a>Azure-Schnellstartvorlagen

Neben Azure Marketplace-Angeboten werden auch Schnellstartvorlagen zur Verfügung gestellt, mit denen Sie EAP in Azure testen können. Diese Schnellstarts enthalten vordefinierte ARM-Vorlagen und ein Skript zum Bereitstellen von JBoss EAP in Azure mit verschiedenen Konfigurationen und Versionskombinationen. Es folgen mögliche Lösungsarchitekturen:

- JBoss EAP unter RHEL (eigenständige VM)
- JBoss EAP unter RHEL (gruppierte VMs)
- JBoss EAP unter RHEL (gruppierte VM-Skalierungsgruppen)

Wählen Sie für den schnellen Einstieg eine der Schnellstartvorlagen aus, die Ihrer Versionskombination von JBoss EAP unter RHEL am ehesten entspricht. Weitere Informationen finden Sie in der Dokumentation zum [Schnellstart für JBoss EAP in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/jboss-eap-on-rhel). 

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Konto mit einem aktiven Abonnement**: Wenn Sie kein Azure-Abonnement haben, können Sie Ihre [Visual Studio Subscription-Abonnentenvorteile (ehemals MSDN)](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder [ein kostenloses Konto erstellen](https://azure.microsoft.com/pricing/free-trial).

- **JBoss EAP-Installation**: Sie benötigen ein Red Hat-Konto mit RHSM-Berechtigung (Red Hat Subscription Management) für JBoss EAP. Mit dieser Berechtigung können Sie die für Red Hat getestete und zertifizierte JBoss EAP-Version herunterladen.  Wenn Sie keine EAP-Berechtigung haben, können Sie sich über [Red Hat Developer Subscription for Individuals](https://developers.redhat.com/register) für ein kostenloses Entwicklerabonnement registrieren. Nach der Registrierung finden Sie die erforderlichen Anmeldeinformationen (Pool-IDs) im [Red Hat-Kundenportal](https://access.redhat.com/management/).

- **RHEL-Optionen**: Wählen Sie zwischen „Nutzungsbasierte Zahlung“ und „Bring-Your-Own-Subscription (BYOS)“. Bei BYOS müssen Sie Ihr [RHEL Gold Image](https://azure.microsoft.com/updates/red-hat-enterprise-linux-gold-images-now-available-on-azure/) für [Red Hat Cloud Access](https://access.redhat.com/) aktivieren, bevor Sie das Marketplace-Angebot mithilfe der Lösungsvorlage bereitstellen. Befolgen Sie [diese Anweisungen](https://access.redhat.com/documentation/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/enabling-and-maintaining-subs_cloud-access), um RHEL Gold-Images für den Einsatz in Microsoft Azure zu aktivieren.

- **[Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/overview)**

- **Java-Quellcode und [Java Development Kit (JDK)](https://www.oracle.com/java/technologies/javase-downloads.html)**

- **[Auf JBoss EAP 7.2 basierende Java-Anwendung](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/development_guide/index#become_familiar_with_java_enterprise_edition_8)** oder **[Auf JBoss EAP 7.3 basierende Java-Anwendung](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html-single/development_guide/index#get_started_developing_applications)**

**RHEL-Optionen**: Wählen Sie zwischen nutzungsbasierter Zahlung oder BYOS. Für BYOS müssen Sie Ihr RHEL Gold Image für [Red Hat Cloud Access](https://access.redhat.com/documentation/red_hat_subscription_management/1/html-single/red_hat_cloud_access_reference_guide/index) aktivieren, bevor Sie das Azure Marketplace-Angebot nutzen. BYOS-Angebote werden im Azure-Portal im Abschnitt „Privates Angebot“ angezeigt. 

**Produktversionen**

* [JBoss EAP 7.2](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/)
* [JBoss EAP 7.3](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.3/)
* [RHEL 7.7](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux77-ARM)
* [RHEL 8.0](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux80-ARM)

## <a name="migration-flow-and-architecture"></a>Migrationsablauf und -architektur

In diesem Abschnitt werden kostenlose Tools zum Migrieren von JBoss EAP-Anwendungen von einem anderen Anwendungsserver zur Ausführung unter JBoss EAP und von herkömmlichen lokalen Servern in die Microsoft Azure-Cloudumgebung beschrieben. 

### <a name="red-hat-migration-toolkit-for-applications-mta"></a>Red Hat Migration Toolkit for Applications (MTA)

Es wird empfohlen, das Red Hat MTA zu Beginn des Planungszyklus für die Migration von Java-Anwendungen zu verwenden, bevor Sie ein EAP-bezogenes Migrationsprojekt umsetzen. Das MTA ist eine Zusammenstellung von Tools, die umfangreiche Java-Anwendungsmodernisierungs- und -Migrationsprojekte für eine Vielzahl von [Transformationen und Anwendungsfällen unterstützen](https://developers.redhat.com/products/mta/use-cases). Es beschleunigt die Anwendungscodeanalyse und Codemigration, unterstützt die Aufwandsschätzung und hilft Ihnen beim Verlagern von Anwendungen in die Cloud und Container.

:::image type="content" source="./media/migration-toolkit.png" alt-text="Abbildung der Dashboardseite der Migration Toolkit-App.":::

Mit dem Red Hat MTA können Sie Anwendungen von anderen Anwendungsservern zu Red Hat JBoss EAP migrieren.

## <a name="pre-migration"></a>Vor der Migration

Führen Sie vor Beginn einer Migration die in den folgenden Abschnitten beschriebenen Schritte zur Bewertung und Bestandsermittlung aus, um eine erfolgreiche Migration zu gewährleisten.

### <a name="validate-the-compatibility"></a>Überprüfen der Kompatibilität

Es wird empfohlen, das aktuelle Bereitstellungsmodell und die aktuelle Version zu überprüfen, bevor Sie die Migration planen. Möglicherweise müssen Sie erhebliche Änderungen an Ihrer Anwendung vornehmen, wenn Ihre aktuelle Version nicht unterstützt wird.

Das MTA unterstützt Migrationen von Unternehmensanwendungsservern anderer Anbieter wie Oracle WebLogic Server zu JBoss EAP und Upgrades auf die neueste Version von JBoss EAP.

In der folgenden Tabelle werden die am häufigsten unterstützten Migrationspfade beschrieben.

**Tabelle – Unterstützte Migrationspfade: Quelle zu Ziel**

|Quellplattform ⇒ | JBoss EAP 6 | JBoss EAP 7 | Red Hat OpenShift | OpenJDK 8 und 11 | Apache Camel 3 | Spring Boot für RH-Runtimes | Quarkus
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Oracle WebLogic Server | &#x2714; | &#x2714; | &#x2714; | &#x2714; | - | - | - |
| IBM WebSphere Application Server | &#x2714; | &#x2714; | &#x2714; | &#x2714; | - | - | - |
| JBoss EAP 4 | &#x2714; | &#x2714; | X<sup>1</sup> | &#x2714; | - | - | - |
| JBoss EAP 5 | &#x2714; | &#x2714; | &#x2714; | &#x2714; | - | - | - |
| JBoss EAP 7 | – | &#x2714; | &#x2714; | &#x2714; | - | - | - |
| Oracle JDK | - | - | &#x2714; | &#x2714; | - | - | - |
| Apache Camel 2 | - | - | &#x2714; | &#x2714; | &#x2714; | - | - |
| SpringBoot | - | - | &#x2714; | &#x2714; | - | &#x2714; | &#x2714; |
| Java-Anwendung | - | - | &#x2714; | &#x2714; | - | - | - |

<sup>1</sup> Obwohl das MTA derzeit keine Regeln für diesen Migrationspfad bereitstellt, kann Red Hat Consulting bei der Migration von jeder Quellplattform zu JBoss EAP 7 helfen.

:::image type="content" source="./media/jboss-cli-image.png" alt-text="Abbildung der Ausgabe im CLI-Fenster.":::

Sie können auch die [Systemanforderungen](https://access.redhat.com/documentation/en/migration_toolkit_for_applications/5.0/html-single/introduction_to_the_migration_toolkit_for_applications/index#system_requirements_getting-started-guide) für das MTA überprüfen.

Überprüfen Sie die [von JBoss EAP 7.3 unterstützten Konfigurationen](https://access.redhat.com/articles/2026253#EAP_73) und die [von JBoss EAP 7.2 unterstützten Konfigurationen](https://access.redhat.com/articles/2026253#EAP_72), bevor Sie die Migration planen.

Melden Sie sich an Ihrem Server an, und führen Sie den folgenden Befehl aus, um Ihre aktuelle Java-Version zu ermitteln:

```
java -version
```

### <a name="validate-operating-mode"></a>Überprüfen des Betriebsmodus

JBoss EAP wird unter RHEL, Windows Server und Oracle Solaris unterstützt. JBoss EAP wird entweder in einem eigenständigen Serverbetriebsmodus für die Verwaltung einzelner Instanzen oder in einem verwalteten Domänenbetriebsmodus für die Verwaltung von Gruppen von Instanzen über einen zentralen Verwaltungspunkt ausgeführt.

Verwaltete JBoss EAP-Domänen werden in Microsoft Azure nicht unterstützt. Nur eigenständige JBoss EAP-Serverinstanzen werden unterstützt. Beachten Sie, dass das Konfigurieren von JBoss EAP-Clustern mithilfe eigenständiger JBoss EAP-Server in Azure unterstützt wird. Auf diese Weise werden mithilfe des Azure Marketplace-Angebots Ihre gruppierten VMs oder VM-Skalierungsgruppen erstellt.

### <a name="inventory-server-capacity"></a>Inventarisieren der Serverkapazität

Dokumentieren Sie die Hardware (Arbeitsspeicher, CPU, Datenträger usw.) der aktuellen Produktionsserver sowie die durchschnittliche und maximale Anzahl von Anforderungen und die Ressourcennutzung. Sie benötigen diese Informationen unabhängig vom gewählten Migrationspfad. Weitere Informationen zu den Größen finden Sie unter [Größen für Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs).

### <a name="inventory-all-secrets"></a>Bestand: Alle Geheimnisse

Vor der Einführung von „Configuration-as-a-Service“-Technologien, z. B. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oder [Azure App Configuration](https://azure.microsoft.com/services/app-configuration/), gab es kein überlegt definiertes Konzept für „Geheimnisse“. Stattdessen haben Sie über viele unterschiedliche Konfigurationseinstellungen verfügt, die quasi als die nun verwendeten „Geheimnisse“ gedient haben. Bei App-Servern, z. B. JBoss EAP, sind diese Geheimnisse in vielen unterschiedlichen Konfigurationsdateien und -speichern enthalten. Überprüfen Sie alle Eigenschaften und Konfigurationsdateien auf den Produktionsservern auf Geheimnisse und Kennwörter. Überprüfen Sie unbedingt *jboss-web.xml* in Ihren WAR-Dateien. Unter Umständen finden Sie in Ihrer Anwendung auch Konfigurationsdateien mit Kennwörtern oder Anmeldeinformationen. Weitere Informationen zu Azure Key Vault finden Sie unter [Grundlegende Konzepte von Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/basic-concepts).

### <a name="inventory-all-certificates"></a>Inventarisieren aller Zertifikate

Dokumentieren Sie alle Zertifikate, die für öffentliche SSL-Endpunkte verwendet werden. Sie können alle Zertifikate auf den Produktionsservern anzeigen, indem Sie den folgenden Befehl ausführen:

```cli
keytool -list -v -keystore <path to keystore>
```

### <a name="inventory-jndi-resources"></a>Bestand: JNDI-Ressourcen

- Erfassen Sie den Bestand aller JNDI-Ressourcen (Java Naming and Directory Interface). Für andere Ressourcen, z. B. JMS-Broker (Java Message Service), ist ggf. eine Migration oder Neukonfiguration erforderlich.

### <a name="insideyyour-application"></a>Untersuchen Ihrer Anwendung 

Untersuchen Sie die Datei WEB-INF/jboss-web.xml und/oder WEB-INF/web.xml.

### <a name="document-data-sources"></a>Dokumentdatenquellen

Verwendet Ihre Anwendung Datenbanken, müssen Sie die folgenden Informationen erfassen:

* Wie lautet der Name der Datenquelle?
* Wie ist der Verbindungspool konfiguriert?
* Wo finde ich die JAR-Datei des JDBC-Treibers (Java Database Connectivity)?

Weitere Informationen finden Sie in der JBoss EAP-Dokumentation unter [Informationen zu JBoss EAP-Datenquellen](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/datasource_management).

### <a name="determine-whether-and-how-the-file-system-is-used"></a>Ermitteln, ob und wie das Dateisystem verwendet wird

Für jegliche Nutzung des Dateisystems auf dem Anwendungsserver sind erneute Konfigurationen oder in selteneren Fällen auch Architekturänderungen erforderlich. Das Dateisystem kann von JBoss EAP-Modulen oder von Ihrem Anwendungscode genutzt werden. Unter Umständen kann es zu den Szenarien kommen, die in den folgenden Abschnitten beschrieben sind.

**Schreibgeschützter statischer Inhalt**

Falls mit Ihrer Anwendung derzeit statischer Inhalt bereitgestellt wird, benötigen Sie dafür einen anderen Speicherort. Sie können beispielsweise erwägen, statischen Inhalt in Azure Blob Storage zu verlagern und [Azure CDN (Azure Content Delivery Network)](https://docs.microsoft.com/azure/cdn/) hinzuzufügen, um global eine sehr hohe Downloadgeschwindigkeit zu erzielen. Weitere Informationen finden Sie unter [Hosten von statischen Websites in Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)und[Schnellstart: Integrieren eines Azure-Speicherkontos in Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-create-a-storage-account-with-cdn).

**Dynamisch veröffentlichter statischer Inhalt**

Wenn Ihre Anwendung statischen Inhalt zulässt, der von Ihrer Anwendung hochgeladen bzw. produziert wird, nach der Erstellung aber unveränderlich ist, können Sie [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/) und Azure CDN wie oben beschrieben nutzen. Hierbei können Sie auch eine [Azure-Funktion](https://docs.microsoft.com/azure/azure-functions/) zum Verarbeiten von Uploads und der CDN-Aktualisierung verwenden. Eine entsprechende Beispielimplementierung finden Sie unter [Hochladen und CDN-Vorabladen von statischem Inhalt mit Azure Functions](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn).

**Dynamischer oder interner Inhalt**

Für Dateien, für die von Ihrer Anwendung häufige Schreib- und Lesevorgänge durchgeführt werden (z. B. temporäre Datendateien), oder für statische Dateien, die nur für Ihre Anwendung sichtbar sind, können Sie [Azure Storage](https://docs.microsoft.com/azure/storage/)-Freigaben als persistente Volumes bereitstellen. Weitere Informationen finden Sie unter [Dynamisches Erstellen und Verwenden eines persistenten Volumes mit Azure Files in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/azure-files-dynamic-pv).

### <a name="determine-whether-a-connection-to-on-premises-is-needed"></a>Ermitteln, ob eine Verbindung mit der lokalen Umgebung erforderlich ist

Wenn Ihre Anwendung auf Ihre lokalen Dienste zugreifen muss, müssen Sie einen der Konnektivitätsdienste von Azure bereitstellen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/). Alternativ müssen Sie Ihre Anwendung so umgestalten, dass öffentlich zugängliche APIs genutzt werden, die von Ihren lokalen Ressourcen verfügbar gemacht werden.

### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>Ermitteln, ob JMS-Warteschlangen oder -Themen verwendet werden

Wenn Ihre Anwendung JMS-Warteschlangen oder -Themen nutzt, müssen diese zu einem extern gehosteten JMS-Server migriert werden. Azure Service Bus und das Advanced Message Queuing Protocol (AMQP) können bei Verwendung von JMS eine hervorragende Migrationsstrategie sein. Weitere Informationen finden Sie unter [Verwenden von JMS mit Azure Service Bus und AMQP 1.0](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp) oder [Senden und Empfangen von Nachrichten für Azure Service Bus-Warteschlangen (JavaScript)](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-queues).

Wenn beständige JMS-Speicher konfiguriert wurden, müssen Sie die zugehörige Konfiguration erfassen und nach dem Migrationsvorgang anwenden.

### <a name="determine-whether-your-application-is-composed-of-multiple-wars"></a>Ermitteln, ob Ihre Anwendung aus mehreren WAR-Dateien besteht

Besteht Ihre Anwendung aus mehreren WAR-Dateien, sollten Sie die einzelnen WAR-Dateien als separate Anwendungen behandeln und für jede Datei diese Anleitung ausführen.

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>Ermitteln, ob Ihre Anwendung als EAR-Datei gepackt ist

Ist Ihre Anwendung als EAR-Datei gepackt, untersuchen Sie die Datei application.xml, und erfassen Sie die Konfiguration.

### <a name="identify-all-outside-processes-and-daemons-running-on-the-production-servers"></a>Ermitteln aller externen Prozesse und Daemons, die auf den Produktionsservern ausgeführt werden

Werden einige Ihrer Prozesse außerhalb des Anwendungsservers ausgeführt (z. B. die Überwachung von Daemons), müssen Sie sie beseitigen oder zu einem anderen Ort migrieren.


## <a name="migration"></a>Migration

### <a name="provision-the-target-infrastructure"></a>Bereitstellen der Zielinfrastruktur

Zum Starten der Migration müssen Sie zunächst die JBoss EAP-Infrastruktur bereitstellen. Es gibt mehrere Bereitstellungsoptionen.

- [**Azure-VM**](https://azure.microsoft.com/overview/what-is-a-virtual-machine/)
- [**Azure-VM-Skalierungsgruppe**](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)
- [**Azure App Service**](https://aka.ms/jboss-app-service-overview)
- [**Azure Red Hat OpenShift (ARO) für Container**](https://azure.microsoft.com/services/openshift)
- [**Azure Container Service**](https://azure.microsoft.com/product-categories/containers/)

Lesen Sie den Abschnitt „Erste Schritte mit Azure Marketplace“, um Ihre Bereitstellungsinfrastruktur zu bewerten, bevor Sie die Umgebung erstellen.

### <a name="perform-the-migration"></a>Durchführen der Migration

Es gibt Tools, die Sie bei der Migration unterstützen können:

* [Red Hat Application Migration Toolkit zum Analysieren von Anwendungen für die Migration](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/migration_guide/index#use_windup_to_analyze_applications_for_migration)
* [JBoss-Servermigrationstool zum Migrieren von Serverkonfigurationen](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/migration_guide/index#migration_tool_server_migration_tool)

Um Ihre Serverkonfiguration von der älteren JBoss EAP-Version zur neueren JBoss EAP-Version zu migrieren, können Sie entweder das [JBoss-Servermigrationstool](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/migration_guide/index#migrate_server_migration_tool_option) verwenden oder mithilfe des [Migrationsvorgangs über die Verwaltungs-CLI](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/migration_guide/index#migrate__migrate_operation_option) eine manuelle Migration vornehmen.

### <a name="run-red-hat-application-migration-toolkit"></a>Ausführen des Red Hat Application Migration Toolkit

Sie können [das JBoss-Servermigrationstool im interaktiven Modus ausführen](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html-single/using_the_jboss_server_migration_tool/index#migration_tool_server_run_interactive_mode). Standardmäßig wird das JBoss-Servermigrationstool interaktiv ausgeführt. In diesem Modus können Sie genau auswählen, welche Serverkonfigurationen migriert werden sollen.

Sie können [das JBoss-Servermigrationstool auch im nicht-interaktiven Modus ausführen](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/using_the_jboss_server_migration_tool/index#migration_tool_server_run_noninteractive_mode). Dieser Modus ermöglicht die Ausführung ohne Eingabeaufforderungen.

### <a name="review-the-result-of-jboss-server-migration-toolkit-execution"></a>Überprüfen des Ergebnisses der Ausführung des Toolkits für die JBoss-Servermigration

Überprüfen Sie nach Abschluss der Migration die migrierten Serverkonfigurationsdateien in den Verzeichnissen *EAP_HOME/standalone/configuration/* und *EAP_HOME/domain/configuration/* . Weitere Informationen finden Sie unter [Überprüfen der Ergebnisse der Ausführung des JBoss-Servermigrationstools](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/html-single/using_the_jboss_server_migration_tool/index#migration_tool_server_results).

### <a name="expose-the-application"></a>Verfügbarmachen der Anwendung

Sie können die Anwendung mit den folgenden Methoden verfügbar machen, die für Ihre Umgebung geeignet sind.

* [Erstellen Sie eine öffentliche IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address) für den Zugriff auf Server und Anwendung.
* [Erstellen Sie eine Jump-VM im gleichen virtuellen Netzwerk (VNet)](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine) in einem anderen (bzw. neuen) Subnetz im gleichen VNet, und greifen Sie über eine Jump-VM auf den Server zu. Mithilfe dieser Jump-VM kann die Anwendung verfügbar gemacht werden.
* [Erstellen Sie eine Jump-VM mit VNet-Peering](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine) in einem anderen virtuellen Netzwerk. Greifen Sie auf den Server zu, und machen Sie die Anwendung mittels [Peering virtueller Netzwerke](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal#peer-virtual-networks) verfügbar.
* Machen Sie die Anwendung mithilfe einer [Application Gateway](https://docs.microsoft.com/azure/application-gateway/quick-create-portal#create-an-application-gateway)-Instanz verfügbar.
* Machen Sie die Anwendung mithilfe eines [externen Lastenausgleichs](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-standard-manage-portal#create-a-standard-load-balancer) verfügbar.

## <a name="post-migration"></a>Nach der Migration

Nachdem Sie die Migrationsziele erreicht haben, die Sie unter Vor der Migration definiert haben, führen Sie einige End-to-End-Akzeptanztests durch. Hiermit soll sichergestellt werden, dass alles wie gewünscht funktioniert. Hier sind einige Themenbereiche für Verbesserungen nach der Migration aufgeführt (keine umfassende Liste):

* Verwenden von Azure Storage zum Bereitstellen von statischem Inhalt auf den VMs. Weitere Informationen finden Sie unter [Anfügen oder Trennen eines Datenträgers an einen oder von einem virtuellen Computer](https://docs.microsoft.com/azure/devtest-labs/devtest-lab-attach-detach-data-disk).
* Stellen Sie Ihre Anwendungen mit Azure DevOps in Ihrem migrierten JBoss-Cluster bereit. Weitere Informationen finden Sie in der [Azure DevOps-Dokumentation zu den ersten Schritten](https://docs.microsoft.com/azure/devops/get-started/?view=azure-devops).
* Erwägen Sie die Verwendung von [Application Gateway](https://docs.microsoft.com/azure/application-gateway/).
* Verbessern Sie Ihre Netzwerktopologie mit erweiterten Diensten für den Lastenausgleich. Weitere Informationen finden Sie unter [Verwenden von Lastenausgleichsdiensten in Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure).
* Nutzen Sie verwaltete Azure-Identitäten, um Geheimnisse zu verwalten und Azure-Ressourcen die rollenbasierte Zugriffssteuerung (RBAC) zuzuweisen. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
* Verwenden Sie Azure Key Vault zum Speichern von Informationen, die als „Geheimnisse“ dienen. Weitere Informationen finden Sie unter [Grundlegende Konzepte von Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/basic-concepts).

## <a name="resource-links-and-support"></a>Links zu Ressourcen und Support

Wenden Sie sich bei Supportfragen, Problemen oder Anpassungsanforderungen an den [Red Hat-Support](https://access.redhat.com/support) oder [Microsoft Azure-Support](https://ms.portal.azure.com/?quickstart=true#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

* Weitere Informationen zu [JBoss EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/getting_started_with_jboss_eap_for_openshift_online/introduction)
* Weitere Informationen zu [Red Hat Subscription Management (Cloudzugriff)](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html-single/red_hat_cloud_access_reference_guide/index)
* Weitere Informationen zu [Azure Virtual Machines](https://azure.microsoft.com/overview/what-is-a-virtual-machine/)
* Weitere Informationen zu [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)
* Weitere Informationen zu [Azure Red Hat OpenShift](https://azure.microsoft.com/services/openshift/)
* Weitere Informationen zu [Azure App Service für Linux](https://docs.microsoft.com/azure/app-service/overview#app-service-on-linux)
* Weitere Informationen zu [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction)
* Weitere Informationen zu [Azure-Netzwerk](https://docs.microsoft.com/azure/networking/networking-overview)

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen von JBoss EAP für RHEL-VM/-VM-Skalierungsgruppen über Azure Marketplace](https://aka.ms/AMP-JBoss-EAP)
* [Konfigurieren einer Java-App für Azure App Service](https://docs.microsoft.com/azure/app-service/configure-language-java)
* [Bereitstellen von JBoss EAP in Azure App Service](https://github.com/JasonFreeberg/jboss-on-app-service): Tutorial
* [Verwenden des Azure App Service-Migrations-Assistenten](https://azure.microsoft.com/services/app-service/migration-assistant/)
* [Verwenden des Red Hat Migration Toolkit for Applications](https://developers.redhat.com/products/mta)
