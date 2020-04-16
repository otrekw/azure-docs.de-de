---
title: Ändern der Umgebung und der Materialien
description: In diesem Tutorial erfahren Sie, wie Sie die Sky-Map und Objektmaterialien in einer Unity-Szene ändern.
author: jakrams
ms.author: jakras
ms.date: 02/03/2020
ms.topic: tutorial
ms.openlocfilehash: 1354ac3cf2c6fc716ac72ae339928fa49171893e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678681"
---
# <a name="tutorial-changing-the-environment-and-materials"></a>Tutorial: Ändern der Umgebung und der Materialien

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Ändern der Umgebungszuordnung einer Szene
> * Ändern von Materialparametern
> * Laden benutzerdefinierter Texturen

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit [Tutorial: Verwenden von Remoteentitäten in Unity](working-with-remote-entities.md) vertraut sind. Sie benötigen jedoch nur ein Unity-Projekt, mit dem Sie Sitzungsverbindungen herstellen und ein Modell laden können, wie unter [Tutorial: Einrichten eines ganz neuen Unity-Projekts](project-setup.md) gezeigt.

> [!TIP]
> Der Ordner *Unity* im [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) enthält vorbereitete Unity-Projekte für alle Tutorials. Diese können als Referenz verwendet werden.

## <a name="change-the-environment-map"></a>Ändern der Umgebungszuordnung

Azure Remote Rendering unterstützt die Verwendung von [Skyboxes](../../overview/features/sky.md) (gelegentlich auch „Umgebungszuordnung“ genannt), um Umgebungslicht zu simulieren. Dies ist besonders hilfreich, wenn Ihre Objekte *[physisch basiertes Rendering](../../overview/features/pbr-materials.md)* verwenden, wie dies bei unseren Beispielmodellen der Fall ist. ARR bietet auch eine Reihe integrierter Himmelstexturen, die in diesem Tutorial verwendet werden.

Erstellen Sie ein neues Skript mit dem Namen **RemoteSky**, und fügen Sie es einem neuen Spielobjekt (GameObject) hinzu. Öffnen Sie die Skriptdatei, und ersetzen Sie sie durch folgenden Code:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteSky : MonoBehaviour
{
    private int skyIndex = 0;

    private LoadTextureFromSASParams[] builtIns =
    {
        new LoadTextureFromSASParams("builtin://Autoshop", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://BoilerRoom", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://ColorfulStudio", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Hangar", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://IndustrialPipeAndValve", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Lebombo", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SataraNight", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SunnyVondelpark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Syferfontein", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://TearsOfSteelBridge", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WhippleCreekRegionalPark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WinterRiver", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://DefaultSky", TextureType.CubeMap)
    };

    public async void ToggleSky()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        skyIndex = (skyIndex + 1) % builtIns.Length;

        var texture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(builtIns[skyIndex]).AsTask();

        Debug.Log($"Switching sky to: {builtIns[skyIndex].TextureId}");

        var settings = RemoteManagerUnity.CurrentSession.Actions.SkyReflectionSettings;

        settings.SkyReflectionTexture = texture;
    }

    private void OnGUI()
    {
        if (RemoteManagerUnity.IsConnected)
        {
            if (GUI.Button(new Rect(10, Screen.height - 50, 175, 30), "Toggle Sky"))
            {
                ToggleSky();
            }
        }
    }
}
```

Beachten Sie, dass oben die Variante `LoadTextureFromSASAsync` verwendet wird, da integrierte Texturen geladen werden. Wenn Sie Daten aus [verknüpften Blobspeichern](../../how-tos/create-an-account.md#link-storage-accounts) laden, verwenden Sie die Variante `LoadTextureAsync`. Ein modellbasiertes Beispiel hierfür finden Sie im [Abschnitt zum Laden von Modellen](../../concepts/models.md#loading-models).

Wenn Sie den Code ausführen und durch die Sky-Maps wechseln, sehen Sie, dass sich das Licht auf Ihrem Modell deutlich verändert. Der Hintergrund bleibt jedoch schwarz, und die eigentliche Himmelstextur ist nicht zu sehen. Dies ist beabsichtigt, da das Rendern eines Hintergrunds bei Verwendung eines Augmented Reality-Geräts störend wäre. In einer richtigen Anwendung sollten Himmeltexturen verwendet werden, die der echten Umgebung ähneln, da die Objekte dadurch realer wirken.

## <a name="modify-materials"></a>Ändern von Materialien

Im vorherigen Tutorial haben wir [Zustandsüberschreibungskomponenten](../../overview/features/override-hierarchical-state.md) verwendet, um die Tönungsfarbe ausgewählter Objekte zu ändern. Hier möchten wir einen ähnlichen Effekt erzielen, ändern dazu aber das [Material](../../concepts/materials.md) eines Objekts.

Als Erstes benötigen wir wie im [zweiten Tutorial](working-with-remote-entities.md) ein Skript zum Auswählen von Objekten. Falls Sie noch nicht über ein Skript vom Typ **RemoteRaycaster** verfügen, erstellen Sie es jetzt. Ersetzen Sie den Inhalt durch folgenden Code:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;

[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRaycaster : MonoBehaviour
{
    public double MaxDistance = 30.0;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        arrService = GetComponent<ARRServiceUnity>();
    }

    private async Task<Entity> RemoteRayCast(Vector3 origin, Vector3 dir)
    {
        Entity entity = null;

        var raycast = new RayCast(origin.toRemotePos(), dir.toRemoteDir(), MaxDistance, HitCollectionPolicy.ClosestHit);

        var hits = await arrService.CurrentActiveSession.Actions.RayCastQueryAsync(raycast).AsTask();

        if (hits != null)
        {
            foreach (var hit in hits)
            {
                var hitEntity = hit.HitEntity;
                if (hitEntity == null)
                {
                    continue;
                }

                entity = hitEntity;
                break;
            }
        }

        return entity;
    }

    private RemoteModelEntity focusedModel = null;

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var remoteEntity = await RemoteRayCast(origin, direction);

        if (focusedModel != null)
        {
            if (focusedModel.Entity == remoteEntity)
            {
                // picked the same object as before
                return;
            }

            ClearFocus();
        }

        if (remoteEntity == null)
        {
            // picked no object at all
            return;
        }

        // get the instance of a Unity GameObject for the ARR entity
        var entityGO = remoteEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the game object has the RemoteModelEntity component
        focusedModel = entityGO.GetComponent<RemoteModelEntity>();
        if (focusedModel == null)
        {
            focusedModel = entityGO.AddComponent<RemoteModelEntity>();
        }

        focusedModel.SetFocus(true);
    }

    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        CleanHierarchy(focusedModel.gameObject);
        focusedModel = null;
    }

    private void CleanHierarchy(GameObject focusedGO)
    {
        var sync = focusedGO?.GetComponent<RemoteEntitySyncObject>();
        if (sync == null || !sync.IsEntityValid)
        {
            return;
        }

        sync.Entity.DestroyGameObject(EntityExtensions.DestroyGameObjectFlags.DestroyEmptyParents | EntityExtensions.DestroyGameObjectFlags.KeepRemoteRoot);
    }

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
        var ray = Camera.main.ScreenPointToRay(position);

        Raycast(ray.origin, ray.direction);
    }
}
```

Fügen Sie die Komponente Ihrem Spielobjekt *RemoteRendering* hinzu. Sie dient dazu, unter dem Mauszeiger befindliche Objekte auszuwählen und den ausgewählten Objekten Komponenten vom Typ *RemoteModelEntity* hinzuzufügen. In dieser Komponentenklasse wird die eigentliche Materialänderungsfunktion implementiert.

Falls Sie noch nicht über ein Skript vom Typ **RemoteModelEntity** verfügen, erstellen Sie es, und ersetzen Sie den Inhalt durch folgenden Code:

```csharp
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

// to prevent namespace conflicts
using ARRTexture = Microsoft.Azure.RemoteRendering.Texture;

public class RemoteModelEntity : MonoBehaviour
{
    private Color HighlightColor = new Color(1.0f, 0.4f, 0.1f, 1.0f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;
    private RemoteEntitySyncObject localSyncObject = null;
    private ARRMeshComponent meshComponent = null;
    private Color4 originalColor;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();

        meshComponent = GetComponent<ARRMeshComponent>();
        if (meshComponent == null)
        {
            var mesh = localSyncObject.Entity.FindComponentOfType<MeshComponent>();
            if (mesh != null)
            {
                gameObject.BindArrComponent<ARRMeshComponent>(mesh);
                meshComponent = gameObject.GetComponent<ARRMeshComponent>();
            }
        }
        meshComponent.enabled = true;
    }

    public void SetFocus(bool on)
    {
        SetMaterialColor(on);
    }

    private void SetMaterialColor(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        // get the list of materials from the picked mesh
        // we only modify the first material; depending on how the model is authored, this may
        // work exactly as desired, or not ;-)
        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        // there are different types of materials, cast as necessary
        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
    }
}
```

Wenn Sie diesen Code ausführen, werden Objekte, auf die Sie mit der Maus zeigen, hervorgehoben. Der Effekt ähnelt dem Effekt aus dem zweiten Tutorial, wird aber auf andere Weise erreicht. In diesem Fall wird die Liste der Materialien für das ausgewählte Objekt abgerufen und anschließend die Albedofarbe des ersten Elements geändert.

> [!IMPORTANT]
> Ob bei dieser Methode die richtigen Teile eines Modells hervorgehoben werden, hängt davon ab, wie das Modell gestaltet ist. Die Methode funktioniert einwandfrei, wenn jedes Objekt genau ein Material verwendet. Bei Modellen, bei denen Teile und Materialen nicht im Verhältnis 1:1 vorliegen, führt der obige einfache Code dagegen nicht zum gewünschten Ergebnis.

## <a name="use-a-different-texture"></a>Verwenden einer anderen Textur

[Texturen](../../concepts/textures.md) sind in der Regel Teil eines Quellmodells. Im Zuge der [Modellkonvertierung](../../quickstarts/convert-model.md) werden alle Texturen in das erforderliche Runtimeformat konvertiert und in die finale Modelldatei gepackt. Wenn Sie eine Textur zur Laufzeit ersetzen möchten, müssen Sie sie im [DDS-Dateiformat](https://en.wikipedia.org/wiki/DirectDraw_Surface) speichern und in Azure Blob Storage hochladen. Eine Schnellstartanleitung zur Erstellung eines Azure-Blobcontainers finden Sie [hier](../../quickstarts/convert-model.md). Wenn Sie über einen Blobcontainer verfügen, können Sie ihn im Azure Storage-Explorer öffnen und Ihre Datei per Drag & Drop hochladen.

Eine Texturressource im Blobspeicher kann runtimeseitig auf zwei Arten adressiert werden:

* Adressieren der Textur anhand ihres SAS-URIs: Klicken Sie hierzu mit der rechten Maustaste auf die hochgeladene Datei, und wählen Sie im Kontextmenü die Option **Shared Access Signature abrufen...** aus. Verwenden Sie diesen SAS-URI mit der Funktionsvariante `LoadTextureFromSASAsync`, wie im Beispielcode weiter unten zu sehen.
* Direktes Adressieren der Textur mithilfe von Blobspeicherparametern (vorausgesetzt, der [Blobspeicher ist mit dem Konto verknüpft](../../how-tos/create-an-account.md#link-storage-accounts)): In diesem Fall ist `LoadTextureAsync` die relevante Ladefunktion.

Öffnen Sie nun das Skript **RemoteModelEntity**, fügen Sie den folgenden Code hinzu, und entfernen Sie doppelte Funktionen:

```csharp
    private string textureFile = ""; //<SAS URI for your texture>
    private ARRTexture originalTexture = null;

    private async void SetMaterialTexture(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        ARRTexture newTexture = originalTexture;

        if (on)
        {
            var textureParams = new LoadTextureFromSASParams(textureFile, TextureType.Texture2D);

            newTexture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(textureParams).AsTask();
        }

        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
    }

    public void SetFocus(bool on)
    {
        if (string.IsNullOrEmpty(textureFile))
        {
            SetMaterialColor(on);
        }
        else
        {
            SetMaterialTexture(on);
        }
    }
```

Führen Sie diesen Code aus, und zeigen Sie auf Ihr Modell. Wenn Ihr Modell über ordnungsgemäße UV-Koordinaten verfügt, sollte Ihre Textur angezeigt werden. Andernfalls ist möglicherweise nur eine Farbänderung zu sehen.

## <a name="next-steps"></a>Nächste Schritte

Damit ist unsere Einführungsreihe zur Verwendung von Azure Remote Rendering mit Unity abgeschlossen. Machen Sie sich als Nächstes am besten mit einigen grundlegenden ARR-Konzepten wie [Sitzungen](../../concepts/sessions.md), [Entitäten](../../concepts/entities.md) und [Modellen](../../concepts/models.md) vertraut. Darüber hinaus gibt es noch verschiedene Features wie [Lichter](../../overview/features/lights.md), [Konturenrendering](../../overview/features/outlines.md), [hierarchische Zustandsüberschreibungen](../../overview/features/override-hierarchical-state.md) und [Materialen](../../concepts/materials.md), mit denen Sie sich eingehender beschäftigen sollten.

> [!div class="nextstepaction"]
> [Interagieren mit Unity-Spielobjekten und -Komponenten](../../how-tos/unity/objects-components.md)
