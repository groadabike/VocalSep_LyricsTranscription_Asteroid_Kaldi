# Automatic Vocal Separation and Lyrics Transcription.

This repository stores the system submitted to [MIREX 2020:Lyrics Transcription task](https://www.music-ir.org/mirex/wiki/2020:Lyrics_Transcription).

## What we submitted?

We submitted a system composed of two modules connected in a pipeline; a source separation and a lyrics transcription module. 
In this system, we utilised [Asteroid Pytorch-based audio source separation toolkit](https://github.com/mpariente/asteroid)[1] 
for the construction of the vocal separation module. The lyrics transcription module was constructed using 
the [Kaldi ASR toolkit](http://kaldi-asr.org/)[2]. 

<div align="center">
<img src="images/system_pipeline.png" width="100%">

**Diagram of the system pipeline.** 

</div>

## Content
- [Installation](#installation)
- [Usage](#Usage)
- [Cite](#Cite)
- [References](#References)

## Installation

### Clone this repository

Clone this repository with:
```bash
git clone https://github.com/groadabike/LyricsTranscription_MIREX2020.git 
cd LyricsTranscription_MIREX2020
```

### Prepare Python environment

If you have Anaconda installed, you can create the environment from the root of this repository:
```bash
conda env update -f environment-mirex_grd-gpu.yml # If you have GPU's
conda env update -f environment-mirex_grd-cpu.yml # If you don't have GPU's
conda activate mirex_grd
```

### Install Kaldi

First, we need to download and install Kaldi toolkit.
The script extras/check_dependencies.sh will tell you if you need to install some Kaldi dependencies.  
Please, ensure you resolve all the comments before you can continue.
Be sure that the `--cuda-dir` parameter is directed to your cuda installation.
```bash
git clone https://github.com/kaldi-asr/kaldi.git kaldi

cd kaldi/tools
mkdir -p python
touch python/.use_default_python
./extras/check_dependencies.sh
make

cd ../src
./configure --shared --cudatk-dir=$CUDA_HOME
make -j clean depend; make -j <NCPU>
```

## Usage
Before run this program for the first time, edit KALDI_ROOT path in `path.sh` file.
```bash
export KALDI_ROOT=path/to/your/kaldi_root
e.g.:
export KALDI_ROOT=/home/<user>/kaldi
```

Run this program as:    
```bash
conda activate mirex_grd

./run.sh [options] %input_audio %output
./run.sh --python_path $CONDA_PREFIX/bin/python --use_gpu 1 %input_audio %output
[options]
--use_gpu       Whether or not us GPU, 
                0 = False | 1 = True
                Default: 1
--enh_samples   (int) Number of samples for chunk inferring.
                Increase for faster inferring.
                Default: 18000 (Suitable for GPU with 2GB RAM)
```

Where %input_audio is the path to the audio to transcribed and %output is the path to the file where the transcription will be saved.
Note that the %output will be overwritten if exist.

### Output 
The output of the system is the transcribed lyrics and will be saved in %output.

## Cite
```
@inproceedings{Roa-Mirex2020,
  title={Automatic Vocal Separation and Lyrics Transcription},
  year={2020},
  booktitle={International Society for Music Information Retrieval (ISMIR)},
  author={Roa Dabike, Gerardo and Barker, Jon}
}
```

## References
```text
[1] Manuel Pariente, Samuele Cornell, Joris Cosentino, Sunit Sivasankaran, Efthymios Tzinis, 
    Jens Heitkaemper, Michel Olvera, Fabian-Robert Sẗoter, Mathieu Hu, Juan M. Martin-Donas, 
    David Ditter, Ariel Frank, Antoine Deleforge, and Emmanuel Vincent. Asteroid: the 
    PyTorch-based audio source separation toolkit for researchers. In Proc. Interspeech, 2020.

[2] Daniel Povey, Arnab Ghoshal, Gilles Boulianne, LukasBurget, Ondrej Glembek, Nagendra Goel, 
    Mirko Hannemann, Petr Motlicek,  Yanmin Qian, Petr Schwarz,Jan Silovsky, Georg Stemmer, 
    and Karel Vesely. The Kaldi speech recognition toolkit. In IEEE 2011 Workshop on Automatic 
    Speech Recognition and Understanding (ASRU), 2011.
```

