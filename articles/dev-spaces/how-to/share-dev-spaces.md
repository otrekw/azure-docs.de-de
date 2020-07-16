---
title: Freigeben von Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Erfahren Sie, wie Sie mit Azure Dev Spaces einen Entwicklungsbereich in Azure Kubernetes Service mit anderen Benutzern in Ihrem Team teilen können.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: 3cef7cd6a5113401cb1df3dd6c76e4dea3a7524d
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229294"
---
# <a name="share-azure-dev-spaces"></a>Freigeben von Azure Dev Spaces

Mit Azure Dev Spaces können Sie Ihren Dev Space (Entwicklungsbereich) für andere Personen in Ihrem Team freigeben. Jeder Entwickler kann in seinem eigenen Bereich arbeiten, ohne befürchten zu müssen, andere zu stören. Darüber hinaus ermöglicht die Zusammenarbeit in einem Bereich das durchgängige Testen (End-to-End-Testen) von Code, ohne Mocks erstellen oder Abhängigkeiten simulieren zu müssen. Im Leitfaden [Weitere Informationen zur Teamentwicklung](../team-development-nodejs.md) können Sie mehr darüber erfahren.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Einrichten eines Entwicklungsbereichs für mehrere Entwickler

1. Erstellen Sie einen Dev Space in Azure. Wählen Sie [.NET Core und VS Code](../get-started-netcore.md), [.NET Core und Visual Studio](../get-started-netcore-visualstudio.md) oder [Node.js und VS Code](../get-started-nodejs.md) aus. Sie müssen über Zugriffsrechte der Rolle „Besitzer“ oder „Mitwirkender“ für das ausgewählte Azure-Abonnement verfügen.
1. Stellen Sie sicher, dass jedes Teammitglied über die [entsprechenden Berechtigungen für den Zugriff auf den Azure Dev Spaces-Controller](../troubleshooting.md#incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis) verfügt. Sie können beispielsweise die **Ressourcengruppe** von Azure Dev Space so konfigurieren, dass jedem Teammitglied [Zugriffsrechte als Mitwirkender gewährt werden](../../role-based-access-control/role-assignments-portal.md). Sie können die Ressourcengruppe eines Entwicklungsbereichs überprüfen, indem Sie den folgenden Befehl ausführen: `azds show-context`
1. Fordern Sie Teammitglieder zum **Auswählen des Entwicklungsbereichs** auf, um ihn für die Entwicklung zu verwenden.
   * **Befehlszeile oder VS Code**: Verwenden Sie den Befehl `azds space list`, um vorhandene Azure Dev Spaces-Instanzen anzuzeigen, auf die Sie zugreifen können. So wählen Sie einen Entwicklungsbereich aus: `azds space select`.
   * **Visual Studio-IDE**: Öffnen Sie ein Projekt in Visual Studio, und wählen Sie im Dropdownmenü „Starteinstellungen“ die Option **Azure Dev Spaces** aus. Wählen Sie im daraufhin angezeigten Dialogfeld einen vorhandenen Cluster aus.

     ![Visual Studio-Dropdownmenü „Starteinstellungen“](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Weitere Informationen zur Teamentwicklung](../team-development-nodejs.md) können Sie mehr darüber erfahren.
