---
title: Vorläufiges Löschen in einem verwalteten Azure Key Vault-HSM | Microsoft-Dokumentation
description: Mit dem vorläufigen Löschen in einem verwalteten HSM können Sie gelöschte HSM-Instanzen und -Schlüssel wiederherstellen.
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: conceptual
author: amitbapat
ms.author: ambapat
ms.date: 06/01/2021
ms.openlocfilehash: 673260fb8b78cb08ccd8581862238dc2e0341362
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111415145"
---
# <a name="managed-hsm-soft-delete-overview"></a>Übersicht über vorläufiges Löschen von verwaltetem HSM

> [!IMPORTANT]
> Das vorläufige Löschen ist für verwaltete HSM-Ressourcen standardmäßig aktiviert. Es kann nicht deaktiviert werden.

> [!IMPORTANT]
> Vorläufig gelöschte verwaltete HSM-Ressourcen werden weiterhin zum vollen Stundensatz abgerechnet, bis sie endgültig gelöscht werden.

Das Feature für das vorläufige Löschen von verwaltetem HSM ermöglicht die Wiederherstellung der gelöschten HSMs und Schlüssel. Diese Schutzmaßnahme bietet insbesondere folgende Sicherheiten:

- Wurde ein HSM oder Schlüssel gelöscht, kann er während eines konfigurierbaren Zeitraums von 7 bis 90 Kalendertagen wiederhergestellt werden. Der Aufbewahrungszeitraum kann während der Erstellung des HSM festgelegt werden. Wird kein Wert angegeben wird, wird der Standardaufbewahrungszeitraum von 90 Tagen festgelegt. So haben Benutzer ausreichend Zeit, ein versehentliches Löschen eines Schlüssels oder HSM zu erkennen und darauf zu reagieren.
- Zwei Vorgänge müssen durchgeführt werden, um einen Schlüssel dauerhaft zu löschen. Zuerst muss ein Benutzer den Schlüssel löschen. Dadurch geht er in den Zustand der vorläufigen Löschung über. Anschließend muss ein Benutzer den Schlüssel im Zustand der vorläufigen Löschung endgültig löschen. Für den Vorgang zum endgültigen Löschen muss dem Benutzer die Rolle „Managed HSM Crypto Officer“ (Kryptoverantwortlicher für verwaltete HSMs) zugewiesen sein. Diese zusätzlichen Schutzmaßnahmen verringern das Risiko, dass ein Benutzer versehentlich oder böswillig einen Schlüssel oder ein HSM löscht.


## <a name="soft-delete-behavior"></a>Verhalten des vorläufigen Löschens

Für verwaltetes HSM ist vorläufiges Löschen standardmäßig aktiviert. Sie können keine verwaltete HSM-Ressource mit deaktivierter Funktion für vorläufiges Löschen erstellen.

Wenn vorläufiges Löschen aktiviert ist, werden als gelöscht gekennzeichnete Ressourcen für einen bestimmten Zeitraum (standardmäßig 90 Tage) aufbewahrt. Darüber hinaus bietet der Dienst einen Mechanismus zum Wiederherstellen der gelöschten HSMs oder Schlüssel, sodass der Löschvorgang rückgängig gemacht werden kann.

Der Standardaufbewahrungszeitraum beträgt 90 Tage. Das Intervall der Aufbewahrungsrichtlinie kann jedoch während der Erstellung der HSM-Ressource auf einen Wert zwischen 7 und 90 Tagen festgelegt werden. Die Aufbewahrungsrichtlinie des Bereinigungsschutzes verwendet das gleiche Intervall. Nach der Festlegung kann das Intervall für die Aufbewahrungsrichtlinie nicht mehr geändert werden.

Sie können den Namen einer HSM-Ressource, die vorläufig gelöscht wurde, erst wiederverwenden, wenn der Aufbewahrungszeitraum verstrichen ist und die HSM-Ressource endgültig gelöscht wurde.

## <a name="purge-protection"></a>Bereinigungsschutz

Der Löschschutz ist ein optionales Verhalten und ist **nicht standardmäßig aktiviert**. Es kann über die [Befehlszeilenschnittstelle](./recovery.md?tabs=azure-cli) oder [PowerShell](./recovery.md?tabs=azure-powershell) aktiviert werden.

Bei aktiviertem Löschschutz kann ein HSM oder ein Schlüssel im gelöschten Zustand erst nach Ablauf des Aufbewahrungszeitraums endgültig gelöscht werden. Vorläufig gelöschte HSMs und Schlüssel können unter Einhaltung der Aufbewahrungsrichtlinie weiterhin wiederhergestellt werden.

Der Standardaufbewahrungszeitraum beträgt 90 Tage. Bei der Erstellung eines HSM kann das Intervall der Aufbewahrungsrichtlinie jedoch auf einen Wert zwischen 7 und 90 Tagen festgelegt werden. Das Intervall der Aufbewahrungsrichtlinie kann nur zum Zeitpunkt der Erstellung eines HSM festgelegt werden. Es kann später nicht geändert werden.

Informationen finden Sie unter [Verwenden des vorläufigen Löschens von verwalteten HSMs mit der Befehlszeilenschnittstelle](./recovery.md?tabs=azure-cli#managed-hsm-cli) oder [Verwenden des vorläufigen Löschens von verwalteten HSMs mit PowerShell](./recovery.md?tabs=azure-powershell#managed-hsm-powershell).

## <a name="managed-hsm-recovery"></a>Wiederherstellung von verwalteten HSMs

Beim Löschen eines HSM erstellt der Dienst eine Proxyressource unter dem Abonnement, wobei genügend Metadaten für die Wiederherstellung hinzugefügt werden. Die Proxyressource ist ein gespeichertes Objekt, das am gleichen Speicherort wie das gelöschte HSM verfügbar ist. 

## <a name="key-recovery"></a>Schlüsselwiederherstellung

Beim Löschen eines Schlüssels versetzt der Dienst ihn in einen gelöschten Zustand, sodass keine Vorgänge mehr darauf zugreifen können. In diesem Zustand können die Schlüssel aufgelistet, wiederhergestellt oder bereinigt (endgültig gelöscht) werden. Verwenden Sie zum Anzeigen der Objekte den Azure CLI-Befehl `az keyvault key list-deleted` (wie unter [Vorläufiges Löschen und Löschschutz für verwaltete HSMs mit der CLI](./recovery.md?tabs=azure-cli#keys-cli) dokumentiert) oder den Azure PowerShell-Parameter `-InRemovedState` (wie unter [Vorläufiges Löschen und Löschschutz für verwaltete HSMs mit PowerShell](./recovery.md?tabs=azure-powershell#keys-powershell) beschrieben).  

Gleichzeitig plant das verwaltete HSM die Ausführung des Löschens der zugrunde liegenden Daten, die dem gelöschten HSM oder Schlüssel entsprechen, nach einem zuvor festgelegten Aufbewahrungsintervall. Der dem HSM entsprechende DNS-Eintrag wird ebenfalls während des Aufbewahrungsintervalls beibehalten.

## <a name="soft-delete-retention-period"></a>Aufbewahrungszeitraum für vorläufig gelöschte Ressourcen

Vorläufig gelöschte Ressourcen werden für einen festgelegten Zeitraum von 90 Tagen beibehalten. Während des Aufbewahrungsintervalls für vorläufig gelöschte Ressourcen gilt Folgendes:

- Sie können alle im vorläufig gelöschten Zustand befindlichen HSMs und Schlüssel für Ihr Abonnement auflisten sowie auf die entsprechenden Löschungs- und Wiederherstellungsinformationen zugreifen.
  - Nur Benutzer mit der Rolle „Mitwirkender für verwaltetes HSM“ können gelöschte HSMs auflisten. Wir empfehlen unseren Benutzern das Erstellen einer benutzerdefinierten Rolle mit diesen speziellen Berechtigungen für den Umgang mit gelöschten Tresoren.
- Es kann kein verwaltetes HSM mit demselben Namen am gleichen Speicherort erstellt werden. Dementsprechend kann auch kein Schlüssel in einem bestimmten HSM erstellt werden, wenn dieses HSM einen Schlüssel mit demselben Namen im gelöschten Zustand enthält.
- Nur Benutzer mit der Rolle „Mitwirkender für verwaltetes HSM“ können verwaltete HSMs auflisten, anzeigen, wiederherstellen und endgültig löschen.
- Nur Benutzer mit der Rolle „Managed HSM Crypto Officer“ (Kryptoverantwortlicher für verwaltete HSMs) können Schlüssel auflisten, anzeigen, wiederherstellen und endgültig löschen.
  
Wenn ein verwaltetes HSM oder ein Schlüssel nicht wiederhergestellt wird, wird das vorläufig gelöschte HSM bzw. der vorläufig gelöschte Schlüssel vom Dienst am Ende des Aufbewahrungsintervalls endgültig gelöscht. Das Löschen von Ressourcen kann nicht neu geplant werden.

### <a name="billing-implications"></a>Hinweise zur Gebührenberechnung

Verwaltetes HSM ist ein Einzelmandantendienst. Wenn Sie ein verwaltetes HSM erstellen, reserviert der Dienst zugrunde liegende Ressourcen, die Ihrem HSM zugeordnet sind. Diese Ressourcen bleiben auch dann zugeordnet, wenn sich das HSM im gelöschten Zustand befindet. Daher wird ihnen das HSM in Rechnung gestellt, während es sich im gelöschten Zustand befindet.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden zwei Handbücher stellen die primären Verwendungsszenarien für vorläufiges Löschen vor.

- [Verwenden des vorläufigen Löschens von verwalteten HSMs mit PowerShell](./recovery.md?tabs=azure-powershell) 
- [Verwenden des vorläufigen Löschens von verwalteten HSMs mit der Befehlszeilenschnittstelle](./recovery.md?tabs=azure-cli)
