---
title: Einrichten Ihrer Umgebung für einen Blaupausenoperator
description: Erfahren Sie, wie Sie Ihre Azure-Umgebung für die Verwendung der integrierten Azure-Rolle „Blaupausenoperator“ konfigurieren.
ms.date: 08/27/2020
ms.topic: how-to
ms.openlocfilehash: ae881ac6d0714401f8c80e880b4d288f594b7cf3
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91535780"
---
# <a name="configure-your-environment-for-a-blueprint-operator"></a>Konfigurieren Ihrer Umgebung für einen Blaupausenoperator

Die Verwaltung der Blaupausendefinitionen und -zuweisungen kann verschiedenen Teams übertragen werden. In der Regel ist ein Architekt oder Governanceteam für die Lebenszyklusverwaltung der Blaupausendefinitionen zuständig, während ein Betriebsteam die Verwaltung der Zuweisungen dieser zentral gesteuerten Blaupausendefinitionen verantwortet.

Die integrierte Rolle **Blaupausenoperator** wurde speziell für diese Art von Szenario entwickelt. Mit der Rolle können Betriebsteams die Zuweisung von Blaupausendefinitionen der Organisation zwar verwalten, haben aber nicht die Möglichkeit, sie zu ändern. Hierfür muss Ihre Azure-Umgebung entsprechend konfiguriert werden. Die erforderlichen Schritte werden in diesem Artikel erläutert.

## <a name="grant-permission-to-the-blueprint-operator"></a>Erteilen der Berechtigung als Blaupausenoperator

Der erste Schritt besteht darin, der Konten- oder Sicherheitsgruppe, die Blaupausen zuweisen soll, die Rolle **Blaupausenoperator** zuzuweisen (empfohlen). Diese Aktion sollte auf der höchsten Ebene der Verwaltungsgruppenhierarchie erfolgen, die alle Verwaltungsgruppen und Abonnements umfasst, auf die das Betriebsteam für die Blaupausenzuweisung Zugriff haben soll. Beim Erteilen dieser Berechtigungen empfiehlt es sich, dem Prinzip des Zugriffs mit den geringsten Rechten zu folgen.

1. (Empfohlen) [Erstellen einer Sicherheitsgruppe und Hinzufügen von Mitgliedern](../../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

1. [Hinzufügen einer Rollenzuweisung](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment) des Typs **Blaupausenoperator** zur Konten- oder Sicherheitsgruppe

## <a name="user-assign-managed-identity"></a>Vom Benutzer zugewiesene verwaltete Identität

Für eine Blaupausendefinition können entweder vom System oder vom Benutzer zugewiesene verwaltete Identitäten verwendet werden. Wenn Sie jedoch die Rolle **Blaupausenoperator** verwenden, muss die Blaupausendefinition für die Verwendung einer vom Benutzer zugewiesenen verwalteten Identität konfiguriert werden. Außerdem muss der Konten- oder Sicherheitsgruppe, der die Rolle **Blaupausenoperator** zugewiesen wird, für die vom Benutzer zugewiesene verwaltete Identität die Rolle **Operator für verwaltete Identität** gewährt werden. Ohne diese Berechtigung schlagen Blaupausenzuweisungen aufgrund fehlender Berechtigungen fehl.

1. [Erstellen einer vom Benutzer zugewiesenen verwalteten Identität](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity) für die Verwendung durch eine zugewiesene Blaupause

1. Fügen Sie der Konten- oder Sicherheitsgruppe eine [Rollenzuweisung](../../../role-based-access-control/role-assignments-portal.md#add-a-role-assignment) des Typs **Operator für verwaltete Identität** hinzu. Begrenzen Sie die Rollenzuweisung auf die neue vom Benutzer zugewiesene verwaltete Identität.

1. Weisen Sie als **Blaupausenoperator** eine [Blaupause ](../create-blueprint-portal.md#assign-a-blueprint) zu, die die neue vom Benutzer zugewiesene verwaltete Identität verwendet.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../concepts/resource-locking.md) können.
- Beheben Sie Probleme bei der Blaupausenzuweisung mithilfe des [allgemeinen Leitfadens zur Problembehandlung](../troubleshoot/general.md).