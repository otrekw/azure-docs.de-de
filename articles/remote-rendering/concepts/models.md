---
title: Modelle
description: Enthält eine Beschreibung der Bedeutung von Modellen in Azure Remote Rendering.
author: jakrams
ms.author: jakras
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: e9c8c4a1209b8bb5be7af87ef22aeab0ffd90b79
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90023770"
---
# <a name="models"></a>Modelle

Ein *Modell* ist in Azure Remote Rendering eine vollständige Objektdarstellung, die aus [Entitäten](entities.md) und [Komponenten](components.md) besteht. Modelle stellen das Hauptverfahren dar, mit dem benutzerdefinierte Daten in den Remote Rendering-Dienst eingebunden werden.

## <a name="model-structure"></a>Modellstruktur

Ein Modell verfügt auf dem Stammknoten über genau eine [Entität](entities.md). Hierunter kann sich eine beliebige Hierarchie mit untergeordneten Entitäten befinden. Beim Laden eines Modells wird ein Verweis auf diese Stammentität zurückgegeben.

An jede Entität können [Komponenten](components.md) angefügt werden. In den meisten Fällen verfügen die Entitäten über *MeshComponents*, die auf [Gittermodellressourcen](meshes.md) verweisen.

## <a name="creating-models"></a>Erstellen von Modellen

Zur Erstellung von Modellen für die Runtime werden aus Dateiformaten wie FBX und GLTF [Eingabemodelle konvertiert](../how-tos/conversion/model-conversion.md). Beim Konvertierungsprozess werden alle Ressourcen extrahiert, z. B. Texturen, Materialien und Gittermodelle, und in optimierte Runtimeformate konvertiert. Außerdem werden die strukturellen Informationen extrahiert und in die Graphstruktur der Entitäten bzw. Komponenten von ARR konvertiert.

> [!IMPORTANT]
> Die [Modellkonvertierung](../how-tos/conversion/model-conversion.md) ist die einzige Möglichkeit, [Gittermodelle](meshes.md) zu erstellen. Gittermodelle können zur Runtime von Entitäten zwar gemeinsam genutzt werden, aber ein Gittermodell kann nur in die Runtime eingebunden werden, indem ein Modell geladen wird.

## <a name="loading-models"></a>Laden von Modellen

Nachdem ein Modell konvertiert wurde, kann es aus Azure-Blobspeicher in die Runtime geladen werden.

Es gibt zwei unterschiedliche Ladefunktionen, die sich danach unterscheiden, wie die Ressource im Blobspeicher hinterlegt ist:

* Auf das Modell kann über den SAS-URI zugegriffen werden. Die relevante Ladefunktion ist `LoadModelFromSASAsync` mit dem Parameter `LoadModelFromSASParams`. Nutzen Sie diese Variante auch beim Laden von [integrierten Modellen](../samples/sample-model.md).
* Auf das Modell kann direkt über Blobspeicherparameter zugegriffen werden (vorausgesetzt, der [Blobspeicher ist mit dem Konto verknüpft](../how-tos/create-an-account.md#link-storage-accounts)). In diesem Fall ist `LoadModelAsync` mit dem Parameter `LoadModelParams` die relevante Ladefunktion.

Die folgenden Codeausschnitte veranschaulichen das Laden von Modellen mit den Funktionen. Verwenden Sie beispielsweise folgenden Code, um ein Modell mit dem SAS-URI zu laden:

```csharp
async void LoadModel(AzureSession session, Entity modelParent, string modelUri)
{
    // load a model that will be parented to modelParent
    var modelParams = new LoadModelFromSASParams(modelUri, modelParent);

    var loadOp = session.Actions.LoadModelFromSASAsync(modelParams);

    loadOp.ProgressUpdated += (float progress) =>
    {
        Debug.Log($"Loading: {progress * 100.0f}%");
    };

    await loadOp.AsTask();
}
```

```cpp
ApiHandle<LoadModelAsync> LoadModel(ApiHandle<AzureSession> session, ApiHandle<Entity> modelParent, std::string modelUri)
{
    LoadModelFromSASParams modelParams;
    modelParams.ModelUrl = modelUri;
    modelParams.Parent = modelParent;

    ApiHandle<LoadModelAsync> loadOp = *session->Actions()->LoadModelFromSASAsync(modelParams);

    loadOp->Completed([](const ApiHandle<LoadModelAsync>& async)
    {
        printf("Loading: finished.");
    });
    loadOp->ProgressUpdated([](float progress)
    {
        printf("Loading: %.1f%%", progress*100.f);
    });

    return loadOp;
}
```

Verwenden Sie Code wie im folgenden Codeausschnitt, wenn Sie ein Modell laden möchten, indem Sie direkt die Blobspeicherparameter verwenden:

```csharp
async void LoadModel(AzureSession session, Entity modelParent, string storageAccount, string containerName, string assetFilePath)
{
    // load a model that will be parented to modelParent
    var modelParams = new LoadModelParams(
        storageAccount, // storage account name + '.blob.core.windows.net', e.g., 'mystorageaccount.blob.core.windows.net'
        containerName,  // name of the container in your storage account, e.g., 'mytestcontainer'
        assetFilePath,  // the file path to the asset within the container, e.g., 'path/to/file/myAsset.arrAsset'
        modelParent
    );

    var loadOp = session.Actions.LoadModelAsync(modelParams);

    // ... (identical to the SAS URI snippet above)
}
```

```cpp
ApiHandle<LoadModelAsync> LoadModel(ApiHandle<AzureSession> session, ApiHandle<Entity> modelParent, std::string storageAccount, std::string containerName, std::string assetFilePath)
{
    LoadModelParams modelParams;
    modelParams.Parent = modelParent;
    modelParams.Blob.StorageAccountName = std::move(storageAccount);
    modelParams.Blob.BlobContainerName = std::move(containerName);
    modelParams.Blob.AssetPath = std::move(assetFilePath);

    ApiHandle<LoadModelAsync> loadOp = *session->Actions()->LoadModelAsync(modelParams);
    // ... (identical to the SAS URI snippet above)
}
```

Anschließend können Sie die Entitätshierarchie durchlaufen und die Entitäten und Komponenten ändern. Beim mehrfachen Laden desselben Modells werden mehrere Instanzen erstellt, die jeweils über eine eigene Kopie der Struktur mit Entitäten bzw. Komponenten verfügen. Da es sich bei Gittermodellen, Materialien und Texturen um [gemeinsam verwendete Ressourcen](../concepts/lifetime.md) handelt, werden die entsprechenden Daten aber nicht noch einmal geladen. Aus diesem Grund fällt für das mehrmalige Instanziieren eines Modells relativ wenig Mehraufwand in Bezug auf den Arbeitsspeicher an.

> [!CAUTION]
> Von allen *Async*-Funktionen in ARR werden asynchrone Vorgangsobjekte zurückgegeben. Sie müssen einen Verweis auf diese Objekte speichern, bis der Vorgang abgeschlossen ist. Andernfalls wird der Vorgang vom C# Garbage Collector unter Umständen zu einem frühen Zeitpunkt gelöscht und kann nicht beendet werden. Im obigen Beispielcode wird mit *await* sichergestellt, dass die lokale Variable „loadOp“ einen Verweis enthält, bis das Laden von Modellen abgeschlossen ist. Falls Sie stattdessen das Element *Completed* verwenden, müssen Sie den asynchronen Vorgang aber in einer Membervariablen speichern.

## <a name="api-documentation"></a>API-Dokumentation

* [C# RemoteManager.LoadModelAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadmodelasync)
* [C# RemoteManager.LoadModelFromSASAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadmodelfromsasasync)
* [C++ RemoteManager::LoadModelAsync()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadmodelasync)
* [C++ RemoteManager::LoadModelFromSASAsync()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadmodelfromsasasync)

## <a name="next-steps"></a>Nächste Schritte

* [Entitäten](entities.md)
* [Gittermodelle](meshes.md)
