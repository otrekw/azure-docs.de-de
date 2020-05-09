---
title: Skript zum Aktivieren von Kdump in SAP HANA (große Instanzen) | Microsoft-Dokumentation
description: Skript zum Aktivieren von Kdump in SAP HANA (große Instanzen) HLI-Typ I und HLI-Typ II
services: virtual-machines-linux
documentationcenter: ''
author: prtyag
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/30/2020
ms.author: prtyag
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 16dc15b4369904643d0138a4b8e5b94c47868d31
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204779"
---
# <a name="enable-kdump-service"></a>Aktivieren des Kdump-Diensts

In diesem Artikel wird das Aktivieren des Kdump-Diensts für große Azure HANA-Instanzen (**Typ I und Typ II**) behandelt.

## <a name="supported-skus"></a>Unterstützte SKUs

|  Typ der großen HANA-Instanz   |  Betriebssystemhersteller   |  Betriebssystem-Paketversion   |  SKU        |
|-----------------------------|--------------|-----------------------|-------------|
|   Typ I                    |  SuSE        |   SLES 12 SP3         |  S224m      |
|   Typ I                    |  SuSE        |   SLES 12 SP4         |  S224m      |
|   Typ I                    |  SuSE        |   SLES 12 SP2         |  S72m       |
|   Typ I                    |  SuSE        |   SLES 12 SP3         |  S72m       |
|   Typ I                    |  SuSE        |   SLES 12 SP2         |  S96        |
|   Typ I                    |  SuSE        |   SLES 12 SP3         |  S96        |
|   Typ II                   |  SuSE        |   SLES 12 SP3         |  S384       |
|   Typ II                   |  SuSE        |   SLES 12 SP3         |  S576m      |
|   Typ II                   |  SuSE        |   SLES 12 SP3         |  S384xm     |
|   Typ II                   |  SuSE        |   SLES 12 SP4         |  S384xm     |

## <a name="prerequisites"></a>Voraussetzungen

- Der Kdump-Dienst verwendet das `/var/crash`-Verzeichnis zum Schreiben von Absturzabbildern. Stellen Sie sicher, dass die Partition, die diesem Verzeichnis entspricht, über ausreichend Speicherplatz verfügt, um Absturzabbilder zu speichern.

## <a name="setup-details"></a>Details zur Konfiguration

- Das Skript zum Aktivieren von Kdump finden Sie [hier](https://github.com/Azure/sap-hana/blob/master/tools/enable-kdump.sh).

- Führen Sie dieses Skript mithilfe des folgenden Befehls in einer großen HANA-Instanz aus.

    > [!NOTE]
    > Zur Ausführung dieses Befehls ist die sudo-Berechtigung erforderlich.

    ```bash
    sudo bash enable-kdump.sh
    ```

- Wenn der Befehl ausgibt, dass Kdump erfolgreich aktiviert wurde, starten Sie das System neu, damit die Änderung übernommen wird. Erst dann ist Kdump erfolgreich aktiviert. Starten Sie das System neu, um die Änderungen zu übernehmen.

- Wenn die Befehlsausgabe einen Fehlschlag angibt, wurde der Kdump-Dienst nicht aktiviert. In diesem Fall finden Sie weitere Informationen im Abschnitt [Supportprobleme](#support-issue).

## <a name="test-kdump"></a>Testen von Kdump

> [!NOTE]
>  Mit dem folgenden Vorgang werden ein Kernel-Absturz und ein Systemneustart ausgelöst.

- Lösen Sie einen Kernel-Absturz aus.

    ```bash
    echo 1 > /proc/sys/kernel/sysrq
    echo c > /proc/sysrq-trigger
    ```

- Überprüfen Sie nach erfolgreichem Systemneustart, ob das `/var/crash`-Verzeichnis Kernel-Absturzprotokolle enthält.

- Wenn `/var/crash` ein Verzeichnis mit dem aktuellen Datum enthält, wurde Kdump erfolgreich aktiviert.

## <a name="support-issue"></a>Supportprobleme

Wenn das Skript mit einem Fehler fehlschlägt oder Kdump nicht aktiviert wurde, wenden Sie sich mit einem Service Request und den folgenden Informationen an das Microsoft-Supportteam.

* HLI-Abonnement-ID

* Servername

* Betriebssystemhersteller

* Betriebssystemversion

* Kernelversion
