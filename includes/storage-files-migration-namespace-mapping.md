---
title: Zuordnen einer Ordnerstruktur zu einer Azure-Dateisynchronisierungstopologie
description: Zuordnung einer vorhandenen Datei- und Ordnerstruktur zu Azure-Dateifreigaben für die Verwendung mit der Azure-Dateisynchronisierung Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 44fb95de88cc86c802e5ba72f0b5379b8708c506
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209321"
---
In diesem Schritt ermitteln Sie, wie viele Azure-Dateifreigaben Sie benötigen. Eine einzelne Windows Server-Instanz (oder ein Cluster) kann bis zu 30 Azure-Dateifreigaben synchronisieren.

Möglicherweise verfügen Sie über weitere Ordner in Ihrer StorSimple-Lösung, die Sie aktuell lokal als SMB-Freigaben für Ihre Benutzer und Apps freigeben. Am einfachsten wäre es, wenn sich eine lokale Freigabe 1:1 einer Azure-Dateifreigabe zuordnen ließe. Wenn diese Zahl relativ gering ist, also pro Windows Server-Instanz maximal 30 Freigaben geplant sind, oder Sie zwei Windows Server-Instanzen (60) planen usw., wird eine 1:1-Zuordnung empfohlen.

Wenn Sie über mehr als 30 Dateifreigaben verfügen, ist es häufig nicht nötig oder sinnvoll, eine lokale Dateifreigabe 1:1 einer Azure-Dateifreigabe zuzuordnen.
Ziehen Sie folgende Möglichkeiten in Betracht:

#### <a name="share-grouping"></a>Gruppierung von Freigaben

Wenn Ihre Personalabteilung beispielsweise über insgesamt 15 Freigaben verfügt, könnten Sie die Speicherung aller Personaldaten in einer einzelnen Azure-Dateifreigabe in Erwägung ziehen. Wenn Sie mehrere lokale Freigaben in einer Azure-Dateifreigabe speichern, können Sie trotzdem die üblichen 15 SMB-Freigaben auf Ihrer lokalen Windows Server-Instanz erstellen. Die Gruppierung der Freigaben bedeutet lediglich, dass Sie die Stammordner dieser 15 Freigaben als Unterordner in einem gemeinsamen Ordner organisieren. Anschließend synchronisieren Sie diesen gemeinsamen Ordner mit einer Azure-Dateifreigabe. Dadurch wird für diese Gruppe lokaler Freigaben nur eine einzige Azure-Dateifreigabe in der Cloud benötigt.

#### <a name="volume-sync"></a>Volumesynchronisierung

Die Azure-Dateisynchronisierung unterstützt das Synchronisieren eines Volumestamms mit einer Azure-Dateifreigabe.
Wenn Sie den Stammordner synchronisieren, werden alle Unterordner und Dateien in derselben Azure-Dateifreigabe abgelegt.

#### <a name="other-best-practices-to-consider"></a>Weitere Best Practices, die Sie berücksichtigen sollten

Neben der maximalen Anzahl von 30 Azure-Dateifreigaben, die pro Server synchronisiert werden können, ist die Effizienz der Synchronisierung ein weiterer wichtiger Aspekt.

Wenn auf Ihrem Server mehrere Freigaben vorhanden sind, die jeweils mit einer eigenen Azure-Dateifreigabe synchronisiert werden, kann die Synchronisierung aller dieser Dateien parallel erfolgen. Maßgeblich für diesen Vorgang ist nicht die Größe aller zu synchronisierenden Dateien. Vielmehr ist die Anzahl von Elementen (Dateien und Ordner) entscheidend, die verarbeitet werden müssen.

Eine einzelne Azure-Dateifreigabe kann bis zu 100 TiB umfassen.
Die Azure-Dateisynchronisierung unterstützt die Synchronisierung von bis zu 100 Millionen Elementen pro Azure-Dateifreigabe.

Das Synchronisieren des Stammvolumes ist daher nicht immer empfehlenswert. Mitunter kann es von Vorteil sein, verschiedene Speicherorte separat zu synchronisieren. Auf diese Weise lässt sich die Anzahl von Elementen pro Synchronisierungsvorgang reduzieren. Doch nicht nur für die Synchronisierung ist es wichtig, die Azure-Dateisynchronisierung mit einer geringeren Anzahl von Elementen einzurichten. Diese Vorgehensweise ist auch für cloudseitige Wiederherstellungen aus Sicherungen sinnvoll und ermöglicht eine schnellere Notfallwiederherstellung bei einem Serverausfall, wenn Sie einen neuen Server bereitstellen, der mit denselben Azure-Dateifreigaben verbunden wird.

Berücksichtigen Sie die oben beschriebenen Konzepte, um zu bestimmen, wie viele Azure-Dateifreigaben Sie benötigen und welche Teile Ihrer vorhandenen StorSimple-Daten in welcher Azure-Dateifreigabe platziert werden sollen.

Notieren Sie die wichtigsten Aspekte, die Sie für Ihre Umgebung berücksichtigen müssen, damit Sie im nächsten Schritt darauf zugreifen können. Beim Erstellen Ihres Zuordnungsplans sollten Sie organisiert vorgehen, um nicht den Überblick zu verlieren, wenn Sie eine große Anzahl von Ressourcen gleichzeitig bereitstellen.
