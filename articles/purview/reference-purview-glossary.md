---
title: Azure Purview-Produktglossar
description: Ein Glossar, in dem die in Azure Purview verwendete Terminologie definiert ist
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.topic: conceptual
ms.date: 06/08/2021
ms.openlocfilehash: ec011e4c9b0cab17bd9427020ba8842734e1f590
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111811910"
---
# <a name="azure-purview-product-glossary"></a>Azure Purview-Produktglossar

Unten finden Sie ein Glossar, in dem die in Azure Purview verwendete Terminologie definiert ist.

## <a name="annotation"></a>Anmerkung
Informationen, die Datenassets in Azure Purview zugeordnet sind, z. B. Glossarbegriffe und Klassifizierungen. Nachdem der Anwendung können Anmerkungen in der Suche verwendet werden, um die Ermittlung der Datenassets zu unterstützen. 
## <a name="approved"></a>Genehmigt
Der Status einer Anforderung, die von der angegebenen Person oder Gruppe, die zum Ändern des Status der Anforderung berechtigt ist, als zufriedenstellend akzeptiert wurde. 
## <a name="asset"></a>Asset
Beliebiges einzelnes Objekt, das in einem Azure Purview-Datenkatalog gespeichert ist.
> [!NOTE]
> Ein einzelnes Objekt im Katalog kann potenziell viele Objekte im Speicher darstellen, z. B. ist ein Ressourcensatz ein Asset, das aber aus vielen Partitionsdateien im Speicher besteht.
## <a name="azure-information-protection"></a>Azure Information Protection
Eine Cloudlösung, die die Bezeichnung von Dokumenten und E-Mails zum Klassifizieren und Schützen von Informationen unterstützt. Bezeichnete Elemente können durch Verschlüsselung geschützt, mit einem Wasserzeichen markiert oder auf bestimmte Aktionen oder Benutzer beschränkt werden und sind an das Element gebunden. Diese cloudbasierte Lösung basiert auf Azure Rights Management Service (RMS) zum Erzwingen von Einschränkungen. 
## <a name="business-glossary"></a>Unternehmensglossar
Eine durchsuchbare Liste spezieller Begriffe, die eine Organisation verwendet, um wichtige Unternehmensbegriffe und deren Definitionen zu beschreiben. Die Verwendung eines Unternehmensglossars ermöglicht eine konsistente Datennutzung in der gesamten Organisation. 
## <a name="classification-report"></a>Klassifizierungsbericht
Ein Bericht, der wichtige Klassifizierungsdetails zu den überprüften Daten anzeigt.  
## <a name="classification"></a>Klassifizierung
Ein Anmerkungstyp, der verwendet wird, um ein Attribut eines Assets oder einer Spalte zu identifizieren, z. B. „Alter“, „E-Mail-Adresse“ und „Anschrift“. Diese Attribute können bei Überprüfungen zugewiesen oder manuell hinzugefügt werden. 
## <a name="classification-rule"></a>Klassifizierungsregel
Eine Klassifizierungsregel ist ein Satz von Bedingungen, die bestimmen, wie überprüfte Daten klassifiziert werden sollen, wenn Inhalt dem angegebenen Muster entspricht.  
## <a name="contact"></a>Contact
Eine Person, die einer Entität im Datenkatalog zugeordnet ist 
## <a name="control-plane"></a>Steuerungsebene
Vorgänge, die Ressourcen in Ihrem Abonnement verwalten, z. B. rollenbasierte Zugriffssteuerung und Azure-Richtlinien, die an den Azure Resource Manager-Endpunkt gesendet werden. 
## <a name="credential"></a>Anmeldeinformationen
Eine Überprüfung der Identität oder des Tools, die bzw. das in einem Zugriffssteuerungssystem verwendet wird. Anmeldeinformationen können zum Authentifizieren einer Person oder Gruppe verwendet werden, um Zugriff auf ein Datenasset zu gewähren. 
## <a name="data-catalog"></a>Datenkatalog
Azure Purview-Features, mit denen Kunden die Metadaten für Assets in Ihrem Datenbestand anzeigen und verwalten können.
## <a name="data-map"></a>Datenzuordnung
Azure Purview-Features, mit denen Kunden ihren Datenbestand verwalten können, z. B. Überprüfung, Herkunft und Bewegung.
## <a name="expert"></a>Experte
Eine Person innerhalb einer Organisation, die den vollständigen Kontext eines Datenassets oder Glossarbegriffs versteht. 
## <a name="fully-qualified-name-fqn"></a>Vollqualifizierter Name (FQN)
Ein Pfad, der den Speicherort eines Assets innerhalb seiner Datenquelle definiert.  
## <a name="glossary-term"></a>Glossarbegriff
Ein Eintrag im Unternehmensglossar, der ein Konzept definiert, das speziell für die jeweilige Organisation gilt. Glossarbegriffe können Informationen zu Synonymen, Akronymen und verwandten Begriffen enthalten. 
## <a name="insights"></a>Einblicke
Ein Bereich in Azure Purview, in dem Sie Berichte anzeigen können, die Informationen zu Ihren Daten zusammenfassen.
## <a name="lineage"></a>Herkunft
Datentransformationen und Datenflüsse während ihrer Bewegung vom Ursprung zum Ziel. Das Verständnis dieser Datenflüsse innerhalb des Datenbestands hilft Organisationen bei der Darstellung des Datenverlaufs und bei der Problembehandlung oder Auswirkungsanalyse. 
## <a name="management-center"></a>Verwaltungscenter
Ein Bereich in Azure Purview, in dem Sie Verbindungen, Benutzer, Rollen und Anmeldeinformationen verwalten können.
## <a name="on-premises-data"></a>Lokale Daten
Daten in einem Rechenzentrum, die von einem Kunden gesteuert werden, d. h. nicht in der Cloud oder als Software-as-a-Service (SaaS). 
## <a name="owner"></a>Besitzer
Eine Person oder Gruppe, die für die Verwaltung eines Datenassets verantwortlich ist.  
## <a name="purview-instance"></a>Azure Purview-Instanz
Eine einzelne Azure Purview-Ressource. 
## <a name="registered-source"></a>Registrierte Quelle
Eine Quelle, die einer Azure Purview-Instanz hinzugefügt wurde und jetzt als Teil des Datenkatalogs verwaltet wird. 
## <a name="related-terms"></a>Verwandte Begriffe
Glossarbegriffe, die mit anderen Begriffen innerhalb der Organisation verknüpft sind.  
## <a name="resource-set"></a>Ressourcensatz
Ein einzelnes Asset, das viele partitionierte Dateien oder Objekte im Speicher darstellt. Azure Purview speichert z. B. eine partitionierte Apache Spark-Ausgabe als einen einzelnen Ressourcensatz und nicht als einzelne Assets für jede einzelne Datei. 
## <a name="role"></a>Role
Berechtigungen, die einem Benutzer innerhalb einer Azure Purview-Instanz zugewiesen sind. Rollen wie „Purview-Datenkurator“ oder „Purview-Datenleseberechtigter“ bestimmen, welche Aktionen innerhalb des Produkts ausgeführt werden können. 
## <a name="scan"></a>Überprüfen
Ein Azure Purview-Prozess, der eine Quelle oder einen Satz von Quellen untersucht und deren Metadaten im Datenkatalog erfasst. Überprüfungen können manuell oder nach einem Zeitplan mithilfe eines Überprüfungstriggers ausgeführt werden. 
## <a name="scan-ruleset"></a>Überprüfungsregelsatz
Eine Reihe von Regeln, die definieren, welche Datentypen und Klassifizierungen eine Überprüfung in einem Katalog erfasst. 
## <a name="scan-trigger"></a>Überprüfungstrigger
Ein Zeitplan für die Wiederholung einer Überprüfung. 
## <a name="sensitivity-label"></a>Vertraulichkeitsbezeichnung
Anmerkungen, die die Daten einer Organisation klassifizieren und schützen. Azure Purview lässt sich in Microsoft Information Protection integrieren, um Vertraulichkeitsbezeichnungen zu erstellen. 
## <a name="sensitivity-label-report"></a>Bericht der Vertraulichkeitsbezeichnungen
Eine Zusammenfassung, welche Vertraulichkeitsbezeichnungen auf den Datenbestand angewendet werden. 
## <a name="service"></a>Dienst
Ein Produkt, das eigenständige Funktionen bereitstellt und Kunden per Abonnement oder Lizenz zur Verfügung steht. 
## <a name="source"></a>`Source`
Ein System, in dem Daten gespeichert werden. Quellen können an verschiedenen Orten gehostet werden, z. B. in einer Cloud oder lokal. Sie registrieren und überprüfen Quellen, damit Sie sie in Azure Purview verwalten können. 
## <a name="source-type"></a>Quelltyp
Eine Kategorisierung der registrierten Quellen, die in einer Azure Purview-Instanz verwendet werden, z. B. Azure SQL-Datenbank, Azure Blob Storage, Amazon S3 oder SAP ECC. 
## <a name="steward"></a>Inhaltsverantwortlicher
Eine Person, die die Standards für einen Glossarbegriff definiert. Sie ist für die Aufrechterhaltung von Qualitätsstandards, Nomenklatur und Regeln für die zugewiesene Entität verantwortlich. 

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Azure Purview finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).