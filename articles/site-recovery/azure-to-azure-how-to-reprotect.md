---
title: Erneutes Schützen virtueller Azure-Computer in die primäre Region mit Azure Site Recovery | Microsoft-Dokumentation
description: Beschreibt das erneute Schützen von virtuellen Azure-Computern nach einem Failover von der sekundären in die primäre Region mithilfe von Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 3b9edab6e908b4506a92c78aa8f3f53277b9c17b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91360870"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Erneutes Schützen von virtuellen Azure-Computern, für die ein Failover zur primären Region durchgeführt wurde

Wenn Sie mithilfe von [Azure Site Recovery](site-recovery-overview.md) ein [Failover](site-recovery-failover.md) für virtuelle Azure-Computer von einer Region zu einer anderen durchführen, werden die virtuellen Computer in der sekundären Region in einem **ungeschützten** Zustand gestartet. Wenn Sie für die virtuellen Computer ein Failback zur primären Region durchführen möchten, führen Sie folgende Schritte aus:

1. Schützen Sie die virtuellen Computer in der sekundären Region erneut, damit ihre Replizierung in der primären Region starten kann.
1. Nachdem das erneute Schützen abgeschlossen ist und die virtuellen Computer replizieren, können Sie ein Failover von der sekundären zur primären Region durchführen.

## <a name="prerequisites"></a>Voraussetzungen

- Das Failover des virtuellen Computers von der primären zur sekundären Region muss committet werden.
- Der primäre Zielstandort muss verfügbar sein, und Sie müssen auf Ressourcen in dieser Region zugreifen bzw. sie erstellen können.

## <a name="reprotect-a-vm"></a>Erneutes Schützen eines virtuellen Computers

1. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover ausgeführt wurde, und wählen Sie **Erneut schützen** aus. Die Richtung des erneuten Schützens sollte als von sekundär zu primär angezeigt werden.

   ![Screenshot: Virtueller Computer mit einem Kontextmenü, in dem die Option „Erneut schützen“ ausgewählt ist.](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

1. Überprüfen Sie Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen. Klicken Sie dann auf **OK**. Wenn als neu markierte Ressourcen vorhanden sind, werden sie im Rahmen des Vorgangs zum erneuten Schützen erstellt.
1. Diese Auftrag zum erneuten Schützen liefert die neuesten Daten an den Zielstandort. Nachdem dieser Vorgang abgeschlossen ist, erfolgt die Deltareplikation. Anschließend können Sie ein Failback zum primären Standort durchführen. Mit der Option zum Anpassen können Sie das Speicherkonto oder Netzwerk auswählen, das Sie während des Vorgangs zum erneuten Schützen verwenden möchten.

   ![Option zum Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Anpassen der Einstellungen des erneuten Schützens

Sie können die folgenden Eigenschaften des virtuellen Zielcomputers beim erneuten Schützen anpassen.

![Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschaft |Notizen  |
|---------|---------|
|Zielressourcengruppe | Ändern Sie die Zielressourcengruppe, in welcher der virtuelle Computer erstellt wird. Der virtuelle Zielcomputer wird im Rahmen des Vorgangs zum erneuten Schützen gelöscht. Sie können eine neue Ressourcengruppe auswählen, unter welcher der virtuelle Computer nach dem Failover erstellt wird. |
|Virtuelles Zielnetzwerk | Das Zielnetzwerk kann während des Austrags zum erneuten Schützen nicht geändert werden. Wiederholen Sie die Netzwerkzuordnung, um das Netzwerk zu ändern. |
|Zielspeicher (sekundäre VM verwendet keine verwalteten Datenträger) | Sie können das Speicherkonto ändern, das der virtuelle Computer nach einem Failover verwendet. |
|Verwaltete Replikatdatenträger (sekundäre VM verwendet verwaltete Datenträger) | Site Recovery erstellt verwaltete Replikatdatenträger in der primären Region, um die verwalteten Datenträger der sekundären VM zu spiegeln. |
|Cachespeicher | Sie können ein Zwischenspeicherkonto angeben, das während der Replikation verwendet wird. Standardmäßig wird ein neues Zwischenspeicherkonto erstellt, wenn keines vorhanden ist. |
|Verfügbarkeitsgruppe | Wenn der virtuelle Computer in der sekundären Region Teil einer Verfügbarkeitsgruppe ist, können Sie eine Verfügbarkeitsgruppe für den virtuellen Zielcomputer in der primären Region auswählen. Standardmäßig versucht Site Recovery die vorhandene Verfügbarkeitsgruppe in der primären Region zu finden und verwenden. Während der Anpassung können Sie eine neue Verfügbarkeitsgruppe angeben. |

### <a name="what-happens-during-reprotection"></a>Was geschieht beim erneuten Schützen?

Standardmäßig geschieht Folgendes:

1. Ein Cachespeicherkonto wird in der Region des virtuellen Computers erstellt, für den ein Failover ausgeführt wurde.
1. Wenn das Zielspeicherkonto (das ursprüngliche Speicherkonto in der primären Region) nicht vorhanden ist, wird ein neues erstellt. Der zugewiesene Name des Speicherkontos ist der vom sekundären virtuellen Computer verwendete Name des Speicherkontos mit dem Suffix `asr`.
1. Wenn Ihr virtueller Computer verwaltete Datenträger verwendet, werden verwaltete Replikatdatenträger in der primären Region zum Speichern der Daten erstellt, die von den Datenträgern der sekundären VM repliziert werden.
1. Wenn die Zielverfügbarkeitsgruppe nicht vorhanden ist, wird im Rahmen des Auftrags zum erneuten Schützen bei Bedarf eine neue erstellt. Wenn Sie die Einstellungen für das erneute Schützen angepasst haben, wird die ausgewählte Gruppe verwendet.

Wenn Sie einen Auftrag zum erneuten Schützen auslösen und der virtuelle Zielcomputer vorhanden ist, geschieht Folgendes:

1. Falls der virtuelle Computer auf der Zielseite ausgeführt wird, wird er ausgeschaltet.
1. Falls der virtuelle Computer verwaltete Datenträger verwendet, wird eine Kopie des ursprünglichen Datenträgers mit dem Suffix `-ASRReplica` erstellt. Die ursprünglichen Datenträger werden gelöscht. Die Kopien mit dem Suffix `-ASRReplica` werden für die Replikation verwendet.
1. Wenn der virtuelle Computer nicht verwaltete Datenträger verwendet, werden die Datenträger des virtuellen Zielcomputers getrennt und für die Replikation verwendet. Eine Kopie des Betriebssystemdatenträgers wird erstellt und an den virtuellen Computer angefügt. Der ursprüngliche Betriebssystemdatenträger wird getrennt und für die Replikation verwendet.
1. Nur Unterschiede zwischen dem Quelldatenträger und dem Zieldatenträger werden synchronisiert. Die Unterschiede werden durch einen Vergleich der beiden Datenträger ermittelt und dann übertragen. Unten finden Sie Angaben zur geschätzten Zeit für den Abschluss des erneuten Schützens.
1. Nach Abschluss der Synchronisierung beginnt die Deltareplikation, und es wird gemäß der Replikationsrichtlinie ein Wiederherstellungspunkt erstellt.

Wenn Sie einen Auftrag zum erneuten Schützen auslösen und der virtuelle Zielcomputer und die Datenträger nicht vorhanden sind, geschieht Folgendes:

1. Falls der virtuelle Computer verwaltete Datenträger verwendet, werden Replikatdatenträger mit dem Suffix `-ASRReplica` erstellt. Die Kopien mit dem Suffix `-ASRReplica` werden für die Replikation verwendet.
1. Falls der virtuelle Computer nicht verwaltete Datenträger verwendet, werden Replikatdatenträger im Zielspeicherkonto erstellt.
1. Die gesamten Datenträger werden von der Region, für die ein Failover ausgeführt wurde, in die neue Zielregion kopiert.
1. Nach Abschluss der Synchronisierung beginnt die Deltareplikation, und es wird gemäß der Replikationsrichtlinie ein Wiederherstellungspunkt erstellt.

#### <a name="estimated-time-to-do-the-reprotection"></a>Geschätzte Zeit für das Einrichten des erneuten Schutzes

In den meisten Fällen repliziert Azure Site Recovery nicht die vollständigen Daten in die Quellregion.
Die folgenden Bedingungen bestimmen, wie viele Daten repliziert werden:

1. Wenn die Daten der Quell-VM aus irgendeinem Grund, wie etwa einer Änderung/Löschung der Ressourcengruppe, gelöscht oder beschädigt werden oder nicht mehr zugänglich sind, erfolgt während des erneuten Schutzes eine vollständige Erstreplikation, da für die Quellregion keine Daten verfügbar sind, die verwendet werden können.
1. Wenn auf die Daten der Quell-VM zugegriffen werden kann, werden nur die Unterschiede durch einen Vergleich der beiden Datenträger ermittelt und dann übertragen. Geschätzte Zeitangaben finden Sie in der Tabelle unten.

|Beispielsituation | Benötigte Zeit zum erneuten Schützen |
|---|---|
|Die Quellregion verwendet 1 VM mit Standarddatenträger mit 1 TB Kapazität.<br/>Nur 127 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Standard mit 60 MBit/s Durchsatz.<br/>Keine Datenänderung nach dem Failover.| Ungefährer Zeitaufwand: 60–90 Minuten<br/> Während des erneuten Schützens füllt Site Recovery die Prüfsumme aller Daten auf. Dies erfolgt mit einem Durchsatz von 45 MBit/s, sodass eine Gesamtzeit von 45 Minuten (127 GB ÷ 45 MBit/s) erforderlich ist.<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung ausführen kann, ungefähr 20–30 Minuten. |
|Die Quellregion verwendet 1 VM mit Standarddatenträger mit 1 TB Kapazität.<br/>Nur 127 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Standard mit 60 MBit/s Durchsatz.<br/>45 GB Datenänderung nach dem Failover.| Ungefährer Zeitaufwand: 2,5–3 Stunden<br/> Während des erneuten Schützens füllt Site Recovery die Prüfsumme aller Daten auf. Dies erfolgt mit einem Durchsatz von 45 MBit/s, sodass eine Gesamtzeit von 45 Minuten (127 GB ÷ 45 MBit/s) erforderlich ist.<br/>Die Übertragungsgeschwindigkeit beträgt etwa 16 % des Durchsatzes, was wiederum 9,6 MBit/s entspricht. Daher beträgt die Übertragungszeit zum Anwenden von Änderungen mit einer Größe von 45 GB etwa 80 Minuten (45 GB ÷ 9,6 MBit/s).<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung ausführen kann, ungefähr 20–30 Minuten. |
|Die Quellregion verwendet 1 VM mit Standarddatenträger mit 1 TB Kapazität.<br/>Nur 20 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Standard mit 60 MBit/s Durchsatz.<br/>Die anfänglichen Daten auf dem Datenträger unmittelbar nach dem Failover betragen 15 GB. Nach dem Failover liegt ein Unterschied von 5 GB an Daten vor. Daher beträgt die Gesamtgröße der aufgefüllten Daten 20 GB.| Ungefährer Zeitaufwand: 1–1,5 Stunden<br/>Da die aufgefüllten Daten auf dem Datenträger weniger als 10 % der Größe des Datenträgers umfassen, wird eine vollständige erste Replikation durchgeführt.<br/> Die Übertragungsgeschwindigkeit beträgt etwa 16 % des Durchsatzes, was wiederum 9,6 MBit/s entspricht. Daher beträgt die Übertragungszeit zum Anwenden von Änderungen mit einer Größe von 20 GB etwa 36 Minuten (20 GB ÷ 9,6 MBit/s).<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung ausführen kann, ungefähr 20–30 Minuten. |
|Die Quellregion enthält eine VM mit einem Premium-Datenträger mit einer Kapazität von 1 TB.<br/>Nur 127 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Premium mit 200 MBit/s Durchsatz.<br/>Keine Datenänderung nach dem Failover.| Ungefährer Zeitaufwand: 2 Stunden.<br/>Während des erneuten Schützens füllt Site Recovery die Prüfsumme aller Daten auf. Dies erfolgt mit einem Durchsatz von 25 MBit/s (bis zu 16 % des Datenträgerdurchsatzes), sodass eine Gesamtzeit von ca. 87 Minuten (127 GB ÷ 25 MBit/s) erforderlich ist.<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung ausführen kann, ungefähr 20–30 Minuten. |
|Die Quellregion enthält eine VM mit einem Premium-Datenträger mit einer Kapazität von 1 TB.<br/>Nur 127 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Premium mit 200 MBit/s Durchsatz.<br/>45 GB Datenänderung nach dem Failover.| Ungefährer Zeitaufwand: 2,5–3 Stunden<br/>Während des erneuten Schützens füllt Site Recovery die Prüfsumme aller Daten auf. Dies erfolgt mit einem Durchsatz von 25 MBit/s (bis zu 16 % des Datenträgerdurchsatzes), sodass eine Gesamtzeit von ca. 87 Minuten (127 GB ÷ 25 MBit/s) erforderlich ist.</br>Die Übertragungsgeschwindigkeit beträgt etwa 16 % des Durchsatzes, was wiederum 32 MBit/s entspricht. Daher beträgt die Übertragungszeit zum Anwenden von Änderungen mit einer Größe von 45 GB etwa 24 Minuten (45 GB ÷ 32 MBit/s).<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung ausführen kann, ungefähr 20–30 Minuten. |
|Die Quellregion enthält eine VM mit einem Premium-Datenträger mit einer Kapazität von 1 TB.<br/>Nur 20 GB werden für Daten verwendet, der Rest des Datenträgers ist leer.<br/>Der Datenträgertyp ist Premium mit 200 MBit/s Durchsatz.<br/>Die anfänglichen Daten auf dem Datenträger unmittelbar nach dem Failover betragen 15 GB. Nach dem Failover liegt ein Unterschied von 5 GB an Daten vor. Daher beträgt die Gesamtgröße der aufgefüllten Daten 20 GB.| Ungefährer Zeitaufwand: 30–45 Minuten<br/>Da die aufgefüllten Daten auf dem Datenträger weniger als 10 % der Größe des Datenträgers umfassen, wird eine vollständige erste Replikation durchgeführt.<br/>Die Übertragungsgeschwindigkeit beträgt etwa 16 % des Durchsatzes, was wiederum 32 MBit/s entspricht. Daher beträgt die Übertragungszeit zum Anwenden von Änderungen mit einer Größe von 20 GB etwa 11 Minuten (20 GB ÷ 32 MBit/s).<br/>Zeitlicher Mehraufwand ist erforderlich, damit Site Recovery die automatische Skalierung durchführen kann, ungefähr 20–30 Minuten. |

Wenn die VM nach einem Failback zur primären Region erneut geschützt wird (wenn die VM also von der primären Region in die DR-Region erneut geschützt wird), wird die Ziel-VM mitsamt ihrer verknüpften NIC(s) gelöscht.

Wenn die VM von der DR-Region zur primären Region erneut geschützt wird, werden die ehemalige primäre VM und ihre verknüpfte(n) NIC(s) nicht gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Nachdem der virtuelle Computer geschützt ist, können Sie ein Failover initiieren. Beim Failover wird der virtuelle Computer in der sekundären Region heruntergefahren und der virtuelle Computer in der primären Region erstellt und gestartet. Während des Prozesses tritt eine kurze Ausfallzeit auf. Es empfiehlt sich daher, für diesen Prozess einen geeigneten Zeitpunkt zu wählen und ein Testfailover auszuführen, bevor Sie ein vollständiges Failover auf den primären Standort einleiten. [Weitere Informationen](site-recovery-failover.md) zum Failover mit Azure Site Recovery.
