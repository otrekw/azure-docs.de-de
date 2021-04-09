---
title: Verknüpfen von Windows-Sicherheitsereignisdaten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Connector für sicherheitsrelevante Ereignisse alle sicherheitsrelevanten Ereignisse von Ihren Windows-Systemen an Ihren Azure Sentinel-Arbeitsbereich streamen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: bc75ee64174957ad6486146b4da6f8a66a2120e5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100570557"
---
# <a name="connect-windows-security-events"></a>Herstellen einer Verbindung mit Windows-Sicherheitsereignissen 

Mit dem Connector für sicherheitsrelevante Ereignisse können Sie alle sicherheitsrelevanten Ereignisse von Ihren Windows-Systemen (Server und Arbeitsstationen, physisch und virtuell) an Ihren Azure Sentinel-Arbeitsbereich streamen. Dadurch können Sie sicherheitsrelevante Windows-Ereignisse in Ihren Dashboards anzeigen, um damit benutzerdefinierte Benachrichtigungen zu erstellen und basierend darauf Ihre Untersuchungen zu verbessern. Hierdurch können Sie zusätzliche Erkenntnisse über das Netzwerk Ihrer Organisation erlangen und Ihre Möglichkeiten für Sicherheitsvorgänge ausbauen. Sie können aus den folgenden Optionen auswählen, welche Ereignisse gestreamt werden sollen: <a name="event-sets"></a>

- **Alle Ereignisse:** alle Windows-Sicherheits- und AppLocker-Ereignisse.
- **Allgemein:** ein Standardsatz von Ereignissen zu Überwachungszwecken. In diesem Satz ist ein vollständiger Benutzerüberwachungspfad enthalten. Diese Gruppe enthält beispielsweise sowohl Ereignisse für Benutzeranmeldungen als auch Ereignisse für Benutzerabmeldungen (Ereignis-IDs 4624 und 4634). Außerdem enthält sie Überwachungsaktionen wie Sicherheitsgruppenänderungen, wichtige Domänencontroller-Kerberos-Vorgänge und andere Ereignisse, die Best Practices entsprechen.

    Die Ereignisgruppe **Common** (Häufige) enthält möglicherweise einige Arten von Ereignissen, die nicht so häufig auftreten.  Der Grund hierfür ist, dass der Hauptzweck der Gruppe **Common** (Häufige) darin besteht, die Anzahl von Ereignissen auf eine besser verwaltbare Anzahl zu senken und dennoch die Funktion eines vollständigen Überwachungspfads beizubehalten.

- **Minimal:** ein kleiner Satz von Ereignissen, die auf potenzielle Bedrohungen hinweisen können. Diese Gruppe enthält keinen vollständigen Überwachungspfad. Sie deckt nur Ereignisse ab, die möglicherweise auf eine erfolgreiche Sicherheitsverletzung hindeuten, sowie andere wichtige Ereignisse, die nur sehr selten auftreten. So enthält diese Gruppe beispielsweise erfolgreiche und nicht erfolgreiche Benutzeranmeldungen (Ereignis-IDs 4624 und 4625), aber keine Informationen zu Abmeldungen (4634). Letztere sind zwar wichtig für die Überwachung, aber nicht für das Erkennen von Sicherheitsverletzungen, und sie treten zudem relativ häufig auf. Der Großteil des Datenvolumens dieser Gruppe ist auf Anmeldeereignisse und auf Prozesserstellungsereignisse (Ereignis-ID 4688) zurückzuführen.

- **Keine:** keine Sicherheits- oder AppLocker-Ereignisse. (Mit dieser Einstellung wird der Connector deaktiviert.)

    Die folgende Liste bietet eine vollständige Aufschlüsselung der Sicherheits- und AppLocker-Ereignis-IDs für jeden Satz:

    | Gruppe von Ereignissen | Erfasste Ereignis-IDs |
    | --- | --- |
    | **Minimal** | 1102, 4624, 4625, 4657, 4663, 4688, 4700, 4702, 4719, 4720, 4722, 4723, 4724, 4727, 4728, 4732, 4735, 4737, 4739, 4740, 4754, 4755, 4756, 4767, 4799, 4825, 4946, 4948, 4956, 5024, 5033, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222 |
    | **Common** | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, 4624, 4625, 4634, 4647, 4648, 4649, 4657, 4661, 4662, 4663, 4665, 4666, 4667, 4688, 4670, 4672, 4673, 4674, 4675, 4689, 4697, 4700, 4702, 4704, 4705, 4716, 4717, 4718, 4719, 4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4733, 4732, 4735, 4737, 4738, 4739, 4740, 4742, 4744, 4745, 4746, 4750, 4751, 4752, 4754, 4755, 4756, 4757, 4760, 4761, 4762, 4764, 4767, 4768, 4771, 4774, 4778, 4779, 4781, 4793, 4797, 4798, 4799, 4800, 4801, 4802, 4803, 4825, 4826, 4870, 4886, 4887, 4888, 4893, 4898, 4902, 4904, 4905, 4907, 4931, 4932, 4933, 4946, 4948, 4956, 4985, 5024, 5033, 5059, 5136, 5137, 5140, 5145, 5632, 6144, 6145, 6272, 6273, 6278, 6416, 6423, 6424, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222, 26401, 30004 |

> [!NOTE]
> Das Erfassen von sicherheitsrelevanten Ereignissen für einen einzelnen Arbeitsbereich kann entweder im Azure Security Center oder in Azure Sentinel konfiguriert werden, aber nicht in beiden Diensten. Wenn Sie ein Onboarding für Azure Sentinel in einem Arbeitsbereich durchführen möchten, der bereits Azure Defender-Benachrichtigungen aus Azure Security Center empfängt und für das Erfassen sicherheitsrelevanter Ereignisse konfiguriert ist, haben Sie zwei Möglichkeiten:
> - Behalten Sie das Erfassen von sicherheitsrelevanten Ereignissen im Azure Security Center bei. Sie können diese Ereignisse sowohl in Azure Sentinel als auch in Azure Defender abfragen und analysieren. Allerdings können Sie in Azure Sentinel weder den Verbindungsstatus des Connectors überwachen noch dessen Konfiguration ändern. Wenn dies wichtig für Sie ist, ziehen Sie die zweite Möglichkeit in Betracht.
>
> - [Deaktivieren Sie das Erfassen von sicherheitsrelevanten Ereignissen](../security-center/security-center-enable-data-collection.md) im Azure Security Center, und fügen Sie dann erst den Connector für sicherheitsrelevante Ereignisse in Azure Sentinel hinzu. Wie bei der ersten Möglichkeit können Sie Ereignisse sowohl in Azure Sentinel als auch in Azure Defender/ASC abfragen und analysieren. Nun können Sie allerdings in Azure Sentinel – und nur dort – den Verbindungsstatus des Connectors überwachen und dessen Konfiguration ändern.

## <a name="set-up-the-windows-security-events-connector"></a>Einrichten des Connectors für sicherheitsrelevante Windows-Ereignisse

Gehen Sie wie folgt vor, um sicherheitsrelevante Windows-Ereignisse in Azure Sentinel zu erfassen:

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors). Klicken Sie in der Liste mit Connectors auf **Sicherheitsereignisse** und dann auf die Schaltfläche **Open connector page** (Connectorseite öffnen) unten rechts. Folgen Sie anschließend wie im Rest dieses Abschnitts beschrieben den Anweisungen auf dem Bildschirm auf der Registerkarte **Anweisungen**.

1. Vergewissern Sie sich, dass Sie über die entsprechenden Berechtigungen verfügen, wie im Abschnitt **Voraussetzungen** auf der Seite „Connector“ beschrieben.

1. Laden Sie auf den Computern, für die Sie sicherheitsrelevante Ereignisse an Azure Sentinel streamen wollen, den [Log Analytics-Agent](../azure-monitor/agents/log-analytics-agent.md) (auch als Microsoft Monitoring Agent oder MMA bezeichnet) herunter, und installieren Sie ihn dort.

    Gehen Sie bei virtuellen Azure-Computern wie folgt vor:
    
    1. Klicken Sie auf **Install agent on Azure Windows Virtual Machine**(Agent auf virtuellem Windows-Computer (Azure) installieren) und anschließend auf den Link, der unten angezeigt wird.
    1. Klicken Sie für jeden virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, auf dessen Namen in der rechts angezeigten Liste, und klicken Sie dann auf **Verbinden**.

    Gehen Sie bei Windows-Computern (physische, virtuelle lokale oder virtuelle in einer anderen Cloud), die keine Azure-Computer sind, wie folgt vor:

    1. Klicken Sie auf **Install agent on non-Azure Windows Machine** (Agent auf einem Windows-Computer (kein Azure) installieren) und anschließend auf den Link, der unten angezeigt wird.
    1. Klicken Sie auf die entsprechenden Downloadlinks, die rechts unter **Windows-Computer** angezeigt werden.
    1. Installieren Sie mit der heruntergeladenen ausführbaren Datei den Agent auf den Windows-Systemen Ihrer Wahl, und konfigurieren Sie ihn mit der **Arbeitsbereich-ID und den Schlüsseln**, die unter den oben erwähnten Downloadlinks angezeigt werden.

    > [!NOTE]
    >
    > Laden Sie über den Link unten rechts das **OMS-Gateway** herunter, und installieren Sie es auf einem separaten Computer, sodass dieser als Proxy fungiert, um es Windows-Systemen zu ermöglichen, auch ohne die erforderliche Internetverbindung weiterhin Ereignisse an Azure Sentinel zu streamen.  Sie müssen dennoch den Log Analytics-Agent auf allen Windows-Systemen installieren, deren Ereignisse Sie erfassen möchten.
    >
    > Weitere Informationen zu diesem Szenario finden Sie in der [**Log Analytics-Gateway**-Dokumentation](../azure-monitor/agents/gateway.md).

    Weitere Installationsmöglichkeiten und Informationen finden Sie in der [**Log Analytics-Agent**-Dokumentation](../azure-monitor/agents/agent-windows.md).

1. Wählen Sie die Gruppe von Ereignissen ([Alle, Häufige oder Minimal](#event-sets)) aus, die Sie streamen wollen.

1. Klicken Sie auf **Aktualisieren**.

1. Tippen Sie `SecurityEvent` in das Abfragefenster, um das relevante Schema in Log Analytics für sicherheitsrelevante Windows-Ereignisse zu verwenden.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann etwa 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

### <a name="configure-the-security-events-connector-for-anomalous-rdp-login-detection"></a>Konfigurieren des Connectors für sicherheitsrelevante Ereignisse zur Erkennung von ungewöhnlichen RDP-Anmeldungen

> [!IMPORTANT]
> Die Erkennung ungewöhnlicher RDP-Anmeldungen befindet sich derzeit in der öffentlichen Vorschauphase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel kann Machine Learning (ML) auf die Daten von sicherheitsrelevanten Ereignissen anwenden, um ungewöhnliche RDP-Anmeldeaktivitäten (Remotedesktopprotokoll) zu erkennen. Mögliche Szenarien:

- **Ungewöhnliche IP-Adresse**: Die IP-Adresse ist in den letzten 30 Tagen nur selten oder gar nicht aufgetreten.

- **Ungewöhnliche Geolocation**: IP-Adresse, Ort, Land und ASN sind in den letzten 30 Tagen nur selten oder gar nicht aufgetreten.

- **Neuer Benutzer**: Ein neuer Benutzer meldet sich über eine IP-Adresse oder Geolocation an, mit deren Auftreten (einzeln oder gemeinsam) anhand der Daten der letzten 30 Tage nicht gerechnet wurde.

**Konfigurationsanweisungen**

1. Sie müssen RDP-Anmeldedaten (Ereignis-ID 4624) über den Datenconnector für **Sicherheitsereignisse** erfassen. Stellen Sie sicher, dass Sie noch einen anderen [Ereignissatz](#event-sets) als „None“ (Keine) für das Streamen an Azure Sentinel ausgewählt haben.

1. Klicken Sie im Azure Sentinel-Portal auf **Analytics** und dann auf die Registerkarte **Regelvorlagen**. Wählen Sie die Regel **(Vorschau) Erkennung anomaler RDP-Anmeldungen** aus, und legen Sie den Schieberegler **Status** auf **Aktiviert** fest.

    > [!NOTE]
    > Da für den Machine Learning-Algorithmus zum Erstellen eines Basisprofils für das Benutzerverhalten die Daten von 30 Tagen benötigt werden, müssen Sie die Erfassung für 30 Tage mit sicherheitsrelevanten Ereignissen zulassen, bevor Incidents erkannt werden können.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Windows-Sicherheitsereignisse mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit dem Erkennen von Bedrohungen mit Azure Sentinel mithilfe von [integrierten](tutorial-detect-threats-built-in.md) oder [benutzerdefinierten](tutorial-detect-threats-custom.md) Regeln.

