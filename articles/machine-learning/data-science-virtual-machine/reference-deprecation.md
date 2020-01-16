---
title: 'Referenz: Veraltete DSVM-Images'
description: Details zu veralteten Azure Data Science Virtual Machine (DSVM)
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 252f9c66034dbadaf7a2e9e6f78665c26d414deb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456287"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referenz: Veraltete DSVM-Images

Im folgenden werden Vorschläge zum Umgang mit anstehendem Veralten bei Azure Data Science Virtual Machine erläutert.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Migrieren von Datenträgern

Der Support des Windows 2012 DSVM-Images wird am 31. Dezember 2019 eingestellt. Führen Sie die folgenden Schritte aus, um einen Datenträger von Ihrer vorhandenen Windows 2012 DSVM zu einer Windows 2016 DSVM zu migrieren:

1. Erstellen Sie eine neue Windows 2016 DSVM, und befolgen Sie die [hier](./provision-vm.md#create-your-dsvm) gezeigten Anweisungen.
1. Trennen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/detach-disk.md) vorhandene Datenträger von Ihrem Windows 2012-Image.
1. Fügen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm) den Datenträger aus dem vorherigen Schritt an Ihr Windows 2016-Image an.
