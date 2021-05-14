---
title: Was ist dediziertes HSM? Azure Dedicated HSM | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Azure Dedicated HSM, einen Azure-Dienst, der Speicherung von kryptografischen Schlüsseln in Azure bietet.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 03/25/2021
ms.author: keithp
ms.openlocfilehash: d4c21835cfb9d3e665f3617047c6934282282c1e
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108125947"
---
# <a name="what-is-azure-dedicated-hsm"></a>Was ist Azure Dedicated HSM?

Azure Dedicated HSM ist ein Azure-Dienst, der Speicherung von kryptografischen Schlüsseln in Azure bietet. Dedicated HSM erfüllt die strengsten Sicherheitsanforderungen. Es ist die optimale Lösung für Kunden, die Geräte mit der Zertifizierung „FIPS 140-2 Level 3“ sowie die vollständige und uneingeschränkte Kontrolle über die HSM-Appliance benötigen. 

 HSM-Geräte werden weltweit über mehrere Azure-Regionen hinweg bereitgestellt. Sie können einfach als ein Gerätepaar bereitgestellt und für Hochverfügbarkeit konfiguriert werden. HSM-Geräte können zum Schutz vor regionalen Ausfällen auch regionsübergreifend bereitgestellt werden. Microsoft stellt den Dedicated HSM-Dienst mithilfe von Geräten vom Typ [Thales Luna 7 HSM Modell A790](https://cpl.thalesgroup.com/encryption/hardware-security-modules/network-hsms) bereit. Dieses Gerät bietet ein Höchstmaß an Leistung und kryptografischen Integrationsoptionen. 

Nachdem sie bereitgestellt wurden, sind die HSM-Geräte direkt mit dem virtuellen Netzwerk eines Kunden verbunden. Auf sie können auch lokale Anwendungen und Verwaltungstools zugreifen, wenn Sie Point-to-Site- oder Site-to-Site-VPN-Konnektivität konfigurieren. Kunden erhalten die Software und die Dokumentation für die Konfiguration und Verwaltung von HSM-Geräten über das [Thales-Portal für den Kundensupport](https://supportportal.thalesgroup.com/csm).

## <a name="why-use-azure-dedicated-hsm"></a>Argumente für die Verwendung von Azure Dedicated HSM

### <a name="fips-140-2-level-3-compliance"></a>Konformität mit „FIPS 140-2 Level 3“

Aufgrund strenger branchenspezifischer Vorschriften müssen in vielen Organisationen die kryptografischen Schlüssel in HSMs gespeichert werden, die gemäß [FIPS 140-2 Level 3](https://csrc.nist.gov/publications/detail/fips/140/2/final) überprüft wurden. Azure Dedicated HSM und das neue Einzelmandantangebot [Verwaltetes Azure Key Vault-HSM](../key-vault/managed-hsm/index.yml) unterstützen Kunden aus verschiedenen Branchensegmenten (etwa Finanzdienstleister, Behörden usw.) dabei, die Anforderungen von „FIPS 140-2 Level-3“ zu erfüllen. Der mehrinstanzenfähige [Azure Key Vault](../key-vault/index.yml)-Dienst von Microsoft nutzt derzeit nur HSMs, die gemäß „FIPS 140-2 Level 2“ überprüft wurden. 

### <a name="single-tenant-devices"></a>Geräte mit nur einem Mandanten

Bei vielen unserer Kunden darf das kryptografische Speichergerät nur über einen einzelnen Mandanten verfügen. Der Dedicated HSM-Dienst ermöglicht die Bereitstellung eines physischen Geräts über eines der weltweit verteilten Datencenter von Microsoft. Nach der Bereitstellung für einen Kunden kann nur noch dieser Kunde auf das Gerät zugreifen.

### <a name="full-administrative-control"></a>Vollständige administrative Kontrolle

Viele Kunden benötigen die vollständige administrative Kontrolle und den alleinigen Zugriff auf ihr Gerät für administrative Zwecke. Nachdem ein Gerät bereitgestellt wurde, besitzt nur noch der jeweilige Kunde Zugriff auf Administrator- oder Anwendungsebene.

 Microsoft verfügt über keinerlei administrative Kontrolle, nachdem der Kunde zum ersten Mal auf das Gerät zugegriffen und das Kennwort geändert hat. Ab diesem Zeitpunkt ist der Kunde ein echter einzelner Mandant mit vollständiger administrativer Kontrolle und uneingeschränkter Anwendungsverwaltung. Microsoft behält den Zugriff auf die Telemetrie über die serielle Schnittstelle auf Überwachungsebene (keine Administratorrolle) bei. Dieser Zugriff umfasst Hardwareüberwachungen wie Temperatur, Integrität der Stromversorgung und Lüfterzustand. 
 
 Dem Kunden steht es frei, diese erforderliche Überwachung zu deaktivieren. Wenn er sie jedoch deaktiviert, erhält er keine proaktiven Integritätswarnungen von Microsoft.

### <a name="high-performance"></a>Hohe Leistung

Das Thales-Gerät wurde aus verschiedenen Gründen für diesen Dienst gewählt. Es bietet eine breite Palette an kryptographischer Algorithmusunterstützung, eine Vielzahl von unterstützten Betriebssystemen und eine breite API-Unterstützung. Das bereitgestellte Modell bietet eine ausgezeichnete Leistung von 10.000 Vorgängen pro Sekunde für RSA-2048. Es unterstützt zehn Partitionen, die jeweils für individuelle Anwendungsinstanzen verwendet werden können. Dieses Gerät zeichnet sich durch geringe Latenz, hohe Kapazität und hohen Durchsatz aus.

### <a name="unique-cloud-based-offering"></a>Einzigartiges cloudbasiertes Angebot

Microsoft hat bei einigen Kunden einen ganz spezifischen Bedarf ausgemacht. Microsoft ist der einzige Cloudanbieter, der neuen Kunden einen Dienst für dedizierte HSMs bietet, der die Anforderungen von „FIPS 140-2 Level 3“ erfüllt und über eine so umfangreiche Integration für cloudbasierte und lokale Anwendungen verfügt.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Ist der Azure-Dienst für dedizierte HSMs das Richtige für Sie?

Azure Dedicated HSM ist ein spezieller Dienst, der die individuellen Anforderungen eines ganz bestimmten Typs großer Organisationen erfüllt. Der Großteil der Azure-Kunden wird daher voraussichtlich keine Verwendung für diesen Dienst haben. In den meisten Fällen dürfte der Azure Key Vault-Dienst besser geeignet und kostengünstiger sein. Damit Sie leichter entscheiden können, ob der Dienst für Sie in Frage kommt, haben wir für Sie die folgenden Kriterien zusammengestellt.

### <a name="best-fit"></a>Optimal geeignet

Azure Dedicated HSM eignet sich am besten für Lift & Shift-Szenarien, die einen direkten und alleinigen Zugriff auf HSM-Geräte erfordern. Beispiele:

- Migrieren von Anwendungen aus der lokalen Umgebung zu Azure Virtual Machines
- Migrieren von Anwendungen von Amazon AWS EC2 zu Virtual Machines mit dem Dienst „AWS Cloud HSM Classic“ (Amazon bietet diesen Dienst nicht für neue Kunden an)
- Ausführen von Software aus dem Handel (etwa Apache/Ngnix SSL Offload, Oracle TDE und ADCS) in Azure Virtual Machines 

### <a name="not-a-fit"></a>Nicht geeignet

Azure Dedicated HSM ist für die folgende Art von Szenario nicht gut geeignet: Für Microsoft-Clouddienste, die die Verschlüsselung mit von Kunden verwalteten Schlüsseln unterstützen (etwa Azure Information Protection, Azure Disk Encryption, Azure Data Lake Store, Azure Storage, Azure SQL-Datenbank, Customer Key für Office 365), steht keine Integration für Azure Dedicated HSM zur Verfügung.

### <a name="it-depends"></a>Das ist unterschiedlich.

Ob Azure Dedicated HSM für Sie funktioniert, hängt von einer potenziell komplexen Mischung aus Anforderungen und Kompromissen ab, die Sie eingehen können oder auch nicht. Ein Beispiel ist die FIPS 140-2 Level 3-Anforderung. Diese Anforderung ist üblich, und derzeit wird sie nur von Azure Dedicated HSM und dem neuen Einzelmandantangebot [Verwaltetes HSM von Azure Key Vault](../key-vault/managed-hsm/index.yml) erfüllt. Wenn diese gesetzlichen Anforderungen nicht relevant sind, besteht häufig die Wahl zwischen Azure Key Vault und Azure Dedicated HSM. Werten Sie Ihre Anforderungen vor einer Entscheidung aus.

Zu den Situationen, in denen Sie Ihre Optionen abwägen müssen, gehören unter anderem: 

- Neuer Code, der auf einem virtuellen Azure-Computer eines Kunden ausgeführt wird
- TDE von SQL Server auf einem virtuellen Azure-Computer
- Clientseitige Verschlüsselung von Azure Storage
- Always Encrypted für SQL Server und Azure SQL-Datenbank

## <a name="next-steps"></a>Nächste Schritte

Dies ist ein hochgradig spezialisierter Dienst. Daher empfehlen wir Ihnen, die Schlüsselkonzepte in dieser Dokumentationssammlung (einschließlich Preisgestaltung, Support und SLAs) vollständig zu verstehen. 

Die [Thales-Integrationsleitfäden](https://cpl.thalesgroup.com/partners/overview) helfen dabei, die Bereitstellung von HSMs in einer vorhandenen virtuellen Netzwerkumgebung zu erleichtern. Es gibt auch Schrittanleitungen, die Sie beim Bestimmen der Einrichtung Ihrer Bereitstellungsarchitektur unterstützen.

* [Hohe Verfügbarkeit](high-availability.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)