---
title: Konfigurieren von Daten basierend auf STIG in Azure Automation State Configuration
description: In diesem Artikel erfahren Sie, wie Sie Daten basierend auf DoD STIG für Azure Automation State Configuration konfigurieren.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: dab0cd7f7d660808b4ed7a91318baad55f80928c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87015135"
---
# <a name="configure-data-based-on-security-technical-information-guide-stig"></a>Konfigurieren von Daten basierend auf STIG (Security Technical Information Guide, Leitfaden mit technischen Informationen zur Sicherheit)

> Gilt für: Windows PowerShell 5.1

Das erstmalige Erstellen von Konfigurationsinhalten kann eine Herausforderung darstellen.
In vielen Fällen besteht das Ziel darin, die Konfiguration von Servern gemäß gewissen Grundwerten („Baseline“) zu automatisieren, die hoffentlich eine Branchenempfehlung erfüllen.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine Lösung, die von der Open Source-Community verwaltet wird.
> Support ist nur in Form der GitHub-Zusammenarbeit erhältlich, nicht von Microsoft.

## <a name="community-project-powerstig"></a>Communityprojekt: PowerSTIG

Ein Communityprojekt namens [PowerSTIG](https://github.com/microsoft/powerstig) zielt darauf ab, dieses Problem zu beheben, indem DSC-Inhalte basierend auf [öffentlichen Informationen](https://public.cyber.mil/stigs/) über STIG (Security Technical Implementation Guide, Sicherheitstechnisches Implementierungshandbuch) generiert werden.

Die Handhabung von Baselines ist komplizierter, als es klingt.
Viele Organisationen müssen [Ausnahmen von Regeln dokumentieren](https://github.com/microsoft/powerstig#powerstigdata) und diese Daten skaliert verwalten.
PowerSTIG löst dieses Problem, indem es [zusammengesetzte Ressourcen](https://github.com/microsoft/powerstig#powerstigdsc) bereitstellt, die jeden Bereich der Konfiguration abdecken, anstatt zu versuchen, den gesamten Bereich von Einstellungen in einer großen Datei zu erfassen.

Sobald die Konfigurationen einmal generiert sind, können Sie die [DSC-Konfigurationsskripts](/powershell/scripting/dsc/configurations/configurations) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines) oder [in Azure](./automation-dsc-onboarding.md#enable-azure-vms) um Konfigurationen abzurufen.

Um PowerSTIG zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/microsoft/powerstig) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in PowerShell DSC finden Sie unter [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview).
- Erfahren Sie mehr über PowerShell DSC-Ressourcen in [DSC-Ressourcen](/powershell/scripting/dsc/resources/resources).
- Weitere Informationen zur Konfiguration des lokalen Konfigurations-Managers finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaconfig).
