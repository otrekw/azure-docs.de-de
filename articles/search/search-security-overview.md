---
title: Sicherheitsübersicht
titleSuffix: Azure Cognitive Search
description: Die kognitive Azure-Suche ist kompatibel mit SOC 2, HIPAA und anderen Zertifizierungen. Verbindungs- und Datenverschlüsselung, Authentifizierung und Identitätszugriff durch Benutzer- und Gruppensicherheits-IDs in Filterausdrücken.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/03/2020
ms.openlocfilehash: cc02890cb5293e48a8065b63f4f9c799c5dda7f7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85081042"
---
# <a name="security-in-azure-cognitive-search---overview"></a>Sicherheit in Azure Cognitive Search: Übersicht

In diesem Artikel werden die wichtigsten Sicherheitsfeatures in Azure Cognitive Search beschrieben, mit denen Inhalte und Vorgänge geschützt werden können. 

+ In der Speicherschicht ist die Verschlüsselung ruhender Daten auf Plattformebene gegeben, aber für Kunden, die doppelten Schutz sowohl mit benutzereigenen als auch mit von Microsoft verwalteten Schlüsseln wünschen, bietet Cognitive Search außerdem eine Option zur „doppelten Verschlüsselung“.

+ Die Eingangssicherheit schützt den Endpunkt des Suchdiensts mit steigendem Sicherheitsniveau: von API-Schlüsseln in der Anforderung über Eingangsregeln in der Firewall bis zu privaten Endpunkten, die Ihren Dienst vollständig vom öffentlichen Internet abschirmen.

+ Die Ausgangssicherheit betrifft Indexer, die Inhalte aus externen Quellen abrufen. Richten Sie für ausgehende Anforderungen eine verwaltete Identität ein, um die Suche zu einem vertrauenswürdigen Dienst zu machen, wenn auf Daten aus Azure Storage, Azure SQL, Cosmos DB oder anderen Azure-Datenquellen zugegriffen wird. Eine verwaltete Identität stellt einen Ersatz für Anmeldeinformationen oder Zugriffsschlüssel für die Verbindung dar. Ausgehende Sicherheit wird in diesem Artikel nicht behandelt. Weitere Informationen zu dieser Funktion finden Sie unter [Herstellen einer Verbindung mit einer Datenquelle mithilfe einer verwalteten Identität](search-howto-managed-identities-data-sources.md).

Eine Übersicht der Sicherheitsarchitektur und der einzelnen Featurekategorien finden Sie in diesem temporeichen Video.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Azure-Cognitive-Search-Whats-new-in-security/player]

## <a name="encrypted-transmissions-and-storage"></a>Verschlüsselte Übertragung und Speicherung

Die Verschlüsselung in Azure Cognitive Search ist durchgehend, beginnend bei Verbindungen und Übertragungen und bis zu den auf Datenträgern gespeicherten Inhalten reichend. Für Suchdienste im öffentlichen Internet lauscht Azure Cognitive Search an HTTPS-Port 443. Alle Client-zu-Dienst-Verbindungen verwenden TLS 1.2-Verschlüsselung. Frühere Versionen (1.0 oder 1.1) werden nicht unterstützt.

### <a name="data-encryption-at-rest"></a>Verschlüsselung ruhender Daten

Azure Cognitive Search speichert Indexdefinitionen und -inhalte, Datenquellendefinitionen, Indexerdefinitionen, Skillsetdefinitionien und Synonymzuordnungen.

In der gesamten Speicherschicht werden Daten auf Datenträgern mithilfe von Schlüsseln verschlüsselt, die von Microsoft verwaltet werden. Sie können die Verschlüsselung nicht aktivieren bzw. deaktivieren oder Verschlüsselungseinstellungen im Portal oder programmgesteuert anzeigen. Die Verschlüsselung ist vollständig integriert, ohne messbare Auswirkungen auf die Durchführungsdauer der Indizierung oder die Indexgröße. Sie wird automatisch auf die gesamte Indizierung angewendet, einschließlich auf inkrementelle Updates für einen nicht vollständig verschlüsselten Index (vor Januar 2018 erstellt).

Intern basiert die Verschlüsselung auf der [256-Bit-AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) von [Microsoft Azure Storage](../storage/common/storage-service-encryption.md).

> [!NOTE]
> Die Verschlüsselung ruhender Daten wurde am 24. Januar 2018 bekannt gegeben und gilt für alle Dienstebenen, einschließlich der Dienstebene „Free“, in sämtlichen Regionen. Damit die Verschlüsselung vollständig angewendet werden kann, müssen vor diesem Datum erstellte Indizes gelöscht und neu erstellt werden. Anderenfalls werden nur neue Daten, die nach dem 24. Januar hinzugefügt wurden, verschlüsselt.

### <a name="customer-managed-key-cmk-encryption"></a>Verschlüsselung mit kundenseitig verwalteten Schlüsseln (Customer-Managed Key, CMK)

Kunden, die zusätzlichen Speicherschutz wünschen, können Daten und Objekte verschlüsseln, bevor sie auf Datenträgern gespeichert und verschlüsselt werden. Dieser Ansatz basiert auf einem benutzereigenen Schlüssel, der unabhängig von Microsoft mithilfe von Azure Key Vault verwaltet und gespeichert wird. Das Verschlüsseln von Inhalten vor dem Verschlüsseln auf dem Datenträger wird als „doppelte Verschlüsselung“ bezeichnet. Aktuell können Sie Indizes und Synonymzuordnungen selektiv doppelt verschlüsseln. Weitere Informationen finden Sie unter [Von Kunden verwaltete Verschlüsselungsschlüssel in Azure Cognitive Search](search-security-manage-encryption-keys.md).

> [!NOTE]
> Die CMK-Verschlüsselung ist allgemein für Suchdienste verfügbar, die nach Januar 2019 erstellt wurden. Für kostenlose (freigegebene) Dienste wird sie nicht unterstützt. 
>
>Durch Aktivieren dieser Funktion wird die Indexgröße erhöht und die Abfrageleistung beeinträchtigt. Basierend auf den bisherigen Beobachtungen können Sie mit einem Anstieg der Abfragezeiten um 30 %–60 % rechnen, wobei die tatsächliche Leistung je nach Indexdefinition und Art der Abfragen variiert. Aufgrund dieser Auswirkungen auf die Leistung wird empfohlen, diese Funktion nur für Indizes zu aktivieren, für die sie wirklich erforderlich ist.

<a name="service-access-and-authentication"></a>

## <a name="inbound-security-and-endpoint-protection"></a>Eingangssicherheit und Endpunktschutz

Features der Eingangssicherheit schützen den Suchdienst-Endpunkt mithilfe ansteigender Sicherheits- und Komplexitätsstufen. Zunächst ist für alle Anforderungen ein API-Schlüssel für den authentifizierten Zugriff erforderlich. Zweitens können Sie optional Firewallregeln festlegen, die den Zugriff auf bestimmte IP-Adressen beschränken. Für erweiterten Schutz kann als dritte Option Azure Private Link aktiviert werden, um Ihren Dienstendpunkt von jeglichem Internetdatenverkehr abzuschirmen.

### <a name="public-access-using-api-keys"></a>Öffentlicher Zugriff mithilfe von API-Schlüsseln

Standardmäßig erfolgt der Zugriff auf einen Suchdienst über die Public Cloud, wobei für den Administrator- oder Abfragezugriff auf den Suchdienst-Endpunkt schlüsselbasierte Authentifizierung verwendet wird. Ein API-Schlüssel ist eine Zeichenfolge, die aus zufällig generierten Zahlen und Buchstaben besteht. Der Typ des Schlüssels (Administrator oder Abfrage) bestimmt die Zugriffsebene. Die Übermittlung eines gültigen Schlüssels gilt als Beleg dafür, dass die Anforderung von einer vertrauenswürdigen Entität stammt. 

Es gibt zwei Ebenen des Zugriffs auf Ihren Suchdienst, die durch die folgenden API-Schlüssel aktiviert werden:

+ Administratorschlüssel (erlaubt Schreib-/Lesezugriff für [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-Vorgänge (Create-Read-Update-Delete, Erstellen-Lesen-Aktualisieren-Löschen) im Suchdienst)

+ Abfrageschlüssel (erlaubt den schreibgeschützten Zugriff auf die Dokumentsammlung eines Index)

*Administratorschlüssel* werden erstellt, wenn der Dienst bereitgestellt wird. Es gibt zwei Administratorschlüssel, die zur besseren Unterscheidung als *primärer* und *sekundärer* Schlüssel bezeichnet werden. In Wirklichkeit sind sie aber austauschbar. Jeder Dienst verfügt über zwei Admin-Schlüssel, sodass Sie für einen Schlüssel ein Rollover durchführen können, ohne den Zugriff auf den Dienst zu verlieren. Sie können gemäß den bewährten Methoden von Azure regelmäßig die [Administratorschlüssel neu generieren](search-security-api-keys.md#regenerate-admin-keys), aber Sie können der Gesamtanzahl der Administratorschlüssel keine weiteren hinzufügen. Pro Suchdienst können maximal zwei Administratorschlüssel vorhanden sein.

*Abfrageschlüssel* werden bei Bedarf erstellt und sind für Clientanwendungen bestimmt, die Abfragen ausgeben. Sie können bis zu 50 Abfrage-Schlüssel generieren. Im Anwendungscode geben Sie die Such-URL und einen Abfrage-API-Schlüssel ein, um den Lesezugriff auf die Dokumentsammlung eines bestimmten Index zuzulassen. Zusammen definieren der Endpunkt, ein API-Schlüssel für den Lesezugriff und ein Zielindex den Bereich und die Zugriffsebene der Verbindung von Ihrer Clientanwendung.

Für jede Anforderung – bestehend aus einem obligatorischen Schlüssel, einem Vorgang und einem Objekt – ist eine Authentifizierung erforderlich. In Kombination lässt sich durch die beiden Berechtigungsstufen (uneingeschränkte bzw. Lese-/Schreibberechtigung) und den Kontext (z.B. eine Indexabfrage) ein vollumfängliches Sicherheitsspektrum für Dienstvorgänge erreichen. Weitere Informationen finden Sie unter [Erstellen und Verwalten von API-Schlüsseln für einen Azure Search-Dienst](search-security-api-keys.md).

### <a name="ip-restricted-access"></a>IP-eingeschränkter Zugriff

Um den Zugriff auf Ihren Suchdienst weiter zu steuern, können Sie Firewall-Eingangsregeln erstellen, die nur den Zugriff auf eine bestimmte IP-Adresse oder einen Bereich von IP-Adressen zulassen. Alle Clientverbindungen müssen über eine zulässige IP-Adresse erfolgen, sonst wird die Verbindung verweigert.

Sie können das Portal zum [Konfigurieren des eingehenden Zugriffs](service-configure-firewall.md) verwenden. 

Alternativ können Sie die Verwaltungs-REST-APIs verwenden. Von API-Version 2020-03-13 an können Sie mit dem [IpRule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule)-Parameter den Zugriff auf Ihren Dienst einschränken, indem Sie die IP-Adressen, die Zugriff auf Ihren Dienst erhalten sollen, einzeln oder als Bereich angeben. 

### <a name="private-endpoint-no-internet-traffic"></a>Privater Endpunkt (kein Internetdatenverkehr)

Ein [privater Endpunkt](../private-link/private-endpoint-overview.md) für Azure Cognitive Search ermöglicht es, dass ein Client in einem [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) über eine [private Verbindung](../private-link/private-link-overview.md) sicher auf Daten in einem Suchindex zugreifen kann. 

Der private Endpunkt verwendet eine IP-Adresse aus dem Adressraum des virtuellen Netzwerks für Verbindungen mit Ihrem Suchdienst. Der Netzwerkdatenverkehr zwischen dem Client und dem Suchdienst wird über das virtuelle Netzwerk und eine private Verbindung im Microsoft-Backbonenetzwerk geleitet, sodass keine Offenlegung im öffentlichen Internet erfolgt. Ein VNET ermöglicht eine sichere Kommunikation zwischen Ressourcen in Ihrem lokalen Netzwerk und im Internet. 

Dies ist zwar die sicherste Lösung, die Verwendung zusätzlicher Dienste bedeutet aber auch zusätzliche Kosten. Sie sollten ein genaues Verständnis der Vorteile besitzen, bevor Sie sich dafür entscheiden. Weitere Informationen zu den Kosten finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/private-link/). Weitere Informationen über das Zusammenwirken dieser Komponenten sehen Sie im Video, das oben in diesem Artikel verlinkt ist. Die Option des privaten Endpunkts wird ab 5:48 m im Video behandelt. Anweisungen zum Einrichten des Endpunkts finden Sie unter [Erstellen eines privaten Endpunkts für Azure Cognitive Search](service-create-private-endpoint.md).

## <a name="index-access"></a>Indexzugriff

In der kognitiven Azure-Suche ist ein einzelner Index kein sicherungsfähiges Objekt. Stattdessen wird der Zugriff auf einen Index durch eine Kombination aus Dienstebene (Lese- oder Schreibzugriff auf den Dienst) und Vorgangskontext bestimmt.

Für den Endbenutzerzugriff können Sie Abfrageanforderungen so strukturieren, dass die Verbindungsherstellung unter Verwendung eines Abfrageschlüssels erfolgt – wodurch alle Abfragen nur schreibgeschützten Zugriff erhalten – und den spezifischen, von Ihrer App verwendeten Index einschließen. Da es in einer Abfrageanforderung kein Konzept für die Verknüpfung von Indizes oder den gleichzeitigen Zugriff auf mehrere Indizes gibt, sind alle Anforderungen definitionsgemäß auf einen einzelnen Index ausgerichtet. Folglich wird die Sicherheitsgrenze durch die Konstruktion der Abfrageanforderung selbst (Kombination aus Schlüssel und einzelnem Zielindex) definiert.

Der Indexzugriff von Administratoren und Entwicklern ist undifferenziert: Beide benötigen Schreibzugriff, um vom Dienst verwaltete Objekte erstellen, löschen und aktualisieren zu können. Ein Benutzer mit einem Administratorschlüssel für Ihren Dienst kann jeden beliebigen Index in diesem Dienst lesen, ändern und löschen. Zum Schutz vor der versehentlichen oder böswilligen Löschung von Indizes können Sie Ihre interne Quellcodeverwaltung für Coderessourcen verwenden und unerwünschte Lösch- oder Änderungsvorgänge für Indizes rückgängig machen. Zur Gewährleistung der Verfügbarkeit bietet die kognitive Azure-Suche ein clusterinternes Failover-Feature. Ihr proprietärer Code zum Erstellen oder Laden von Indizes wird jedoch nicht gespeichert oder ausgeführt.

Mehrinstanzenfähige Lösungen, die Sicherheitsgrenzen auf der Indexebene erfordern, enthalten in der Regel eine mittlere Ebene, die Kunden für die Indexisolierung verwenden. Weitere Informationen zum mehrinstanzenfähigen Anwendungsfall finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und kognitive Azure-Suche](search-modeling-multitenant-saas-applications.md).

## <a name="user-access"></a>Benutzerzugriff

Wie ein Benutzer auf einen Index und andere Objekte zugreift, hängt vom Typ des API-Schlüssels für die Anforderung ab. Die meisten Entwickler erstellen und weisen [*Abfrageschlüssel*](search-security-api-keys.md) für clientseitige Suchanforderungen zu. Ein Abfrageschlüssel gewährt schreibgeschützten Zugriff auf alle durchsuchbaren Inhalte innerhalb des Index.

Wenn Sie eine detaillierte Kontrolle über Suchergebnisse pro Benutzer benötigen, können Sie Sicherheitsfilter für Ihre Abfragen erstellen, sodass mit einer bestimmten Sicherheitsidentität verknüpfte Dokumente zurückgegeben werden. Anstelle vordefinierter Rollen und Rollenzuweisungen wird eine identitätsbasierte Zugriffssteuerung als *Filter* implementiert, die die Suchergebnisse von Dokumenten und Inhalten auf der Grundlage von Identitäten einschränken. In der folgenden Tabelle werden zwei Ansätze zur Einschränkung von Suchergebnissen für nicht autorisierte Inhalte beschrieben.

| Vorgehensweise | BESCHREIBUNG |
|----------|-------------|
|[Einschränkung aus Sicherheitsgründen auf der Grundlage von Identitätsfiltern](search-security-trimming-for-azure-search.md)  | Beschreibt den grundlegenden Workflow für die Implementierung der Zugriffssteuerung auf der Grundlage der Benutzeridentität. Der Artikel behandelt das Hinzufügen von Sicherheitsbezeichnern zu einem Index sowie das Filtern nach dem entsprechenden Feld, um unzulässige Inhalte aus den Ergebnissen zu entfernen. |
|[Einschränkung aus Sicherheitsgründen auf der Grundlage von Azure Active Directory-Identitäten](search-security-trimming-for-azure-search-with-aad.md)  | Dieser Artikel baut auf dem vorherigen Artikel auf und enthält Schritte zum Abrufen von Identitäten aus Azure Active Directory (AAD) – einem der [kostenlosen Dienste](https://azure.microsoft.com/free/) der Azure-Cloudplattform. |

## <a name="administrative-rights"></a>Administratorrechte

[Rollenbasierter Zugriff (RBAC)](../role-based-access-control/overview.md) ist ein Autorisierungssystem für die Bereitstellung von Azure-Ressourcen, das auf [Azure Resource Manager](../azure-resource-manager/management/overview.md) basiert. In Azure Cognitive Search wird der Resource Manager zum Erstellen oder Löschen des Diensts, zum Verwalten der API-Schlüssel und zum Skalieren des Diensts verwendet. Daher bestimmen die RBAC-Rollenzuweisungen, wer diese Aufgaben ausführen kann, unabhängig davon, ob dazu das [Portal](search-manage.md), [PowerShell](search-manage-powershell.md) oder die [Management REST-APIs](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api) verwendet werden.

Im Gegensatz dazu werden Administratorrechte für Inhalte, die vom Dienst gehostet werden, z. B. die Möglichkeit zum Erstellen oder Löschen eines Indexes, über API-Schlüssel übertragen, wie im [vorherigen Abschnitt](#index-access) beschrieben.

> [!TIP]
> Mithilfe von Azure-weit gültigen Mechanismen können Sie ein Abonnement oder eine Ressource sperren, um die versehentliche oder nicht autorisierte Löschung Ihres Suchdiensts durch Benutzer mit Administratorrechten zu verhindern. Weitere Informationen finden Sie unter [Sperren von Ressourcen zum Verhindern der unerwarteten Löschung](../azure-resource-manager/management/lock-resources.md).

## <a name="certifications-and-compliance"></a>Zertifizierungen und Compliance

Azure Cognitive Search wurde als konform mit mehreren globalen, regionalen und branchenspezifischen Standards sowohl für die öffentliche Cloud als auch für Azure Government zertifiziert. Um die vollständige Liste zu erhalten, laden Sie das Whitepaper [**Microsoft Azure-Complianceangebote**](https://azure.microsoft.com/resources/microsoft-azure-compliance-offerings/) von der offiziellen Seite mit Überwachungsberichten herunter.

## <a name="see-also"></a>Weitere Informationen

+ [Grundlegende Azure-Sicherheitsinformationen](../security/fundamentals/index.yml)
+ [Sicherheit von Azure](https://azure.microsoft.com/overview/security)
+ [Azure Security Center](../security-center/index.yml)