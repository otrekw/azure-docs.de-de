---
title: Referenz zu Azure Sentinel-UEBA-Anreicherungen | Microsoft-Dokumentation
description: In diesem Artikel werden die Entitätsanreicherungen gezeigt, die von der Entitätsverhaltensanalyse von Azure Sentinel generiert werden.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 01/04/2021
ms.author: yelevin
ms.openlocfilehash: daba8fc1f645b51dc8668c806be63744b6ae0842
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901613"
---
# <a name="azure-sentinel-ueba-enrichments-reference"></a>Referenz zu Azure Sentinel-UEBA-Anreicherungen

In diesen Tabellen werden Entitätsanreicherungen aufgeführt und beschrieben, die verwendet werden können, um die Untersuchung von Sicherheitsvorfällen zu fokussieren und zu vertiefen.

Die ersten beiden Tabellen, **Benutzererkenntnisse** und **Geräteerkenntnisse**, enthalten Entitätsinformationen aus Active Directory/Azure AD- und Microsoft Threat Intelligence-Quellen.

<a name="baseline-explained"></a>Die übrigen Tabellen unter **Aktivitätserkenntnissetabelle** enthalten Entitätsinformationen, die auf den aus der Entitätsverhaltensanalyse von Azure Sentinel resultierenden Verhaltensprofilen basieren. Die Aktivitäten werden anhand einer Baseline analysiert, die bei jeder Verwendung dynamisch kompiliert wird. Jede Aktivität verfügt über ihren eigenen definierten Rückblickzeitraum, von dem diese dynamische Baseline abgeleitet ist. Dieser Zeitraum wird in der Spalte [**Baseline**](#activity-insights-tables) in dieser Tabelle angegeben.

> [!NOTE] 
> In allen drei Tabellen werden im Feld **Anreicherungsname** zwei Zeilen mit Informationen angezeigt. Die erste in **Fett** ist der „Anzeigename“ der Anreicherung. Der zweite *(kursiv und in Klammern)* ist der Feldname der Anreicherung, der in der [**Behavior Analytics-Tabelle**](identify-threats-with-entity-behavior-analytics.md#data-schema) gespeichert ist.

## <a name="user-insights-table"></a>Benutzererkenntnissetabelle

| Anreicherungsname | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Anzeigename des Kontos**<br>*(AccountDisplayName)* | Der Kontoanzeigename des Benutzers. | Admin, Hayden Cook |
| **Kontodomäne**<br>*(AccountDomain)* | Der Kontodomänenname des Benutzers. |  |
| **Kontoobjekt-ID**<br>*(AccountObjectID)* | Die Kontoobjektobjekt-ID des Benutzers. | a58df659-5cab-446c-9dd0-5a3af20ce1c2 |
| **Auswirkungsgrad**<br>*(BlastRadius)* | Der Auswirkungsgrad wird anhand verschiedener Faktoren berechnet: der Position des Benutzers in der Organisationsstruktur und der Azure Active Directory-Rollen und -Berechtigungen des Benutzers. | Niedrig, Mittel, Hoch |
| **Konto im Ruhezustand**<br>*(IsDormantAccount)* | Das Konto wurde für die letzten 180 Tage nicht verwendet. | TRUE, FALSE |
| **Lokaler Administrator**<br>*(IsLocalAdmin)* | Das Konto verfügt über lokale Administratorberechtigungen. | TRUE, FALSE |
| **Neues Konto**<br>*(IsNewAccount)* | Das Konto wurde innerhalb der letzten 30 Tage erstellt. | TRUE, FALSE |
| **Lokale SID**<br>*(OnPremisesSID)* | Die lokale SID des Benutzers, der mit der Aktion verknüpft ist. | S-1-5-21-1112946627-1321165628-2437342228-1103 |
|

## <a name="device-insights-table"></a>Geräteerkenntnissetabelle

| Anreicherungsname | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Browser**<br>*(Browser)* | Der in der Aktion verwendete Browser. | Microsoft Edge, Chrome |
| **Gerätefamilie**<br>*(DeviceFamily)* | Die in der Aktion verwendete Gerätefamilie. | Windows |
| **Gerätetyp**<br>*(DeviceType)* | Der in der Aktion verwendete Clientgerätetyp. | Desktop |
| **ISP**<br>*(ISP)* | Der in der Aktion verwendete Internetdienstanbieter. |  |
| **Betriebssystem**<br>*(OperatingSystem)* | Das in der Aktion verwendete Betriebssystem. | Windows 10 |
| **Threat Intelligence-Indikatorbeschreibung**<br>*(ThreatIntelIndicatorDescription)* | Beschreibung des beobachteten Bedrohungsindikators, der aus der in der Aktion verwendeten IP-Adresse aufgelöst wurde. | Host ist Mitglied von Botnet: azorult |
| **Threat Intelligence-Indikatortyp**<br>*(ThreatIntelIndicatorType)* | Der Typ des Bedrohungsindikators, der aus der in der Aktion verwendeten IP-Adresse aufgelöst wurde. | Botnet, C2, CryptoMining, Darknet, Ddos, MaliciousUrl, Malware, Phishing, Proxy, PUA, Watchlist |
| **Benutzer-Agent**<br>*(UserAgent)* | Der in der Aktion verwendete Benutzer-Agent. | Microsoft Azure Graph Client Library 1.0,<br>Swagger-Codegen/1.4.0.0/csharp,<br>EvoSTS |
| **Benutzer-Agent-Familie**<br>*(UserAgentFamily)* | Die in der Aktion verwendete Benutzer-Agent-Familie. | Chrome, Microsoft Edge, Firefox |
|

## <a name="activity-insights-tables"></a>Aktivitätserkenntnissetabelle

#### <a name="action-performed"></a>Ausgeführte Aktion

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Zum ersten Mal vom Benutzer ausgeführte Aktion**<br>*(FirstTimeUserPerformedAction)* | 180 | Die Aktion wurde zum ersten Mal vom Benutzer ausgeführt. | TRUE, FALSE |
| **Ungewöhnlicherweise vom Benutzer ausgeführte Aktion**<br>*(ActionUncommonlyPerformedByUser)* | 10 | Die Aktion wird vom Benutzer üblicherweise nicht ausgeführt. | TRUE, FALSE |
| **Ungewöhnlicherweise unter Peers ausgeführte Aktion**<br>*(ActionUncommonlyPerformedAmongPeers)* | 180 | Die Aktion wird üblicherweise nicht unter Peers des Benutzers ausgeführt. | TRUE, FALSE |
| **Zum ersten Mal im Mandanten ausgeführte Aktion**<br>*(FirstTimeActionPerformedInTenant)* | 180 | Die Aktion wurde zum ersten Mal von jemand in der Organisation ausgeführt. | TRUE, FALSE |
| **Ungewöhnlicherweise im Mandanten ausgeführte Aktion**<br>*(ActionUncommonlyPerformedInTenant)* | 180 | Die Aktion wird üblicherweise nicht in der Organisation ausgeführt. | TRUE, FALSE |
|

#### <a name="app-used"></a>Verwendete App

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Zum ersten Mal vom Benutzer verwendete App**<br>*(FirstTimeUserUsedApp)* | 180 | Die App wurde zum ersten Mal vom Benutzer verwendet. | TRUE, FALSE |
| **Üblicherweise nicht vom Benutzer verwendete App**<br>*(AppUncommonlyUsedByUser)* | 10 | Die App wird vom Benutzer üblicherweise nicht verwendet. | TRUE, FALSE |
| **Üblicherweise nicht unter Peers verwendete App**<br>*(AppUncommonlyUsedAmongPeers)* | 180 | Die App wird in der Regel nicht unter Peers des Benutzers verwendet. | TRUE, FALSE |
| **Zum ersten Mal im Mandanten beobachtete App**<br>*(FirstTimeAppObservedInTenant)* | 180 | Die App wurde zum ersten Mal in der Organisation beobachtet. | TRUE, FALSE |
| **Üblicherweise nicht im Mandanten verwendete App**<br>*(AppUncommonlyUsedInTenant)* | 180 | Die App wird in der Organisation nicht häufig verwendet. | TRUE, FALSE |
| 

#### <a name="browser-used"></a>Verwendeter Browser

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Erstmalige Verbindung des Benutzers über den Browser**<br>*(FirstTimeUserConnectedViaBrowser)* | 30 | Der Browser wurde zum ersten Mal vom Benutzer verwendet. | TRUE, FALSE |
| **Üblicherweise nicht vom Benutzer verwendeter Browser**<br>*(BrowserUncommonlyUsedByUser)* | 10 | Der Browser wird vom Benutzer üblicherweise nicht verwendet. | TRUE, FALSE |
| **Üblicherweise nicht unter Peers verwendeter Browser**<br>*(BrowserUncommonlyUsedAmongPeers)* | 30 | Der Browser wird in der Regel nicht unter Peers des Benutzers verwendet. | TRUE, FALSE |
| **Zum ersten Mal im Mandanten beobachteter Browser**<br>*(FirstTimeBrowserObservedInTenant)* | 30 | Der Browser wurde zum ersten Mal in der Organisation beobachtet. | TRUE, FALSE |
| **Üblicherweise nicht im Mandanten verwendeter Browser**<br>*(BrowserUncommonlyUsedInTenant)* | 30 | Der Browser wird in der Organisation nicht häufig verwendet. | TRUE, FALSE |
| 

#### <a name="country-connected-from"></a>Land, von dem aus die Verbindung hergestellt wurde

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Erstmalige Verbindung des Benutzers von diesem Land aus**<br>*(FirstTimeUserConnectedFromCountry)* | 90 | Von diesem geografischen Standort laut Auflösung der IP-Adresse aus wurde vom Benutzer zum ersten Mal eine Verbindung hergestellt. | TRUE, FALSE |
| **Land, von dem aus der Benutzer üblicherweise keine Verbindung herstellt**<br>*(CountryUncommonlyConnectedFromByUser)* | 10 | Von diesem geografischen Standort laut Auflösung der IP-Adresse aus wird vom Benutzer üblicherweise keine Verbindung hergestellt. | TRUE, FALSE |
| **Land, von dem aus üblicherweise keine Verbindung unter Peers hergestellt wird**<br>*(CountryUncommonlyConnectedFromAmongPeers)* | 90 | Von diesem geografischen Standort laut Auflösung der IP-Adresse aus wird üblicherweise keine Verbindung unter Peers hergestellt. | TRUE, FALSE |
| **Zum ersten Mal im Mandanten beobachtete Verbindung von diesem Land aus**<br>*(FirstTimeConnectionFromCountryObservedInTenant)* | 90 | Zum ersten Mal wurde von jemandem in der Organisation eine Verbindung von diesem Land aus hergestellt. | TRUE, FALSE |
| **Land, von dem aus im Mandanten üblicherweise keine Verbindung hergestellt wird**<br>*(CountryUncommonlyConnectedFromInTenant)* | 90 | Von diesem geografischen Standort laut Auflösung der IP-Adresse aus wird von der Organisation üblicherweise keine Verbindung hergestellt. | TRUE, FALSE |
| 

#### <a name="device-used-to-connect"></a>Zum Herstellen der Verbindung verwendetes Gerät

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Erstmalige Verbindung des Benutzers von diesem Gerät aus**<br>*(FirstTimeUserConnectedFromDevice)* | 30 | Vom Quellgerät aus wurde vom Benutzer zum ersten Mal eine Verbindung hergestellt. | TRUE, FALSE |
| **Üblicherweise nicht vom Benutzer verwendetes Gerät**<br>*(DeviceUncommonlyUsedByUser)* | 10 | Das Gerät wird vom Benutzer üblicherweise nicht verwendet. | TRUE, FALSE |
| **Üblicherweise nicht unter Peers verwendetes Gerät**<br>*(DeviceUncommonlyUsedAmongPeers)* | 180 | Das Gerät wird in der Regel nicht unter Peers des Benutzers verwendet. | TRUE, FALSE |
| **Zum ersten Mal im Mandanten beobachtetes Gerät**<br>*(FirstTimeDeviceObservedInTenant)* | 30 | Das Gerät wurde zum ersten Mal in der Organisation beobachtet. | TRUE, FALSE |
| **Üblicherweise nicht im Mandanten verwendetes Gerät**<br>*(DeviceUncommonlyUsedInTenant)* | 180 | Das Gerät wird in der Organisation nicht häufig verwendet. | TRUE, FALSE |
| 

#### <a name="other-device-related"></a>Sonstige gerätebezogene Aspekte

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Der Benutzer hat sich zum ersten Mal beim Gerät angemeldet**<br>*(FirstTimeUserLoggedOnToDevice)* | 180 | Der Benutzer hat zum ersten Mal eine Verbindung mit dem Zielgerät hergestellt. | TRUE, FALSE |
| **Üblicherweise nicht im Mandanten verwendete Gerätefamilie**<br>*(DeviceFamilyUncommonlyUsedInTenant)* | 30 | Die Gerätefamilie wird in der Organisation nicht häufig verwendet. | TRUE, FALSE |
| 

#### <a name="internet-service-provider-used-to-connect"></a>Zum Herstellen der Verbindung verwendeter Internetdienstanbieter (ISP)

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Erstmalige Verbindung des Benutzers über den ISP**<br>*(FirstTimeUserConnectedViaISP)* | 30 | Der ISP wurde zum ersten Mal vom Benutzer verwendet. | TRUE, FALSE |
| **Üblicherweise nicht vom Benutzer verwendeter ISP**<br>*(ISPUncommonlyUsedByUser)* | 10 | Der ISP wird vom Benutzer üblicherweise nicht verwendet. | TRUE, FALSE |
| **Üblicherweise nicht unter Peers verwendeter ISP**<br>*(ISPUncommonlyUsedAmongPeers)* | 30 | Der ISP wird in der Regel nicht unter Peers des Benutzers verwendet. | TRUE, FALSE |
| **Erstmalige Verbindungsherstellung über den ISP im Mandanten**<br>*(FirstTimeConnectionViaISPInTenant)* | 30 | Der ISP wurde zum ersten Mal in der Organisation beobachtet. | TRUE, FALSE |
| **Üblicherweise nicht im Mandanten verwendeter ISP**<br>*(ISPUncommonlyUsedInTenant)* | 30 | Der ISP wird in der Organisation nicht häufig verwendet. | TRUE, FALSE |
| 

#### <a name="resource-accessed"></a>Aufgerufene Ressource

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Erstmaliger Zugriff des Benutzers auf die Ressource**<br>*(FirstTimeUserAccessedResource)* | 180 | Der Benutzer hat zum ersten Mal auf die Ressource zugegriffen. | TRUE, FALSE |
| **Ressource, auf die vom Benutzer nicht häufig zugegriffen wird**<br>*(ResourceUncommonlyAccessedByUser)* | 10 | Der Benutzer greift in der Regel nicht auf die Ressource zu. | TRUE, FALSE |
| **Ressource, auf die unter Peers nicht häufig zugegriffen wird**<br>*(ResourceUncommonlyAccessedAmongPeers)* | 180 | Unter Peers des Benutzers wird in der Regel nicht auf die Ressource zugegriffen. | TRUE, FALSE |
| **Erstmaliger Zugriff auf Ressource im Mandanten**<br>*(FirstTimeResourceAccessedInTenant)* | 180 | Auf die Ressource wurde zum ersten Mal von jemand in der Organisation zugegriffen. | TRUE, FALSE |
| **Ressource, auf die im Mandanten nicht häufig zugegriffen wird**<br>*(ResourceUncommonlyAccessedInTenant)* | 180 | Auf die Ressource wird in der Organisation in der Regel nicht zugegriffen. | TRUE, FALSE |
| 

#### <a name="miscellaneous"></a>Verschiedenes

| Anreicherungsname | [Baseline](#baseline-explained) (Tage) | BESCHREIBUNG | Beispielwert |
| --- | --- | --- | --- |
| **Beim letzten Mal vom Benutzer ausgeführte Aktion**<br>*(LastTimeUserPerformedAction)* | 180 | Beim letzten Mal hat der Benutzer die gleiche Aktion ausgeführt. | <Timestamp> |
| **Eine ähnliche Aktion wurde in der Vergangenheit nicht ausgeführt.**<br>*(SimilarActionWasn'tPerformedInThePast)* | 30 | Keine Aktion wurde vom Benutzer im gleichen Ressourcenanbieter durchgeführt. | TRUE, FALSE |
| **Quell-IP-Standort**<br>*(SourceIPLocation)* | *N/V* | Das aus der Quell-IP der Aktion aufgelöste Land. | [Surrey, England] |
| **Ungewöhnlich hohe Anzahl an Vorgängen**<br>*(UncommonHighVolumeOfOperations)* | 7 | Ein Benutzer hat einen Burst ähnlicher Vorgänge innerhalb desselben Anbieters durchgeführt. | TRUE, FALSE |
| **Ungewöhnliche Anzahl von „Bedingter Zugriff“-Azure AD-Fehlern**<br>*(UnusualNumberOfAADConditionalAccessFailures)* | 5 | Eine ungewöhnliche Anzahl von Benutzern konnte sich aufgrund des bedingten Zugriffs nicht authentifizieren. | TRUE, FALSE |
| **Ungewöhnliche Anzahl von Geräten wurde hinzugefügt**<br>*(UnusualNumberOfDevicesAdded)* | 5 | Ein Benutzer hat eine ungewöhnliche Anzahl von Geräten hinzugefügt. | TRUE, FALSE |
| **Ungewöhnliche Anzahl von Geräten wurde gelöscht**<br>*(UnusualNumberOfDevicesDeleted)* | 5 | Ein Benutzer hat eine ungewöhnliche Anzahl von Geräten gelöscht. | TRUE, FALSE |
| **Ungewöhnliche Anzahl von Benutzern wurde der Gruppe hinzugefügt**<br>*(UnusualNumberOfUsersAddedToGroup)* | 5 | Ein Benutzer hat eine ungewöhnliche Anzahl von Benutzern einer Gruppe hinzugefügt. | TRUE, FALSE |
|