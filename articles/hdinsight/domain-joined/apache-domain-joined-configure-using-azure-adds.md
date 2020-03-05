---
title: Unternehmenssicherheit mit Azure AD DS – Azure HDInsight
description: Erfahren Sie mehr über das Einrichten und Konfigurieren eines HDInsight-Clusters des Enterprise-Sicherheitspakets mithilfe von Azure Active Directory Domain Services.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seodec18
ms.date: 02/03/2020
ms.openlocfilehash: cf239cbf69f3816e5ec03e07e2bd5fe370308f22
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196857"
---
# <a name="enterprise-security-package-configurations-with-azure-active-directory-domain-services-in-hdinsight"></a>Konfigurationen des Enterprise-Sicherheitspakets mit Azure Active Directory Domain Services in Azure HDInsight

ESP-Cluster (Enterprise Security Package, Enterprise-Sicherheitspaket) ermöglichen Mehrbenutzerzugriff in Azure HDInsight-Clustern. HDInsight-Cluster mit ESP sind mit einer Domäne verbunden, sodass Domänenbenutzer ihre Domänenanmeldeinformationen zum Authentifizieren beim Cluster verwenden und Big Data-Aufträge ausführen können.

In diesem Artikel erfahren Sie, wie Sie einen HDInsight-Cluster mit ESP mit Azure Active Directory Domain Services (Azure AD DS) einrichten und konfigurieren.

> [!NOTE]  
> ESP ist in HDInsight 3.6 und 4.0 für diese Clustertypen allgemein verfügbar: Apache Spark, Interactive, Hadoop und HBase. ESP für den Clustertyp Apache Kafka befindet sich in der Vorschauphase mit bestmöglichem Support. ESP-Cluster, die vor dem Datum der allgemeinen Verfügbarkeit von ESP (1. Oktober 2018) erstellt wurden, werden nicht unterstützt.

## <a name="enable-azure-ad-ds"></a>Aktivieren von Azure AD DS

> [!NOTE]  
> Nur Mandantenadministratoren verfügen über die Berechtigungen zum Aktivieren von Azure AD DS. Wenn Azure Data Lake Storage Gen1 oder Gen2 als Clusterspeicher verwendet wird, müssen Sie die Azure Multi-Factor Authentication nur für Benutzer deaktivieren, die mit grundlegenden Kerberos-Authentifizierungen auf den Cluster zugreifen müssen. 
>
> Sie können [vertrauenswürdige IP-Adressen](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) oder [bedingten Zugriff](../../active-directory/conditional-access/overview.md) verwenden, um Multi-Factor Authentication *nur* für bestimmte Benutzer zu deaktivieren, wenn diese auf den IP-Adressbereich für das virtuelle Netzwerk des HDInsight-Clusters zugreifen. Bei Verwendung von bedingtem Zugriff muss der Active Directory-Dienstendpunkt im virtuellen HDInsight-Netzwerk aktiviert sein.
>
> Deaktivieren Sie Multi-Factor Authentication nicht, wenn der Clusterspeicher Azure Blob Storage ist.

Das Aktivieren von Azure AD DS ist eine Voraussetzung zum Erstellen eines HDInsight-Clusters mit ESP. Weitere Informationen finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/tutorial-create-instance.md). 

Wenn Azure AD DS aktiviert ist, beginnen alle Benutzer und Objekte standardmäßig mit der Synchronisierung von Azure Active Directory (Azure AD) zu Azure AD DS. Die Dauer des Synchronisierungsvorgangs hängt von der Anzahl von Objekten in Azure AD ab. Die Synchronisierung kann bei Hunderttausenden von Objekten einige Tage dauern. 

Der mit Azure AD DS verwendete Domänenname darf maximal 39 Zeichen lang sein, da er ansonsten nicht mit HDInsight verwendet werden kann.

Sie können auswählen, nur die Gruppen zu synchronisieren, die Zugriff auf die HDInsight-Cluster benötigen. Diese Option, nur bestimmte Gruppen zu synchronisieren, wird als *bereichsbezogene Synchronisierung* bezeichnet. Anweisungen finden Sie unter [Konfigurieren der bereichsbezogenen Synchronisierung von Azure AD mit Ihrer verwalteten Domäne](../../active-directory-domain-services/scoped-synchronization.md).

Wenn Sie Secure LDAP aktivieren, geben Sie den Domänennamen im Antragstellernamen und im alternativen Antragstellernamen im Zertifikat ein. Wenn Ihr Domänenname beispielsweise *contoso100.onmicrosoft.com* lautet, stellen Sie sicher, dass der genaue Name im Antragstellernamen und im alternativen Antragstellernamen des Zertifikats vorhanden ist. Weitere Informationen finden Sie unter [Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne](../../active-directory-domain-services/tutorial-configure-ldaps.md). 

Im folgenden Beispiel wird ein selbstsigniertes Zertifikat erstellt. Der Domänenname *contoso100.onmicrosoft.com* befindet sich sowohl in `Subject` (Antragstellername) als auch in `DnsName` (alternativer Antragstellername).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Überprüfen des AD DS-Integritätsstatus
Zeigen Sie den Integritätsstatus von den Azure Active Directory Domain Services an, indem Sie **Integrität** unter der Kategorie **Verwalten** auswählen. Stellen Sie sicher, dass der Status von AD DS grün (wird ausgeführt) und die Synchronisierung abgeschlossen ist.

![Azure AD DS-Integrität](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Erstellen und Autorisieren einer verwalteten Identität

Sie können eine *benutzerseitig zugewiesene verwaltete Identität* verwenden, um Domänendienstvorgänge zu vereinfachen und zu schützen. Wenn Sie der verwalteten Identität die Rolle **HDInsight-Domänendienste: Mitwirkender** zuweisen, kann sie Domänendienstvorgänge lesen, erstellen, ändern und löschen. 

Bestimmte Domänendienstvorgänge wie das Erstellen von Organisationseinheiten und Dienstprinzipalen sind für das HDInsight-Enterprise-Sicherheitspaket erforderlich. Sie können in jedem Abonnement verwaltete Identitäten erstellen. Weitere allgemeine Informationen zu verwalteten Identitäten finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md). Weitere Informationen zur Funktionsweise verwalteter Identitäten in Azure HDInsight finden Sie unter [Verwaltete Identitäten in Azure HDInsight](../hdinsight-managed-identities.md).

Erstellen Sie zum Einrichten von ESP-Clustern eine benutzerseitig zugewiesene verwaltete Identität, falls noch keine vorhanden ist. Die genaue Vorgehensweise finden Sie unter [Erstellen, Auflisten, Löschen oder Zuweisen einer Rolle zu einer benutzerseitig zugewiesenen verwalteten Identität über das Azure-Portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). 

Weisen Sie der verwalteten Identität anschließend in der **Zugriffssteuerung** für Azure AD DS die Rolle **HDInsight-Domänendienste: Mitwirkender** zu. Sie benötigen Azure AD DS-Administratorrechte, um diese Rollenzuweisung vorzunehmen.

![Azure Active Directory Domain Services-Zugriffssteuerung](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Die Zuweisung der Rolle **HDInsight-Domänendienste: Mitwirkender** stellt sicher, dass die Identität über den richtigen Zugriff (im Auftrag von) verfügt, um Domänendienstvorgänge in der Azure AD DS-Domäne auszuführen. Diese Vorgänge umfassen das Erstellen und Löschen von Organisationseinheiten.

Wenn die verwaltete Identität erstellt und der richtigen Rolle zugewiesen wurde, kann der Azure AD DS-Administrator festlegen, wer diese verwaltete Identität verwenden darf. Zuerst wählt der Administrator die verwaltete Identität im Portal aus, und dann wählt er unter **Übersicht** die Option **Zugriffssteuerung (IAM)** aus. Anschließend weist der Administrator auf der rechten Seite den Benutzern oder Gruppen, die HDInsight ESP-Cluster erstellen möchten, die Rolle **Operator für verwaltete Identität** zu. 

Der Azure AD DS-Administrator kann diese Rolle z. B. der Gruppe **MarketingTeam** für die verwaltete Identität **sjmsi** zuweisen, wie in der folgenden Abbildung dargestellt. Diese Zuweisung stellt sicher, dass die richtigen Personen in der Organisation die verwaltete Identität zum Erstellen von ESP-Clustern verwenden können.

![Rollenzuweisung „Operator für verwaltete Identität“ in HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="network-considerations"></a>Netzwerküberlegungen

> [!NOTE]  
> Azure AD DS muss in einem Azure Resource Manager-basierten virtuellen Netzwerk bereitgestellt werden. Klassische virtuelle Netzwerke werden für Azure AD DS nicht unterstützt. Weitere Informationen finden Sie unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network).

Wenn Sie Azure AD DS aktiviert haben, wird ein lokaler DNS-Server auf den Azure Directory-VMs ausgeführt. Konfigurieren Sie Ihr virtuelles Azure AD DS-Netzwerk, um diese benutzerdefinierten DNS-Server zu verwenden. Wählen Sie **Eigenschaften** in der Kategorie **Verwalten** aus, und sehen Sie unter **IP-Adresse im virtuellen Netzwerk** nach, um die richtigen IP-Adressen zu finden.

![Suchen von IP-Adressen für lokale DNS-Server](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png)

Ändern Sie die Konfiguration der DNS-Server im virtuellen Azure AD DS-Netzwerk, um diese benutzerdefinierten IP-Adressen zu verwenden, indem Sie in der Kategorie **Einstellungen** die Option **DNS-Server** auswählen. Wählen Sie dann die Option **Benutzerdefiniert** aus, geben Sie die erste IP-Adresse in das Textfeld ein, und wählen Sie **Speichern** aus. Gehen Sie genauso vor, um weitere IP-Adressen hinzuzufügen.

![Aktualisieren der DNS-Konfiguration des virtuellen Netzwerks](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Es ist einfacher, die Azure AD DS-Instanz und den HDInsight-Cluster im gleichen virtuellen Azure-Netzwerk zu platzieren. Wenn Sie verschiedene virtuelle Netzwerke verwenden möchten, müssen Sie ein Peering für diese virtuellen Netzwerke ausführen, sodass der Domänencontroller für HDInsight-VMs sichtbar ist. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md). 

Nachdem die virtuellen Netzwerke verknüpft wurden, konfigurieren Sie das virtuelle HDInsight-Netzwerk so, dass es einen benutzerdefinierten DNS-Server verwendet, und geben Sie die privaten Azure AD DS-IP-Adressen als DNS-Serveradressen ein. Wenn beide virtuellen Netzwerke die gleichen DNS-Server verwenden, wird Ihr benutzerdefinierter Domänenname in die richtige IP-Adresse aufgelöst und ist über HDInsight erreichbar. Wenn Ihr Domänenname also beispielsweise `contoso.com` lautet, muss `ping contoso.com` nach diesem Schritt in die richtige Azure AD DS-IP-Adresse aufgelöst werden.

![Konfigurieren von benutzerdefinierten DNS-Servern für ein virtuelles Netzwerk mit Peering](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Wenn Sie in Ihrem HDInsight-Subnetz Regeln für Netzwerksicherheitsgruppen (NSG) verwenden, müssen Sie die [erforderlichen IP-Adressen](../hdinsight-management-ip-addresses.md) für den eingehenden und ausgehenden Datenverkehr zulassen.

Verknüpfen Sie eine Windows-VM mit dem virtuellen HDInsight-Netzwerk oder -Subnetz, und pingen Sie den Domänenname, um zu testen, ob Ihr Netzwerk ordnungsgemäß eingerichtet ist. Dies sollte in eine IP-Adresse aufgelöst werden. Führen Sie **ldp.exe** aus, um auf die Azure AD DS-Domäne zuzugreifen. Verknüpfen Sie dann diese Windows-VM mit der Domäne, um zu bestätigen, dass alle erforderlichen RPC-Aufrufe zwischen Client und Server erfolgreich sind. 

Sie können auch **nslookup** verwenden, um den Netzwerkzugriff auf Ihr Speicherkonto oder eine beliebige externe Datenbank zu bestätigen (z. B. externer Hive-Metastore oder Ranger-Datenbank). Stellen Sie sicher, dass alle [erforderlichen Ports](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) in den NSG-Regeln des Azure AD DS-Subnetzes zulässig sind, wenn eine Netzwerksicherheitsgruppe bei der Sicherung von Azure AD DS hilft. Wenn der Domänenbeitritt dieses virtuellen Windows-Computers erfolgreich ist, können Sie mit dem nächsten Schritt fortfahren und ESP-Cluster erstellen.

## <a name="create-an-hdinsight-cluster-with-esp"></a>Erstellen eines HDInsight-Clusters mit ESP

Wenn Sie die vorherigen Schritte korrekt festgelegt haben, erstellen Sie im nächsten Schritt den HDInsight-Cluster mit aktiviertem ESP. Beim Erstellen eines HDInsight-Clusters können Sie das Enterprise-Sicherheitspaket auf der Registerkarte **Sicherheit + Netzwerkbetrieb** aktivieren. Wenn Sie die Bereitstellung über eine Azure Resource Manager-Vorlage ausführen möchten, laden Sie im Portal einmalig die vorausgefüllte Vorlage auf der Seite **Überprüfen + Erstellen** herunter, um diese später wieder verwenden zu können. 

Sie können das Feature [HDInsight-Identitätsbroker](identity-broker.md) während der Clustererstellung aktivieren. Mit dem Feature „Identitätsbroker“ können Sie sich bei Ambari anmelden, indem Sie Multi-Factor Authentication verwenden und die erforderlichen Kerberos-Tickets erhalten, ohne dass Kennworthashes in Azure AD DS erforderlich sind.

> [!NOTE]  
> Die ersten sechs Zeichen des ESP-Clusternamens müssen in Ihrer Umgebung eindeutig sein. Wählen Sie beispielsweise bei mehreren ESP-Clustern in verschiedenen virtuellen Netzwerken eine Namenskonvention aus, die sicherstellt, dass die ersten sechs Zeichen des Clusternamens eindeutig sind.

![Domänenüberprüfung für das Azure HDInsight Enterprise-Sicherheitspaket](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png)

Sobald Sie ESP aktivieren, werden häufige Fehlkonfigurationen im Zusammenhang mit Azure AD DS automatisch erkannt und validiert. Nachdem Sie diese Fehler behoben haben, können Sie mit dem nächsten Schritt fortfahren.

![Fehlgeschlagene Domänenüberprüfung beim Azure HDInsight Enterprise-Sicherheitspaket](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png)

Zum Erstellen eines HDInsight-Clusters mit ESP müssen Sie die folgenden Parameter angeben:

- **Clusteradministrator**: Wählen Sie aus Ihrer synchronisierten Azure AD DS-Instanz einen Administrator für Ihren Cluster aus. Dieses Domänenkonto muss bereits synchronisiert und in Azure AD DS verfügbar sein.

- **Clusterzugriffsgruppen**: Die Sicherheitsgruppen, deren Benutzer Sie synchronisieren möchten und deren Benutzer Zugriff auf den Cluster haben sollen, sollten in Azure AD DS verfügbar sein. Ein Beispiel hierfür ist die Gruppe HiveUsers. Weitere Informationen dazu finden Sie in [Erstellen einer Gruppe in Azure Active Directory und Hinzufügen von Mitgliedern](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS-URL**: z. B. `ldaps://contoso.com:636`.

Die von Ihnen erstellte verwaltete Identität kann beim Erstellen eines neuen Clusters aus der Dropdownliste für die **benutzerseitig zugewiesene verwaltete Identität** ausgewählt werden.

![Verwaltete Active Directory Domain Services-Identität für Azure HDInsight-Sicherheitspaket (ESP)](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png)erforderlich.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Apache Hive-Richtlinien in HDInsight mit dem Enterprise-Sicherheitspaket](apache-domain-joined-run-hive.md).
* Informationen zum Verwenden von HDInsight-Clustern mit ESP finden Sie unter [Verwenden von SSH mit Linux-basiertem Apache Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
