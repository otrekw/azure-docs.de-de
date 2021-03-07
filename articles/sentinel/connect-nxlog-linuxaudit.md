---
title: Verbinden von NXLog LinuxAudit-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem NXLog LinuxAudit-Datenconnector entsprechende LinuxAudit-Protokolle in Azure Sentinel pullen. Zeigen Sie LinuxAudit-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: 2010b21a3cdb81f2b2aa4180f87857862cd02bf5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743294"
---
# <a name="connect-your-nxlog-linuxaudit-to-azure-sentinel"></a>Verbinden von NXLog LinuxAudit mit Azure Sentinel

> [!IMPORTANT]
> Der NXLog LinuxAudit-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Mit dem [NXLog LinuxAudit](https://nxlog.co/documentation/nxlog-user-guide/im_linuxaudit.html)-Connector können Sie ganz einfach alle Linux-Sicherheitsereignisse in Echtzeit nach Azure Sentinel exportieren und erhalten so einen Einblick in die Aktivität der Domänennamenserver in Ihrem Unternehmen. Das NXLog LinuxAudit-Modul unterstützt benutzerdefinierte Überwachungsregeln und erfasst Protokolle ohne „auditd“ oder andere Softwarekomponenten im Benutzerbereich. IP-Adressen und Gruppen/Benutzer-IDs werden in die jeweiligen Namen aufgelöst, sodass die Protokolle der[Linux-Überwachung](https://nxlog.co/documentation/nxlog-user-guide/linux-audit.html) für Sicherheitsanalysten besser lesbar sind. Die Integration zwischen NXLog LinuxAudit und Azure Sentinel wird über die REST-API ermöglicht.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-nxlog-linuxaudit"></a>Konfigurieren und Verbinden von NXLog LinuxAudit

NXLog kann so konfiguriert werden, dass Ereignisse im JSON-Format direkt an Azure Sentinel gesendet werden.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **NXLog LinuxAudit** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Schrittanleitungen im *NXLog-Benutzerleitfaden* im Thema zur Integration von [Microsoft Azure Sentinel](https://nxlog.co/documentation/nxlog-user-guide/sentinel.html), um die Weiterleitung über die REST-API zu konfigurieren.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter dem Abschnitt **Benutzerdefinierte Protokolle** in der Tabelle *LinuxAudit_CL* angezeigt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie NXLog LinuxAudit zum Erfassen von Linux-Sicherheitsereignissen in Azure Sentinel verwenden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
