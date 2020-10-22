---
title: Verwenden der Funktion „öffentliche IP-Adresse“ in Virtual WAN
description: In diesem Artikel wird erläutert, wie Sie die Funktion „öffentliche IP-Adresse“ in Azure Virtual WAN verwenden.
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 61ed6487bc000a35fd25cabde2b562b6eb08da46
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048303"
---
# <a name="how-to-use-the-public-ip-functionality-in-azure-virtual-wan"></a>Verwenden der Funktion „öffentliche IP-Adresse“ in Azure Virtual WAN

Die öffentliche IP-Adresse ist eine neue Funktion in der Azure VMware Solution-Konnektivität, und Kunden können den öffentlichen Internetzugang auf zwei Arten aktivieren. 

- Anwendungen können unter dem Application Gateway-Lastenausgleich für HTTP-/HTTPS-Datenverkehr gehostet und veröffentlicht werden.
- Sie können über Funktionen für öffentliche IP-Adressen in Azure Virtual WAN veröffentlicht werden.

Als Teil der privaten Cloud-Bereitstellung von Azure VMware Solution werden bei Aktivierung der Funktion „öffentliche IP-Adresse“ die erforderlichen Komponenten mit Automatisierung automatisch erstellt und aktiviert:

-  Virtual WAN

-  Virtual WAN-Hub mit ExpressRoute-Konnektivität

-  Azure Firewall-Dienste mit öffentlicher IP-Adresse

In diesem Artikel wird ausführlich beschrieben, wie Sie die Funktion „öffentliche IP-Adresse“ in Virtual WAN verwenden können, um Ressourcen wie Webserver, virtuelle Computer (VMs) und Hosts über ein öffentliches Netzwerk zugänglich zu machen.

## <a name="prerequisites"></a>Voraussetzungen

- Azure VMware Solution-Umgebung
- Ein Webserver, der in der Azure VMware Solution-Umgebung ausgeführt wird.
- Ein neuer, nicht überlappender IP-Adressbereich für die Virtual WAN-Hubbereitstellung, in der Regel `/24`.

## <a name="reference-architecture"></a>Referenzarchitektur

:::image type="content" source="media/public-ip-usage/public-ip-architecture-diagram.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="false" lightbox="media/public-ip-usage/public-ip-architecture-diagram.png":::

Das Architekturdiagramm zeigt einen Kundenwebserver, der in der Azure VMware Solution-Umgebung gehostet wird und mit privaten RFC1918-IP-Adressen konfiguriert ist.  Dieser Webdienst wird im Internet über die Funktion „öffentliche IP-Adresse“ des Virtual WAN verfügbar gemacht.  Die öffentliche IP-Adresse ist in der Regel eine in Azure Firewall übersetzte Ziel-NAT. Bei DNAT-Regeln übersetzt die Firewallrichtlinie öffentliche IP-Adressanforderungen in eine private Adresse (Webserver) mit einem Port.

Benutzeranforderungen erreichen die Firewall unter einer öffentlichen IP-Adresse, die wiederum mithilfe von DNAT-Regeln in der Azure-Firewall in eine private IP-Adresse übersetzt wird. Die Firewall prüft die NAT-Tabelle, und wenn die Anforderung mit einem Eintrag übereinstimmt, leitet sie den Datenverkehr an die übersetzte Adresse und den Port in der Azure VMware Solution-Umgebung weiter.

Der Webserver empfängt die Anforderung und antwortet der Firewall mit den angeforderten Informationen bzw. der angeforderten Seite. Anschließend leitet die Firewall die Informationen an den Benutzer unter der öffentlichen IP-Adresse weiter.

## <a name="test-case"></a>Testfall
In diesem Szenario müssen Sie den IIS-Webserver im Internet veröffentlichen. Verwenden Sie die Funktion „öffentliche IP-Adresse“ in Azure VMware Solution, um die Website unter einer öffentlichen IP-Adresse zu veröffentlichen.  Sie konfigurieren NAT-Regeln für die Firewall und greifen auf die Azure VMware Solution-Ressource (VMs mit Webserver) mit öffentlicher IP-Adresse zu.

## <a name="deploy-virtual-wan"></a>Bereitstellen von Virtual WAN

1. Melden Sie sich beim Azure-Portal an, suchen Sie nach **Azure VMware Solution**, und wählen Sie diese Option aus.

1. Wählen Sie die private Azure VMware Solution-Cloud aus.

   :::image type="content" source="media/public-ip-usage/avs-private-cloud-resource.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/avs-private-cloud-resource.png":::

1. Wählen Sie unter **Verwalten** die Option **Konnektivität** aus.

   :::image type="content" source="media/public-ip-usage/avs-private-cloud-manage-menu.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/avs-private-cloud-manage-menu.png":::

1. Wählen Sie die Registerkarte **Öffentliche IP-Adresse** aus, und wählen Sie dann **Konfigurieren** aus.

   :::image type="content" source="media/public-ip-usage/connectivity-public-ip-tab.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/connectivity-public-ip-tab.png":::

1. Übernehmen Sie die Standardwerte, oder ändern sie diese, und wählen Sie dann **Erstellen** aus.

   - Ressourcengruppe im virtuellen Fernnetz

   - Name des virtuellen Fernnetzes

   - Adressblock des virtuellen Hubs (mit neuem nicht überlappendem IP-Adressbereich)

   - Anzahl öffentlicher IP-Adressen (1–100)

Die Bereitstellung sämtlicher Komponenten dauert ungefähr eine Stunde. Diese Bereitstellung muss nur einmal erfolgen, um alle zukünftigen öffentlichen IP-Adressen für diese Azure VMware Solution-Umgebung zu unterstützen.  

>[!TIP]
>Sie können den Status über den Bereich **Benachrichtigung** überwachen. 

## <a name="view-and-add-public-ip-addresses"></a>Anzeigen und Hinzufügen öffentlicher IP-Adressen

Sie können weitere öffentliche IP-Adressen überprüfen und hinzufügen, indem Sie die folgenden Schritte ausführen.

1. Suchen Sie im Azure-Portal nach der **Firewall**, und wählen Sie diese aus.

1. Wählen Sie eine bereitgestellte Firewall aus, und wählen Sie dann **Azure Firewall Manager zum Konfigurieren und Verwalten dieser Firewall besuchen** aus.

   :::image type="content" source="media/public-ip-usage/configure-manage-deployed-firewall.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/configure-manage-deployed-firewall.png":::

1. Wählen Sie **Geschützte virtuelle Hubs** aus, und wählen Sie einen virtuellen Hub aus der Liste aus.

   :::image type="content" source="media/public-ip-usage/select-virtual-hub.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" lightbox="media/public-ip-usage/select-virtual-hub.png":::

1. Wählen Sie auf der Seite „Virtueller Hub“ die Option **Öffentliche IP-Konfiguration** aus, und wählen Sie zum Hinzufügen weiterer öffentlicher IP-Adressen die Option **Hinzufügen** aus. 

   :::image type="content" source="media/public-ip-usage/virtual-hub-page-public-ip-configuration.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/virtual-hub-page-public-ip-configuration.png":::

1. Geben Sie die Anzahl der benötigten IP-Adressen an, und wählen Sie **Hinzufügen** aus.

   :::image type="content" source="media/public-ip-usage/add-number-of-ip-addresses-required.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true":::


## <a name="create-firewall-policies"></a>Erstellen von Firewallrichtlinien

Nachdem alle Komponenten bereitgestellt wurden, können Sie diese in der hinzugefügten Ressourcengruppe sehen. Der nächste Schritt besteht darin, eine Firewallrichtlinie hinzuzufügen.

1. Suchen Sie im Azure-Portal nach der **Firewall**, und wählen Sie diese aus.

1. Wählen Sie eine bereitgestellte Firewall aus, und wählen Sie dann **Azure Firewall Manager zum Konfigurieren und Verwalten dieser Firewall besuchen** aus.

   :::image type="content" source="media/public-ip-usage/configure-manage-deployed-firewall.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/configure-manage-deployed-firewall.png":::

1. Wählen Sie **Azure-Firewallrichtlinien** aus, und wählen Sie dann **Azure-Firewallrichtlinie erstellen** aus.

   :::image type="content" source="media/public-ip-usage/create-firewall-policy.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/create-firewall-policy.png":::

1. Geben Sie auf der Registerkarte **Grundlagen** die erforderlichen Details ein, und wählen Sie **Weiter: DNS-Einstellungen** aus. 

1. Wählen Sie auf der Registerkarte **DNS** die Option **Deaktivieren** aus, und klicken Sie dann auf **Weiter: Regeln**.

1. Wählen Sie **Regelsammlung hinzufügen** aus, und geben Sie die folgenden Details an. Wählen Sie **Hinzufügen** aus, und wählen Sie dann **Weiter: Threat Intelligence** aus.

   -  name
   -  Regelsammlungstyp – DNAT
   -  Priorität
   -  Regelsammlungsaktion – Zulassen
   -  Name der Regel
   -  Quelltyp – **IPaddress**
   -  Quelle – **\***
   -  Protokoll – **TCP**
   -  Zielport – **80**
   -  Zieltyp – **IP-Adresse**
   -  Ziel – **öffentliche IP-Adresse**
   -  Übersetzte Adresse –  **private Azure VMware Solution-Webserver-IP-Adresse**
   -  Übersetzter Port – **Azure VMware Solution-Webserverport**

1. Übernehmen Sie den Standardwert, und wählen Sie dann **Weiter: Hubs**.

1. Wählen Sie **Virtuellen Hub zuordnen** aus.

   :::image type="content" source="media/public-ip-usage/associate-virtual-hubs-azure-firewall-policy.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/associate-virtual-hubs-azure-firewall-policy.png":::

1. Wählen Sie einen Hub aus der Liste aus, und wählen Sie **Hinzufügen** aus.

   :::image type="content" source="media/public-ip-usage/secure-hubs-with-azure-firewall-polcy.png" alt-text="Diagramm der Architektur öffentlicher IP-Adressen" border="true" lightbox="media/public-ip-usage/secure-hubs-with-azure-firewall-polcy.png":::

1. Wählen Sie **Weiter: Tags**. 

1. (Optional) Erstellen Sie Name-Wert-Paare, um Ihre Ressourcen zu kategorisieren. 

1. Wählen Sie **Weiter: Überprüfen und erstellen** und dann **Erstellen** aus.

## <a name="limitations"></a>Einschränkungen

Pro SDDC sind 100 öffentliche IP-Adressen zulässig.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Nutzen öffentlicher IP-Adressen unter Verwendung von [Azure Virtual WAN](../virtual-wan/virtual-wan-about.md).

