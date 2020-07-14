---
title: Automatische Updates von Mobility Service in Azure Site Recovery
description: Überblick über automatische Updates von Mobility Service bei der Replikation virtueller Azure-Computer mit Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/02/2020
ms.author: rajanaki
ms.openlocfilehash: b57ce89979225015dc87bbfb17f9603897ef6d6b
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135841"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Automatische Updates von Mobility Service bei der Replikation zwischen Azure-Regionen

Jeden Monat werden Azure Site Recovery-Releases veröffentlicht, um Probleme zu beheben, Funktionen zu verbessern und neue Funktion hinzuzufügen. Wenn Sie immer die aktuellste Version des Dienstes verwenden möchten, sollten Sie jeden Monat Patchimplementierungen einplanen. Wenn Sie sich nicht um jedes Update kümmern möchten, können Sie Site Recovery die Komponentenupdates verwalten lassen.

Wie unter [Architektur der Notfallwiederherstellung von Azure zu Azure](azure-to-azure-architecture.md) beschrieben, wird auf allen Azure-VMs, auf denen die Replikation von einer Region in eine andere aktiviert ist, Mobility Service installiert. Wenn Sie automatische Updates verwenden, wird Mobility Service durch jedes neue Release aktualisiert.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-automatic-updates-work"></a>Funktionsweise automatischer Updates

Wenn Site Recovery Ihre Updates verwaltet, wird ein globales Runbook (das von Azure-Diensten verwendet wird) über ein Automatisierungskonto bereitgestellt, das im selben Abonnement wie der Tresor erstellt wird. Jeder Tresor verwendet ein Automatisierungskonto. Das Runbook überprüft jeden virtuellen Computer in einem Tresor auf aktive automatische Updates. Wenn eine neuere Version der Mobility Service-Erweiterung verfügbar ist, wird das Update installiert.

Standardmäßig führt das Runbook dies jeden Tag um 0:00 Uhr in der Zeitzone des geografischen Raums der replizierten VM durch. Sie können diese Zeit über das Automatisierungskonto anpassen.

> [!NOTE]
> Ab dem [Updaterollup 35](site-recovery-whats-new.md#updates-march-2019) können Sie ein vorhandenes Automatisierungskonto auswählen, um es für Updates zu verwenden. Vor dem Updaterollup 35 wurde das Automatisierungskonto standardmäßig von Site Recovery erstellt. Sie können diese Option nur auswählen, wenn Sie die Replikation für eine VM aktivieren. Sie ist nicht für VMs verfügbar, für die die Replikation bereits aktiviert ist. Die ausgewählte Einstellung gilt für alle Azure-VMs, die in demselben Tresor geschützt werden.

Das Aktivieren automatischer Updates erfordert keinen Neustart der Azure-VMs und hat keinen Einfluss auf eine laufende Replikation.

Die Abrechnung des Automatisierungskontos erfolgt auf Grundlage der Gesamtzahl an Minuten, die pro Monat zur Auftragsausführung verwendet wurden. Die Auftragsausführung kann jeden Tag zwischen wenigen Sekunden und einer Minute in Anspruch nehmen und ist in den kostenlosen Einheiten enthalten. Standardmäßig verfügt jedes Automatisierungskonto über 500 kostenlose Minuten, wie in der folgenden Tabelle gezeigt:

| Inbegriffenen kostenlosen Einheiten (pro Monat) | Preis |
|---|---|
| 500 Minuten Auftragsausführung | ₹0,14/Minute

## <a name="enable-automatic-updates"></a>Aktivieren automatischer Updates

Es gibt mehrere Möglichkeiten, wie die Erweiterungsupdates von Site Recovery verwaltet werden können:

- [Verwaltung im Rahmen der Aktivierung der Replikation](#manage-as-part-of-the-enable-replication-step)
- [Durch Ein-/Ausschalten der Erweiterungsupdateeinstellungen im Tresor](#toggle-the-extension-update-settings-inside-the-vault)
- [Manuelles Verwalten von Updates](#manage-updates-manually)

### <a name="manage-as-part-of-the-enable-replication-step"></a>Verwaltung im Rahmen der Aktivieren der Replikation

Wenn Sie die Replikation für einen virtuellen Computer aktivieren, indem Sie entweder [von der Ansicht der VM](azure-to-azure-quickstart.md) oder [vom Recovery Services-Tresor](azure-to-azure-how-to-enable-replication.md) aus starten, können Sie optional auswählen, entweder das Verwalten von Updates für die Site Recovery-Erweiterung durch Site Recovery zuzulassen oder diese manuell zu verwalten.

:::image type="content" source="./media/azure-to-azure-autoupdate/enable-rep.png" alt-text="Erweiterungseinstellungen":::

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Durch Ein-/Ausschalten der Erweiterungsupdateeinstellungen im Tresor

1. Wählen Sie im Recovery Services-Tresor **Verwalten** > **Site Recovery-Infrastruktur** aus.
1. Wählen Sie unter **For Azure Virtual Machines** (Für Azure-VMs) > **Erweiterungsupdateeinstellungen** > **Verwaltung durch Site Recovery zulassen** die Option **Ein** aus.

   Wählen Sie **Aus**, um die Erweiterung manuell zu verwalten.

1. Wählen Sie **Speichern** aus.

:::image type="content" source="./media/azure-to-azure-autoupdate/vault-toggle.png" alt-text="Erweiterungsupdateeinstellungen":::

> [!IMPORTANT]
> Wenn Sie **Verwaltung durch Site Recovery zulassen** auswählen, wird die Einstellung auf alle virtuellen Computer im Tresor angewendet.

> [!NOTE]
> Bei beiden Optionen werden Sie über das Automatisierungskonto informiert, das zum Verwalten der Updates verwendet wird. Wenn Sie dieses Feature zum ersten Mal in einem Tresor verwenden, wird standardmäßig ein neues Automatisierungskonto erstellt. Alternativ können Sie die Einstellung anpassen und ein vorhandenes Automatisierungskonto auswählen. Für alle nachfolgenden Aufgaben zum Aktivieren der Replikation in demselben Tresor wird das zuvor erstellte Automatisierungskonto verwendet. Im Dropdownmenü werden derzeit nur Automatisierungskonten aufgeführt, die sich in derselben Ressourcengruppe wie der Tresor befinden.

### <a name="manage-updates-manually"></a>Manuelles Verwalten von Updates

1. Wenn es neue Updates für die Mobility Service-Instanz auf Ihrer VM gibt, wird die folgende Benachrichtigung angezeigt: **Ein neues Site Recovery-Replikations-Agent-Update ist verfügbar. Klicken Sie, um es zu installieren.**

   :::image type="content" source="./media/vmware-azure-install-mobility-service/replicated-item-notif.png" alt-text="Fenster „Replizierte Elemente“":::

1. Klicken Sie auf die Benachrichtigung, um die Auswahlseite für VMs zu öffnen.
1. Wählen Sie die VM, die Sie aktualisieren möchten, und klicken Sie dann auf **OK**. Das Update für Mobility Service wird für jede ausgewählte VM durchgeführt.

   :::image type="content" source="./media/vmware-azure-install-mobility-service/update-okpng.png" alt-text="VM-Liste mit replizierten Elementen":::

## <a name="common-issues-and-troubleshooting"></a>Häufige Probleme und Problembehandlungen

Wenn ein Problem mit den automatischen Updates auftritt, werden Sie mit einer Fehlermeldung unter **Konfigurationsprobleme** im Tresordashboard benachrichtigt.

Wenn Sie keine automatischen Updates aktivieren konnten, finden Sie in den häufigen Problemen und empfohlenen Maßnahmen weitere Informationen:

- **Fehler:** Sie verfügen nicht über die Berechtigungen, um ein ausführendes Azure-Konto (Dienstprinzipal) zu erstellen und dem Dienstprinzipal die Rolle „Mitwirkender“ zuzuweisen.

  **Empfohlene Maßnahme:** Achten Sie darauf, dass das angemeldete Konto als Mitwirkender zugewiesen ist, und versuchen Sie es erneut. Weitere Informationen zum Zuweisen von Berechtigungen finden Sie im Abschnitt zu den erforderlichen Berechtigungen unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

  Die meisten Probleme nach dem Aktivieren von automatischen Updates können Sie mit **Reparieren** beheben. Für den Fall, dass die Schaltfläche „Reparieren“ nicht verfügbar ist, beachten Sie die Fehlermeldung, die im Bereich „Erweiterungsupdateeinstellungen“ angezeigt wird.

  :::image type="content" source="./media/azure-to-azure-autoupdate/repair.png" alt-text="Schaltfläche „Reparieren“ im Site Recovery-Dienst in den Erweiterungsupdateeinstellungen":::

- **Fehler:** Das ausführende Konto verfügt nicht über die Berechtigung zum Zugriff auf die Recovery Services-Ressource.

  **Empfohlene Maßnahme:** Löschen Sie das Konto, und [erstellen Sie das ausführende Konto neu](../automation/manage-runas-account.md). Stellen Sie alternativ sicher, dass das ausführende Automatisierungskonto der Azure Active Directory-Anwendung auf die Recovery Services-Ressource zugreifen kann.

- **Fehler:** Das ausführende Konto wurde nicht gefunden. Eine der folgenden Komponenten wurde gelöscht oder nicht erstellt – Azure Active Directory-Anwendung, Dienstprinzipal, Rolle, Automation-Zertifikatasset, Automation-Verbindungsasset – oder der Fingerabdruck im Zertifikat ist nicht identisch mit dem der Verbindung.

  **Empfohlene Maßnahme:** Löschen Sie das Konto, und [erstellen Sie das ausführende Konto neu](../automation/manage-runas-account.md).

- **Fehler:** Das vom Automatisierungskonto verwendete Zertifikat für das ausführende Azure-Konto wird bald ablaufen.

  Das für das ausführende Konto erstellte selbstsignierte Zertifikat läuft ein Jahr nach dem Datum seiner Erstellung ab. Sie können es vor dem Ablaufdatum jederzeit erneuern. Wenn Sie sich für E-Mail-Benachrichtigungen registriert haben, erhalten Sie außerdem E-Mails, wenn Ihrerseits eine Maßnahme erforderlich ist. Dieser Fehler wird zwei Monate vor dem Ablaufdatum angezeigt und in einen kritischen Fehler geändert, wenn das Zertifikat abgelaufen ist. Nach Ablauf des Zertifikats funktioniert die automatische Aktualisierung erst wieder, wenn es erneuert wurde.

  **Empfohlene Maßnahme:** Wählen Sie **Reparieren** und dann **Zertifikat erneuern** aus, um dieses Problem zu beheben.

  :::image type="content" source="./media/azure-to-azure-autoupdate/automation-account-renew-runas-certificate.PNG" alt-text="Zertifikat erneuern":::

  > [!NOTE]
  > Nachdem Sie das Zertifikat erneuert haben, aktualisieren Sie die Seite, um den aktuellen Status anzuzeigen.
