---
title: Verwenden privater Endpunkte für den sicheren Zugriff auf Purview
description: In diesem Artikel wird beschrieben, wie Sie einen privaten Endpunkt für Ihr Purview-Konto einrichten.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/02/2021
ms.openlocfilehash: 09fa10e7f7751321601c5c4871b2cf36ccf6f01f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104720882"
---
# <a name="use-private-endpoints-for-your-purview-account"></a>Verwenden privater Endpunkte für Ihr Purview-Konto

Sie können private Endpunkte für Ihre Purview-Konten verwenden, um Clients und Benutzern in einem virtuellen Netzwerk (VNET) den sicheren Zugriff auf den Katalog über Private Link zu ermöglichen. Der private Endpunkt verwendet eine IP-Adresse aus dem VNET-Adressraum für Ihr Purview-Konto. Der Netzwerkdatenverkehr zwischen den Clients im VNET und dem Purview-Konto wird über das VNET und eine private Verbindung im Microsoft-Backbone-Netzwerk geleitet, sodass keine Offenlegung im öffentlichen Internet erfolgt.

## <a name="create-purview-account-with-a-private-endpoint"></a>Erstellen eines Purview-Kontos mit einem privaten Endpunkt

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com) und dann zu Ihrem Purview-Konto.

1. Geben Sie die grundlegenden Informationen an, und legen Sie auf der Registerkarte **Netzwerk** „Konnektivitätsmethode“ auf „Privater Endpunkt“ fest. Richten Sie die privaten Erfassungsendpunkte ein, indem Sie Informationen über das **Abonnement, VNET und Subnetz** bereitstellen, die Sie dem privaten Endpunkt zuordnen möchten.

    > [!NOTE]
    > Sie sollten nur dann einen privaten Endpunkt für die Erfassung erstellen, wenn Sie die Netzwerkisolation für End-to-End-Überprüfungsszenarios sowohl für Azure- als auch für lokale Quellen aktivieren möchten. Private Erfassungsendpunkte für AWS-Quellen werden derzeit nicht unterstützt.

    :::image type="content" source="media/catalog-private-link/create-pe-azure-portal.png" alt-text="Erstellen eines privaten Endpunkts im Azure-Portal":::

1. Optional können Sie auch für jeden privaten Erfassungsendpunkt eine **Private DNS-Zone** einrichten.

1. Klicken Sie auf „Hinzufügen“, um einen privaten Endpunkt für Ihr Purview-Konto hinzuzufügen.

1. Legen Sie auf der Seite „Privaten Endpunkt erstellen“ für „Untergeordnete Purview-Ressource“ die Option **account** fest, wählen Sie das virtuelle Netzwerk, das Subnetz und die private DNS-Zone aus, in der das DNS registriert wird. (Sie können auch eigene DNS-Server nutzen oder DNS-Einträge mithilfe von Hostdateien auf den virtuellen Computern erstellen.)

    :::image type="content" source="media/catalog-private-link/create-pe-account.png" alt-text="Auswahl bei der Erstellung eines privaten Endpunkts":::

1. Klicken Sie auf **OK**.

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite Überprüfen und erstellen weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus.

    :::image type="content" source="media/catalog-private-link/validation-passed.png" alt-text="Erfolgreiche Überprüfung der Kontoerstellung":::

## <a name="create-a-private-endpoint-for-the-purview-web-portal"></a>Erstellen eines privaten Endpunkts für das Purview-Webportal

1. Navigieren Sie zu dem soeben erstellten Purview-Konto, und wählen Sie im Abschnitt „Einstellungen“ die Option „Verbindungen mit privatem Endpunkt“ aus.

1. Klicken Sie auf „+ Privater Endpunkt“, um einen neuen privaten Endpunkt zu erstellen.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Erstellen eines privaten Portal-Endpunkts":::

1. Geben Sie die grundlegenden Informationen an.

1. Wählen Sie auf der Registerkarte „Ressource“ als „Ressourcentyp“ die Option **Microsoft.Purview/accounts** aus.

1. Wählen Sie für „Ressource“ das neu erstellte Purview-Konto und für „Untergeordnete Zielressource“ **portal** aus.
    :::image type="content" source="media/catalog-private-link/pe-portal-details.png" alt-text="Details für den privaten Portal Endpunkt":::

1. Wählen Sie auf der Registerkarte „Konfiguration“ das „Virtuelle Netzwerk“ und die „Private DNS-Zone“ aus. Navigieren Sie zur Seite „Zusammenfassung“, und klicken Sie auf **Erstellen**, um den privaten Portal-Endpunkt zu erstellen.

## <a name="enabling-access-to-azure-active-directory"></a>Aktivieren des Zugriffs auf Azure Active Directory

> [!NOTE]
> Wenn die VM, das VPN-Gateway oder das VNET-Peeringgateway über öffentlichen Internetzugriff verfügt, kann die VM bzw. das Gateway auf das Purview-Portal und das Purview-Konto zugreifen, für das private Endpunkte aktiviert sind, und Sie müssen die restlichen Anweisungen nicht befolgen. Wenn für Ihr privates Netzwerk NSG-Regeln festgelegt sind, die den gesamten öffentlichen Internetdatenverkehr ablehnen, müssen Sie Regeln hinzufügen, um den AAD-Zugriff zu aktivieren. Befolgen Sie hierzu die unten stehenden Anweisungen.

Die nachfolgenden Anweisungen gelten für den sicheren Zugriff auf Purview von einer Azure-VM. Wenn Sie ein VPN-Gateway oder ein anderes VNET-Peeringgateway verwenden, müssen Sie ähnliche Schritte ausführen.

1. Navigieren Sie im Azure-Portal zu der VM, und wählen Sie im Abschnitt „Einstellungen“ die Registerkarte „Netzwerk“ aus. Wählen Sie dann „Regeln für ausgehende Ports“ aus, und klicken Sie auf „Regel für ausgehenden Port hinzufügen“.

   :::image type="content" source="media/catalog-private-link/outbound-rule-add.png" alt-text="Hinzufügen von Regel für ausgehenden Port":::

2. Wählen Sie auf dem Blatt „Regel für ausgehenden Port hinzufügen“ für *Ziel* „Diensttag“, für „Zieldiensttag“ **AzureActiveDirectory**, für „Zielportbereiche“ den Eintrag „*“ und für „Aktion“ die Option „Zulassen“ aus. **Die Priorität muss höher als die Priorität der Regel sein, durch die der gesamte Internetdatenverkehr abgelehnt wird**. Erstellen Sie die Regel.

   :::image type="content" source="media/catalog-private-link/outbound-rule-details.png" alt-text="Hinzufügen der Details der Regel für ausgehenden Port":::

3. Führen Sie die gleichen Schritte aus, um eine weitere Regel zu erstellen, die das Diensttag **AzureResourceManager** zulässt. Wenn Sie auf das Azure-Portal zugreifen müssen, können Sie auch eine Regel für das Diensttag *AzurePortal* hinzufügen.

4. Stellen Sie eine Verbindung mit dem virtuellen Computer her, öffnen Sie den Browser, navigieren Sie zur Browserkonsole (STRG+UMSCHALT+J), und wechseln Sie zur Registerkarte „Netzwerk“, um Netzwerkanforderungen zu überwachen. Geben Sie im Feld „URL“ die Adresse „web.purview.azure.com“ ein, und versuchen Sie, sich mit Ihren AAD-Anmeldeinformationen anzumelden. Wahrscheinlich schlägt die Anmeldung fehl, und Sie können auf der Registerkarte „Netzwerk“ der Konsole erkennen, dass AAD versucht, auf „aadcdn.msauth.net“ zuzugreifen, der Zugriff aber blockiert wird.

   :::image type="content" source="media/catalog-private-link/login-fail.png" alt-text="Details der fehlgeschlagenen Anmeldung":::

5. Öffnen Sie in diesem Fall auf dem virtuellen Computer eine Eingabeaufforderung, und pingen Sie diese URL („aadcdn.msauth.net“). Nachdem Sie die zugehörige IP-Adresse abgerufen haben, fügen Sie in den Netzwerksicherheitsregeln der VM eine Regel für ausgehenden Port für die IP-Adresse hinzu. Legen Sie „Ziel“ auf „IP-Adressen“ fest, und legen Sie „Ziel-IP-Adressen/CIDR-Bereiche“ auf die IP-Adresse des AAD-CDN fest. Aufgrund von Load Balancer und Traffic Manager ist die IP-Adresse von AAD CDN möglicherweise eine dynamische IP-Adresse. Nachdem Sie die IP-Adresse abgerufen haben, sollten Sie sie der Hostdatei der VM hinzufügen, um zu erzwingen, dass der Browser diese IP-Adresse besucht, um Zugriff auf AAD CDN zu erhalten.

   :::image type="content" source="media/catalog-private-link/ping.png" alt-text="Testping":::

   :::image type="content" source="media/catalog-private-link/aadcdn-rule.png" alt-text="AAD CDN-Regel":::

6. Nachdem die neue Regel erstellt wurde, navigieren Sie zurück zur VM, und melden Sie sich erneut mit Ihren AAD-Anmeldeinformationen an. Wenn die Anmeldung erfolgreich ist, kann das Purview-Portal verwendet werden. In einigen Fällen erfolgt jedoch eine Umleitung zu anderen Domänen, um sich basierend auf dem Kontotyp des Kunden anzumelden. Beispielsweise erfolgt für ein live.com-Konto eine Umleitung zu „live.com“, um sich anzumelden, und diese Anforderungen werden dann erneut blockiert. Für Microsoft-Mitarbeiterkonten greift AAD auf „msft.sts.microsoft.com“ zu, um Anmeldeinformationen zu erhalten. Überprüfen Sie auf der Registerkarte „Netzwerk“ der Browserkonsole die Netzwerkanforderungen, um die Domäne zu ermitteln, deren Anforderungen blockiert werden. Wiederholen Sie dann den vorherigen Schritt, um ihre IP-Adresse abzurufen, und fügen Sie in der Netzwerksicherheitsgruppe Regeln für ausgehenden Port hinzu, um Anforderungen für diese IP-Adresse zuzulassen. (Fügen Sie nach Möglichkeit der Hostdatei der VM die URL und IP-Adresse hinzu, um die DNS-Auflösung zu korrigieren.) Wenn Sie die genauen IP-Adressbereiche für die Anmeldung bei der Domäne kennen, können Sie sie auch direkt den Netzwerkregeln hinzufügen.

7. Die Anmeldung bei AAD sollte jetzt erfolgreich sein. Das Purview-Portal wird erfolgreich geladen, es können jedoch nicht alle Purview-Konten aufgeführt werden,da nur auf ein bestimmtes Purview-Konto zugegriffen werden kann. Geben Sie *web.purview.azure.com/resource/{PurviewAccountName}* ein, um direkt auf das Purview-Konto zuzugreifen, für das Sie erfolgreich einen privaten Endpunkt eingerichtet haben.
 
## <a name="ingestion-private-endpoints-and-scanning-sources-in-private-networks-vnets-and-behind-private-endpoints"></a>Private Endpunkte für die Erfassung und das Überprüfen von Quellen in privaten Netzwerken, VNETs und hinter privaten Endpunkten

Führen Sie die folgenden Schritte aus, wenn Sie sich vergewissern möchten, dass die Netzwerkisolation für Ihre Metadaten zwischen der Quelle, die gerade überprüft wird, und Purview DataMap aktiviert ist:
1. Führen Sie die [hier](#creating-an-ingestion-private-endpoint) beschriebenen Schritte aus, um einen **privaten Endpunkt für die Erfassung** zu aktivieren.
1. Überprüfen Sie die Quelle mithilfe einer **selbstgehosteten Integration Runtime**.
 
    1. Alle lokalen Quelltypen wie z. B. SQL Server, Oracle oder SAP werden derzeit nur über Überprüfungen mit einer selbstgehosteten Integration Runtime (IR) unterstützt. Die selbstgehostete IR muss innerhalb Ihres privaten Netzwerks ausgeführt und mit Ihrem VNET in Azure per Peering verbunden werden. Aktivieren Sie anschließend Ihr VNET in Azure auf Ihrem privaten Endpunkt für die Erfassung, indem Sie die [hier](#creating-an-ingestion-private-endpoint) beschriebenen Schritte ausführen. 
    1. Für alle **Azure**-Quelltypen wie z. B. Azure Blob Storage oder Azure SQL-Datenbank müssen Sie die Überprüfung mithilfe der selbstgehosteten IR explizit ausführen, um die Netzwerkisolation sicherzustellen. Führen Sie die [hier](manage-integration-runtimes.md) beschriebenen Schritte aus, um eine selbstgehostete Integration Runtime einzurichten. Richten Sie anschließend die Überprüfung für die Azure-Quelle ein, indem Sie in der Dropdownliste **Verbindung über Integration Runtime herstellen** Ihre selbstgehostete IR auswählen, um die Netzwerkisolation sicherzustellen. 
    
    :::image type="content" source="media/catalog-private-link/shir-for-azure.png" alt-text="Ausführen der Überprüfung in Azure mithilfe der selbstgehosteten Integration Runtime":::

> [!NOTE]
> Anmeldungen über MSI werden derzeit nicht unterstützt, wenn Sie Ihre Azure-Quellen mithilfe einer selbstgehosteten IR überprüfen. Verwenden Sie für diese Azure-Quelle eine der anderen unterstützten Anmeldemethoden.

## <a name="enable-private-endpoint-on-existing-purview-accounts"></a>Aktivieren eines privaten Endpunkts für vorhandene Purview-Konten

Es gibt zwei Möglichkeiten, nach dem Erstellen Ihres Purview-Kontos private Purview-Endpunkte hinzuzufügen:

- Mit dem Azure-Portal (Purview-Konto)
- Mit dem Private Link-Center

### <a name="using-the-azure-portal-purview-account"></a>Mit dem Azure-Portal (Purview-Konto)

1. Navigieren Sie im Azure-Portal zum Purview-Konto, und wählen Sie im Abschnitt **Netzwerk** von **Einstellungen** „Verbindungen mit privatem Endpunkt“ aus.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Erstellen eines privaten Endpunkts für das Konto":::

1. Klicken Sie auf „+ Privater Endpunkt“, um einen neuen privaten Endpunkt zu erstellen.

1. Geben Sie die grundlegenden Informationen an.

1. Wählen Sie auf der Registerkarte „Ressource“ als „Ressourcentyp“ die Option **Microsoft.Purview/accounts** aus.

1. Wählen Sie für „Ressource“ das Purview-Konto und für „Untergeordnete Zielressource“ **account** aus.

1. Wählen Sie auf der Registerkarte „Konfiguration“ das **Virtuelle Netzwerk** und die **Private DNS-Zone** aus. Navigieren Sie zur Seite „Zusammenfassung“, und klicken Sie auf **Erstellen**, um den privaten Portal-Endpunkt zu erstellen.

> [!NOTE]
> Sie müssen die gleichen Schritte wie oben für die untergeordnete Zielressource ausführen, für die **portal** ausgewählt wurde.

#### <a name="creating-an-ingestion-private-endpoint"></a>Erstellen eines privaten Endpunkts für die Erfassung

1. Navigieren Sie im Azure-Portal zum Purview-Konto, und wählen Sie im Abschnitt **Netzwerk** von **Einstellungen** „Verbindungen mit privatem Endpunkt“ aus.
1. Navigieren Sie zur Registerkarte **Private Endpunktverbindungen für die Erfassung**, und klicken Sie auf **Neu**, um einen neuen privaten Endpunkt für die Erfassung zu erstellen.

1. Geben Sie die grundlegenden Informationen und VNET-Details an.
 
    :::image type="content" source="media/catalog-private-link/ingestion-pe-fill-details.png" alt-text="Angeben von Details zum privaten Endpunkt":::

1. Klicken Sie auf **Erstellen**, um die Einrichtung abzuschließen.

> [!NOTE]
> Private Endpunkte für die Erfassung können nur über die oben beschriebene Benutzeroberfläche im Purview Azure-Portal erstellt werden. Sie können nicht über Private Link Center erstellt werden.

### <a name="using-the-private-link-center"></a>Mit dem Private Link-Center

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

2. Suchen Sie in der Suchleiste am oberen Rand der Seite nach „Private Link“, und navigieren Sie zum Blatt „Private Link“, indem Sie auf die erste Option klicken.

3. Klicken Sie auf „+ Hinzufügen“, und geben Sie die grundlegenden Informationen an.

   :::image type="content" source="media/catalog-private-link/private-link-center.png" alt-text="Erstellen eines privaten Endpunkts über das Private Link-Center":::

4. Wählen Sie für „Ressource“ das bereits erstellte Purview-Konto und für „Untergeordnete Zielressource“ **account** aus.

5. Wählen Sie auf der Registerkarte „Konfiguration“ das „Virtuelle Netzwerk“ und die „Private DNS-Zone“ aus. Navigieren Sie zur Seite „Zusammenfassung“, und klicken Sie auf **Erstellen**, um den privaten Endpunkt für die untergeordnete Zielressource „account“ zu erstellen.

> [!NOTE]
> Sie müssen die gleichen Schritte wie oben für die untergeordnete Zielressource ausführen, für die **portal** ausgewählt wurde.

## <a name="firewalls-to-restrict-public-access"></a>Firewalls zur Einschränkung des öffentlichen Zugriffs

Führen Sie die folgenden Schritte aus, um den Zugriff auf das Purview-Konto über das öffentliche Internet vollständig zu unterbrechen. Diese Einstellung gilt dann sowohl für private Endpunktverbindungen als auch für private Endpunktverbindungen für die Erfassung.

1. Navigieren Sie im Azure-Portal zum Purview-Konto, und wählen Sie im Abschnitt **Netzwerk** von **Einstellungen** „Verbindungen mit privatem Endpunkt“ aus.
1. Navigieren Sie zur Registerkarte „Firewall“, und vergewissern Sie sich, dass die Umschaltfläche auf **Verweigern** festgelegt ist.

    :::image type="content" source="media/catalog-private-link/private-endpoint-firewall.png" alt-text="Firewalleinstellungen für den privaten Endpunkt":::

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)

- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)
