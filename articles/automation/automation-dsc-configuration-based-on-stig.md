---
title: Auf STIG basierte Konfiguration zur Verwendung in der Zustandskonfiguration – Azure Automation
description: Erfahren Sie über Konfigurationen, die auf STIG basieren, zur Verwendung in der Zustandskonfiguration in Azure Automation.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 275b3bd25f931b73e8a378433899ef9ade4d47c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028278"
---
# <a name="configuration-based-on-stig"></a>Konfiguration basierend auf STIG

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

Sobald die Konfigurationen einmal generiert sind, können Sie die [DSC-Konfigurationsskripts](/powershell/scripting/dsc/configurations/configurations) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) oder [in Azure](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) um Konfigurationen abzurufen.

Um PowerSTIG zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/microsoft/powerstig) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview)
- [DSC-Ressourcen](/powershell/scripting/dsc/resources/resources)
- [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaconfig)
