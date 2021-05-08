---
author: baanders
description: Umfasst Datei, die die mandantenübergreifende Einschränkung mit Azure Digital Twins beschreibt
ms.service: digital-twins
ms.topic: include
ms.date: 4/13/2021
ms.author: baanders
ms.openlocfilehash: 16684f8c5947f8b6a09a9a785968dabf3e644c18
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589338"
---
Daher erfordern Anforderungen an die Azure Digital Twins-APIs einen Benutzer oder Dienstprinzipal, der Teil desselben Mandanten ist, in dem sich die Azure Digital Twins-Instanz befindet. Um böswillige Überprüfungen von Azure Digital Twins-Endpunkten zu verhindern, wird für Anforderungen mit Zugriffstoken von außerhalb des ursprünglichen Mandanten die Fehlermeldung „404 Sub-Domain nicht gefunden“ zurückgegeben. Dieser Fehler wird *auch dann* zurückgegeben, wenn dem Benutzer oder Dienstprinzipal über[Azure AD B2B](../articles/active-directory/external-identities/what-is-b2b.md)-Zusammenarbeit eine Azure Digital Twins [Rolle](../articles/digital-twins/concepts-security.md) „Datenbesitzer“ oder „Azure Digital Twins Datenleser“ erteilt wurde. 