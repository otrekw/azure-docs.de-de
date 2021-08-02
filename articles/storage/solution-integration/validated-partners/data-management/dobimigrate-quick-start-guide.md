---
title: Migrieren von Dateidaten zu Azure mit Datadobi DobiMigrate
titleSuffix: Azure Storage
description: Enthält einen Leitfaden für erste Schritte zum Implementieren von Datadoph DobiMigrate und Migrieren von Daten zur Azure Files-, Azure NetApp Files- oder ISV NAS-Lösung
author: dukicn
ms.author: nikoduki
ms.date: 04/27/2021
ms.topic: conceptual
ms.service: storage
ms.subservice: partner
ms.openlocfilehash: 6984230044dc70e6e5e05c7ae0dcb4789065c436
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111955356"
---
# <a name="migrate-data-to-azure-with-datadobi-dobimigrate"></a>Migrieren von Daten zu Azure mit Datadobi DobiMigrate

Dieser Artikel unterstützt Sie bei der Integration der Datadobi DobiMigrate-Infrastruktur in Azure Storage. Er enthält Informationen zu Voraussetzungen, Überlegungen sowie Anleitungen zu Implementierung und Betrieb.

DobiMigrate ermöglicht Datei- und Objektmigrationen zwischen Speicherplattformen. Es migriert Daten schnell, einfach und kostengünstig von der lokalen Umgebung zu Azure.

## <a name="reference-architecture"></a>Referenzarchitektur

Referenzarchitektur für Sicherungsbereitstellungen vom lokalen Standort zu Azure und für Azure-interne Bereitstellungen.

:::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-reference-architecture.png" alt-text=" Referenzarchitektur, die die grundlegende Einrichtung für DobiMigrate beschreibt":::

Ihre vorhandene DobiMigrate-Bereitstellung kann problemlos in Azure integriert werden, indem eine Azure-Verbindung hinzugefügt und konfiguriert wird.

## <a name="before-you-begin"></a>Voraussetzungen

Ein wenig Planung im Vorfeld hilft Ihnen dabei, Azure als Ziel für die Offsitesicherung und als Wiederherstellungssite zu verwenden.

### <a name="get-started-with-azure"></a>Erste Schritte mit Azure

Microsoft bietet ein Framework, das Ihnen den Einstieg in Azure erleichtert. Das [Cloud Adoption Framework](/azure/architecture/cloud-adoption/) (CAF) ist ein detailliertes Konzept für die digitale Transformation in Unternehmen und ein umfassender Leitfaden für die Planung der Cloudeinführung in einer Produktionsumgebung. Das CAF enthält eine Schrittanleitung zum [Einrichten von Azure](/azure/cloud-adoption-framework/ready/azure-setup-guide/), damit Sie es schnell und sicher in Betrieb nehmen können. Eine interaktive Version finden Sie im [Azure-Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade). Sie finden dort Beispielarchitekturen, spezielle Best Practices für die Bereitstellung von Anwendungen sowie kostenlose Trainingsressourcen, mit denen Sie die notwendigen Fachkenntnisse zu Azure erhalten.

### <a name="considerations-for-migrations"></a>Überlegungen zu Migrationen

Bei der Migration von Dateidaten zu Azure sind mehrere wichtige Aspekte zu berücksichtigen. Bevor Sie fortfahren, erfahren Sie mehr:

- [Übersicht über die Speichermigration](../../../common/storage-migration-overview.md)
- neueste von DobiMigrate unterstützte Features in der [Vergleichsmatrix für Migrationstools](./migration-tools-comparison.md)

Denken Sie daran, dass Sie genügend Netzwerkkapazität benötigen, um Migrationen zu unterstützen, ohne Produktionsanwendungen zu beeinträchtigen. In diesem Abschnitt werden die Tools und Techniken beschrieben, die für die Bewertung Ihrer Netzwerkanforderungen verfügbar sind.

#### <a name="determine-unutilized-internet-bandwidth"></a>Nicht ausgelastete Internetbandbreite ermitteln

Es ist wichtig zu wissen, wie viel Spielraum an ungenutzter Bandbreite (oder welchen *Toleranzbereich*) Sie täglich haben. Auf diese Weise können Sie bewerten, ob Sie Ihre Ziele für Folgendes erreichen können:

- Anfangszeit für Migrationen, wenn Sie nicht Azure Data Box für die Offlinemethode verwenden
- erforderliche Zeit für die inkrementelle Neusynchronisierung vor dem endgültigen Wechsel zum Zieldateidienst

Verwenden Sie die folgenden Methoden, um den Spielraum an Bandbreite zu ermitteln, der für Azure zur Verfügung steht.

- Wenn Sie ein vorhandener Azure ExpressRoute-Kunde sind, können Sie die [Inanspruchnahme](../../../../expressroute/expressroute-monitoring-metrics-alerts.md#circuits-metrics) im Azure-Portal anzeigen.
- Wenden Sie sich an Ihren ISP, und fordern Sie Berichte an, die Ihre vorhandene tägliche und monatliche Nutzung zeigen.
- Es gibt eine Reihe von Tools, die durch Überwachung des Netzwerkdatenverkehrs auf Ebene des Routers bzw. Switches die Nutzung messen können:
  - [SolarWinds Bandwidth Analyzer Pack](https://www.solarwinds.com/network-bandwidth-analyzer-pack?CMP=ORG-BLG-DNS)
  - [Paessler PRTG](https://www.paessler.com/bandwidth_monitoring)
  - [Cisco Network Assistant](https://www.cisco.com/c/en/us/products/cloud-systems-management/network-assistant/index.html)
  - [WhatsUp Gold](https://www.whatsupgold.com/network-traffic-monitoring)

## <a name="implementation-guidance"></a>Implementierungsleitfaden

Dieser Abschnitt enthält eine kurze Anleitung zum Hinzufügen einer Azure Files-Freigabe zu einer lokalen Azure DobiMigrate-Bereitstellung. 

1. Öffnen Sie das Azure-Portal, und suchen Sie nach **Speicherkonten**. 

    :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-locate-storage-account.png" alt-text="Zeigt, wo Sie im Suchfeld des Azure-Portals das Wort „Speicher“ eingegeben haben.":::

    Sie können auch auf das standardmäßige Symbol **Speicherkonten** klicken.

    :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-portal.png" alt-text="Zeigt das Hinzufügen von Speicherkonten im Azure-Portal.":::

2. Wählen Sie **Erstellen** aus, um ein Konto hinzuzufügen:
   1. Wählen Sie eine vorhandene Ressourcengruppe aus, oder klicken Sie auf **Neu erstellen**.
   2. Geben Sie einen eindeutigen Namen für Ihr Speicherkonto an.
   3. Wählen Sie die Region aus.
   4. Wählen Sie je nach Ihren Anforderungen die Leistungsoption **Standard** oder **Premium** aus. Wenn Sie **Premium** auswählen, klicken Sie unter **Premium account type** (Premium-Kontotyp) auf **Dateifreigaben**.
   5. Wählen Sie die **[Redundanz](../../../common/storage-redundancy.md)** aus, die Ihre Datenschutzanforderungen erfüllt.
   
   :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-account-create-1.png" alt-text="Zeigt die Speicherkontoeinstellungen im Portal.":::

3. Wir empfehlen, die Standardeinstellungen aus dem Bildschirm **Erweitert** zu übernehmen. Bei der Migration zu Azure Files empfiehlt es sich, **Große Dateifreigaben** zu aktivieren (sofern verfügbar).

   :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-account-create-2.png" alt-text="Zeigt die Registerkarte „Erweiterte Einstellungen“ im Portal an.":::

4. Behalten Sie die standardmäßigen Netzwerkoptionen zunächst bei, und fahren Sie mit **Datenschutz** fort. Sie können die Option „Vorläufiges Löschen“ aktivieren, die es Ihnen ermöglicht, versehentlich gelöschte Daten innerhalb des definierten Aufbewahrungszeitraums wiederherzustellen. Diese Option bietet Schutz vor versehentlichen oder böswilligen Löschvorgängen.

   :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-account-create-3.png" alt-text="Zeigt die Datenschutzeinstellungen im Portal an.":::

5. Fügen Sie ggf. Tags zum Organisieren hinzu, und klicken Sie auf **Erstellen**, um Ihr Konto zu erstellen.
 
6. Sie müssen nur noch zwei schnelle Schritte ausführen, dann können Sie das Konto Ihrer DobiMigrate-Umgebung hinzufügen. Navigieren Sie im Azure-Portal zu dem soeben erstellten Konto, und wählen Sie im Dateidienstmenü die Option „Dateifreigaben“ aus. Fügen Sie eine Dateifreigabe hinzu, und wählen Sie einen aussagekräftigen Namen aus. Navigieren Sie dann in den Einstellungen zu Zugriffsschlüssel, und kopieren Sie den Namen des Speicherkontos und einen der beiden Zugriffsschlüssel.

   :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-access-key.png" alt-text="Zeigt die Einstellungen für Zugriffsschlüssel im Portal an.":::

7. Navigieren Sie zu den Eigenschaften der Azure-Dateifreigabe, und notieren Sie die URL-Adresse. Sie ist erforderlich, um die Azure-Verbindung in DobiMigrate hinzuzufügen:

   :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-files-endpoint.png" alt-text="Suchen Sie nach dem Azure Files-Endpunkt.":::

8. _(Optional)_ Sie können Ihrer Bereitstellung zusätzliche Sicherheitsschichten hinzufügen.
 
   1. Konfigurieren Sie den rollenbasierten Zugriff, um den Kreis der Personen einzuschränken, die Änderungen an Ihrem Speicherkonto vornehmen dürfen. Weitere Informationen finden Sie unter [Integrierte Rollen für Verwaltungsvorgänge](../../../common/authorization-resource-provider.md#built-in-roles-for-management-operations).
 
   2.  Beschränken Sie den Kontozugriff mithilfe der [Einstellungen für die Storage-Firewall](../../../common/storage-network-security.md) auf bestimmte Netzwerksegmente. Konfigurieren Sie Firewalleinstellungen, um Zugriff von außerhalb Ihres Unternehmensnetzwerks zu verhindern.

       :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-storage-firewall.png" alt-text="Zeigt Storage Firewall-Einstellungen im Portal an.":::

   3.  Legen Sie eine [Löschsperre](../../../../azure-resource-manager/management/lock-resources.md) für das Storage-Konto fest, um ein versehentliches Löschen des Kontos zu verhindern.

       :::image type="content" source="./media/dobimigrate-quick-start-guide/azure-resource-lock.png" alt-text="Zeigt das Festlegen einer Löschsperre im Portal.":::

   4.  Konfigurieren Sie zusätzliche [bewährte Sicherheitsmethoden](../../../blobs/security-recommendations.md).

9.  Navigieren Sie in DobiMigrate zu „Configuration“ -> „File Servers“. Klicken Sie auf **Add**, um Microsoft Azure Files als Dateiservertyp hinzuzufügen:

    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-server-type.png" alt-text="Hinzufügen von Microsoft Azure Files als Servertyp":::

10. Geben Sie den Namen, Azure Files-Verbindungsdetails und die Anmeldeinformationen für das Speicherkonto an:
 
    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-connection-details.png" alt-text="Konfigurieren von Azure Files-Verbindungsdetails":::

11. Weisen Sie die Proxys der Azure Files-Verbindung zu, und klicken Sie auf **Test connection**, um zu bestätigen, dass die Proxys mit Azure Files kommunizieren können:
 
    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-test-connection.png" alt-text="Testen der Verbindungsdetails":::

    Die Ergebnisse des Verbindungstests werden angezeigt:

    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-test-results.png" alt-text="Anzeige der Ergebnisse des Verbindungstests":::

12. Unter **SMB Migration Shares** werden alle Azure Files-Freigaben angezeigt, die unter diesem Speicherkonto bereitgestellt werden. Legen Sie **Mapping** für die Freigaben, die sich in Ihrem Migrationsbereich befinden, auf **Manual** fest, z. B.:
 
    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobiprotect-azure-files-shares.png" alt-text="Anzeige der verfügbaren Freigaben":::

13. Klicken Sie auf **Finish**, um die Azure Files-Konfiguration abzuschließen. Anschließend können Sie eine neue Migrationsaufgabe starten.

### <a name="start-a-new-migration"></a>Starten einer neuen Migration

DobiMigrate kann eine neue Migration einrichten, indem Migrationspfade manuell hinzugefügt oder massenimportiert werden. Beim Massenimport werden mehrere Migrationen mit allgemeinen Migrationsoptionen hinzugefügt.

So starten Sie eine neue Migration:

1. Klicken Sie auf dem Dashboard auf die Schaltfläche **Neue Migration**.
   
    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-new-migration.png" alt-text="Starten Sie einen neuen Migrationsauftrag.":::

1. Wählen Sie die Quelle und die zu migrierenden Pfade aus.

    :::image type="content" source="./media/dobimigrate-quick-start-guide/dobimigrate-select-source.png" alt-text="Wählen Sie die Quelle und die zu migrierenden Pfade aus.":::

1. Wählen Sie das **Ziel** aus.
2. Überprüfen Sie die Protokolle, und bestätigen Sie die Migrationsoptionen.
3. Klicken Sie auf **Fertigstellen**, um den Migrationsprozess abschließen.

## <a name="support"></a>Support 

Wenn Sie Hilfe bei der Migration zur Azure-Lösung benötigen, sollten Sie sowohl bei Datadobi als auch bei Azure einen Supportfall eröffnen.

### <a name="to-open-a-case-with-datadobi"></a>So eröffnen Sie einen Supportfall bei Datadobi

Navigieren Sie zur [Datadobi Support-Website](https://support.datadobi.com/s/), melden Sie sich an, und eröffnen Sie einen Fall.

### <a name="to-open-a-case-with-azure"></a>So eröffnen Sie einen Supportfall bei Azure

Suchen Sie im [Azure-Portal](https://portal.azure.com/) auf der Suchleiste oben nach **Support**. Wählen Sie **Hilfe und Support** -> **Neue Supportanfrage**.

## <a name="marketplace"></a>Marketplace

Datadobi hat die Bereitstellung der Datadobi-Lösung in Azure zum Schutz von Azure-VMs und vielen weiteren Azure-Diensten sehr einfach gestaltet. Weitere Informationen finden Sie in den folgenden Referenzen:

- [Migrieren von Dateidaten zu Azure mit DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=overview)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in unseren Leitfäden:

- [Übersicht über die Azure Storage-Migration](../../../common/storage-migration-overview.md)
- [DobiMigrate-Benutzerhandbuch](https://downloads.datadobi.com/NAS/olh/latest/dobimigrate.html)
- [Leitfaden zu den Voraussetzungen für DobiMigrate](https://downloads.datadobi.com/NAS/guides/latest/prerequisites.html)
- [Installationshandbuch für DobiMigrate](https://downloads.datadobi.com/NAS/guides/latest/installguide.html)