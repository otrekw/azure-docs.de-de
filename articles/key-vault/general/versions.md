---
title: Azure Key Vault-Versionen
description: Die verschiedenen Versionen von Azure Key Vault
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: mbaldwin
ms.openlocfilehash: 4959d530e1f7a3c777aa3db21fe577c853fbca13
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85855994"
---
# <a name="key-vault-versions"></a>Key Vault-Versionen

Dies sind die Neuerungen bei Azure Key Vault. Neue Funktionen und Verbesserungen werden außerdem im [Key Vault-Kanal von Azure Updates](https://azure.microsoft.com/updates/?category=security&query=Key%20vault) angekündigt.

## <a name="june-2020"></a>Juni 2020

Azure Monitor für Key Vault ist jetzt als Vorschauversion erhältlich.  Azure Monitor ermöglicht eine umfassende Überwachung Ihrer Schlüsseltresore. Zu diesem Zweck wird eine einheitliche Darstellung der Anforderungen, Leistung, Fehler und Wartezeit im Zusammenhang mit Key Vault bereitgestellt. Weitere Informationen finden Sie unter [Azure Monitor für Key Vault (Vorschau).](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="may-2020"></a>Mai 2020

Key Vault-BYOK („Bring-Your-Own-Key“) ist jetzt allgemein verfügbar. Mehr dazu finden Sie in der [Azure Key Vault-BYOK-Spezifikation](../keys/byok-specification.md), und erfahren Sie, wie Sie [HSM-geschützte Schlüssel in Key Vault (BYOK) importieren](../keys/hsm-protected-keys-byok.md).

## <a name="march-2020"></a>März 2020

Private Endpunkte sind jetzt als Vorschauversion erhältlich. Mit dem Azure Private Link-Dienst können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure Key Vault sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen.  Weitere Informationen zum [Integrieren von Key Vault in Azure Private Link](private-link-service.md).

## <a name="2019"></a>2019

- Veröffentlichung der nächsten Generation von Azure Key Vault SDKs. Beispiele zur Verwendung finden Sie in den Schnellstartanleitungen zu Azure Key Vault-Geheimnissen für [Python](../secrets/quick-create-python.md), [.NET](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md) und [Node.js](../secrets/quick-create-node.md)
- Neue Azure-Richtlinien zum Verwalten von Key Vault-Zertifikaten. Mehr dazu erfahren Sie unter [Integrierte Azure Policy-Definitionen für Key Vault](../policy-samples.md).
- Die Azure Key Vault-Erweiterung für virtuelle Computer ist jetzt allgemein verfügbar.  Informationen finden Sie unter [Key Vault-VM-Erweiterung für Linux](../../virtual-machines/extensions/key-vault-linux.md) und [Key Vault-VM-Erweiterung für Windows](../../virtual-machines/extensions/key-vault-windows.md).
- Die ereignisgesteuerte Verwaltung von Geheimnissen für Azure Key Vault ist jetzt im Azure Event Grid verfügbar. Weitere Informationen finden Sie im [Event Grid-Schema für Ereignisse in Azure Key Vault](../../event-grid/event-schema-key-vault.md], und erfahren Sie mehr über das [Empfangen von und Antworten auf Schlüsseltresor-Benachrichtigungen mit Azure Event Grid](event-grid-tutorial.md).

## <a name="2018"></a>2018

In diesem Jahr veröffentlichte neue Features und Integrationen:

- Integration in Azure Functions. Ein Beispielszenario, in dem [Azure Functions](../../azure-functions/index.yml) für Schlüsseltresorvorgänge genutzt wird, finden Sie unter [Automate the rotation of a secret](../secrets/tutorial-rotation.md) (Automatisieren der Rotation von Geheimnissen). 
- [Integration in Azure Databricks](/azure/databricks/scenarios/store-secrets-azure-key-vault). Hiermit unterstützt Azure Databricks jetzt zwei Typen von Geheimnisbereichen: Von Azure Key Vault unterstützte und von Databricks unterstützte. Weitere Informationen finden Sie unter [Erstellen eines von Azure Key Vault unterstützten Geheimnisbereichs](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)
- [VNET-Dienstendpunkte für Azure Key Vault](overview-vnet-service-endpoints.md).

## <a name="2016"></a>2016

In diesem Jahr veröffentlichte neue Features:

- Schlüssel für verwaltete Speicherkonten. Das hinzugefügte Feature „Speicherkontoschlüssel“ erleichtert die Integration in Azure Storage. Weitere Informationen finden Sie im Übersichtsthema [Azure Key Vault-Speicherkontoschlüssel](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).
- Vorläufiges Löschen. Das Feature „Vorläufiges Löschen“ verbessert den Datenschutz Ihrer Schlüsseltresore und der Schlüsseltresorobjekte. Weitere Informationen finden Sie im Übersichtsthema [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015"></a>2015

In diesem Jahr veröffentlichte neue Features:
- Zertifikatverwaltung. Als Funktion der GA-Version 2015-06-01 am 26. September 2016 hinzugefügt.

Die allgemein verfügbare Version 2015-06-01 wurde am 24. Juni 2015 angekündigt. Die folgenden Änderungen wurden in dieser Version vorgenommen: 
- Löschen eines Schlüssels: Feld zur Verwendung entfernt.
- Abrufen von Informationen über einen Tresor: Feld zur Verwendung entfernt.
- Importieren eines Schlüssels in einen Tresor: Feld zur Verwendung entfernt.
- Wiederherstellen eines Schlüssels: Feld zur Verwendung entfernt.     
- Für RSA-Algorithmen „RSA_OAEP“ in „RSA-OAEP“ geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-certificates.md).    
 
Die zweite Vorschauversion (Version 2015-02-01-preview) wurde am 20. April 2015 angekündigt. Weitere Informationen finden Sie im Blogbeitrag zum [REST-API-Update](https://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx). Die folgenden Aufgaben wurden aktualisiert:
 
- Auflisten der Schlüssel in einem Tresor: Unterstützung für Paginierung wurde zum Vorgang hinzugefügt.
- Auflisten der Versionen eines Schlüssels: Vorgang zum Auflisten der Versionen eines Schlüssels wurde hinzugefügt.  
- Auflisten der Geheimnisse in einem Tresor: Unterstützung für Paginierung wurde hinzugefügt.
- Auflisten der Versionen eines Geheimnisses: Vorgang zum Auflisten der Versionen eines Geheimnisses wurde hinzugefügt.  
- Alle Vorgänge: Attributen wurden Zeitstempel der Erstellung/Aktualisierung hinzugefügt.  
- Erstellen eines Geheimnisses:Geheimnissen wurde ein Inhaltstyp hinzugefügt.
- Erstellen eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Erstellen eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Maximale Größe für Geheimnisse wurde von 10.000 in 25.000 Bytes geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-certificates.md).    
 
## <a name="2014"></a>2014
 
Die erste Vorschauversion (Version 2014-12-08-preview) wurde am 8. Januar 2015 angekündigt.  
 
## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-certificates.md)
- [Schlüssel](../keys/index.yml)
- [Geheimnisse](../secrets/index.yml)
- [Zertifikate](../certificates/index.yml)
