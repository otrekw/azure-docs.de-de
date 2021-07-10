---
title: Russian Open Speech To Text
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset „Russian Open Speech To Text“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 7dc004de8b4c01fca8eebf54332a9fa5c177ae13
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038809"
---
# <a name="russian-open-speech-to-text"></a>Russian Open Speech To Text

Eine Sammlung von Sprachbeispielen, die aus verschiedenen Sprachquellen stammen. Das Dataset enthält kurze Audiodateien auf Russisch.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Dieses STT-Dataset (Spracherkennung) für Russisch umfasst Folgendes:
- ~16 Millionen Äußerungen
- ~20.000 Stunden
- 2,3 TB (unkomprimiert im WAV-Format in int16), 356G in OPUS
- Alle Dateien (außer Datasets zur Validierung) wurden ins OPUS-Format konvertiert

Das Dataset dient hauptsächlich dem Trainieren von Spracherkennungsmodellen.

## <a name="dataset-composition"></a>Zusammensetzung des Datasets

Die Datasetgröße wird für WAV-Dateien angegeben.

| DATASET | ÄUSSERUNGEN | STUNDEN | GB | SEKUNDEN/ZEICHEN | COMMENT | ANMERKUNG | QUALITÄT/RAUSCHEN |
|-|-|-|-|-|-|-|-|
| radio_v4 (*) | 7\.603.192 | 10.430 | 1\.195 | 5 Sek./68 | Radio | Ausrichten | 95 %/klar |
| public_speech (*) | 1\.700.060 | 2\.709 | 301 | 6 Sek/79 | Öffentlicher Vortrag | Ausrichten | 95 %/klar |
| audiobook_2 | 1\.149.404 | 1\.511 | 162 | 5 Sek./56 | Bücher | Ausrichten | 95 %/klar |
| radio_2 | 651.645 | 1\.439 | 154 | 8 Sek./110 | Radio | Ausrichten | 95 %/klar |
| public_youtube1120 | 1\.410.979 | 1.104 | 237 | 3 Sek./34 | Youtube | Untertitel | 95 %/~klar |
| public_youtube700 | 759.483 | 701 | 75 | 3 Sek./43 | Youtube | Untertitel | 95 %/überwiegend klar |
| tts_russian_addresses | 1\.741.838 | 754 | 81 | 2 Sek./20 | Adressen | TTS, 4 Stimmen | 100 %/klar |
| asr_public_phone_calls_2 | 603.797 | 601 | 66 | 4 Sek./37 | Telefonanrufe | ASR | 70 %/rauschend |
| public_youtube1120_hq | 369.245 | 291 | 31 | 3 Sek./37 | YouTube (hohe Qualität) | Untertitel | 95 %/überwiegend klar |
| asr_public_phone_calls_1 | 233.868 | 211 | 23 | 3 Sek./29 | Telefonanrufe | ASR | 70 %/rauschend |
| radio_v4_add (*) | 92.679 | 157 | 18 | 6 Sek/80 | Radio | Ausrichten | 95 %/klar |
| asr_public_stories_2 | 78.186 | 78 | 9 | 4 Sek./43 | Bücher | ASR | 80 %/klar |
| asr_public_stories_1 | 46.142 | 38 | 4 | 3 Sek./30 | Bücher | ASR | 80 %/klar |
| public_series_1 | 20.243 | 17 | 2 | 3 Sek./38 | Youtube | Untertitel | 95 %/überwiegend klar |
| asr_calls_2_val | 12.950 | 7,7 | 2 | 2 Sek./34 | Telefonanrufe | Manuelle Annotation | 99 %/klar |
| public_lecture_1 | 6\.803 | 6 | 1 | 3 Sek./47 | Vorlesungen | Untertitel | 95 %/klar |
| buriy_audiobooks_2_val | 7\.850 | 4,9 | 1 | 2 Sek./31 | Bücher | Manuelle Annotation | 99 %/klar |
| public_youtube700_val | 7\.311 | 4,5 | 1 | 2 Sek./35 | Youtube | Manuelle Annotation | 99 %/klar |

(*) Nur eine Stichprobe der Daten wird in den TXT-Dateien bereitgestellt.

## <a name="annotation-methodology"></a>Annotationsmethodik

Das Dataset wurde aus öffentlichen Quellen zusammengestellt. Lange Sequenzen werden mithilfe von Sprechaktivitätserkennung und -angleichung in kleine Audiodateien aufgeteilt. Einige Audioformate werden automatisch annotiert und statistisch mithilfe der Heuristik verifiziert.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Die Gesamtgröße des Datasets beträgt 350 GB. Die Gesamtgröße des Datasets mit öffentlich freigegebenen Bezeichnungen beträgt 130 GB.

Es ist unwahrscheinlich, dass das Dataset in Zukunft hinsichtlich der Abwärtskompatibilität aktualisiert wird. Nutzen Sie das Originalrepository für Benchmarks und zum Ausschließen von Dateien.

Neue Fachbereiche und Sprachen werden in Zukunft ggf. hinzugefügt.

## <a name="audio-normalization"></a>Audionormalisierung

Alle Dateien sind für einfachere und schnellere Anstiege der Runtime normalisiert. Die Verarbeitung erfolgt wie folgt:

- Bei Bedarf wurden sie in Mono konvertiert.
- Bei Bedarf wurden sie in eine Samplingrate von 16 kHz konvertiert.
- Sie wurden als 16-Bit-Integer gespeichert.
- Sie wurden ins OPUS-Format konvertiert.

## <a name="on-disk-db-methodology"></a>Datenbankmethodik auf dem Datenträger

Jede Audiodatei (WAV-Dateien, Binärdateien) wurde gehasht. Der Hash wird verwendet, um eine Ordnerhierarchie für einen verbesserten Betrieb des Dateisystems zu erstellen.

```python
target_format = 'wav'
wavb = wav.tobytes()

f_hash = hashlib.sha1(wavb).hexdigest()

store_path = Path(root_folder,
                  f_hash[0],
                  f_hash[1:3],
                  f_hash[3:15] + '.' + target_format)
```

## <a name="downloads"></a>Downloads

Das Dataset wird in zwei Formaten bereitgestellt:

- Archive, die über Azure Blob Storage und/oder direkte Links verfügbar sind
- Originaldateien, die über Azure Blob Storage verfügbar sind. Alles wird in https://azureopendatastorage.blob.core.windows.net/openstt/ gespeichert.

Ordnerstruktur:

```
└── ru_open_stt_opus                                            <= archived folders
│   │
│   ├── archives
│   │    ├── asr_calls_2_val.tar.gz                             <= tar.gz archives with opus and wav files
│   │    │   ...                                                <= see the below table for enumeration
│   │    └── tts_russian_addresses_rhvoice_4voices.tar.gz
│   │
│   └── manifests
│        ├── asr_calls_2_val.csv                                <= csv files with wav_path, text_path, duration (see notebooks)
│        │   ...
│        └── tts_russian_addresses_rhvoice_4voices.csv
│
└── ru_open_stt_opus_unpacked                                   <= a separate folder for each uploaded domain
    ├── public_youtube1120
    │    ├── 0                                                  <= see "On disk DB methodology" for details
    │    ├── 1
    │    │   ├── 00
    │    │   │  ...
    │    │   └── ff
    │    │        ├── *.opus                                   <= actual files
    │    │        └── *.txt
    │    │   ...
    │    └── f
    │
    ├── public_youtube1120_hq
    ├── public_youtube700_val
    ├── asr_calls_2_val
    ├── radio_2
    ├── private_buriy_audiobooks_2
    ├── asr_public_phone_calls_2
    ├── asr_public_stories_2
    ├── asr_public_stories_1
    ├── public_lecture_1
    ├── asr_public_phone_calls_1
    ├── public_series_1
    └── public_youtube700
```

| DATASET | GB, WAV | GB, ARCHIV | ARCHIVIEREN | QUELLE | MANIFEST |
|-|-|-|-|-|-|
| Trainieren |  |  |  |  |  |
| Stichprobe für Radio und öffentliche Vorträge | - | 11.4 | OPUS- und TXT-Format | - | manifest |
| audiobook_2 | 162 | 25,8 | OPUS- und TXT-Format | Internet und Angleichung | manifest |
| radio_2 | 154 | 24,6 | OPUS- und TXT-Format | Radio | manifest |
| public_youtube1120 | 237 | 19,0 | OPUS- und TXT-Format | YouTube-Videos | manifest |
| asr_public_phone_calls_2 | 66 | 9.4 | OPUS- und TXT-Format | Internet und ASR | manifest |
| public_youtube1120_hq | 31 | 4,9 | OPUS- und TXT-Format | YouTube-Videos | manifest |
| asr_public_stories_2 | 9 | 1.4 | OPUS- und TXT-Format | Internet und Angleichung | manifest |
| tts_russian_addresses_rhvoice_4voices | 80.9 | 12,9 | OPUS- und TXT-Format | TTS | manifest |
| public_youtube700 | 75.0 | 12,2 | OPUS- und TXT-Format | YouTube-Videos | manifest |
| asr_public_phone_calls_1 | 22.7 | 3.2 | OPUS- und TXT-Format | Internet und ASR | manifest |
| asr_public_stories_1 | 4,1 | 0.7 | OPUS- und TXT-Format | Öffentliche Storys | manifest |
| public_series_1 | 1.9 | 0,3 | OPUS- und TXT-Format | Öffentliche Reihen | manifest |
| public_lecture_1 | 0.7 | 0.1 | OPUS- und TXT-Format | Internet und manuell | manifest |
| Val |  |  |  |  |  |
| asr_calls_2_val | 2 | 0.8 | WAV- und TXT-Format | Internet | manifest |
| buriy_audiobooks_2_val | 1 | 0,5 | WAV- und TXT-Format | Bücher und manuell | manifest |
| public_youtube700_val | 2 | 0,13 | WAV- und TXT-Format | YouTube-Videos und manuell | manifest |

## <a name="download-instructions"></a>Downloadanweisungen

### <a name="direct-download"></a>Direkter Download

Anweisungen zum direkten Herunterladen des Datasets finden Sie auf der [GitHub-Seite mit Anweisungen zum Herunterladen](https://github.com/snakers4/open_stt#download-instructions).

## <a name="additional-information"></a>Zusätzliche Informationen

Wenn Sie Hilfe oder Fragen zu den Daten haben, wenden Sie sich unter aveysov@gmail.com an die Verfasser der Daten.

Diese Lizenz gewährt es Benutzern, diese Materialien für nicht kommerzielle Zwecke über ein beliebiges Medium und in beliebigem Format zu verbreiten, zu mischen, anzuwenden oder weiterzuentwickeln, sofern eine Zuschreibung an den Ersteller verfasst wird. Sie besteht aus folgenden Elementen:
* BY: Der Ersteller muss erwähnt werden.
* NC: Nur die nicht kommerzielle Verwendung der Arbeit ist zulässig.

Die CC-BY-NC-Nutzung und die kommerzielle Nutzung sind nur mit Genehmigung des Datasetautors zulässig.

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=open-speech-to-text -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=open-speech-to-text)** .

## <a name="helper-functions--dependencies"></a>Hilfsfunktionen/Abhängigkeiten

### <a name="building-libsndfile"></a>Erstellen von libsndfile

Eine effiziente Möglichkeit zum Lesen von OPUS-Dateien in Python, die keinen erheblichen Mehraufwand verursachen, ist die Verwendung von pysoundfile (ein Python CFFI-Wrapper um libsoundfile).

Die OPUS-Unterstützung wurde upstream implementiert, wurde aber nicht ordnungsgemäß freigegeben. Deshalb haben wir uns für das Erstellen von benutzerdefinierten Builds und Affen-Patches entschieden.

Normalerweise müssen Sie dies in Ihrer Shell mit sudo-Zugriff ausführen:

```bash
apt-get update
apt-get install cmake autoconf autogen automake build-essential libasound2-dev \
libflac-dev libogg-dev libtool libvorbis-dev libopus-dev pkg-config -y

cd /usr/local/lib
git clone https://github.com/erikd/libsndfile.git
cd libsndfile
git reset --hard 49b7d61
mkdir -p build && cd build

cmake .. -DBUILD_SHARED_LIBS=ON
make && make install
cmake --build .
```
### <a name="helper-functions--dependencies"></a>Hilfsfunktionen/Abhängigkeiten

Installieren Sie die folgenden Bibliotheken:

```
pandas
numpy
scipy
tqdm
soundfile
librosa
```

Manifeste sind CSV-Dateien mit den folgenden Spalten:

- Pfad zu Audio
- Pfad zur Textdatei
- Duration

Sie erwiesen sich als das einfachste Format für den Zugriff auf Daten.

Zur einfacheren Verwendung wurde allen Manifesten bereits ein neuer Stamm zugeordnet. Alle darin enthaltenen Pfade sind relativ. Sie müssen einen Stammordner bereitstellen.

```python
# manifest utils
import os
import numpy as np
import pandas as pd
from tqdm import tqdm
from urllib.request import urlopen


def reroot_manifest(manifest_df,
                    source_path,
                    target_path):
    if source_path != '':
        manifest_df.wav_path = manifest_df.wav_path.apply(lambda x: x.replace(source_path,
                                                                              target_path))
        manifest_df.text_path = manifest_df.text_path.apply(lambda x: x.replace(source_path,
                                                                                target_path))
    else:
        manifest_df.wav_path = manifest_df.wav_path.apply(lambda x: os.path.join(target_path, x))
        manifest_df.text_path = manifest_df.text_path.apply(lambda x: os.path.join(target_path, x))    
    return manifest_df


def save_manifest(manifest_df,
                  path,
                  domain=False):
    if domain:
        assert list(manifest_df.columns) == ['wav_path', 'text_path', 'duration', 'domain']
    else:
        assert list(manifest_df.columns) == ['wav_path', 'text_path', 'duration']

    manifest_df.reset_index(drop=True).sort_values(by='duration',
                                                   ascending=True).to_csv(path,
                                                                          sep=',',
                                                                          header=False,
                                                                          index=False)
    return True


def read_manifest(manifest_path,
                  domain=False):
    if domain:
        return pd.read_csv(manifest_path,
                        names=['wav_path',
                               'text_path',
                               'duration',
                               'domain'])
    else:
        return pd.read_csv(manifest_path,
                        names=['wav_path',
                               'text_path',
                               'duration'])


def check_files(manifest_df,
                domain=False):
    orig_len = len(manifest_df)
    if domain:
        assert list(manifest_df.columns) == ['wav_path', 'text_path', 'duration']
    else:
        assert list(manifest_df.columns) == ['wav_path', 'text_path', 'duration', 'domain']
    wav_paths = list(manifest_df.wav_path.values)
    text_path = list(manifest_df.text_path.values)

    omitted_wavs = []
    omitted_txts = []

    for wav_path, text_path in zip(wav_paths, text_path):
        if not os.path.exists(wav_path):
            print('Dropping {}'.format(wav_path))
            omitted_wavs.append(wav_path)
        if not os.path.exists(text_path):
            print('Dropping {}'.format(text_path))
            omitted_txts.append(text_path)

    manifest_df = manifest_df[~manifest_df.wav_path.isin(omitted_wavs)]
    manifest_df = manifest_df[~manifest_df.text_path.isin(omitted_txts)]
    final_len = len(manifest_df)

    if final_len != orig_len:
        print('Removed {} lines'.format(orig_len-final_len))
    return manifest_df


def plain_merge_manifests(manifest_paths,
                          MIN_DURATION=0.1,
                          MAX_DURATION=100):

    manifest_df = pd.concat([read_manifest(_)
                             for _ in manifest_paths])
    manifest_df = check_files(manifest_df)

    manifest_df_fit = manifest_df[(manifest_df.duration>=MIN_DURATION) &
                                  (manifest_df.duration<=MAX_DURATION)]

    manifest_df_non_fit = manifest_df[(manifest_df.duration<MIN_DURATION) |
                                      (manifest_df.duration>MAX_DURATION)]

    print(f'Good hours: {manifest_df_fit.duration.sum() / 3600:.2f}')
    print(f'Bad hours: {manifest_df_non_fit.duration.sum() / 3600:.2f}')

    return manifest_df_fit


def save_txt_file(wav_path, text):
    txt_path = wav_path.replace('.wav','.txt')
    with open(txt_path, "w") as text_file:
        print(text, file=text_file)
    return txt_path


def read_txt_file(text_path):
    #with open(text_path, 'r') as file:
    response = urlopen(text_path)
    file = response.readlines()
    for i in range(len(file)):
        file[i] = file[i].decode('utf8')
    return file 

def create_manifest_from_df(df, domain=False):
    if domain:
        columns = ['wav_path', 'text_path', 'duration', 'domain']
    else:
        columns = ['wav_path', 'text_path', 'duration']
    manifest = df[columns]
    return manifest


def create_txt_files(manifest_df):
    assert 'text' in manifest_df.columns
    assert 'wav_path' in manifest_df.columns
    wav_paths, texts = list(manifest_df['wav_path'].values), list(manifest_df['text'].values)
    # not using multiprocessing for simplicity
    txt_paths = [save_txt_file(*_) for _ in tqdm(zip(wav_paths, texts), total=len(wav_paths))]
    manifest_df['text_path'] = txt_paths
    return manifest_df


def replace_encoded(text):
    text = text.lower()
    if '2' in text:
        text = list(text)
        _text = []
        for i,char in enumerate(text):
            if char=='2':
                try:
                    _text.extend([_text[-1]])
                except:
                    print(''.join(text))
            else:
                _text.extend([char])
        text = ''.join(_text)
    return text
```

```python
# reading opus files
import os
import soundfile as sf



# Fx for soundfile read/write functions
def fx_seek(self, frames, whence=os.SEEK_SET):
    self._check_if_closed()
    position = sf._snd.sf_seek(self._file, frames, whence)
    return position


def fx_get_format_from_filename(file, mode):
    format = ''
    file = getattr(file, 'name', file)
    try:
        format = os.path.splitext(file)[-1][1:]
        format = format.decode('utf-8', 'replace')
    except Exception:
        pass
    if format == 'opus':
        return 'OGG'
    if format.upper() not in sf._formats and 'r' not in mode:
        raise TypeError("No format specified and unable to get format from "
                        "file extension: {0!r}".format(file))
    return format


#sf._snd = sf._ffi.dlopen('/usr/local/lib/libsndfile/build/libsndfile.so.1.0.29')
sf._subtypes['OPUS'] = 0x0064
sf.SoundFile.seek = fx_seek
sf._get_format_from_filename = fx_get_format_from_filename


def read(file, **kwargs):
    return sf.read(file, **kwargs)


def write(file, data, samplerate, **kwargs):
    return sf.write(file, data, samplerate, **kwargs)
```

```python
# display utils
import gc
from IPython.display import HTML, Audio, display_html
pd.set_option('display.max_colwidth', 3000)
#Prepend_path is set to read directly from Azure. To read from local replace below string with path to the downloaded dataset files
prepend_path = 'https://azureopendatastorage.blob.core.windows.net/openstt/ru_open_stt_opus_unpacked/'


def audio_player(audio_path):
    return '<audio preload="none" controls="controls"><source src="{}" type="audio/wav"></audio>'.format(audio_path)

def display_manifest(manifest_df):
    display_df = manifest_df
    display_df['wav'] = [audio_player(prepend_path+path) for path in display_df.wav_path]
    display_df['txt'] = [read_txt_file(prepend_path+path) for path in tqdm(display_df.text_path)]
    audio_style = '<style>audio {height:44px;border:0;padding:0 20px 0px;margin:-10px -20px -20px;}</style>'
    display_df = display_df[['wav','txt', 'duration']]
    display(HTML(audio_style + display_df.to_html(escape=False)))
    del display_df
    gc.collect()
```

## <a name="play-with-a-dataset"></a>Wiedergabe mit einem Dataset

### <a name="play-a-sample-of-files"></a>Wiedergabe eines Beispiels von Dateien

Die Browser der meisten Plattformen unterstützen die native Audiowiedergabe. Wir können also HTML5-Audioplayer verwenden, um unsere Daten anzuzeigen.

```python
manifest_df = read_manifest(prepend_path +'/manifests/public_series_1.csv')
#manifest_df = reroot_manifest(manifest_df,
                              #source_path='',
                              #target_path='../../../../../nvme/stt/data/ru_open_stt/')

```

```python
sample = manifest_df.sample(n=20)
display_manifest(sample)
```

### <a name="read-a-file"></a>Lesen einer Datei

```python
!ls ru_open_stt_opus/manifests/*.csv
```

Einige Beispiele, die veranschaulichen, wie WAV- und OPUS-Dateien am besten gelesen werden.

Scipy ist die schnellste Option für WAV-Dateien. Pysoundfile ist insgesamt am besten für OPUS-Dateien geeignet.

```python
%matplotlib inline

import librosa
from scipy.io import wavfile
from librosa import display as ldisplay
from matplotlib import pyplot as plt
```

#### <a name="read-a-wav"></a>Lesen einer WAV-Datei

```python
manifest_df = read_manifest(prepend_path +'manifests/asr_calls_2_val.csv')
#manifest_df = reroot_manifest(manifest_df,
                              #source_path='',
                              #target_path='../../../../../nvme/stt/data/ru_open_stt/')
```

```python
sample = manifest_df.sample(n=5)
display_manifest(sample)
```

```python
from io import BytesIO

wav_path = sample.iloc[0].wav_path
response = urlopen(prepend_path+wav_path)
data = response.read()
sr, wav = wavfile.read(BytesIO(data))
wav.astype('float32')
absmax = np.max(np.abs(wav))
wav =  wav / absmax
```

```python
# shortest way to plot a spectrogram
D = librosa.amplitude_to_db(np.abs(librosa.stft(wav)), ref=np.max)
plt.figure(figsize=(12, 6))
ldisplay.specshow(D, y_axis='log')
plt.colorbar(format='%+2.0f dB')
plt.title('Log-frequency power spectrogram')
# shortest way to plot an envelope
plt.figure(figsize=(12, 6))
ldisplay.waveplot(wav, sr=sr, max_points=50000.0, x_axis='time', offset=0.0, max_sr=1000, ax=None)
```

#### <a name="read-opus"></a>Lesen einer OPUS-Datei

```python
manifest_df = read_manifest(prepend_path +'manifests/asr_public_phone_calls_2.csv')
#manifest_df = reroot_manifest(manifest_df,
                              #source_path='',
                              #target_path='../../../../../nvme/stt/data/ru_open_stt/')
```

```python
sample = manifest_df.sample(n=5)
display_manifest(sample)
```

```python
opus_path = sample.iloc[0].wav_path
response = urlopen(prepend_path+opus_path)
data = response.read()
wav, sr = sf.read(BytesIO(data))
wav.astype('float32')
absmax = np.max(np.abs(wav))
wav =  wav / absmax
```

```python
# shortest way to plot a spectrogram
D = librosa.amplitude_to_db(np.abs(librosa.stft(wav)), ref=np.max)
plt.figure(figsize=(12, 6))
ldisplay.specshow(D, y_axis='log')
plt.colorbar(format='%+2.0f dB')
plt.title('Log-frequency power spectrogram')
# shortest way to plot an envelope
plt.figure(figsize=(12, 6))
ldisplay.waveplot(wav, sr=sr, max_points=50000.0, x_axis='time', offset=0.0, max_sr=1000, ax=None)
```

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.