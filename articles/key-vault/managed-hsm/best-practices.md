---
title: Bewährte Methoden bei der Verwendung verwalteter HSMs von Azure Key Vault
description: In diesem Dokument werden einige der bewährten Methoden zum Verwenden von Key Vault erläutert.
services: key-vault
author: amitbapat
tags: azure-key-vault
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: conceptual
ms.date: 09/17/2020
ms.author: ambapat
ms.openlocfilehash: 7a30a7ab6689b602bc9ad4f696a6fe54c80f2151
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90992929"
---
# <a name="best-practices-when-using-managed-hsm"></a>Bewährte Methoden bei der Verwendung verwalteter HSMs

## <a name="control-access-to-your-managed-hsm"></a>Steuern des Zugriffs auf Ihr verwaltetes HSM

Verwaltetes HSM ist ein Clouddienst zum Schutz von Verschlüsselungsschlüsseln. Da diese Schlüssel vertraulich und unternehmenskritisch sind, stellen Sie sicher, dass der Zugriff auf Ihre verwalteten HSMs geschützt ist, indem Sie nur autorisierte Anwendungen und Benutzer zulassen. In diesem [Artikel](access-control.md) finden Sie eine Übersicht über das Zugriffsmodell. Es erläutert die Authentifizierung und Autorisierung sowie die rollenbasierte Zugriffssteuerung.
- Erstellen Sie eine [Azure Active Directory-Sicherheitsgruppe](../../active-directory/fundamentals/active-directory-manage-groups.md) für die HSM-Administratoren (anstatt einzelnen Personen die Administratorrolle zuzuweisen). Dadurch wird eine „Verwaltungssperre“ im Falle der Löschung einzelner Konten verhindert.
- Sperren Sie den Zugriff auf Ihre Verwaltungsgruppen, Abonnements, Ressourcengruppen und verwaltete HSMs – Verwenden Sie Azure RBAC, um den Zugriff auf Ihre Verwaltungsgruppen, Abonnements und Ressourcengruppen zu steuern.
- Erstellen von Rollenzuweisungen pro Schlüssel mit [lokaler RBAC für verwaltete HSMs](access-control.md#data-plane-and-managed-hsm-local-rbac)
- Verwenden des Prinzipals mit den geringsten Zugriffsrechten zum Zuweisen von Rollen

## <a name="choose-regions-that-support-availability-zones"></a>Auswählen von Regionen, die Verfügbarkeitszonen unterstützen

- Wählen Sie Azure-Regionen aus, in denen [Verfügbarkeitszonen](../../availability-zones/az-overview.md) unterstützt werden, um für die beste Hochverfügbarkeit und Zonenresilienz zu sorgen. Diese Regionen werden im Azure-Portal als „Empfohlene Regionen“ angezeigt.

## <a name="backup"></a>Backup

- Stellen Sie sicher, dass Sie regelmäßig Sicherungen Ihrer HSM durchführen. Sicherungen können auf der HSM-Ebene und für bestimmte Schlüssel durchgeführt werden. 

## <a name="turn-on-logging"></a>Aktivieren Sie die Protokollierung.

- [Aktivieren Sie die Protokollierung](logging.md) für Ihre HSM Richten sie auch Warnungen ein.

## <a name="turn-on-recovery-options"></a>Aktivieren von Wiederherstellungsoptionen

- Standardmäßig ist das [vorläufige Löschen](../general/soft-delete-overview.md) aktiviert.
- Aktivieren Sie den Löschschutz, wenn Sie sich auch nach dem Aktivieren des vorläufigen Löschens vor dem erzwungenen Löschen der HSM schützen möchten.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur vollständigen HSM-Sicherung/-Wiederherstellung finden Sie unter [Vollständige Sicherung/Wiederherstellung](backup-restore.md).
- Informationen zum Verwenden von Azure Monitor zum Konfigurieren der Protokollierung finden Sie unter [Protokollierung für verwaltete HSMs](logging.md).
- Informationen zur Schlüsselverwaltung finden Sie unter [Verwalten verwalteter HSM-Schlüssel](key-management.md).
- Informationen zum Verwalten von Rollenzuweisungen finden Sie unter [Rollenverwaltung für verwaltetes HSM](role-management.md).
