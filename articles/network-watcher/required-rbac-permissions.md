---
title: Erforderliche RBAC-Berechtigungen (Role-Based Access Control) für die Verwendung von Funktionen
titleSuffix: Azure Network Watcher
description: In diesem Artikel erfahren Sie, welche Azure RBAC-Berechtigungen (Role-Based Access Control) zum Arbeiten mit Network Watcher-Funktionen erforderlich sind.
services: network-watcher
documentationcenter: ''
author: damendo
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: damendo
ms.openlocfilehash: f8743f19d6cd262ad140659be55a4fc57e842564
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840552"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Erforderliche RBAC-Berechtigungen (Role-Based Access Control) für die Verwendung von Network Watcher-Funktionen

Durch Azure RBAC-Berechtigungen (Role-Based Access Control) können Sie Mitgliedern Ihrer Organisation nur bestimmte Aktionen zuweisen, die sie benötigen, um die ihnen übertragenen Aufgaben durchzuführen. Um die Network Watcher-Funktionen verwenden zu können, muss das Konto, mit dem Sie sich bei Azure anmelden, den internen Rollen [Besitzer](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Mitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor) oder [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) zugewiesen werden, die den Aktionen für die einzelnen Network Watcher-Funktionen in den folgenden Abschnitten zugeordnet sind. Weitere Informationen zu Network Watcher-Funktionen finden Sie unter [Was ist Azure Network Watcher?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Network Watcher

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Network Watcher abrufen                                          |
| Microsoft.Network/networkWatchers/write                             | Network Watcher erstellen oder aktualisieren                             |
| Microsoft.Network/networkWatchers/delete                            | Network Watcher löschen                                       |

## <a name="nsg-flow-logs"></a>NSG-Flussprotokolle

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Ein Datenflussprotokoll konfigurieren                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Status für ein Datenflussprotokoll abfragen                                    |

## <a name="connection-troubleshoot"></a>Problembehandlung für Verbindungen

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectivityCheck/action          | Initiieren eines Tests zur Problembehandlung für Verbindungen
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Ergebnisse von einem Test zur Problembehandlung für Verbindungen abfragen                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Einen Test zur Problembehandlung für Verbindungen ausführen                             |

## <a name="connection-monitor"></a>Verbindungsmonitor

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Verbindungsmonitor starten                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Verbindungsmonitor anhalten                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Verbindungsmonitor abfragen                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Verbindungsmonitor abrufen                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Erstellen eines Verbindungsmonitors                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Verbindungsmonitor löschen                                    |

## <a name="packet-capture"></a>Paketerfassung

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Status einer Paketerfassung abfragen                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Beenden einer Paketerfassung                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Abrufen einer Paketerfassung                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Eine Paketerfassung erstellen                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Löschen einer Paketerfassung                                        |

## <a name="ip-flow-verify"></a>IP-Datenflussüberprüfung

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Einen IP-Fluss überprüfen                                              |

## <a name="next-hop"></a>Nächster Hop

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Nächsten Hop von einer VM abrufen                                     |

## <a name="network-security-group-view"></a>Netzwerksicherheitsgruppen-Ansicht

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Sicherheitsgruppen anzeigen                                           |

## <a name="topology"></a>Topologie

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topologie abrufen                                                   |

## <a name="reachability-report"></a>Erreichbarkeitsbericht

| Aktion                                                              | Beschreibung                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Einen Azure-Erreichbarkeitsbericht abrufen                               |


## <a name="additional-actions"></a>Zusätzliche Aktionen

Network Watcher-Funktionen erfordern folgende Aktionen:

| Aktion(en)                                                           | Beschreibung                                                    |
| ---------                                                           | -------------                                                  |
| Microsoft.Authorization/\*/Read                                     | Wird zum Abrufen von RBAC-Rollenzuweisungen und Richtliniendefinitionen verwendet.          |
| Microsoft.Resources/subscriptions/resourceGroups/Read               | Wird zum Auflisten aller Ressourcengruppen eines Abonnements verwendet.    |
| Microsoft.Storage/storageAccounts/Read                              | Wird verwendet, um die Eigenschaften für das angegebene Speicherkonto abzurufen.   |
| Microsoft.Storage/storageAccounts/listServiceSas/Action, </br> Microsoft.Storage/storageAccounts/listAccountSas/Action, <br> Microsoft.Storage/storageAccounts/listKeys/Action| Wird zum Abrufen von Shared Access Signatures (SAS) verwendet, die den [sicheren Zugriff auf das Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-sas-overview) und das Schreiben in das Speicherkonto ermöglichen. |
| Microsoft.Compute/virtualMachines/Read, </br> Microsoft.Compute/virtualMachines/Write| Wird für das Anmelden an der VM, Durchführen einer Paketerfassung und Hochladen in das Speicherkonto verwendet.|
| Microsoft.Compute/virtualMachines/extensions/Read </br> Microsoft.Compute/virtualMachines/extensions/Write| Wird verwendet, um das Vorhandensein der Network Watcher-Erweiterung zu überprüfen und bei Bedarf die Installation durchzuführen. |
| Microsoft.Compute/virtualMachineScaleSets/Read, </br> Microsoft.Compute/virtualMachineScaleSets/Write| Wird zum Zugreifen auf VM-Skalierungsgruppen, Durchführen von Paketerfassungen und Hochladen in ein Speicherkonto verwendet.|
| Microsoft.Compute/virtualMachineScaleSets/extensions/Read, </br> Microsoft.Compute/virtualMachineScaleSets/extensions/Write| Wird verwendet, um das Vorhandensein der Network Watcher-Erweiterung zu überprüfen und bei Bedarf die Installation durchzuführen. |
| Microsoft.Insights/alertRules/*                                     | Wird zum Einrichten von Metrikwarnungen verwendet.                                     |
| Microsoft.Support/*                                                 | Wird verwendet, um Supporttickets über Network Watcher zu erstellen und zu aktualisieren. |
