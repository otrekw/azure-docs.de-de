---
title: Herunterladen einer Liste von Gruppen im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Laden Sie Gruppeneigenschaften in einem Massenvorgang im Azure Admin Center in Azure Active Directory herunter.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 11/15/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5bf7d0a650d73127a87475d4fcda9cff266040eb
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94650866"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Herunterladen einer Liste von Gruppen in einem Massenvorgang in Azure Active Directory

Im Azure Active Directory-Portal (Azure AD) können Sie die Liste aller Gruppen in Ihrer Organisation per Massenvorgang in eine CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) herunterladen.

## <a name="to-download-a-list-of-groups"></a>So laden Sie eine Liste von Gruppen herunter

1. Melden Sie sich mit dem Konto eines Administrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie in Azure AD **Gruppen** > **Gruppen herunterladen** aus.
1. Wählen Sie auf der Seite **Gruppen herunterladen** die Option **Start** aus, um eine CSV-Datei mit der Auflistung Ihrer Gruppen herunterzuladen.

   ![Der Befehl „Gruppen herunterladen“ befindet sich auf der Seite „Alle Gruppen“.](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Überprüfen des Downloadstatus

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen** anzeigen.

[![Überprüfen des Status auf der Seite „Ergebnisse von Massenvorgängen“](./media/groups-bulk-download/bulk-center.png)](./media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Grenzwerte für den Massendownloaddienst

Jede Massenaktivität zum Herunterladen einer Gruppenliste kann bis zu einer Stunde dauern. Auf diese Weise können Sie eine Liste von mindestens 300.000 Gruppen herunterladen.

## <a name="next-steps"></a>Nächste Schritte

- [Massenentfernung von Gruppenmitgliedern](groups-bulk-remove-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
