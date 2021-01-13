---
ms.openlocfilehash: 7219a457a2631f9ff6beee06eff34bce0ff5a23f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67177793"
---
Wenn ein Azure AD-Token nicht festgelegt ist, müssen Sie wie bei Zugriffstoken das TokenRequired-Ereignis behandeln oder die tokenRequired-Methode auf dem Delegatenprotokoll implementieren.

Sie können das Ereignis synchron behandeln, indem Sie die Eigenschaft für die Ereignisargumente festlegen.
