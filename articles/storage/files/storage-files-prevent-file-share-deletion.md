---
title: Verhindern des versehentlichen Löschens von Azure-Dateifreigaben
description: Erfahren Sie, wie Sie das vorläufige Löschen für Azure-Dateifreigaben aktivieren, um die Wiederherstellung von Daten zu ermöglichen und versehentliche Löschvorgänge zu verhindern.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 96e3d5001d11455337ae092776a1a4c5c3738012
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83882933"
---
# <a name="prevent-accidental-deletion-of-azure-file-shares"></a>Verhindern eines versehentlichen Löschens von Azure-Dateifreigaben

Azure Storage bietet jetzt für Dateifreigaben vorläufiges Löschen. Dadurch können Sie Ihre Daten leichter wiederherstellen, wenn diese irrtümlich von einer Anwendung oder einem anderen Benutzer des Speicherkontos gelöscht wurden.

## <a name="how-soft-delete-works"></a>Funktionsweise des vorläufigen Löschens

Falls aktiviert, ermöglicht Ihnen vorläufiges Löschen das Speichern und Wiederherstellen Ihrer Dateifreigaben, nachdem sie gelöscht wurden. Wenn Daten gelöscht werden, gehen sie in einen vorläufig gelöschten Zustand über, anstatt dauerhaft gelöscht zu werden. Sie können den Zeitraum konfigurieren, in dem vorläufig gelöschte Daten wiederhergestellt werden können, ehe sie dauerhaft gelöscht werden.

Vorläufiges Löschen kann für neue oder vorhandene Dateifreigaben aktiviert werden. Da vorläufiges Löschen auch abwärtskompatibel ist, müssen Sie keine Änderungen an Ihren Anwendungen vornehmen, um in den Genuss der Schutzvorteile dieses Features zu kommen. 

Bei vorläufig gelöschten Premium-Dateifreigaben wird das Dateifreigabekontingent (die bereitgestellte Größe einer Dateifreigabe) bei der Berechnung des gesamten Speicherkontokontingents bis zum Ablaufdatum der vorläufig gelöschten Freigabe genutzt, an dem die Freigabe vollständig gelöscht wird.

### <a name="availability"></a>Verfügbarkeit

Vorläufiges Löschen für Azure-Dateifreigaben ist für alle Speicherebenen, Speicherkontotypen und alle Regionen verfügbar, in denen Azure Files angeboten wird.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

Vorläufiges Löschen für Dateifreigaben wird auf Speicherkontoebene aktiviert. Die Einstellungen für vorläufiges Löschen gelten für alle Dateifreigaben eines Speicherkontos. Wenn Sie ein neues Speicherkonto erstellen, ist vorläufiges Löschen standardmäßig deaktiviert. Vorläufiges Löschen ist auch für vorhandene Speicherkonten standardmäßig deaktiviert. Sie können vorläufiges Löschen jederzeit ein- und ausschalten.

Wenn Sie vorläufiges Löschen für Dateifreigaben aktivieren, einige Dateifreigaben löschen und dann vorläufiges Löschen deaktivieren, können Sie, wenn die Freigaben in diesem Zeitraum gespeichert wurden, weiterhin auf diese Dateifreigaben zugreifen und sie wiederherstellen. Wenn Sie vorläufiges Löschen aktivieren, müssen Sie auch den Aufbewahrungszeitraum festlegen.

Der Aufbewahrungszeitraum gibt die Zeitspanne an, für die vorläufig gelöschte Datenfreigaben gespeichert werden und zur Wiederherstellung verfügbar sind. Für Dateifreigaben, die explizit gelöscht werden, beginnt der Aufbewahrungszeitraum, wenn die Daten gelöscht werden. Derzeit können Sie vorläufig gelöschte Daten 1 bis 365 Tage aufbewahren.

Sie können den Aufbewahrungszeitraum für vorläufiges Löschen jederzeit ändern. Eine Änderung des Aufbewahrungszeitraums gilt nur für Freigaben, die nach erfolgter Änderung des Aufbewahrungszeitraums gelöscht werden. Freigaben, die vor der Änderung des Aufbewahrungszeitraums gelöscht wurden, laufen entsprechend dem Aufbewahrungszeitraum ab, der beim Löschen dieser Daten festgelegt war.

Um eine Dateifreigabe mit aktiviertem vorläufigen Löschen vor dem Ablaufdatum dauerhaft zu löschen, müssen Sie das Löschen der Freigabe rückgängig machen, vorläufiges Löschen deaktivieren und dann die Freigabe erneut löschen. Anschließend müssen Sie vorläufiges Löschen reaktivieren, da alle anderen Dateifreigaben in diesem Speicherkonto anfällig für ein versehentliches Löschen sind, solange vorläufiges Löschen deaktiviert ist.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Sowohl Standard- als auch Premium-Dateifreigaben werden nach genutzter Kapazität in Rechnung gestellt, wenn sie vorläufig gelöscht werden, und nicht nach bereitgestellter Kapazität. Darüber hinaus werden Premium-Dateifreigaben mit dem Tarif für Momentaufnahmen in Rechnung gestellt, solange vorläufiges Löschen für sie aktiviert ist. Standarddateifreigaben werden bei aktiviertem vorläufigen Löschen mit dem Standardtarif in Rechnung gestellt. Für Daten, die nach Ablauf des festgelegten Aufbewahrungszeitraums dauerhaft gelöscht werden, fallen keine Kosten an.

Weitere Informationen zu Preisen für Azure File Storage im Allgemeinen finden Sie auf der Seite [Preise für Azure File Storage](https://azure.microsoft.com/pricing/details/storage/files/).

Wenn Sie vorläufiges Löschen erstmals aktivieren, wird ein kurzer Aufbewahrungszeitraum empfohlen, um besser zu verstehen, wie sich das Feature auf Ihre Rechnung auswirkt.

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie Sie vorläufiges Löschen aktivieren und nutzen können, fahren Sie mit [Aktivieren des vorläufigen Löschens](storage-files-enable-soft-delete.md) fort.