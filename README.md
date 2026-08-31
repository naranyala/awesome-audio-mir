# Awesome Audio Programming & Music Information Retrieval

> A practical, language-agnostic map of tools, datasets, papers, and communities for working with sound, music, and **music information retrieval (MIR)**.

MIR is the engineering and research discipline of turning music into searchable, comparable, and meaningful information. It covers everything from reading a WAV file and computing a spectrogram to beat tracking, chord recognition, music recommendation, source separation, and symbolic score analysis.

The list is intentionally broader than one framework or programming language. It favors maintained, documented, reproducible resources and includes classic projects when they remain useful for learning or comparison.

> **License note:** “free to use”, “source available”, “open source”, and “free software” are not interchangeable. Check each project's license, model-card terms, and dataset restrictions before redistribution or commercial use.

## Contents

- [Start Here](#start-here)
- [MIR at a Glance](#mir-at-a-glance)
- [Audio Programming Foundations](#audio-programming-foundations)
- [MIR Toolkits](#mir-toolkits)
- [By Language and Environment](#by-language-and-environment)
- [Symbolic Music](#symbolic-music)
- [Datasets and Benchmarks](#datasets-and-benchmarks)
- [Annotations and Interchange](#annotations-and-interchange)
- [Learning Resources](#learning-resources)
- [Papers and Publications](#papers-and-publications)
- [Evaluation and Reproducibility](#evaluation-and-reproducibility)
- [Communities and Events](#communities-and-events)
- [Contributing](#contributing)

## Start Here

### A sensible learning path

1. Learn digital audio basics: sampling, decibels, filters, convolution, and the Fourier transform.
2. Build a small pipeline: decode audio, resample it, frame it, compute an STFT and visualize it.
3. Add musical representations: chroma, mel spectrograms, onset strength, tempo, and MFCCs.
4. Choose one task and one dataset. Establish a simple baseline before using a neural network.
5. Evaluate with the metric and split expected by the dataset, then inspect errors by ear and by plot.
6. Record versions, preprocessing, licenses, and random seeds so another person can reproduce the result.

### A minimal MIR pipeline

```text
audio or MIDI
    -> decode and validate
    -> resample / normalize (only when justified)
    -> frame or tokenize
    -> representation: waveform, STFT, mel, chroma, or symbolic events
    -> algorithm: DSP, nearest-neighbor, classifier, or neural model
    -> prediction: tags, beats, key, chords, segments, embeddings, or transcription
    -> evaluation, visualization, and listening tests
```

## MIR at a Glance

| Area | Typical questions | Useful representations |
| --- | --- | --- |
| Retrieval and similarity | Which tracks sound alike? Find this tune by humming? | MFCCs, chroma, learned embeddings, hashes |
| Tagging and classification | What genre, mood, instrument, or event is present? | Mel spectrograms, CQT, embeddings |
| Rhythm | Where are the beats? What is the tempo or meter? | Onsets, tempograms, beat activations |
| Harmony | What key, chords, or pitch classes are present? | Chroma, HPCP, CQT, tonal centroid |
| Structure | Where do verses, choruses, and repetitions occur? | Self-similarity matrices, novelty curves |
| Melody and pitch | What notes are sung or played? | F0, salience, piano-roll-like activations |
| Transcription | Convert a performance to notes or notation | Onsets, offsets, F0, MIDI events |
| Separation and enhancement | Isolate vocals or instruments; remove noise | STFT masks, time-domain models |
| Recommendation | What should a listener hear next? | Content features, collaborative signals, embeddings |
| Symbolic MIR | How are scores and performances alike or different? | MIDI, MusicXML, note events, graphs |

## Audio Programming Foundations

### Concepts and references

- [The Scientist and Engineer's Guide to Digital Signal Processing](https://www.dspguide.com/) - Free, approachable DSP textbook.
- [DAFX: Digital Audio Effects](https://www.dafx.de/) - Effects, filters, spatial audio, and audio analysis.
- [CCRMA Music Information Retrieval](https://ccrma.stanford.edu/courses/422/) - Stanford course material connecting DSP and music.
- [Spectral Audio Signal Processing](https://ccrma.stanford.edu/~jos/sasp/) - Detailed online reference for spectra, STFTs, filters, and audio DSP.
- [Digital Audio Fundamentals](https://www.soundonsound.com/techniques/digital-audio) - Practical explanations of sample rate, bit depth, and level.

### File formats and low-level I/O

- [libsndfile](https://libsndfile.github.io/libsndfile/) - Portable C library for reading and writing common PCM audio formats.
- [FFmpeg](https://ffmpeg.org/) - Broad codec and container support; useful for dataset preparation and batch conversion.
- [GStreamer](https://gstreamer.freedesktop.org/) - LGPL multimedia pipeline framework for decoding, streaming, and composing audio applications.
- [SoX](https://sox.sourceforge.net/) - The Swiss Army knife of command-line audio processing.
- [FFTW](https://www.fftw.org/) - High-performance Fourier transforms in C and C++.
- [KissFFT](https://github.com/mborgerding/kissfft) - Small, portable FFT implementation.
- [miniaudio](https://miniaud.io/) - Single-file C library for decoding, playback, capture, and basic audio processing.
- [WavPack](https://www.wavpack.com/) - Open lossless audio codec worth considering when metadata and source fidelity matter.
- [Rubber Band](https://breakfastquay.com/rubberband/) - High-quality time-stretching and pitch-shifting library; review its GPL/commercial licensing options.

### Real-time audio and plug-ins

- [PipeWire](https://pipewire.org/) - Low-latency Linux audio and multimedia graph; also provides compatibility layers for JACK, PulseAudio, and ALSA applications.
- [JACK](https://jackaudio.org/) - Low-latency audio and MIDI connection system for Linux, macOS, and Windows.
- [ALSA](https://www.alsa-project.org/) ([alsa-lib](https://github.com/alsa-project/alsa-lib)) - Linux kernel sound architecture and userspace audio API.
- [OpenAL Soft](https://github.com/kcat/openal-soft) - OpenAL implementation with 3D audio, HRTF, and cross-platform backends.
- [Steam Audio](https://github.com/ValveSoftware/steam-audio) - Spatial audio SDK with HRTF, occlusion, reflections, and reverb.
- [JUCE](https://juce.com/) - Cross-platform C++ framework for plug-ins, applications, MIDI, and audio devices; review its GPL/commercial licensing.
- [iPlug2](https://github.com/iPlug2/iPlug2) - Lightweight C++ framework for audio plug-ins and standalone apps.
- [PortAudio](https://www.portaudio.com/) - Portable real-time audio I/O.
- [RtAudio](https://github.com/thestk/rtaudio) - Common C++ API for audio devices.
- [Oboe](https://github.com/google/oboe) - High-performance Android audio API built around AAudio and OpenSL ES.
- [FluidSynth](https://github.com/FluidSynth/fluidsynth) - Real-time SoundFont synthesizer with C API and command-line tools.
- [RNNoise](https://gitlab.xiph.org/xiph/rnnoise) - Recurrent-neural-network noise suppression library.
- [SoundTouch](https://www.surina.net/soundtouch/) - Open-source library for tempo change, pitch shifting, and audio rate adjustment.
- [Faust](https://faust.grame.fr/) ([source](https://github.com/grame-cncm/faust)) - Functional language and compiler for portable, efficient real-time DSP.
- [Vamp SDK](https://www.vamp-plugins.org/) - Plug-in API for audio feature extraction, used by Sonic Visualiser and Sonic Annotator.
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) - Native browser graph for real-time audio and analysis.
- [MIDI 2.0](https://midi.org/midi-2-0) - Modern protocol and specifications for performance data.

### Open plug-in standards and hosts

- [LV2](https://lv2plug.in/) - Extensible open standard for audio effects, instruments, and analysis plug-ins.
- [CLAP](https://github.com/free-audio/clap) - Modern open plug-in API designed for audio and music production workflows.
- [VST3 SDK](https://github.com/steinbergmedia/vst3sdk) - Official SDK for VST3 plug-ins; review its license and distribution terms.
- [LADSPA](https://www.ladspa.org/) - Lightweight Linux Audio Developer's Simple Plugin API.
- [DPF](https://github.com/DISTRHO/DPF) - C++ framework for LV2, VST, CLAP, and standalone plug-ins.
- [Carla](https://github.com/falkTX/Carla) - Open plug-in host for LV2, VST, JACK, and related formats.
- [libremidi](https://github.com/celtera/libremidi) - C++20 library for real-time MIDI 1.0/2.0, ports, and Standard MIDI Files.

### Open-source applications

- [Audacity](https://github.com/audacity/audacity) - Cross-platform audio editor useful for inspection, cleanup, batch scripting, and annotation preparation.
- [Ardour](https://ardour.org/) - GPL digital audio workstation for recording, editing, routing, mixing, and plug-in hosting.

## MIR Toolkits

### General-purpose audio and MIR

- [librosa](https://librosa.org/) ([Python](https://github.com/librosa/librosa)) - Accessible audio analysis: spectrograms, chroma, rhythm, pitch, and visualization.
- [Essentia](https://essentia.upf.edu/) ([C++ / Python / JavaScript](https://github.com/MTG/essentia)) - Large collection of production-oriented audio descriptors and MIR algorithms.
- [madmom](https://github.com/CPJKU/madmom) ([Python](https://madmom.readthedocs.io/)) - Strong beat, downbeat, onset, and music processing components.
- [aubio](https://aubio.org/) ([C / Python](https://github.com/aubio/aubio)) - Real-time onset, pitch, beat, and tempo detection.
- [MARSYAS](https://marsyas.info/) ([C++](https://github.com/marsyas/marsyas)) - An extensible framework for audio processing and MIR research.
- [MIRtoolbox](https://github.com/olivierlar/mirtoolbox) ([MATLAB / Octave](https://github.com/martinarielhartmann/mirtooloct)) - Feature extraction and analysis functions for music audio.
- [mirdata](https://github.com/mir-dataset-loaders/mirdata) ([Python](https://mirdata.readthedocs.io/)) - Standardized download, validation, and loading of MIR datasets.
- [MSAF](https://github.com/urinieto/msaf) ([Python](https://msaf.readthedocs.io/)) - Framework and reference implementations for music structure analysis.
- [audioFlux](https://github.com/libAudioFlux/audioFlux) ([C / Python](https://audioflux.top/)) - High-performance time-frequency transforms and audio feature extraction.
- [Sonic Visualiser](https://www.sonicvisualiser.org/) - Inspect waveforms, spectrograms, annotations, and Vamp features interactively.
- [Sonic Annotator](https://github.com/sonic-visualiser/sonic-annotator) - Batch feature extraction from Vamp plug-ins.

### Python I/O, preprocessing, and augmentation

- [SoundFile](https://github.com/bastibe/python-soundfile) - Python bindings to libsndfile for reliable audio file I/O.
- [SoundDevice](https://python-sounddevice.readthedocs.io/) - PortAudio bindings for recording and playback from Python.
- [PyAV](https://github.com/PyAV-Org/PyAV) - Pythonic bindings for FFmpeg's containers, codecs, and filter graphs.
- [resampy](https://github.com/bmcfee/resampy) - Band-limited sample-rate conversion for Python audio pipelines.
- [python-soxr](https://github.com/dofuuz/python-soxr) - High-quality libsoxr bindings for sample-rate conversion.
- [audiomentations](https://github.com/iver56/audiomentations) - Audio augmentation transforms for machine-learning datasets.
- [torch-audiomentations](https://github.com/asteroid-team/torch-audiomentations) - Batched, GPU-compatible audio augmentations for PyTorch.

### Fingerprinting, metadata, and music libraries

- [Chromaprint](https://github.com/acoustid/chromaprint) - Acoustic fingerprinting library used by the AcoustID music identification service.
- [AcoustID](https://acoustid.org/) - Open music identification service and fingerprint database.
- [audfprint](https://github.com/dpwe/audfprint) - Robust audio fingerprinting and query-by-example toolkit.
- [Panako](https://github.com/JorenSix/Panako) - Acoustic fingerprinting system for large-scale audio identification and matching.
- [MusicBrainz](https://musicbrainz.org/) - Open music encyclopedia, identifiers, and metadata database.
- [MusicBrainz Picard](https://github.com/metabrainz/picard) - Cross-platform open-source music tagger built around MusicBrainz metadata.
- [Mutagen](https://github.com/quodlibet/mutagen) - Python library for reading and writing audio metadata and tags.
- [TagLib](https://taglib.org/) - C++ library for reading and editing audio metadata.
- [beets](https://beets.io/) - Open music library manager with metadata lookup, organization, and plug-ins.

### Deep learning and differentiable audio

- [PyTorch](https://pytorch.org/) - Open-source tensor and neural-network framework commonly used for MIR research.
- [TensorFlow](https://www.tensorflow.org/) - Open-source machine-learning framework with audio, serving, and edge deployment tooling.
- [PyTorch Audio](https://github.com/pytorch/audio) - Audio I/O and transforms for PyTorch projects; check the repository for current maintenance status.
- [TensorFlow Audio](https://www.tensorflow.org/io/tutorials/audio) - TensorFlow I/O and audio examples.
- [nnAudio](https://github.com/KinWaiCheuk/nnAudio) - GPU-friendly, differentiable spectrogram and CQT transforms in PyTorch.
- [torchlibrosa](https://github.com/qiuqiangkong/torchlibrosa) - Differentiable spectrogram, mel, and augmentation layers.
- [JAX](https://docs.jax.dev/) - Fast array programming for custom differentiable DSP and neural audio models.
- [Asteroid](https://github.com/asteroid-team/asteroid) - PyTorch toolkit for speech and music source separation.
- [Demucs](https://github.com/facebookresearch/demucs) - Archived but influential music source separation models and research code.
- [Hugging Face Audio](https://huggingface.co/tasks/audio-classification) - Models, datasets, and task recipes for audio classification and related tasks.
- [OpenL3](https://github.com/marl/openl3) - General-purpose audio embeddings trained with audio-visual correspondence; check dependency compatibility.
- [ONNX Runtime](https://github.com/microsoft/onnxruntime) - Cross-platform open-source inference runtime for deploying trained audio models.
- [ExecuTorch](https://github.com/pytorch/executorch) - PyTorch edge-inference stack for mobile, embedded, and constrained audio systems.

### Task-specific MIR systems

- [mir_eval](https://github.com/mir-evaluation/mir_eval) - Reference metrics and evaluation utilities for beat, chord, melody, transcription, and retrieval tasks.
- [museval](https://github.com/sigsep/sigsep-mus-eval) - Standardized evaluation for music source separation, including BSS Eval metrics.
- [Spleeter](https://github.com/deezer/spleeter) - Pretrained source-separation models and command-line tools.
- [Open-Unmix](https://github.com/sigsep/open-unmix-pytorch) - PyTorch reference implementation for music source separation.
- [Basic Pitch](https://github.com/spotify/basic-pitch) - Lightweight audio-to-MIDI automatic transcription with pitch-bend support.
- [MT3](https://github.com/magenta/mt3) - Research code for multi-instrument automatic music transcription.
- [piano_transcription_inference](https://github.com/qiuqiangkong/piano_transcription_inference) - Inference toolkit for piano transcription models.
- [torchcrepe](https://github.com/maxrmorrison/torchcrepe) - GPU-compatible PyTorch implementation of CREPE pitch tracking.

### Music representations and pretrained models

- [musicnn](https://github.com/jordipons/musicnn) - Pretrained convolutional models for music auto-tagging.
- [CLAP](https://github.com/LAION-AI/CLAP) - Contrastive language-audio pretraining for joint text and audio representations.
- [MERT](https://huggingface.co/m-a-p/MERT-v1-330M) - Pretrained music representation model; check the model card's non-commercial license.
- [VGGish](https://github.com/tensorflow/models/tree/master/research/audioset/vggish) - AudioSet embedding model and reference implementation.
- [AudioSet Tagging CNN](https://github.com/qiuqiangkong/audioset_tagging_cnn) - Pretrained audio classification models and feature extraction code.

### Generative and multimodal music research

- [AudioCraft](https://github.com/facebookresearch/audiocraft) - Open-source audio generation library including MusicGen and EnCodec; check model licenses separately.
- [Magenta](https://github.com/magenta/magenta) - Archived research project for music and art generation, symbolic models, and interactive tools.
- [Jukebox](https://github.com/openai/jukebox) - Archived neural music generation research code.

### Search, embeddings, and scalable data

- [FAISS](https://github.com/facebookresearch/faiss) - Fast similarity search over dense vectors.
- [Annoy](https://github.com/spotify/annoy) - Memory-efficient approximate nearest-neighbor search.
- [Qdrant](https://qdrant.tech/) - Vector database with filtering and payloads for audio search prototypes.
- [DVC](https://dvc.org/) - Version datasets, features, and model artifacts alongside code.
- [WebDataset](https://github.com/webdataset/webdataset) - Stream large collections of audio and annotations from sharded archives.

## By Language and Environment

The language is a design choice, not an MIR boundary. Use the ecosystem that best fits your latency, deployment, research, or teaching constraints.

### Python

- [NumPy](https://numpy.org/) and [SciPy Signal](https://docs.scipy.org/doc/scipy/reference/signal.html) - Arrays, windows, filters, convolution, and reference DSP.
- [librosa](https://librosa.org/) - A friendly starting point for offline MIR experiments.
- [Essentia](https://essentia.upf.edu/) - Rich descriptors and C++-backed performance.
- [madmom](https://github.com/CPJKU/madmom) - Rhythm and beat tracking.
- [mirdata](https://mirdata.readthedocs.io/) - Dataset loaders and metadata.
- [music21](https://music21.org/) - Computational musicology, notation, harmony, and symbolic analysis.
- [libfmp](https://github.com/groupmm/libfmp) - Reference implementations accompanying the Fundamentals of Music Processing notebooks.
- [pretty_midi](https://github.com/craffel/pretty-midi) - Convenient MIDI parsing and synthesis helpers.
- [partitura](https://github.com/CPJKU/partitura) - Symbolic music processing, score-performance alignment, and MusicXML/MIDI.

### C and C++

- [Essentia](https://github.com/MTG/essentia) - C++ core with bindings for several languages.
- [aubio](https://github.com/aubio/aubio) - Small real-time analysis library.
- [MARSYAS](https://github.com/marsyas/marsyas) - Research framework for modular audio analysis.
- [JUCE](https://juce.com/) - Full application and plug-in development.
- [Vamp](https://www.vamp-plugins.org/) - Interoperable feature extraction plug-ins.
- [RtAudio](https://github.com/thestk/rtaudio), [PortAudio](https://www.portaudio.com/), and [FFTW](https://www.fftw.org/) - Building blocks for custom systems.

### JavaScript and TypeScript

- [Essentia.js](https://essentia.upf.edu/essentiajs/) - Essentia in the browser and Node.js through WebAssembly.
- [Meyda](https://meyda.js.org/) - Real-time feature extraction with the Web Audio API.
- [Tone.js](https://tonejs.github.io/) - Scheduling, synthesis, effects, and interactive music applications.
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) - Browser-native audio graph and analyzers.
- [wavesurfer.js](https://wavesurfer.xyz/) - Interactive waveform and region visualization.

### Rust

- [Symphonia](https://github.com/pdeljanov/Symphonia) - Pure Rust media demuxing and audio decoding.
- [cpal](https://github.com/RustAudio/cpal) - Cross-platform audio I/O.
- [dasp](https://github.com/RustAudio/dasp) - Digital audio signal processing primitives.
- [fundsp](https://github.com/SamiPerttu/fundsp) - Real-time audio DSP graph library.
- [rustfft](https://github.com/ejmahler/RustFFT) - Fast FFT implementation.
- [rubato](https://github.com/HEnquist/rubato) - Sample-rate conversion.

### MATLAB and GNU Octave

- [MIRtoolbox](https://github.com/olivierlar/mirtoolbox) - High-level MIR feature extraction and analysis.
- [Audio Toolbox](https://www.mathworks.com/products/audio.html) - Proprietary MATLAB toolbox for audio I/O, feature extraction, deep learning, and deployment.
- [Signal Processing Toolbox](https://www.mathworks.com/products/signal.html) - Proprietary MATLAB toolbox for filters, transforms, and spectral processing.
- [GNU Octave](https://octave.org/) - Open-source MATLAB-compatible numerical environment.

### R

- [tuneR](https://cran.r-project.org/package=tuneR) - Read, write, and manipulate audio files.
- [seewave](https://cran.r-project.org/package=seewave) - Spectral, temporal, and oscillographic analysis.
- [audio](https://cran.r-project.org/package=audio) - Audio I/O and playback from R.
- [warbleR](https://cran.r-project.org/package=warbleR) - Acoustic analysis workflows that are also useful for music and soundscape corpora.

### Julia

- [DSP.jl](https://github.com/JuliaDSP/DSP.jl) - Digital signal processing algorithms.
- [FFTW.jl](https://github.com/JuliaMath/FFTW.jl) - FFT bindings.
- [WAV.jl](https://github.com/dancasimiro/WAV.jl) - WAV file I/O.
- [MusicProcessing.jl](https://github.com/JuliaMusic/MusicProcessing.jl) - Julia ecosystem for music processing.

### Java

- [TarsosDSP](https://github.com/JorenSix/TarsosDSP) - Pure-Java real-time DSP and MIR algorithms, including pitch, onset, beat, and time-stretching tools.

### .NET and C#

- [NAudio](https://github.com/naudio/NAudio) - .NET audio and MIDI library with playback, recording, DSP helpers, and plug-in hosting.

### Swift and Apple platforms

- [AudioKit](https://github.com/AudioKit/AudioKit) - Swift framework for synthesis, processing, sequencing, MIDI, and audio analysis on Apple platforms.

### Kotlin

- [ktmidi](https://github.com/atsushieno/ktmidi) - Kotlin Multiplatform MIDI 1.0/2.0, Standard MIDI Files, MIDI-CI, and platform access.

### Go

- [Oto](https://github.com/ebitengine/oto) - Low-level cross-platform audio playback for Go.
- [Beep](https://github.com/gopxl/beep) - Go package for decoding, playback, effects, mixing, and audio processing.
- [go-dsp](https://github.com/madelynnblue/go-dsp) - Go DSP utilities including FFT, spectral analysis, WAV I/O, and windows.

### Creative coding and visual environments

- [SuperCollider](https://supercollider.github.io/) - Real-time synthesis, analysis, and live coding.
- [Pure Data](https://puredata.info/) - Visual patching for interactive audio and music systems.
- [Max](https://cycling74.com/products/max) - Proprietary visual environment for media, signal processing, and performance.
- [ChucK](https://chuck.stanford.edu/) - Strongly timed audio programming language for live and generative music.
- [Csound](https://github.com/csound/csound) - Sound synthesis and music-computing system with APIs for multiple host languages.
- [Sonic Pi](https://github.com/sonic-pi-net/sonic-pi) - Open-source live-coding environment for music, built with Ruby and SuperCollider.
- [Tidal Cycles](https://tidalcycles.org/) - Haskell-based live-coding language for pattern-driven music.
- [Euterpea](https://github.com/Euterpea/Euterpea) - Haskell library and educational environment for computer music.
- [Overtone](https://github.com/overtone/overtone) - Clojure environment for expressive live coding and synthesis.
- [FoxDot](https://github.com/Qirky/FoxDot) - Python live-coding environment that sends patterns to SuperCollider.

## Symbolic Music

Audio MIR works from recordings; symbolic MIR works from notes, scores, performances, and musical structure. The two meet in transcription, alignment, and score-informed analysis.

- [music21](https://music21.org/) - Music theory, notation, corpus analysis, and symbolic transformations.
- [Partitura](https://partitura.readthedocs.io/) - Load and analyze scores and performances; align symbolic representations.
- [pretty_midi](https://github.com/craffel/pretty-midi) - Simple Python interface for MIDI manipulation.
- [miditoolkit](https://github.com/YatingMusic/miditoolkit) - MIDI parsing and editing for symbolic music research.
- [Mido](https://mido.readthedocs.io/) - MIDI messages, ports, and files in Python.
- [MusPy](https://github.com/salu133445/muspy) - Python toolkit for symbolic music generation, analysis, and dataset handling.
- [MidiTok](https://github.com/Natooz/MidiTok) - Tokenizers for MIDI and symbolic music models.
- [pypianoroll](https://github.com/salu133445/pypianoroll) - Piano-roll representation and processing utilities.
- [midicsv](https://www.fourmilab.ch/webtools/midicsv/) - Convert MIDI files to a human-readable CSV representation.
- [Verovio](https://www.verovio.org/) - Render and process Music Encoding Initiative (MEI) scores.
- [MuseScore](https://musescore.org/) - Open-source notation editor with MusicXML and MIDI support.
- [SMuFL](https://www.smufl.org/) - Standard music font glyph mapping for notation software.
- [Humdrum Toolkit](https://www.humdrum.org/) - Command-line tools and representations for symbolic music analysis.
- [Lakh MIDI Dataset](https://colinraffel.com/projects/lmd/) - Large collection of MIDI files with metadata and derived subsets.

## Datasets and Benchmarks

> Always read the original license and terms. A dataset being downloadable does not automatically make it suitable for redistribution, commercial use, or model training.

### Audio collections and tags

- [FMA: Free Music Archive Dataset](https://github.com/mdeff/fma) - A widely used, metadata-rich collection with genre labels and predefined subsets.
- [MTG-Jamendo Dataset](https://github.com/MTG/mtg-jamendo-dataset) - Audio tracks with genre, instrument, and mood/theme tags.
- [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/) - Audio features and metadata at scale; the audio itself is not included.
- [AudioSet](https://research.google.com/audioset/) - Large-scale ontology and human-labeled audio segments, including music events.
- [DALI](https://github.com/gabolsgabs/DALI) - Audio, lyrics, and vocal-note annotations for singing voice and melody research.

### Rhythm, melody, harmony, and structure

- [Ballroom](https://mirdata.readthedocs.io/en/stable/source/quick_reference.html) - Dance-music excerpts for tempo and beat-related tasks.
- [GTZAN](http://marsyas.info/downloads/datasets.html) - Classic genre-recognition benchmark; use documented artist/duplication corrections.
- [ISMIR 2004 Genre](https://ismir2004.ismir.net/genre_contest/index.html) - Historical genre classification benchmark.
- [MedleyDB](https://medleydb.weebly.com/) - Multitrack recordings with instrument, melody, and structural annotations.
- [Harmonix Set](https://github.com/urinieto/harmonixset) - Beat and downbeat annotations for commercial music.
- [GiantSteps](https://github.com/GiantSteps/giantsteps-mtg-key-dataset) - Electronic music for tempo and key estimation.
- [RWC Music Database](https://staff.aist.go.jp/m.goto/RWC-MDB/) - Carefully annotated research music database; access may require a license.
- [Isophonics](http://isophonics.net/datasets) - Chords, beats, downbeats, sections, and keys for popular-music recordings.
- [Saraga](https://mtg.github.io/saraga/) - Annotated Indian Art Music collections with audio, melody, tonic, and structural information.

### Transcription, separation, and performance

- [MAESTRO](https://magenta.tensorflow.org/datasets/maestro) - Aligned piano audio and MIDI performances.
- [MUSDB18](https://sigsep.github.io/datasets/musdb.html) - Multitrack music for source separation.
- [DSD100](https://sigsep.github.io/datasets/dsd100.html) - Source-separation benchmark with stems.
- [Slakh](https://github.com/ethman/Slakh) - Synthetic multitrack dataset with MIDI-aligned stems.
- [MedleyDB](https://medleydb.weebly.com/) - Annotated multitrack recordings and instrument stems.
- [MusicNet](https://homes.cs.washington.edu/~thickstn/musicnet.html) - Annotated classical recordings with note events.
- [GuitarSet](https://guitarset.weebly.com/) - Guitar audio with synchronized tablature, chords, beats, and performance annotations.
- [ASAP](https://github.com/fosfrancesco/asap-dataset) - Aligned piano scores and performances for expressive-performance research.
- [POP909](https://github.com/music-x-lab/POP909-Dataset) - Symbolic pop piano performances with note and structural annotations.
- [GiantMIDI-Piano](https://github.com/bytedance/GiantMIDI-Piano) - Large-scale piano MIDI collection for symbolic music research.
- [SymphonyNet](https://github.com/symphonynet/SymphonyNet) - Large-scale symbolic music dataset and generation research code.

### Dataset discovery

- [mirdata datasets](https://mirdata.readthedocs.io/en/stable/source/quick_reference.html) - Common loaders and checks for many MIR datasets.
- [Papers With Code: Music](https://paperswithcode.com/area/music) - Benchmark and paper discovery across music tasks.
- [Hugging Face Datasets: Audio](https://huggingface.co/datasets?modality=modality.audio) - Searchable hub for audio and music datasets.

## Annotations and Interchange

- [JAMS](https://jams.readthedocs.io/) - JSON annotation format for music tasks such as beats, chords, melody, sections, and tags.
- [jams](https://github.com/marl/jams) - Python implementation and validation tools for JAMS.
- [MusicXML](https://www.musicxml.com/) - Interchange format for digital sheet music.
- [MEI](https://music-encoding.org/) - Community standard for encoding scholarly music notation.
- [MIDI](https://midi.org/midi-1-0) - Event-based protocol and file format for musical performance data.
- [Freesound](https://freesound.org/) - Large community sound repository with tags and API access; check each recording's license.
- [Sonic Visualiser](https://www.sonicvisualiser.org/) - Practical time-aligned annotation workflows.
- [Label Studio](https://github.com/HumanSignal/label-studio) - General-purpose open-source annotation platform with audio labeling support.

## Learning Resources

### Books and notebooks

- [Fundamentals of Music Processing and FMP Notebooks](https://www.audiolabs-erlangen.de/FMP) - Meinard Müller; executable notebooks covering audio, Fourier analysis, rhythm, harmony, structure, and learning.
- [Music Information Retrieval](https://musicinformationretrieval.com/) - Instructional material on computational audio and music.
- [The Audio Programmer](https://www.youtube.com/@TheAudioProgrammer) - Practical C++ audio and plug-in development.
- [Audio Programming Book](https://mitpress.mit.edu/9780262014465/the-audio-programming-book/) - Low-level audio programming and DSP concepts.

### Courses and tutorials

- [Audio Signal Processing for Music Applications](https://www.coursera.org/learn/audio-signal-processing) - Coursera course from Universitat Pompeu Fabra.
- [AudioLabs Tutorials](https://www.audiolabs-erlangen.de/resources) - Research-grade tutorials and code from Erlangen.
- [Stanford CCRMA](https://ccrma.stanford.edu/courses/) - Courses in sound synthesis, DSP, acoustics, and music technology.
- [Music Technology Group](https://www.upf.edu/web/mtg) - Research, software, datasets, and publications from a major MIR group.
- [PyTorch Audio Tutorials](https://docs.pytorch.org/audio/main/tutorials/audio_feature_extractions_tutorial.html) - Practical recipes for loading, transforming, and modeling audio.

## Papers and Publications

### Foundational software and representations

- [librosa: Audio and Music Signal Analysis in Python](https://doi.org/10.25080/majora-7b98e3ed-003) - McFee et al.
- [Essentia: An Open-Source Library for Audio Analysis](https://doi.org/10.5281/zenodo.3528050) - Bogdanov et al.
- [Calculation of a Constant Q Spectral Transform](https://doi.org/10.1121/1.400476) - Brown's foundational work on logarithmic-frequency analysis.

### Find current research

- [ISMIR proceedings](https://ismir.net/) - Primary annual conference for MIR research.
- [ISMIR proceedings archive](https://archives.ismir.net/) - Searchable collection of conference papers.
- [MIREX](https://www.music-ir.org/mirex/wiki/MIREX_HOME) - Community evaluation campaigns and task definitions.
- [ISMIR Bibliography](https://www.music-ir.org/) - Community gateway to MIR literature and resources.
- [arXiv: Audio and Speech](https://arxiv.org/list/eess.AS/recent) - Preprints covering audio ML, speech, and music research.
- [IEEE Xplore: Music Information Retrieval](https://ieeexplore.ieee.org/search/searchresult.jsp?newsearch=true&queryText=music%20information%20retrieval) - Signal-processing and engineering literature.

## Evaluation and Reproducibility

Good MIR systems are not defined by a single impressive score. Make the experiment auditable.

- Split by artist, album, or source when near-duplicate tracks could cross train and test sets.
- Keep the original audio, sample rate, channel layout, loudness, and metadata traceable.
- State the exact annotation policy: tolerance windows, beat phase conventions, chord vocabulary, and ignored labels.
- Use task-appropriate metrics: F-measure for events, accuracy or weighted scores for classification, agreement metrics for tags, and retrieval metrics such as precision@k, recall@k, or nDCG.
- Report confidence intervals or repeated runs where practical; include a simple baseline.
- Inspect false positives and false negatives with synchronized audio, plots, and annotations.
- Track dataset, code, model, dependency, and hardware versions with tools such as [DVC](https://dvc.org/), [MLflow](https://mlflow.org/), and [Weights & Biases](https://wandb.ai/).
- Respect consent, copyright, privacy, cultural context, and dataset-specific restrictions.
- Prefer open implementations and archival releases such as [Zenodo](https://zenodo.org/) when publishing results.

## Communities and Events

- [ISMIR Society](https://ismir.net/) - International community for music information retrieval.
- [ISMIR Conference](https://ismir.net/conferences/) - Annual research conference and proceedings.
- [MIREX](https://www.music-ir.org/mirex/wiki/MIREX_HOME) - Annual community evaluation campaign.
- [Audio Engineering Society](https://aes2.org/) - Audio engineering research, standards, and conferences.
- [IEEE ICME](https://www.ieeeicme.org/) - Multimedia conference with music and audio tracks.
- [ICASSP](https://ieeeicassp.org/) - Signal processing conference with audio and music contributions.
- [WASPAA](https://waspaa.com/) - Workshop on Applications of Signal Processing to Audio and Acoustics.

## Contributing

Contributions are welcome. Add resources that are relevant to audio programming or MIR and that you have used, tested, or carefully verified.

Please:

- Use the existing `- [Name](URL) - short description.` format.
- Put a resource in the smallest appropriate section; avoid duplicates.
- Prefer official project pages, documentation, papers, and dataset landing pages over link aggregators.
- Mention the language or runtime when it is not obvious.
- Include license or access caveats for datasets and proprietary tools.
- Check that links work and that projects are not abandoned when a maintained alternative exists.
- Keep descriptions factual, concise, and free of marketing claims.

## License

This list is provided under the [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) dedication unless stated otherwise by the repository owner.
