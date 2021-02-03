---
title: Allgemeine Richtlinien für die Unternehmenssicherheit in Azure HDInsight
description: Einige bewährte Methoden, mit denen die Bereitstellung und Verwaltung des Enterprise-Sicherheitspakets erleichtert wird.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 92ad8362f75cdf0613d4ee95f39c23aa6d4819bb
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98933572"
---
# <a name="enterprise-security-general-information-and-guidelines-in-azure-hdinsight"></a>Allgemeine Informationen und Richtlinien für die Unternehmenssicherheit in Azure HDInsight

Beim Bereitstellen eines sicheren HDInsight-Clusters gibt es einige bewährte Methoden, mit denen die Bereitstellung und Clusterverwaltung erleichtert wird. In diesem Artikel werden einige allgemeine Informationen und Richtlinien erläutert.

## <a name="use-of-secure-cluster"></a>Verwendung eines sicheren Clusters

### <a name="recommended"></a>Empfohlen

* Der Cluster wird von mehreren Benutzern gleichzeitig verwendet.
* Die Benutzer haben verschiedene Zugriffsebenen für dieselben Daten.

### <a name="not-necessary"></a>Nicht erforderlich

* Sie planen, nur automatisierte Aufträge auszuführen (z. B. ein einzelnes Benutzerkonto), und ein Standardcluster ist ausreichend.
* Sie können den Datenimport mit einem Standardcluster durchführen und verwenden dasselbe Speicherkonto auf einem anderen sicheren Cluster, in dem die Benutzer Analyseaufträge ausführen können.

## <a name="use-of-local-account"></a>Verwendung eines lokalen Kontos

* Wenn Sie ein gemeinsam genutztes Benutzerkonto oder ein lokales Konto verwenden, ist es schwierig herauszufinden, wer das Konto zum Ändern der Konfiguration oder des Diensts verwendet hat.
* Die Verwendung lokaler Konten ist problematisch, wenn Benutzer nicht mehr der Organisation angehören.

## <a name="ranger"></a>Ranger

### <a name="policies"></a>Richtlinien

* Standardmäßig verwendet Ranger **Ablehnen** als Richtlinie.

* Wenn der Datenzugriff über einen Dienst erfolgt, bei dem die Autorisierung aktiviert ist:
  * Das Ranger-Autorisierungs-Plug-In wird aufgerufen und erhält den Kontext der Anforderung.
  * Ranger wendet die für den Dienst konfigurierten Richtlinien an. Wenn bei den Ranger-Richtlinien ein Fehler auftritt, wird die Zugriffsüberprüfung auf das Dateisystem verschoben. Einige Dienste wie MapReduce überprüfen nur, ob sich die Datei bzw. der Ordner im Besitz desselben Benutzers befindet, der die Anforderung sendet. Dienste wie Hive überprüfen, ob entweder eine Übereinstimmung des Besitzers oder entsprechende Dateisystemberechtigungen (`rwx`) vorliegen.

* Bei Hive sollte der Benutzer außer über die Berechtigungen zum Erstellen/Aktualisieren/Löschen auch über `rwx`-Berechtigungen für das Verzeichnis im Speicher und alle Unterverzeichnisse verfügen.

* Richtlinien können (vorzugsweise) auf Gruppen statt auf Einzelpersonen angewendet werden.

* Der Ranger-Autorisierer wertet alle Ranger-Richtlinien für diesen Dienst für jede Anforderung aus. Diese Auswertung kann sich auf den Zeitraum auswirken, der für die Annahme des Auftrags oder der Abfrage benötigt wird.

### <a name="storage-access"></a>Speicherzugriff

* Wenn der Speichertyp WASB lautet, ist kein OAuth-Token einbezogen.
* Wenn Ranger die Autorisierung durchgeführt hat, erfolgt der Speicherzugriff mithilfe der verwalteten Identität.
* Hat Ranger keine Autorisierung durchgeführt, erfolgt der Speicherzugriff über das OAuth-Token des Benutzers.

### <a name="hierarchical-name-space"></a>Hierarchischer Namespace

Wenn der hierarchische Namespace nicht aktiviert ist:

* Es gibt keine geerbten Berechtigungen.
* Die einzige Dateisystemberechtigung, die funktioniert, ist die Azure-Rolle **Speicherdaten XXXX**, die dem Benutzer direkt im Azure-Portal zugewiesen werden muss.

### <a name="default-hdfs-permissions"></a>HDFS-Standardberechtigungen

* Standardmäßig haben Benutzer keinen Zugriff auf den Ordner **/** im HDFS (sie müssen der Rolle der Speicherblobbesitzer angehören, damit der Zugriff erfolgreich ist).
* Für das Stagingverzeichnis für MapReduce und andere wird ein benutzerspezifisches Verzeichnis erstellt und es werden `sticky _wx`-Berechtigungen bereitgestellt. Benutzer können darunter Dateien und Ordner erstellen, aber keine anderen Elemente anzeigen.

### <a name="url-auth"></a>URL-Authentifizierung

Wenn die URL-Authentifizierung aktiviert ist:

* Die Konfiguration gibt an, welche Präfixe in der URL-Authentifizierung enthalten sind (z. B. `adl://`).
* Wenn der Zugriff auf diese URL erfolgt, prüft Ranger, ob der Benutzer in der Zulassungsliste enthalten ist.
* Ranger überprüft keine der differenzierten Richtlinien.

## <a name="resource-groups"></a>Ressourcengruppen

Verwenden Sie für jeden Cluster eine neue Ressourcengruppe, um zwischen Clusterressourcen unterscheiden zu können.

## <a name="nsgs-firewalls-and-internal-gateway"></a>NSGs, Firewalls und internes Gateway

* Verwenden Sie Netzwerksicherheitsgruppen (NSGs), um virtuelle Netzwerke zu sperren.
* Verwenden Sie Firewalls für Richtlinien für den ausgehenden Zugriff.
* Verwenden Sie das interne Gateway, das nicht für das öffentliche Internet geöffnet ist.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft.

### <a name="policies"></a>Richtlinien

* Deaktivieren Sie die Richtlinie für bedingten Zugriff mithilfe der auf IP-Adressen basierenden Richtlinie. Hierfür ist es erforderlich, dass Dienstendpunkte in den VNETs aktiviert werden, in denen die Cluster bereitgestellt werden. Wenn Sie einen externen Dienst für MFA verwenden (einen anderen als AAD), funktioniert die auf IP-Adressen basierende Richtlinie nicht.

* Für Verbundbenutzer ist die Richtlinie `AllowCloudPasswordValidation` erforderlich. Da HDInsight den Benutzernamen/das Kennwort direkt zum Abrufen von Token aus Azure AD verwendet, muss diese Richtlinie für alle Verbundbenutzer aktiviert werden.

* Aktivieren Sie Dienstendpunkte, wenn Sie eine Umgehung des bedingten Zugriffs mithilfe vertrauenswürdiger IP-Adressen benötigen.

### <a name="groups"></a>Gruppen

* Stellen Sie Cluster immer mit einer Gruppe bereit.
* Verwenden Sie Azure AD zum Verwalten von Gruppenmitgliedschaften (einfacher als die Verwaltung der einzelnen Dienste im Cluster).

### <a name="user-accounts"></a>Benutzerkonten

* Verwenden Sie für jedes Szenario ein eindeutiges Benutzerkonto. Verwenden Sie beispielsweise ein Konto für den Import, ein anderes für die Abfrage und wieder ein anderes für das Verarbeiten von Aufträgen.
* Verwenden Sie gruppenbasierte Ranger-Richtlinien anstelle von Einzelrichtlinien.
* Planen Sie die Verwaltung von Benutzern, die keinen Zugriff mehr auf Cluster haben sollen.

## <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services

[Azure Active Directory Domain Services](../../active-directory-domain-services/overview.md) (Azure AD DS) stellt verwaltete Domänendienste bereit, z. B. Domänenbeitritt, Gruppenrichtlinie, LDAP (Lightweight Directory Access Protocol) und Kerberos-/NTLM-Authentifizierung, die mit Windows Server Active Directory vollständig kompatibel sind.

Azure AD DS ist erforderlich, damit sichere Cluster einer Domäne beitreten können.
HDInsight kann nicht von lokalen Domänencontrollern oder benutzerdefinierten Domänencontrollern abhängig sein, da dies zu viele Fehlerpunkte, eine gemeinsame Verwendung von Anmeldeinformationen, DNS-Berechtigungen usw. mit sich bringt. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure AD DS](../../active-directory-domain-services/faqs.md).

### <a name="azure-ad-ds-instance"></a>Azure AD DS-Instanz

* Erstellen Sie die Instanz mit `.onmicrosoft.com domain`. Auf diese Weise sind nicht mehrere DNS-Server für die Domäne vorhanden.
* Erstellen Sie ein selbstsigniertes Zertifikat für LDAPS, und laden Sie es in Azure AD DS hoch.
* Verwenden Sie für die Bereitstellung von Clustern ein virtuelles Netzwerk mit Peering (dies ist hilfreich, wenn Sie über mehrere Teams verfügen, die HDInsight ESP-Cluster bereitstellen). Dadurch wird sichergestellt, dass Sie keine Ports (NSGs) im virtuellen Netzwerk mit dem Domänencontroller öffnen müssen.
* Konfigurieren Sie das DNS für das virtuelle Netzwerk ordnungsgemäß (der Azure AD DS-Domänenname sollte ohne Hostdateieinträge aufgelöst werden).
* Wenn Sie den ausgehenden Datenverkehr einschränken, stellen Sie sicher, dass Sie sich über die [Firewall-Unterstützung in HDInsight](../hdinsight-restrict-outbound-traffic.md) informiert haben.

### <a name="properties-synced-from-azure-ad-to-azure-ad-ds"></a>Synchronisierung von Eigenschaften aus Azure AD mit Azure AD DS

* Azure AD Connect synchronisiert lokale Einträge mit Azure AD.
* Azure AD DS wird mit Azure AD synchronisiert.

Azure AD DS synchronisiert Objekte regelmäßig mit Azure AD. Auf dem Blatt für Azure AD DS im Azure-Portal wird der Synchronisierungsstatus angezeigt. In jeder Phase der Synchronisierung können eindeutige Eigenschaften in Konflikt geraten und umbenannt werden. Achten Sie auf die Eigenschaftenzuordnung zwischen Azure AD und Azure AD DS.

Weitere Informationen finden Sie unter [Auffüllung des UserPrincipalName-Attributs in Azure AD](../../active-directory/hybrid/plan-connect-userprincipalname.md) und [Funktionsweise der Azure AD DS-Synchronisierung](../../active-directory-domain-services/synchronization.md).

### <a name="password-hash-sync"></a>Kennworthashsynchronisierung

* Kennwörter werden anders als andere Objekttypen synchronisiert. Es werden nur nicht umkehrbare Kennworthashes in Azure AD und Azure AD DS synchronisiert.
* Die Synchronisierung lokaler Einträge mit Azure AD muss über AD Connect aktiviert werden.
* Die Synchronisierung von Azure AD mit Azure AD DS erfolgt automatisch (Latenzen liegen unter 20 Minuten).
* Kennworthashes werden nur synchronisiert, wenn ein geändertes Kennwort vorliegt. Wenn Sie die Kennworthashsynchronisierung aktivieren, werden vorhandene Kennwörter nicht automatisch synchronisiert, da sie als nicht umkehrbar gespeichert sind. Wenn Sie das Kennwort ändern, werden die Kennworthashes synchronisiert.

### <a name="computer-objects-location"></a>Speicherort von Computerobjekten

Jedem Cluster ist eine einzelne Organisationseinheit zugeordnet. Ein interner Benutzer wird in der Organisationseinheit bereitgestellt. Alle Knoten sind in derselben Organisationseinheit in die Domäne eingebunden.

### <a name="active-directory-administrative-tools"></a>Active Directory-Verwaltungstools

Die Schritte zum Installieren der Active Directory-Verwaltungstools auf einer Windows Server-VM finden Sie unter [Installieren von Verwaltungstools](../../active-directory-domain-services/tutorial-create-management-vm.md).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="cluster-creation-fails-repeatedly"></a>Bei der Clustererstellung tritt wiederholt ein Fehler auf

Häufigste Ursachen:

* Die DNS-Konfiguration ist nicht richtig, beim Domänenbeitritt von Clusterknoten tritt ein Fehler auf.
* NSGs sind zu restriktiv und verhindern einen Domänenbeitritt.
* Die verwaltete Identität besitzt keine ausreichenden Berechtigungen.
* Die ersten sechs Zeichen des Clusternamens sind nicht eindeutig (stimmen entweder mit einem anderen aktiven Cluster oder mit einem gelöschten Cluster überein).

## <a name="authentication-setup-and-configuration"></a>Einrichtung und Konfiguration der Authentifizierung

### <a name="user-principal-name-upn"></a>Benutzerprinzipalname (User Principal Name, UPN)

* Verwenden Sie für alle Dienste Kleinbuchstaben. Bei UPNs wird die Groß-/Kleinschreibung in ESP-Clustern nicht beachtet. Allerdings gilt:
* Das UPN-Präfix muss beiden SAMAccountName-Werten in Azure AD DS entsprechen. Eine Übereinstimmung mit dem E-Mail-Feld ist nicht erforderlich.

### <a name="ldap-properties-in-ambari-configuration"></a>LDAP-Eigenschaften in der Ambari-Konfiguration

Eine vollständige Liste der Ambari-Eigenschaften, die sich auf die Konfiguration Ihres HDInsight-Cluster auswirken, finden Sie unter [Einrichtung der Ambari-LDAP-Authentifizierung](https://ambari.apache.org/1.2.1/installing-hadoop-using-ambari/content/ambari-chap2-4.html).

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurationen des Enterprise-Sicherheitspakets mit Azure Active Directory Domain Services in Azure HDInsight](./apache-domain-joined-configure-using-azure-adds.md)

* [Synchronisieren von Azure Active Directory-Benutzern in einen HDInsight-Cluster](../hdinsight-sync-aad-users-to-cluster.md)
