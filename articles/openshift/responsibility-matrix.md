---
title: Azure Red Hat OpenShift Matrix für die Zuweisung der Zuständigkeit
description: Erfahren Sie mehr über den Besitz von Zuständigkeiten für den Betrieb eines Azure Red Hat OpenShift Clusters
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 4/12/2021
author: sakthi-vetrivel
ms.author: suvetriv
keywords: aro, openshift, az aro, red hat, cli, RACI, support
ms.openlocfilehash: 477fe40dc565bf5e20627b4f4cd0d53273240ec3
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108319609"
---
# <a name="overview-of-responsibilities-for-azure-red-hat-openshift"></a>Übersicht über die Zuständigkeiten für Azure Red Hat OpenShift

In diesem Dokument werden die Zuständigkeiten von Microsoft, Red Hat und Kunden für Azure Red Hat OpenShift beschrieben. Weitere Informationen zu Azure Red Hat OpenShift und den Komponenten finden Sie in der Azure Red Hat OpenShift Service Definition.

Während Microsoft und Red Hat den Azure Red Hat OpenShift verwalten, teilt sich der Kunde die Verantwortung für die Funktionalität des Clusters. Während Azure Red Hat OpenShift Cluster in Azure-Kundenabonnements auf Azure-Ressourcen gehostet werden, wird aus der Ferne auf sie zugegriffen. Die zugrundeliegende Plattform und Datensicherheit befindet sich im Besitz von Microsoft und Red Hat.

## <a name="overview"></a>Übersicht
<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong><a href="#incident-and-operations-management">Incident- und Betriebsverwaltung</a></strong>
   </td>
   <td><strong><a href="#change-management">Change Management</a></strong>
   </td>
   <td><strong><a href="#identity-and-access-management">Identitäts- und Zugriffsverwaltung</a></strong>
   </td>
   <td><strong><a href="#security-and-regulation-compliance">Einhaltung von Sicherheits- und Regulierungsanforderungen</a></strong>
   </td>
  </tr>
  <tr>
   <td><a href="#customer-data-and-applications">Kundendaten</a>
   </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
  </tr>
  <tr>
   <td><a href="#customer-data-and-applications">Kundenanwendungen</a>
   </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
  </tr>
  <tr>
   <td><a href="#customer-data-and-applications">Entwicklerdienste </a>
   </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
   <td>Kunde </td>
  </tr>
  <tr>
   <td>Plattformüberwachung </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Protokollierung </td>
   <td>Microsoft und Red Hat </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Shared </td>
  </tr>
  <tr>
   <td>Anwendungsnetzwerke </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Clusternetzwerke </td>
   <td>Microsoft und Red Hat </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Virtuelle Netzwerke </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Shared </td>
   <td>Shared </td>
  </tr>
  <tr>
   <td>Knoten der Steuerungsebene </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Workerknoten </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Clusterversion </td>
   <td>Microsoft und Red Hat </td>
   <td>Shared </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Capacity Management </td>
   <td>Microsoft und Red Hat </td>
   <td>Shared </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Virtueller Speicher </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
  <tr>
   <td>Physische Infrastruktur und Sicherheit </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
   <td>Microsoft und Red Hat </td>
  </tr>
</table>


Tabelle 1. Zuständigkeiten nach Ressource


## <a name="tasks-for-shared-responsibilities-by-area"></a>Aufgaben für gemeinsame Zuständigkeiten nach Bereich 

### <a name="incident-and-operations-management"></a>Incident- und Betriebsverwaltung 

Der Kunde und Microsoft und Red Hat sind gemeinsam für die Überwachung und Wartung eines Azure Red Hat OpenShift verantwortlich. Der Kunde ist für die Verwaltung von Incidents und Vorgängen der [Anwendungsdaten von Kunden](#customer-data-and-applications) und für alle benutzerdefinierten Netzwerke verantwortlich, die der Kunde möglicherweise konfiguriert hat.

<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong>Zuständigkeiten von Microsoft und Red Hat</strong>
   </td>
   <td><strong>Kunden-Zuständigkeiten</strong>
   </td>
  </tr>
  <tr>
   <td>Anwendungsnetzwerke </td>
   <td>
<ul>

<li>Überwachen von (mehreren) Cloudlastenausgleichen und nativen OpenShift-Routerdiensten und Reaktion auf Warnungen.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Überwachen sie die Integrität von Dienst-Endpunkten für den Lastenausgleich.

<li>Überwachen Sie die Integrität von Anwendungsrouten und deren Endpunkten.

<li>Melden von Ausfällen an Microsoft und Red Hat.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Virtuelle Netzwerke
   </td>
   <td>
<ul>

<li>Überwachen Sie cloudbasierte Lastenausgleichskomponenten, Subnetze und Azure-Cloudkomponenten, die für standardbasierte Plattformnetzwerke erforderlich sind, und reagieren Sie auf Warnungen.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Überwachen Sie den Netzwerkdatenverkehr, der optional über eine VNet-zu-VNet-Verbindung, eine VPN-Verbindung oder Private Link-Verbindung konfiguriert ist, um potenzielle Probleme oder Sicherheitsbedrohungen zu erkennen.
</li>
</ul>
   </td>
  </tr>
</table>


Tabelle 2: Gemeinsame Zuständigkeiten für die Verwaltung von Vorfällen und Vorgängen


### <a name="change-management"></a>Change Management

Microsoft und Red Hat sind dafür verantwortlich, Änderungen an der Clusterinfrastruktur und den Diensten zu ermöglichen, die der Kunde steuert, sowie für die Verwaltung der Versionen, die für die Masterknoten, Infrastrukturdienste und Workerknoten verfügbar sind. Der Kunde ist für die Initiierung von Infrastrukturänderungen sowie die Installation und Wartung optionaler Dienste und Netzwerkkonfigurationen im Cluster sowie für alle Änderungen an Kundendaten und Kundenanwendungen verantwortlich.


<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong>Zuständigkeiten von Microsoft und Red Hat</strong>
   </td>
   <td><strong>Kunden-Zuständigkeiten</strong>
   </td>
  </tr>
  <tr>
   <td>Protokollierung </td>
   <td>
<ul>

<li>Zentrales Aggregieren und Überwachen von Plattform-Überwachungsprotokollen.

<li>Bereitstellen der Dokumentation für den Kunden, um die Anwendungsprotokollierung mit Log Analytics über Azure Monitor für Container zu aktivieren.

<li>Stellen Sie Überwachungsprotokolle auf Kundenanforderung bereit.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Installieren Sie den optionalen Standardoperator für die Anwendungsprotokollierung im Cluster.

<li>Installieren, konfigurieren und verwalten Sie alle optionalen App-Protokollierungslösungen, z. B. die Protokollierung von Sidecar-Containern oder Protokollierungsanwendungen von Drittanbietern.

<li>Optimieren Sie die Größe und Häufigkeit von Anwendungsprotokollen, die von Kundenanwendungen erstellt werden, wenn sie sich auf die Stabilität des Clusters auswirken.

<li>Fordern Sie Plattformüberwachungsprotokolle über einen Supportfall an, um bestimmte Vorfälle zu untersuchen.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Anwendungsnetzwerke
   </td>
   <td>
<ul>

<li>Einrichten des Lastenausgleichs für öffentliche Clouds

<li>Richten Sie den nativen OpenShift-Routerdienst ein. Stellen Sie die Möglichkeit bereit, den Router als privat festzulegen und bis zu einem zusätzlichen Router-Shard hinzuzufügen.

<li>Installieren, konfigurieren und verwalten Sie OpenShift SDN-Komponenten für internen Pod-Standarddatenverkehr.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Konfigurieren Sie nicht standardmäßige Podnetzwerkberechtigungen für Projekt- und Podnetzwerke, Podeingang und Podausgang mithilfe von NetworkPolicy-Objekten.

<li>Fordern Sie zusätzliche Dienstlastenausgleiche für bestimmte Dienste an, und konfigurieren Sie sie.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Clusternetzwerke
   </td>
   <td>
<ul>

<li>Richten Sie Clusterverwaltungskomponenten wie öffentliche oder private Dienst-Endpunkte und die erforderliche Integration in virtuelle Netzwerkkomponenten ein.

<li>Richten Sie interne Netzwerkkomponenten ein, die für die interne Clusterkommunikation zwischen Worker- und Masterknoten erforderlich sind.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Geben Sie optionale, nicht standardmäßige IP-Adressbereiche für Computer-CIDR, Dienst-CIDR und Pod-CIDR an, falls dies bei der Bereitstellung des Clusters über den OpenShift-Cluster-Manager erforderlich ist.

<li>Fordern Sie an, dass der API-Dienstendpunkt bei der Clustererstellung oder nach der Clustererstellung über die Azure CLI veröffentlicht wird.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Virtuelle Netzwerke
   </td>
   <td>
<ul>

<li>Richten Sie virtuelle Netzwerkkomponenten ein, die für die Bereitstellung des Clusters erforderlich sind, einschließlich virtueller privater Clouds, Subnetze, Lastenausgleichselemente, Internetgateways, NAT-Gateways usw.

<li>Bieten Sie dem Kunden die Möglichkeit, VPN-Konnektivität mit lokalen Ressourcen, VNet-zu-VNet-Konnektivität und Private Link über den OpenShift-Cluster-Manager zu verwalten.

<li>Ermöglichen Sie Kunden das Erstellen und Bereitstellen von Lastenausgleichs-basierten öffentlichen Clouds für die Verwendung mit Dienstlastenausgleichen.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Richten Sie optionale Netzwerkkomponenten der öffentlichen Cloud ein, z. B. VNet-zu-VNet-Verbindung, VPN-Verbindung oder Private Link-Verbindung, und warten Sie sie.

<li>Fordern Sie zusätzliche Dienstlastenausgleiche für bestimmte Dienste an, und konfigurieren Sie sie.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Clusterversion
   </td>
   <td>
<ul>

<li>Kommunizieren von Zeitplan und Status von Upgrades für Neben- und Wartungsversionen

<li>Veröffentlichen von Änderungs- und Versionshinweisen für kleinere Upgrades und Wartungsupgrades
</li>
</ul>
   </td>
   <td>
<ul>

<li>Initiieren des Upgrades des Clusters

<li>Testen von Kundenanwendungen auf Neben- und Wartungsversionen, um die Kompatibilität sicherzustellen
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Capacity Management
   </td>
   <td>
<ul>

<li>Überwachen der Auslastung der Steuerungsebene (Masterknoten)

<li>Skalieren und/oder Ändern der Größe von Steuerungsebenen-Knoten zur Aufrechterhaltung der Dienstqualität

<li>Überwachen Sie die Nutzung von Kundenressourcen, einschließlich Netzwerk-, Speicher- und Computekapazität. Wenn Features für die automatische Skalierung nicht aktiviert sind, werden Kunden bei Änderungen benachrichtigt, die an Clusterressourcen erforderlich sind (z. B. neue zu skalierende Computeknoten, zusätzlicher Speicher usw.)
</li>
</ul>
   </td>
   <td>
<ul>

<li>Verwenden Sie die bereitgestellten OpenShift Cluster Manager-Steuerelemente, um zusätzliche Workerknoten nach Bedarf hinzuzufügen oder zu entfernen.

<li>Reagieren auf Microsoft- und Red Hat-Benachrichtigungen zu Clusterressourcenanforderungen.
</li>
</ul>
   </td>
  </tr>
</table>


Tabelle 3: Gemeinsame Zuständigkeiten für Change Management


### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung 

Die Identitäts- und Zugriffsverwaltung umfasst alle Zuständigkeiten, um sicherzustellen, dass nur authorisierte Personen Zugriff auf Cluster-, Anwendungs- und Infrastrukturressourcen haben. Dies schließt Aufgaben wie die Bereitstellung von Zugriffssteuerungsmechanismen, die Authentifizierung, Autorisierung und die Verwaltung des Zugriffs auf Ressourcen ein.


<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong>Zuständigkeiten von Microsoft und Red Hat</strong>
   </td>
   <td><strong>Kunden-Zuständigkeiten</strong>
   </td>
  </tr>
  <tr>
   <td>Protokollierung </td>
   <td>
<ul>

<li>Halten Sie sich an einen auf Branchenstandards basierenden mehrstufigen internen Zugriffsprozess für Plattform-Überwachungsprotokolle.

<li>Stellen Sie native OpenShift-RBAC-Funktionen bereit.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Konfigurieren Sie OpenShift RBAC, um den Zugriff auf Projekte und die Anwendungsprotokolle eines Projekts zu steuern.

<li>Bei Protokollierungslösungen von Drittanbietern oder benutzerdefinierten Anwendungen ist der Kunde für die Zugriffsverwaltung verantwortlich.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Anwendungsnetzwerke
   </td>
   <td>
<ul>

<li>Stellen Sie native OpenShift-RBAC-Funktionen bereit.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Konfigurieren Sie OpenShift RBAC, um den Zugriff auf die Routenkonfiguration nach Bedarf zu steuern.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Clusternetzwerke
   </td>
   <td>
<ul>

<li>Stellen Sie native OpenShift-RBAC-Funktionen bereit.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Verwalten der Red Hat-Organisationsmitgliedschaft von Red Hat-Konten.

<li>Verwalten Sie Organisationsadministratoren für Red Hat-Organisationen, um Zugriff auf OpenShift Cluster Manager zu gewähren.

<li>Konfigurieren Sie OpenShift RBAC, um den Zugriff auf die Routenkonfiguration nach Bedarf zu steuern.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Virtuelle Netzwerke
   </td>
   <td>
<ul>

<li>Bereitstellen von Kundenzugriffssteuerungen über OpenShift Cluster Manager.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Verwalten Sie den optionalen Benutzerzugriff auf öffentliche Cloudkomponenten über den OpenShift-Cluster-Manager.
</li>
</ul>
   </td>
  </tr>
</table>


Tabelle 4: Einige weitere Möglichkeiten zur Identitäts- und Zugriffsverwaltung


### <a name="security-and-regulation-compliance"></a>Einhaltung von Sicherheits- und Regulierungsanforderungen 

Die Einhaltung von Sicherheits- und Regulierungsanforderungen umfassen alle Zuständigkeiten und Kontrollen, die die Einhaltung relevanter Gesetze, Richtlinien und Vorschriften sicherstellen.


<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong>Zuständigkeiten von Microsoft und Red Hat</strong>
   </td>
   <td><strong>Kunden-Zuständigkeiten</strong>
   </td>
  </tr>
  <tr>
   <td>Protokollierung </td>
   <td>
<ul>

<li>Senden sie Clusterüberwachungsprotokolle an Microsoft und Red Hat SIEM, um Sicherheitsereignisse zu analysieren. Behalten Sie Überwachungsprotokolle für einen definierten Zeitraum bei, um forensische Analysen zu unterstützen.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Analysieren sie Anwendungsprotokolle auf Sicherheitsereignisse. Senden Sie Anwendungsprotokolle an einen externen Endpunkt, indem Sie Sidecar-Container oder Protokollierungsanwendungen von Drittanbietern protokollieren, wenn eine längere Aufbewahrung erforderlich ist, als vom Standardprotokollstapel angeboten wird.
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Virtuelle Netzwerke
   </td>
   <td>
<ul>

<li>Überwachen Sie virtuelle Netzwerkkomponenten auf potenzielle Probleme und Sicherheitsbedrohungen.

<li>Verwenden Sie weitere öffentliche Microsoft- und Red Hat Azure-Tools für zusätzliche Überwachung und Schutz.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Überwachen Sie optional konfigurierte virtuelle Netzwerkkomponenten auf potenzielle Probleme und Sicherheitsbedrohungen.

<li>Konfigurieren Sie alle erforderlichen Firewallregeln oder Rechenzentrumsschutz nach Bedarf.
</li>
</ul>
   </td>
  </tr>
</table>


Tabelle 5. Gemeinsame Verantwortung für die Einhaltung von Sicherheits- und Regulierungsanforderungen


## <a name="customer-responsibilities-when-using-azure-red-hat-openshift"></a>Kundenaufgaben bei der Verwendung Azure Red Hat OpenShift 


### <a name="customer-data-and-applications"></a>Kundendaten und -anwendungen

Der Kunde ist für die Anwendungen, Workloads und Daten verantwortlich, die er für die Azure Red Hat OpenShift bereitstellt. Microsoft und Red Hat stellen jedoch verschiedene Tools zur Verfügung, mit denen der Kunde Daten und Anwendungen auf der Plattform verwalten kann.


<table>
  <tr>
   <td><strong>Ressource</strong>
   </td>
   <td><strong>Hilfe von Microsoft und Red Hat</strong>
   </td>
   <td><strong>Kunden-Zuständigkeiten</strong>
   </td>
  </tr>
  <tr>
   <td>Kundendaten </td>
   <td>
<ul>

<li>Halten Sie Standards für die Datenverschlüsselung auf Plattformebene gemäß den Branchensicherheits- und Konformitätsstandards ein. 

<li>Stellen Sie OpenShift-Komponenten zum Verwalten von Anwendungsdaten wie Geheimnissen zur Verfügung.

<li>Aktivieren Sie die Integration in Datendienste von Drittanbietern (z. B. Azure SQL), um Daten außerhalb des Clusters und/oder Microsoft und Red Hat Azure zu speichern und zu verwalten.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Übernehmen Sie die Verantwortung für alle auf der Plattform gespeicherten Kundendaten und darüber, wie Kundenanwendungen diese Daten nutzen und verfügbar machen.

<li>Etcd-Verschlüsselung
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>Kundenanwendungen
   </td>
   <td>
<ul>

<li>Stellen Sie Cluster mit installierten OpenShift-Komponenten bereit, damit Kunden auf die OpenShift- und Kubernetes-APIs zugreifen können, um Containeranwendungen bereitzustellen und zu verwalten.

<li>Bieten Sie Zugriff auf OpenShift-APIs, mit denen ein Kunde Operatoren einrichten kann, um dem Cluster Community-, Drittanbieter-, Microsoft- und Red Hat- und Red Hat-Dienste hinzuzufügen. 

<li>Stellen Sie Speicherklassen und Plug-Ins bereit, um persistente Volumes für die Verwendung mit Kundenanwendungen zu unterstützen.
</li>
</ul>
   </td>
   <td>
<ul>

<li>Übernehmen Sie die Verantwortung für Kunden- und Drittanbieteranwendungen, Daten und den gesamten Lebenszyklus.

<li>Wenn ein Kunde Red Hat-, Community-, Drittanbieter-, eigene oder andere Dienste mithilfe von Operatoren oder externen Images zum Cluster hinzufügt, ist der Kunde für diese Dienste und für die Zusammenarbeit mit dem entsprechenden Anbieter (einschließlich Red Hat) zur Behandlung von Problemen verantwortlich.

<li>Verwenden Sie die bereitgestellten Tools und Features zum <a href="https://docs.openshift.com/aro/4/architecture/understanding-development.html#application-types">Konfigurieren und Bereitstellen</a>; <a href="https://docs.openshift.com/aro/4/applications/deployments/deployment-strategies.html">auf dem neuesten Stand halten</a>; <a href="https://docs.openshift.com/dedicated/4/applications/working-with-quotas.html">Einrichten von Ressourcenanforderungen und -grenzwerten</a>; <a href="https://docs.openshift.com/dedicated/4/getting_started/scaling-your-cluster.html">Größe des Clusters so einrichten, dass genügend Ressourcen zum Ausführen von Apps vorhanden sind</a>; <a href="https://docs.openshift.com/dedicated/4/administering_a_cluster/cluster-admin-role.html">Einrichten von Berechtigungen</a>; Integration in andere Dienste; <a href="https://docs.openshift.com/aro/4/openshift_images/images-understand.html">Verwalten aller Imagestreams oder Vorlagen, die der Kunde bereitstellt</a>; <a href="https://docs.openshift.com/dedicated/4/cloud_infrastructure_access/dedicated-understanding-aws.html">extern bedienen</a>; Speichern, Sichern und Wiederherstellen von Daten und andernfalls Verwalten ihrer hoch verfügbaren und resilienten Workloads.

<li>Behalten Sie die Verantwortung für die Überwachung der Anwendungen, die auf Azure Red Hat OpenShift ausgeführt werden. einschließlich der Installation und des Betriebs von Software, um Metriken zu erfassen und Warnungen zu erstellen.
</li>
</ul>
   </td>
  </tr>
</table>


Tabelle 7. Kundenaufgaben für Kundendaten, Kundenanwendungen und Dienste
