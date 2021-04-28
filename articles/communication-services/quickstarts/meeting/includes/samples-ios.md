---
title: Verwenden von Azure Communication Services Teams Embed für iOS
description: In dieser Übersicht erfahren Sie, wie Sie die Azure Communication Services-Bibliothek zum Einbetten von Teams für iOS verwenden.
author: palatter
ms.author: palatter
ms.date: 24/02/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 0a1dd8f69cb79e42e56ab44981820e31abf204e1
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104803054"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Eine bereitgestellte Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein `User Access Token`, um den Anrufclient zu aktivieren. Weitere Informationen zum [Abrufen eines `User Access Token`](../../access-tokens.md)
- Gehen Sie den Schnellstart [Erste Schritte beim Hinzufügen von Teams Embed zu Ihrer Anwendung](../getting-started-with-teams-embed.md) durch.

## <a name="teams-embed-events"></a>Teams Embed-Ereignisse

Fügen Sie Ihrer Klasse den `MeetingUIClientDelegate` hinzu.

```swift
class ViewController: UIViewController, MeetingUIClientDelegate {

    private var meetingClient: MeetingUIClient?
```

Legen Sie `meetingUIClientDelegate` auf `self` fest.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    meetingClient?.meetingUIClientDelegate = self
}
```

Implementieren Sie die Funktionen `didUpdateCallState` und `didUpdateRemoteParticipantCount`.

```swift
    func meetingUIClient(didUpdateCallState callState: CallState) {
        switch callState {
        case .connecting:
            print("Call state has changed to 'Connecting'")
        case .connected:
            print("Call state has changed to 'Connected'")
        case .waitingInLobby:
            print("Call state has changed to 'Waiting in Lobby'")
        case .ended:
            print("Call state has changed to 'Ended'")
        }
    }
    
    func meetingUIClient(didUpdateRemoteParticipantCount remoteParticpantCount: UInt) {
        print("Remote participant count has changed to: \(remoteParticpantCount)")
    }
```

## <a name="assigning-avatars-for-users"></a>Zuweisen von Avataren für Benutzer

Fügen Sie Ihrer Klasse den `MeetingUIClientIdentityProviderDelegate` hinzu.

```swift
class ViewController: UIViewController, MeetingUIClientIdentityProviderDelegate {

    private var meetingClient: MeetingUIClient?
```

Legen Sie den `MeetingUIClientIdentityProviderDelegate` auf `self` fest, bevor Sie an der Besprechung teilnehmen.

```swift
private func joinMeeting() {
    meetingClient?.meetingUIClientIdentityProviderDelegate = self
    let meetingJoinOptions = MeetingJoinOptions(displayName: "John Smith")

    meetingClient?.join(meetingUrl: "<MEETING_URL>", meetingJoinOptions: meetingJoinOptions, completionHandler: { (error: Error?) in
        if (error != nil) {
            print("Join meeting failed: \(error!)")
        }
    })
}
```

Ordnen Sie jeden `userMri` dem entsprechenden Avatar zu.

```swift
    func avatarFor(userIdentifier: String, completionHandler: @escaping (UIImage?) -> Void) {
        if (userIdentifier.starts(with: "8:teamsvisitor:")) {
            // Anonymous teams user will start with prefix 8:teamsvistor:
            let image = UIImage (named: "avatarPink")
            completionHandler(image!)
        }
        else if (userIdentifier.starts(with: "8:orgid:")) {
            // OrgID user will start with prefix 8:orgid:
            let image = UIImage (named: "avatarDoctor")
            completionHandler(image!)
        }
        else if (userIdentifier.starts(with: "8:acs:")) {
            // ACS user will start with prefix 8:acs:
            let image = UIImage (named: "avatarGreen")
            completionHandler(image!)
        }
        else {
            completionHandler(nil)
        }
}
```