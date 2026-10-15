# Prisma 0.4.0 — sorgenti dei motori multimediali

Questa distribuzione contiene soltanto componenti di terze parti e il materiale necessario a compilarli. Il codice sorgente dell’app Prisma non è incluso.

[Scarica gli archivi completi](https://github.com/GitAndreaHub/Prisma/releases/download/v0.4.0/Prisma-0.4.0-ThirdParty-Sources.tar.gz).

## Motori inclusi nel DMG, build 8

| Componente | Apple Silicon | Intel |
| --- | --- | --- |
| FFmpeg e FFprobe principali | AtlasYang n9.0.1, build3, LGPL-2.1-or-later | AtlasYang n9.0.1, build3, LGPL-2.1-or-later |
| Encoder aggiuntivi H.264/MP3 | FFmpeg 9.0.1 + x264 + LAME 3.100, GPL-2.0-or-later | FFmpeg 9.0.1 + x264 + LAME 3.100, GPL-2.0-or-later |

La build 8 sostituisce i precedenti motori Intel con motori per i quali sono disponibili sorgenti versionati e script di compilazione. Non distribuisce i binari Intel Evermeet 9.0.2. Le operazioni di codifica H.264 e MP3 vengono eseguite dal supplemento su entrambe le architetture.

## Contenuto dell’archivio

- **atlas-main/sources/**: FFmpeg n9.0.1 e le versioni fissate di zlib, Ogg, Vorbis, Opus, VPX, dav1d, AOM e WebP.
- **atlas-main/build-scripts/**: snapshot del repository AtlasYang al tag `ffmpeg-9.0.1-build3`, inclusa la configurazione CI.
- **atlas-main/Atlas-ARM-configure.txt** e **Atlas-Intel-configure.txt**: configurazioni, versioni e dipendenze dei binari originali.
- **codec-supplement/vendor/build-sources/downloads/**: archivi esatti di FFmpeg 9.0.1, x264, LAME 3.100, pkgconf 2.5.1 e NASM 2.16.03, con checksum.
- **codec-supplement/vendor/build-sources/**: script e log di compilazione dei supplementi.
- **notices/**: licenze, copyright e avvisi sui brevetti dei componenti.
- **reports/**: hash dei binari effettivamente inclusi e riepilogo delle verifiche.
- **SHA256SUMS.txt**: integrità dei file della distribuzione dei sorgenti.

Il supplemento Apple Silicon conservato è stato compilato con `build-arm64-compat.sh`; quello Intel è stato compilato con `build-compat.sh x86_64`. I log possono avere i percorsi locali di lavoro sostituiti da un percorso convenzionale, senza modifiche ai comandi o ai flag.

## Ricompilazione dei supplementi

Su macOS con i Command Line Tools di Apple, dalla radice di questo archivio:

```sh
cd codec-supplement
mkdir -p vendor/arm64
bash vendor/build-sources/build-arm64-compat.sh
```

Il comando precedente richiede un Mac Apple Silicon e produce `vendor/arm64/ffmpeg-compat`.

Per Intel, su un Mac Intel oppure su Apple Silicon con Rosetta disponibile:

```sh
cd codec-supplement
bash vendor/build-sources/build-compat.sh x86_64
```

Il risultato è `vendor/build-sources/tools/ffmpeg-compat`. Lo script compila pkgconf e NASM come strumenti locali, poi x264, LAME e FFmpeg per x86_64/macOS 11, senza installare pacchetti di sistema e senza scaricare altri sorgenti. Gli strumenti di compilazione non vengono inclusi nell’app.

## Motori principali AtlasYang

La provenienza è [AtlasYang/ffmpeg-static-builds, tag ffmpeg-9.0.1-build3](https://github.com/AtlasYang/ffmpeg-static-builds/releases/tag/ffmpeg-9.0.1-build3). I due report e il file dei checksum identificano esattamente gli archivi binari usati. Gli script originali definiscono versioni, toolchain, opzioni e sequenza di compilazione per ciascuna architettura; gli archivi dei sorgenti corrispondenti sono conservati in `atlas-main/sources/`.

Per ricompilare i motori principali usare la procedura macOS dello snapshot in `atlas-main/build-scripts/README.md` e `.github/workflows/build.yml`, con `FFMPEG_TAG=n9.0.1`. Gli script originali usano Git per ottenere i tag pubblici fissati e richiedono gli strumenti generici elencati dal distributore. Non ricompilano né richiedono Prisma.

## Licenze

Ogni componente conserva la licenza dei rispettivi autori. FFmpeg principale è LGPL-2.1-or-later; i supplementi con x264 sono GPL-2.0-or-later. LAME è LGPL-2.0-or-later. I testi e gli avvisi sono in `notices/` e negli archivi sorgenti originali. Gli script locali `build-arm64-compat.sh` e `build-compat.sh` sono distribuiti sotto GPL-2.0-or-later. Gli script e gli avvisi dei distributori upstream sono conservati come ricevuti.

Questa distribuzione è accessibile gratuitamente e senza registrazione. Il file `SHA256SUMS.txt` identifica i contenuti archiviati; il manifest dei binari identifica i motori del DMG a cui si riferiscono.
