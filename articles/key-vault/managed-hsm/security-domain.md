---
title: Informationen zur Sicherheitsdomäne für verwaltete Azure-HSMs
description: Übersicht über die Sicherheitsdomäne für die verwaltete HSM, eine Reihe von wesentlichen Anmeldeinformationen, die zur Wiederherstellung einer verwalteten HSM benötigt werden
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: conceptual
author: amitbapat
ms.author: ambapat
ms.date: 09/15/2020
ms.openlocfilehash: 37e2541d0b53c96fd3f85da31b2c0ce5b68b551a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90993295"
---
# <a name="about-the-managed-hsm-security-domain"></a>Informationen zur Sicherheitsdomäne für verwaltete HSMs

Die Sicherheitsdomäne (SD) für verwaltete HSMs besteht aus einer Reihe von wesentlichen Anmeldeinformationen, die zur Notfallwiederherstellung einer verwalteten HSM erforderlich sind. Die Sicherheitsdomäne wird in der verwalteten HSM-Hardware und den Dienstsoftwareenklaven generiert und stellt das „Eigentum“ der HSM dar.

Jede verwaltete HSM muss über eine Sicherheitsdomäne verfügen, um betrieben werden zu können. Wenn Sie eine neue verwaltete HSM anfordern, wird sie bereitgestellt, aber erst aktiviert, wenn Sie die Sicherheitsdomäne herunterladen. Wenn eine verwaltete HSM den Zustand „Bereitgestellt“, aber nicht den Zustand „Aktiviert“ aufweist, gibt es zwei Möglichkeiten, um sie zu aktivieren:
- Das Herunterladen Ihrer Sicherheitsdomäne ist die Standardmethode und ermöglicht es Ihnen, die Sicherheitsdomäne sicher zu speichern, um sie mithilfe einer anderen verwalteten HSM zu verwenden oder eine Notfallwiederherstellung durchzuführen.
- Laden Sie eine vorhandene Sicherheitsdomäne hoch, die Sie bereits besitzen, wodurch Sie mehrere Instanzen der verwalteten HSM erstellen können, die dieselbe Sicherheitsdomäne gemeinsam nutzen.

## <a name="download-your-security-domain"></a>Herunterladen Ihrer Sicherheitsdomäne

Wenn eine verwaltete HSM bereitgestellt, aber nicht aktiviert ist, werden beim Herunterladen der Sicherheitsdomäne die wichtigsten Anmeldeinformationen erfasst, die zur Wiederherstellung nach einem vollständigen Verlust der gesamten Hardware erforderlich sind. Zum Herunterladen der Sicherheitsdomäne müssen Sie mindestens drei (maximal zehn) RSA-Schlüsselpaare erstellen und diese öffentlichen Schlüssel an den Dienst senden, wenn Sie das Herunterladen der Sicherheitsdomäne anfordern. Sie müssen auch die Mindestanzahl der erforderlichen Schlüssel (Quorum) angeben, um die Sicherheitsdomäne in Zukunft zu entschlüsseln. Die verwaltete HSM initialisiert die Sicherheitsdomäne und verschlüsselt sie mit den von Ihnen bereitgestellten öffentlichen Schlüsseln mithilfe des [Shamir's Secret Sharing-Algorithmus](https://dl.acm.org/doi/10.1145/359168.359176). Der Blob der Sicherheitsdomäne, den Sie herunterladen, kann nur entschlüsselt werden, wenn mindestens ein Quorum an privaten Schlüsseln verfügbar ist. Sie müssen die privaten Schlüssel sicher aufbewahren, um die Sicherheit der Sicherheitsdomäne sicherzustellen. Sobald der Download abgeschlossen ist, befindet sich die verwaltete HSM in einem aktivierten Zustand und ist einsatzbereit.  

> [!IMPORTANT]
> Für eine vollständige Notfallwiederherstellung benötigen Sie die Sicherheitsdomäne und das Quorum der privaten Schlüssel, die zu ihrer Sicherung verwendet wurden, sowie eine vollständige HSM-Sicherung. Wenn Sie die Sicherheitsdomäne oder eine ausreichende Anzahl der RSA-Schlüssel (private Schlüssel) verlieren, um das Quorum zu verlieren, und keine aktiven Instanzen der verwalteten HSM vorhanden sind, ist keine Notfallwiederherstellung möglich.

## <a name="upload-a-security-domain"></a>Hochladen einer Sicherheitsdomäne

Wenn eine verwaltete HSM bereitgestellt, aber nicht aktiviert ist, können Sie einen Wiederherstellungsprozess für die Sicherheitsdomäne einleiten. Die verwaltete HSM generiert ein RSA-Schlüsselpaar und gibt den öffentlichen Schlüssel zurück. Anschließend können Sie die Sicherheitsdomäne in die verwaltete HSM hochladen. Vor dem Hochladen muss dem Client (Azure CLI oder PowerShell) die minimale Quorumanzahl von privaten Schlüsseln zur Verfügung gestellt werden, die Sie beim Herunterladen der Sicherheitsdomäne verwendet haben. Der Client wird die Sicherheitsdomäne mithilfe dieses Quorums entschlüsseln und sie mit dem öffentlichen Schlüssel, den Sie bei der Anforderung der Wiederherstellung heruntergeladen haben, erneut verschlüsseln. Sobald der Upload abgeschlossen ist, befindet sich die verwaltete HSM im aktivierten Zustand.

## <a name="backup-and-restore-behavior"></a>Sichern und Wiederherstellen – Verhalten

Sicherungen (entweder eine vollständige Sicherung oder eine Sicherung mit einem einzelnen Schlüssel) können nur dann erfolgreich wiederhergestellt werden, wenn die verwaltete HSM der Quelle (wo die Sicherung erstellt wurde) und die verwaltete HSM des Ziels (wo die Sicherung wiederhergestellt wird) dieselbe Sicherheitsdomäne teilen. Auf diese Weise definiert eine Sicherheitsdomäne auch eine kryptografische Grenze für jede verwaltete HSM.

## <a name="next-steps"></a>Nächste Schritte

- Verschaffen Sie sich einen [Überblick über verwaltetes HSM](overview.md).
- Erfahren Sie mehr über das [Verwalten einer verwalteten HSM mit Azure CLI](key-management.md).
- Sehen Sie sich die [bewährten Methoden für verwaltetes HSM](best-practices.md) an.
