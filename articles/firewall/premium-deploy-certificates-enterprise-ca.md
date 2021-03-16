---
title: Bereitstellen und Konfigurieren von Zertifikaten der Unternehmenszertifizierungsstelle für die Vorschauversion von Azure Firewall Premium
description: Hier erfahren Sie, wie Sie Zertifikate der Unternehmenszertifizierungsstelle für die Vorschauversion von Azure Firewall Premium bereitstellen und konfigurieren können.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: how-to
ms.date: 03/09/2021
ms.author: victorh
ms.openlocfilehash: fba95214a6bbb0482166eab8f77f30911986fbb7
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102525453"
---
# <a name="deploy-and-configure-enterprise-ca-certificates-for-azure-firewall-preview"></a>Bereitstellen und Konfigurieren von Zertifikaten der Unternehmenszertifizierungsstelle für die Vorschauversion von Azure Firewall Premium

> [!IMPORTANT]
> Azure Firewall Premium ist zurzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Die Vorschauversion von Azure Firewall Premium enthält ein TLS-Überprüfungsfeature, das eine Zertifikatauthentifizierungskette erfordert. Für Produktionsbereitstellungen sollten Sie mithilfe einer Unternehmens-PKI die Zertifikate generieren, die Sie bei Azure Firewall Premium verwenden. Anhand dieses Artikels können Sie ein Zertifikat der Zwischenzertifizierungsstelle für die Vorschauversion von Azure Firewall Premium erstellen und verwalten.

Weitere Informationen zu den von der Vorschauversion für Azure Firewall Premium verwendeten Zertifikaten finden Sie unter [Zertifikate der Vorschauversion von Azure Firewall Premium](premium-certificates.md).

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Wenn Sie ein Zertifikat zum Einsatz bei der Vorschauversion von Azure Firewall Premium über eine Unternehmenszertifizierungsstelle generieren möchten, müssen Sie folgende Ressourcen haben: 

- eine Active Directory-Gesamtstruktur 
- eine Active Directory-Stammzertifizierungsstelle für Zertifizierungsdienste mit aktivierter Webregistrierung 
- eine Firewallrichtlinie für Azure Firewall Premium mit Premium-Tarif 
- einen Azure Key Vault 
- eine verwaltete Identität mit Leseberechtigungen für **Zertifikate und Geheimnisse**, die in der Key Vault-Zugriffsrichtlinie definiert sind 

## <a name="request-and-export-a-certificate"></a>Anfordern und Exportieren eines Zertifikats

1. Greifen Sie auf die Webregistrierungswebsite in der Stammzertifizierungsstelle zu, normalerweise `https://<servername>/certsrv`, und wählen Sie **Zertifikat anfordern** aus.
1. Wählen Sie **Erweiterte Zertifikatanforderung** aus.
1. Wählen Sie **Eine Anforderung an diese Zertifizierungsstelle erstellen und einreichen** aus.
1. Füllen Sie das Formular mithilfe der Vorlage „Untergeordnete Zertifizierungsstelle“ aus, wie hier gezeigt wird:
1. Reichen Sie die Anforderung ein, und installieren Sie das Zertifikat.
1. Wenn diese Anforderung von einem Windows-Server mithilfe von Internet Explorer ausgeführt wird, öffnen Sie **Internetoptionen**.
1. Navigieren Sie zur Registerkarte **Inhalt**, und wählen Sie **Zertifikate** aus.
1. Wählen Sie das gerade ausgestellte Zertifikat und dann **Exportieren** aus.
1. Wählen Sie **Weiter** aus, um den Assistenten zu starten. Wählen Sie **Ja, privaten Schlüssel exportieren** und dann **Weiter** aus.
1. Das PFX-Dateiformat ist standardmäßig ausgewählt. Deaktivieren Sie **Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen**. Wenn Sie die gesamte Zertifikatkette exportieren, schlägt der Importvorgang in Azure Firewall fehl.
1. Weisen Sie ein Kennwort zu, um den Schlüssel zu schützen, und bestätigen Sie es. Wählen Sie dann **Weiter** aus.
1. Wählen Sie einen Dateinamen und einen Exportspeicherort und dann **Weiter** aus.
1. Wählen Sie **Fertigstellen** aus, und verschieben Sie das exportierte Zertifikat an einen sicheren Speicherort.

## <a name="add-the-certificate-to-a-firewall-policy"></a>Hinzufügen des Zertifikats zu einer Firewallrichtlinie

1. Navigieren Sie im Azure-Portal zur Seite „Zertifikate“ Ihrer Key Vault, und wählen Sie **Generieren/Importieren** aus.
1. Wählen Sie als Erstellungsmethode **Importieren** aus, benennen Sie das Zertifikat, wählen Sie die exportierte PFX-Datei aus, geben Sie das Kennwort ein, und wählen Sie **Erstellen** aus.
1. Navigieren Sie zur Seite **TLS-Überprüfung (Vorschau)** Ihrer Firewallrichtlinie, und wählen Sie Ihre „Verwaltete Identität“, „Key Vault“ und „Zertifikat“ aus. 
1. Wählen Sie **Speichern** aus.
   :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/tls-inspection.png" alt-text="TLS-Überprüfung":::

## <a name="validate-tls-inspection"></a>Überprüfen der TLS-Überprüfung

1. Erstellen Sie eine Anwendungsregel mithilfe der TLS-Überprüfung für die Ziel-URL oder des vollqualifizierten Domänennamens (FQDN) Ihrer Wahl.  Beispiel: `*bing.com`.
1. Navigieren Sie auf einem in die Domäne eingebundenen Computer innerhalb des Quellbereichs der Regel zu Ihrem Ziel, und wählen Sie in Ihrem Browser das Sperrsymbol neben der Adressleiste aus. Das Zertifikat sollte zeigen, dass es von Ihrer Unternehmenszertifizierungsstelle statt einer öffentlichen Zertifizierungsstelle ausgestellt wurde.
1. Zeigen Sie das Zertifikat an, um weitere Details zu sehen, einschließlich des Zertifikatpfads.
   :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/certificate-details.png" alt-text="Zertifikatdetails":::
1. Führen Sie in Log Analytics die folgende KQL-Abfrage aus, um alle Anforderungen zurückzugeben, bei denen die TLS-Überprüfung durchgeführt wurde:
   ```
   AzureDiagnostics 
   | where ResourceType == "AZUREFIREWALLS" 
   | where Category == "AzureFirewallApplicationRule" 
   | where msg_s contains "Url:" 
   | sort by TimeGenerated desc
   ```
   Im Ergebnis wird die vollständige URL des überprüften Datenverkehrs gezeigt: :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/kql-query.png" alt-text="KQL-Abfrage":::

## <a name="next-steps"></a>Nächste Schritte

[Vorschauversion von Azure Firewall Premium im Azure-Portal](premium-portal.md)
