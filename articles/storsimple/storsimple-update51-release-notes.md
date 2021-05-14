---
title: Versionshinweise zu Update 5.1 der StorSimple 8000-Serie
description: Beschreibt die neuen Features sowie Probleme und Problemumgehungen für Update 5.1 der StorSimple 8000-Serie.
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.topic: conceptual
ms.date: 04/14/2021
ms.author: alkohli
ms.openlocfilehash: 735d7c1a25acb767f9d6352f6c8887af4b9464c0
ms.sourcegitcommit: 19dcad80aa7df4d288d40dc28cb0a5157b401ac4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107895917"
---
# <a name="storsimple-8000-series-update-51-release-notes"></a>Versionshinweise zu Update 5.1 der StorSimple 8000-Serie

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise beschreiben die neuen Features und weisen auf wichtige offene Punkte bei Update 5.1 der StorSimple 8000-Serie hin. Es enthält auch eine Liste der StorSimple-Software-Updates, die in dieser Version enthalten sind.

Update 5.1 kann auf alle StorSimple-Geräte angewendet werden, auf denen Update 5 ausgeführt wird. Wenn Sie mit einer früheren Version als 5 arbeiten, wenden Sie zuerst Update 5 und dann Update 5.1 an. Update 5.1 ist die Geräteversion 6.3.9600.17885 zugeordnet.

Lesen Sie vor der Bereitstellung des Updates in Ihrer StorSimple-Lösung die Informationen in den Versionsanmerkungen sorgfältig durch.

> [!IMPORTANT]
>
> * Update 5.1 ist ein obligatorisches Update und muss sofort installiert werden, um den Betrieb des Geräts sicherzustellen. Update 5.0 ist eine minimal unterstützte Version.
> * Update 5.1 enthält Sicherheitsupdates, deren Installation ca. 30 Minuten dauert. Weitere Informationen finden Sie unter [Anwenden von Update 5.1](storsimple-8000-install-update-51.md).

## <a name="whats-new-in-update-51"></a>Neuerungen in Update 5.1

Die folgenden wichtigen Verbesserungen und Fehlerbehebungen wurden in Update 5.1 vorgenommen:

* **TLS 1.2**: Dieses StorSimple-Update erzwingt TLS 1.2 auf allen Clients. TLS 1.2 ist ein obligatorisches Update für alle Geräte der StorSimple 8000-Serie.

   Wenn die folgende Warnung angezeigt wird, müssen Sie die Software auf dem Gerät aktualisieren, bevor Sie den Vorgang fortsetzen:

   Auf mindestens einem StorSimple-Gerät wird eine ältere Softwareversion ausgeführt. Das neueste verfügbare Update für TLS 1.2 ist ein obligatorisches Update und sollte sofort auf diesen Geräten installiert werden. TLS 1.2 wird für die gesamte Kommunikation des Azure-Portals verwendet, und ohne dieses Update kann das Gerät nicht mit dem StorSimple-Dienst kommunizieren.

## <a name="known-issues-in-update-51-from-previous-releases"></a>Bekannte Probleme in Update 5.1 aus vorherigen Versionen

Es gibt keine neuen bekannten Probleme in Update 5.1. Eine Liste der Probleme, die aus vorherigen Versionen in Update 5.1 übertragen wurden, finden Sie unter [Versionshinweise zu Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="storsimple-cloud-appliance-updates-in-update-51"></a>Aktualisierungen an der StorSimple Cloud Appliance in Update 5.1

Dieses Update gilt nicht für die StorSimple Cloud Appliance (auch bekannt als das virtuelle Gerät). Sie müssen neue Cloudgeräte mithilfe des Update 5.1-Images erstellen. Weitere Informationen zum Erstellen einer StorSimple Cloud Appliance finden Sie unter [Bereitstellen und Verwalten einer StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Nächster Schritt

Erfahren Sie, wie Sie [Update 5.1 auf Ihrem StorSimple-Gerät installieren](storsimple-8000-install-update-51.md).
