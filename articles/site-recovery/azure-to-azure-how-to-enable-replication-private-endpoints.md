---
title: Aktivieren der Replikation für private Endpunkte in Azure Site Recovery
description: In diesem Artikel erfahren Sie, wie Sie die Replikation virtueller Computer mit privaten Endpunkten zwischen Azure-Regionen mithilfe von Site Recovery konfigurieren.
author: Harsha-CS
ms.author: harshacs
ms.service: site-recovery
ms.topic: article
ms.date: 07/14/2020
ms.custom: references_regions
ms.openlocfilehash: 86f18be73966cb07489630191420b846622e45b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98629826"
---
# <a name="replicate-machines-with-private-endpoints"></a>Replizieren von Computern mit privaten Endpunkten

Mit Azure Site Recovery können Sie private [Azure Private Link](../private-link/private-endpoint-overview.md)-Endpunkte für die Replikation Ihrer Computer von einem isolierten virtuellen Netzwerk aus verwenden. Der Zugriff auf einen Wiederherstellungstresor über private Endpunkte wird in allen kommerziellen Azure-Regionen und Azure Government-Regionen unterstützt.

Dieser Artikel enthält Anweisungen für die folgenden Schritte:

- Erstellen eines Azure Backup Recovery Services-Tresors, um Ihre Computer zu schützen
- Aktivieren einer verwalteten Identität für den Tresor und Erteilen der erforderlichen Berechtigungen für den Zugriff auf Kundenspeicherkonten, um Datenverkehr von Quell- zu Zielspeicherorten zu replizieren. (Speicherzugriff auf verwaltete Identitäten ist nötig, wenn der Private Link-Zugriff auf den Tresor eingerichtet wird)
- Vornehmen von notwendigen DNS-Änderungen für private Endpunkte
- Erstellen und Genehmigen privater Endpunkte für einen Tresor in einem virtuellen Netzwerk
- Erstellen privater Endpunkte für Speicherkonten: Sie können nach Bedarf weiterhin öffentlichen Zugriff oder Firewallzugriff auf den Speicher zulassen. Die Erstellung eines privaten Endpunkts für den Speicherzugriff ist für Azure Site Recovery optional.
  
Im Folgenden finden Sie eine Referenzarchitektur, die Änderungen am Replikationsworkflow mit privaten Endpunkten veranschaulicht.

:::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/architecture.png" alt-text="Referenzarchitektur für Site Recovery mit privaten Endpunkten":::

## <a name="prerequisites-and-caveats"></a>Voraussetzungen und Einschränkungen

- Private Endpunkte können nur für neue Recovery Services-Tresore erstellt werden, bei denen keine Elemente im Tresor registriert sind. Daher **müssen private Endpunkte erstellt werden, bevor dem Tresor Elemente hinzugefügt werden**. Machen Sie sich mit der Preisstruktur für [private Endpunkte](https://azure.microsoft.com/pricing/details/private-link/) vertraut.
- Wenn ein privater Endpunkt für einen Tresor erstellt wird, wird der Tresor gesperrt und **ist nur über Netzwerke mit privaten Endpunkten zugänglich**.
- Azure Active Directory unterstützt derzeit keine privaten Endpunkte. Daher muss für IPs und vollqualifizierte Domänennamen, die erforderlich sind, damit Azure Active Directory in einer Region funktioniert, der ausgehende Zugriff aus dem gesicherten Netzwerk zugelassen werden. Sie können das Netzwerksicherheitsgruppen-Tag „Azure Active Directory“ und die Azure Firewall-Tags verwenden, um den Zugriff auf Azure Active Directory ggf. zuzulassen.
- In den Subnetzen sowohl der Quellcomputer als auch der Wiederherstellungscomputer **sind mindestens sieben IP-Adressen erforderlich**. Wenn Sie einen privaten Endpunkt für den Tresor erstellen, erstellt Site Recovery fünf private Links für den Zugriff auf die zugehörigen Microservices. Wenn Sie die Replikation aktivieren, werden außerdem zwei weitere private Links für die Kopplung von Quell- und Zielregion hinzugefügt.
- Sowohl im Quell- als auch Wiederherstellungssubnetz **ist eine weitere IP-Adresse erforderlich**. Diese IP-Adresse ist allerdings nur erforderlich, wenn Sie private Endpunkte verwenden müssen, die eine Verbindung mit Cachespeicherkonten herstellen.
  Private Speicherendpunkte können nur im Tarif „Universell v2“ erstellt werden. Machen Sie sich mit der Preisstruktur für [Datenübertragung im Tarif „Universell v2“](https://azure.microsoft.com/pricing/details/storage/page-blobs/) vertraut.

 ## <a name="creating-and-using-private-endpoints-for-site-recovery"></a>Erstellen und Verwenden privater Endpunkte für Site Recovery

 Dieser Abschnitt befasst sich mit den Schritten zum Erstellen und Verwenden privater Endpunkte für Azure Site Recovery innerhalb Ihrer virtuellen Netzwerke.

> [!NOTE]
> Es wird ausdrücklich empfohlen, diese Schritte in der angegebenen Reihenfolge zu befolgen. Andernfalls kann der gerenderte Tresor ggf. keine privaten Endpunkte verwenden, und Sie müssen den Vorgang mit einem neuen Tresor wiederholen.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein Recovery Services-Tresor ist eine Entität, die die Replikationsinformationen von Computern enthält und verwendet wird, um Site Recovery-Vorgänge zu initiieren. Weitere Informationen finden Sie unter [Erstellen eines Recovery Services-Tresors](./azure-to-azure-tutorial-enable-replication.md#create-a-recovery-services-vault).

## <a name="enable-the-managed-identity-for-the-vault"></a>Aktivieren der verwalteten Identität für den Tresor

Eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) ermöglicht dem Tresor den Zugriff auf die Speicherkonten des Kunden. Je nach Szenario benötigt Site Recovery Zugriff auf Quell- und Zielspeicherkonto sowie die Cache- und Protokollspeicherkonten.
Der Zugriff auf verwaltete Identitäten wird benötigt, wenn Sie den Private Links-Dienst für den Tresor verwenden.

1. Navigieren Sie zu Ihrem Recovery Services-Tresor. Klicken Sie unter _Einstellungen_ auf **Identität**.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/enable-managed-identity-in-vault.png" alt-text="Recovery Services-Seite im Azure-Portal":::

1. Ändern Sie den **Status** in _Ein_, und klicken Sie auf **Speichern**.

1. Eine **Objekt-ID** wird generiert, die anzeigt, dass der Tresor jetzt bei Azure Active Directory registriert ist.

## <a name="create-private-endpoints-for-the-recovery-services-vault"></a>Erstellen privater Endpunkte für den Recovery Services-Tresor

Um sowohl Failover als auch Failback für virtuelle Azure-Computer zu aktivieren, benötigen Sie zwei private Endpunkte für den Tresor. Einen privaten Endpunkt für den Schutz von Computern im Quellnetzwerk und einen weiteren für den erneuten Schutz von Computern, für die ein Failover ausgeführt wurde, im Wiederherstellungsnetzwerk.

Stellen Sie sicher, dass Sie während dieses Setups auch ein virtuelles Wiederherstellungsnetzwerk in der Zielregion erstellen.

Erstellen Sie den ersten privaten Endpunkt für Ihren Tresor in Ihrem virtuellen Quellnetzwerk mithilfe des Private Link-Centers im Portal oder über [Azure PowerShell](../private-link/create-private-endpoint-powershell.md). Erstellen Sie den zweiten privaten Endpunkt für den Tresor in Ihrem Wiederherstellungsnetzwerk. Im folgenden werden die Schritte zum Erstellen des privaten Endpunkts im Quellnetzwerk beschrieben. Wiederholen Sie die Schritte zum Erstellen des zweiten privaten Endpunkts.

1. Suchen Sie über die Suchleiste des Azure-Portals nach „Private Link“, und klicken Sie auf das Ergebnis. Dadurch gelangen Sie zum Private Link-Center.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/search-private-links.png" alt-text="Suche im Azure-Portal nach dem Private Link-Center":::

1. Klicken Sie auf der linken Navigationsleiste auf **Private Endpunkte**. Sobald Sie sich auf der Seite „Private Endpunkte“ befinden, klicken Sie auf **\+Hinzufügen**, um mit der Erstellung eines privaten Endpunkts für Ihren Tresor zu beginnen.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints.png" alt-text="Privaten Endpunkt im Private Link-Center erstellen":::

1. Sobald Sie sich auf der Seite „Privaten Endpunkt erstellen“ befinden, müssen Sie Details für die Herstellung der Verbindung mit Ihrem privaten Endpunkt angeben.

   1. **Grundlagen**: Geben Sie die grundlegenden Details für Ihre privaten Endpunkte ein. Die Region muss mit der der Quellcomputer identisch sein.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-basic-tab.png" alt-text="Registerkarte „Grundeinstellungen“, Projektdetails, Abonnement und andere Felder für die Erstellung eines privaten Endpunkts im Azure-Portal":::

   1. **Ressource:** Auf dieser Registerkarte müssen Sie die PaaS-Ressource (Platform-as-a-Service) angeben, für die Sie eine Verbindung herstellen möchten. Wählen Sie _Microsoft.RecoveryServices/vaults_ als **Ressourcentyp** für Ihr ausgewähltes Abonnement aus. Legen Sie anschließend den Namen Ihres Recovery Services-Tresors auf **Ressource** fest, und wählen Sie _Azure Site Recovery_ als **Zielunterressource** aus.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-resource-tab.png" alt-text="Registerkarte „Ressource“, Ressourcentyp, Ressource und untergeordnete Zielressource für die Verknüpfung mit einem privaten Endpunkt im Azure-Portal":::

   1. **Konfiguration:** Geben Sie in der Konfiguration das virtuelle Netzwerk und Subnetz an, in dem der private Endpunkt erstellt werden soll. Dieses virtuelle Netzwerk ist das Netzwerk, in dem der virtuelle Computer vorhanden ist. Aktivieren Sie die Integration mit einer privaten DNS-Zone, indem Sie auf **Ja** klicken. Wählen Sie eine bereits erstellte DNS-Zone aus, oder erstellen Sie eine neue. Wenn Sie auf **Ja** klicken, wird die Zone automatisch mit dem virtuellen Quellnetzwerk verknüpft, und es werden die DNS-Einträge für die DNS-Auflösung der neuen IP-Adressen und vollqualifizierten Domänennamen hinzugefügt, die für den privaten Endpunkt erstellt wurden.

      Stellen Sie sicher, dass Sie für jeden neuen privaten Endpunkt, der eine Verbindung mit dem gleichen Tresor herstellt, eine neue DNS-Zone erstellen. Wenn Sie eine vorhandene private DNS-Zone auswählen, werden die vorherigen CNAME-Einträge überschrieben. Lesen Sie den [Leitfaden für private Endpunkte](../private-link/private-endpoint-overview.md#private-endpoint-properties), bevor Sie fortfahren.

      Wenn Ihre Umgebung auf einem Hub-Spoke-Modell basiert, benötigen Sie nur einen privaten Endpunkt und eine private DNS-Zone für das gesamte Setup, da das Peering für alle virtuellen Netzwerke bereits aktiviert ist. Weitere Informationen finden Sie unter [DNS-Konfiguration für private Azure-Endpunkte](../private-link/private-endpoint-dns.md#virtual-network-workloads-without-custom-dns-server).

      Befolgen Sie die Schritte unter [Erstellen privater DNS-Zonen und manuelles Hinzufügen von DNS-Einträgen](#create-private-dns-zones-and-add-dns-records-manually), um die private DNS-Zone manuell zu erstellen.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-configuration-tab.png" alt-text="Registerkarte „Konfiguration“ mit Netzwerk und DNS-Integration für die Konfiguration eines privaten Endpunkts im Azure-Portal":::

   1. **Tags**: Optional können Sie Tags für Ihren privaten Endpunkt hinzufügen.

   1. **Überprüfen \+ erstellen**: Klicken Sie nach Abschluss der Validierung auf **Erstellen**, um den privaten Endpunkt zu erstellen.

Nachdem der private Endpunkt erstellt wurde, werden dem privaten Endpunkt fünf vollqualifizierte Domänennamen hinzugefügt. Diese Links ermöglichen den Computern im virtuellen Netzwerk den Zugriff auf alle erforderlichen Site Recovery-Microservices im Kontext des Tresors. Wenn Sie später die Replikation aktivieren, werden dem gleichen privaten Endpunkt zwei zusätzliche vollständig qualifizierte Domänennamen hinzugefügt.

Die fünf Domänennamen werden nach folgendem Muster formatiert:

`{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

## <a name="approve-private-endpoints-for-site-recovery"></a>Genehmigen privater Endpunkte für Site Recovery

Wenn der Benutzer, der den privaten Endpunkt erstellt, auch der Besitzer des Recovery Services-Tresors ist, wird der zuvor erstellte private Endpunkt innerhalb weniger Minuten automatisch genehmigt. Andernfalls muss der Besitzer des Tresors den privaten Endpunkt genehmigen, bevor Sie diesen nutzen können. Navigieren Sie auf der Seite des Wiederherstellungstresors unter „Einstellungen“ zu **Verbindungen mit privatem Endpunkt**, um eine angeforderte private Endpunktverbindung zu genehmigen oder zu verweigern.

Sie können zur privaten Endpunktressource wechseln, um den Status der Verbindung zu überprüfen, bevor Sie fortfahren.

:::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/vault-private-endpoint-connections.png" alt-text="Seite „Verbindungen mit privatem Endpunkt“ des Tresors und Verbindungsliste im Azure-Portal":::

## <a name="optional-create-private-endpoints-for-the-cache-storage-account"></a><a name="create-private-endpoints-for-the-cache-storage-account"></a>(Optional) Erstellen privater Endpunkte für das Cachespeicherkonto

Es kann ein privater Endpunkt für Azure Storage verwendet werden. Das Erstellen privater Endpunkte für den Speicherzugriff ist für die Azure Site Recovery-Replikation _optional_. Beim Erstellen eines privaten Endpunkts für den Speicher gelten die folgenden Anforderungen:

- Sie benötigen einen privaten Endpunkt für das Cache-/Protokollspeicherkonto in Ihrem virtuellen Quellnetzwerk.
- Zum Zeitpunkt des erneuten Schutzes der Computer, für die ein Failover ausgeführt wurde, benötigen Sie einen zweiten privaten Endpunkt im Wiederherstellungsnetzwerk. Dieser private Endpunkt ist für das neue Speicherkonto bestimmt, das in der Zielregion erstellt wird.

> [!NOTE]
> Auch wenn keine privaten Endpunkte im Speicherkonto aktiviert sind, wäre der Schutz erfolgreich. Der Replikationsdatenverkehr würde jedoch zu den öffentlichen Endpunkten von Azure Site Recovery geleitet. Um sicherzustellen, dass der Replikationsdatenverkehr über private Links fließt, müssen private Endpunkte für das Speicherkonto aktiviert sein.

> [!NOTE]
> Private Speicherendpunkte können nur in Speicherkonten mit dem Tarif **Universell v2** erstellt werden. Preisinformationen finden Sie unter [Standardpreise für Seitenblobs](https://azure.microsoft.com/pricing/details/storage/page-blobs/).

Befolgen Sie den [Leitfaden zur Erstellung von privatem Speicher](../private-link/tutorial-private-endpoint-storage-portal.md#create-storage-account-with-a-private-endpoint), um ein Speicherkonto mit privatem Endpunkt zu erstellen. Klicken Sie auf **Ja**, um die Integration mit einer privaten DNS-Zone zu aktivieren. Wählen Sie eine bereits erstellte DNS-Zone aus, oder erstellen Sie eine neue.

## <a name="grant-required-permissions-to-the-vault"></a>Erteilen der erforderlichen Berechtigungen für den Tresor

Wenn Ihre virtuellen Computer verwaltete Datenträger verwenden, müssen Sie die Berechtigungen der verwalteten Identität nur den Cachespeicherkonten erteilen. Wenn die virtuellen Computer verwaltete Datenträger verwenden, müssen Sie die Berechtigungen der verwalteten Identität den Quell-, Cache- und Zielspeicherkonten erteilen. In diesem Fall müssen Sie das Zielspeicherkonto im Voraus erstellen.

Bevor Sie die Replikation virtueller Computer aktivieren, benötigt die verwaltete Identität des Tresors je nach Art des Speicherkontos folgende Rollenberechtigungen:

- Resource Manager-basierte Speicherkonten (Standard):
  - [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor)
  - [Mitwirkender an Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)
- Resource Manager-basierte Speicherkonten (Premium):
  - [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor)
  - [Besitzer von Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-owner)
- Klassische Speicherkonten:
  - [Mitwirkender von klassischem Speicherkonto](../role-based-access-control/built-in-roles.md#classic-storage-account-contributor)
  - [Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“](../role-based-access-control/built-in-roles.md#classic-storage-account-key-operator-service-role)

In den folgenden Schritten wird beschrieben, wie Sie Ihren Speicherkonten nacheinander eine Rollenzuweisung hinzufügen:

1. Navigieren Sie in Ihrem Speicherkonto zu **Zugriffssteuerung (IAM)** auf der linken Seite.

1. Klicken Sie auf der Seite **Zugriffssteuerung (IAM)** im Feld „Rollenzuweisung hinzufügen“ auf **Hinzufügen**.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/storage-role-assignment.png" alt-text="Seite „Zugriffssteuerung (IAM)“ im Speicherkonto und Schaltfläche „Rollenzuweisung hinzufügen“ im Azure-Portal":::

1. Wählen Sie auf der Seite „Rollenzuweisung hinzufügen“ aus der Dropdownliste **Rolle** die gewünschte Rolle aus. Geben Sie den **Tresornamen** ein, und klicken Sie auf **Speichern**.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/storage-role-assignment-select-role.png" alt-text="Seite „Zugriffssteuerung (IAM)“ eines Speicherkontos mit Optionen zur Auswahl der Rolle und des Prinzipals, dem diese Rolle im Azure-Portal zugewiesen werden soll":::

Zusätzlich zu diesen Berechtigungen muss auch vertrauenswürdigen Microsoft-Diensten der Zugriff gewährt werden. Aktivieren Sie unter „Firewalls und virtuelle Netzwerke“ das Kontrollkästchen „Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben“ unter **Ausnahmen**.

## <a name="protect-your-virtual-machines"></a>Schützen Ihrer virtuellen Computer

Nachdem alle oben aufgeführten Konfigurationen abgeschlossen sind, können Sie die Replikation für Ihre virtuellen Computer fortsetzen. Alle Site Recovery-Vorgänge funktionieren ohne weitere Schritte, wenn die DNS-Integration beim Erstellen privater Endpunkte im Tresor verwendet wurde. Wenn die DNS-Zonen jedoch manuell erstellt und konfiguriert werden, sind zusätzliche Schritte erforderlich, um nach dem Aktivieren der Replikation den Quell- und Ziel-DNS-Zonen bestimmte DNS-Einträge hinzuzufügen. Weitere Informationen finden Sie unter [Erstellen privater DNS-Zonen und manuelles Hinzufügen von DNS-Einträgen](#create-private-dns-zones-and-add-dns-records-manually).

## <a name="create-private-dns-zones-and-add-dns-records-manually"></a>Erstellen privater DNS-Zonen und manuelles Hinzufügen von DNS-Einträgen

Wenn Sie die Option für die Integration mit privaten DNS-Zonen nicht bei der Erstellung des privaten Endpunkts für den Tresor ausgewählt haben, befolgen Sie diese Schritte:

Erstellen Sie eine private DNS-Zone, um dem Mobilitäts-Agent das Auflösen voll qualifizierter Private Link-Domänennamen in private IPs zu ermöglichen.

1. Erstellen einer privaten DNS-Zone

   1. Suchen Sie auf der Suchleiste **Alle Dienste** nach „Private DNS-Zone“, und wählen Sie aus der Dropdownliste „Private DNS-Zonen“ aus.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/search-private-dns-zone.png" alt-text="Suche nach „Private DNS-Zone“ auf der Seite „Neue Ressourcen“ im Azure-Portal":::

   1. Klicken Sie im auf der Seite „Private DNS-Zonen“ auf die Schaltfläche **\+Hinzufügen**, um mit dem Erstellen einer neuen Zone zu beginnen.

   1. Geben Sie auf der Seite „Private DNS-Zone erstellen“ die erforderlichen Details ein. Geben Sie den Namen der privaten DNS-Zone im Format `privatelink.siterecovery.windowsazure.com` ein. Sie können eine beliebige Ressourcengruppe und ein beliebiges Abonnements auswählen, um diese zu erstellen.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-dns-zone.png" alt-text="Registerkarte „Grundeinstellungen“ der Seite „Private DNS-Zone erstellen“ und Projektdetails im Azure-Portal":::

   1. Überprüfen und erstellen Sie die DNS-Zone anschließend über die Registerkarte **Überprüfen \+ erstellen**.

1. Verknüpfen einer privaten DNS-Zone mit Ihrem virtuellen Netzwerk

   Die zuvor erstellten privaten DNS-Zonen müssen nun mit dem virtuellen Netzwerk verbunden werden, in dem sich Ihre Server derzeit befinden. Außerdem müssen Sie die private DNS-Zone im Voraus mit dem virtuellen Zielnetzwerk verknüpfen.

   1. Wechseln Sie zu der privaten DNS-Zone, die Sie im vorherigen Schritt erstellt haben, und navigieren Sie dann auf der linken Seite zu **VNET-Verknüpfungen**. Klicken Sie dort auf die Schaltfläche **\+Hinzufügen**.

   1. Geben Sie die erforderlichen Details ein. Die Felder **Abonnement** und **Virtuelles Netzwerk** müssen mit entsprechenden Angaben zum virtuellen Netzwerk Ihrer Server ausgefüllt werden. Die anderen Felder müssen unverändert bleiben.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/add-virtual-network-link.png" alt-text="Seite zum Hinzufügen eines VNET-Links mit Linkname, Abonnement und zugehörigen virtuellen Netzwerken im Azure-Portal":::

1. Hinzufügen von DNS-Einträgen

   Sobald Sie die erforderlichen privaten DNS-Zonen und die privaten Endpunkte erstellt haben, müssen Sie Ihren DNS-Zonen DNS-Einträge hinzufügen.

   > [!NOTE]
   > Wenn Sie eine benutzerdefinierte private DNS-Zone verwenden, müssen Sie dafür sorgen, dass ähnliche Einträge wie folgt ergänzt werden.

   Hierfür müssen Sie Einträge für jeden vollqualifizierten Domänennamen im privaten Endpunkt in der privaten DNS-Zone erstellen.

   1. Wechseln Sie zu Ihrer privaten DNS-Zone, und navigieren Sie auf der linken Seite zum Abschnitt **Übersicht**. Klicken Sie dort auf **\+Ressourceneintragssatz** hinzufügen, um mit dem Hinzufügen von Einträgen zu beginnen.

   1. Fügen Sie auf der daraufhin geöffneten Seite „Datensatzgruppe hinzufügen“ für jeden vollqualifizierten Domänennamen und jede private IP-Adresse einen _A-Eintrag_ hinzu. Die Liste der vollqualifizierten Domänennamen und IP-Adressen finden Sie auf der Seite „Privater Endpunkt“ unter **Übersicht**. Wie im nachstehenden Beispiel veranschaulicht wird, wird der erste vollqualifizierte Domänenname aus dem privaten Endpunkt der in der privaten DNS-Zone festgelegten Datensatzgruppe hinzugefügt.

      Diese vollqualifizierten Domänennamen entsprechen folgendem Muster: `{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/add-record-set.png" alt-text="Seite zum Hinzufügen eines DNS-A-Eintrags für den vollqualifizierten Domänennamen zum privaten Endpunkt im Azure-Portal":::

   > [!NOTE]
   > Nachdem Sie die Replikation aktiviert haben, werden zwei weitere vollständig qualifizierte Domänennamen in beiden Regionen auf den privaten Endpunkten erstellt. Stellen Sie sicher, dass Sie auch die DNS-Einträge für diese neu erstellten vollständig qualifizierten Domänennamen hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre privaten Endpunkte jetzt erfolgreich für die Replikation virtueller Computer aktiviert. Weitere Informationen finden Sie auf den folgenden Seiten:

- [Replizieren von Azure-VMs in eine andere Azure-Region](./azure-to-azure-how-to-enable-replication.md)
- [Tutorial: Einrichten der Notfallwiederherstellung für Azure-VMs](./azure-to-azure-tutorial-enable-replication.md)