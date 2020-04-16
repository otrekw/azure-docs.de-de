---
title: 'Referenz: Veraltete virtuelle Computerimages für Data Science'
titleSuffix: Azure Data Science Virtual Machine
description: Details zu veralteten Azure Data Science Virtual Machine
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 04/03/2020
ms.topic: reference
ms.openlocfilehash: 6004ae0c1fd2fa25a07ab84776ab74789f6e7da9
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754773"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referenz: Veraltete DSVM-Images

Im folgenden werden Vorschläge zum Umgang mit anstehendem Veralten bei Azure Data Science Virtual Machine erläutert.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Migrieren von Datenträgern

Der Support des Windows 2012 DSVM-Images wird am 31. Dezember 2019 eingestellt. Führen Sie die folgenden Schritte aus, um einen Datenträger von Ihrer vorhandenen Windows 2012 DSVM zu einer Windows 2016 DSVM zu migrieren:

1. Erstellen Sie eine neue Windows 2016 DSVM, und befolgen Sie die [hier](./provision-vm.md#create-your-dsvm) gezeigten Anweisungen.
1. Trennen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/detach-disk.md) vorhandene Datenträger von Ihrem Windows 2012-Image.
1. Fügen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm) den Datenträger aus dem vorherigen Schritt an Ihr Windows 2016-Image an.

## <a name="centos"></a>CentOS

Neue Benutzer sollten die neuesten Ubuntu- oder Windows-Images verwenden. CentOS bleibt für die Verwendung mit vorhandenen Lösungsvorlagen weiterhin verfügbar.