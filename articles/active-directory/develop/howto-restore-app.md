---
title: 'Gewusst wie: Wiederherstellen oder Entfernen einer kürzlich gelöschten Anwendung mit der Microsoft Identity Platform | Azure'
titleSuffix: Microsoft identity platform
description: In dieser Vorgehensweise erfahren Sie, wie Sie eine kürzlich gelöschte Anwendung, die bei Microsoft Identity Platform registriert ist, wiederherstellen oder endgültig löschen.
services: active-directory
author: arcrowe
manager: dastrock
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 3/22/2021
ms.author: arcrowe
ms.custom: aaddev
ms.openlocfilehash: dbe3e16bf56c5480fbe4aff8dad78bbbb5591f67
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079787"
---
# <a name="restore-or-remove-a-recently-deleted-application-with-the-microsoft-identity-platform"></a>Wiederherstellen oder Entfernen einer kürzlich gelöschten Anwendung mit der Microsoft Identity Platform
Nachdem Sie eine App-Registrierung gelöscht haben, verbleibt die App 30 Tage lang in einem gesperrten Zustand. In diesem Zeitfenster von 30 Tagen kann die App-Registrierung mit allen zugehörigen Eigenschaften wiederhergestellt werden. Nach Ablauf des Zeitraums von 30 Tagen können App-Registrierungen nicht wiederhergestellt werden. Die endgültige Löschung kann automatisch gestartet werden.  Diese Funktion gilt nur für Anwendungen, die einem Verzeichnis zugeordnet sind.  Sie ist nicht für Anwendungen eines persönlichen Microsoft-Kontos verfügbar. Dieses können nicht wieder hergestellt werden.

Mithilfe der App-Registrierungsfunktionalität unter Azure Active Directory (Azure AD) im Azure-Portal können Sie die gelöschten Anwendungen anzeigen, eine gelöschte Anwendung wiederherstellen oder eine Anwendung endgültig löschen.

Beachten Sie, dass weder Sie noch der Microsoft-Kundensupport eine endgültig gelöschte Anwendung oder eine Anwendung, die vor mehr als 30 Tagen gelöscht wurde, wiederherstellen können.

> [!IMPORTANT]
> Das Feature für gelöschte Anwendungen in der Benutzeroberfläche des Portals [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Sie müssen über eine der folgenden Rollen verfügen, um Anwendungen endgültig löschen zu können.

- Globaler Administrator
- Anwendungsadministrator
- Cloudanwendungsadministrator
- Hybrididentitätsadministrator
- Anwendungsbesitzer

Sie müssen über eine der folgenden Rollen verfügen, um Anwendungen wiederherstellen zu können.

- Globaler Administrator
- Anwendungsbesitzer

### <a name="view-your-deleted-applications"></a>Anzeigen gelöschter Anwendungen
Sie können alle Anwendungen in einem vorläufig gelöschten Zustand anzeigen.  Nur Anwendungen, die vor weniger als 30 Tagen gelöscht wurden, können wiederhergestellt werden.

#### <a name="to-view-your-restorable-applications"></a>So zeigen Sie wiederherstellbaren Anwendungen an
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Navigieren Sie zu **Azure Active Directory**, wählen Sie **App-Registrierungen** und wählen Sie dann die Registerkarte **Gelöschte Anwendungen (Vorschau)** aus.

Schauen Sie sich die Liste der Anwendungen an. Nur Anwendungen, die in den letzten 30 Tagen gelöscht wurden, können wiederhergestellt werden. Wenn Sie die Vorschau der App-Registrierungssuche verwenden, können Sie nach der Spalte mit dem Löschdatum filtern, um lediglich diese Anwendungen anzuzeigen.

## <a name="restore-a-recently-deleted-application"></a>Wiederherstellen einer kürzlich gelöschten Anwendung

Wenn eine App-Registrierung in der Organisation gelöscht wird, befindet sich die App im gesperrten Zustand. Deren Konfigurationen werden aber beibehalten. Wenn Sie eine App-Registrierung wiederherstellen, werden die zugehörigen Konfigurationen ebenfalls wiederhergestellt.  Wenn jedoch in **Unternehmensanwendungen** für den Basismandanten der Anwendung organisationsspezifische Einstellungen vorhanden waren, werden diese nicht wieder hergestellt.  

Dies liegt daran, dass organisationsspezifische Einstellungen in einem separaten Objekt gespeichert werden, dem sogenannten Dienstprinzipal.  Zu den Einstellungen des Dienstprinzipals gehören bewilligte Berechtigung sowie Benutzer- und Gruppenzuweisungen für eine bestimmte Organisation. Diese Konfigurationen werden nicht wieder hergestellt, wenn die App wieder hergestellt wird. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md). 


### <a name="to-restore-an-application"></a>So stellen Sie eine Anwendung wieder her
1. Suchen Sie auf der Registerkarte **Gelöschte Anwendungen (Vorschau)** nach einer Anwendung, die vor weniger als 30 Tagen gelöscht wurde, und wählen Sie diese aus.
2. Wählen Sie **App-Registrierung wiederherstellen** aus.

## <a name="permanently-delete-an-application"></a>Endgültiges Löschen einer Anwendung
Sie können eine Anwendung in Ihrer Organisation manuell endgültig löschen. Eine endgültig gelöschte Anwendung kann weder von Ihnen, noch von einem anderen Administrator oder vom Microsoft-Kundendienst wiederhergestellt werden.

### <a name="to-permanently-delete-an-application"></a>So löschen Sie eine Anwendung endgültig

1. Suchen Sie auf der Registerkarte **Gelöschte Anwendungen (Vorschau)** nach einer verfügbaren Anwendung, und wählen Sie diese aus.
2. Wählen Sie **Endgültig löschen** aus.
3. Lesen Sie den Text der Warnung, und wählen Sie **Ja** aus.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die App wiederhergestellt oder endgültig gelöscht haben, haben Sie folgende Möglichkeiten:

- [Hinzufügen einer Anwendung](quickstart-register-app.md).
- Weitere Informationen über [Anwendungs- und Dienstprinzipalobjekte](app-objects-and-service-principals.md) in der Microsoft Identity Platform
