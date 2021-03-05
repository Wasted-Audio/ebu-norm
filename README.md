# ebu-norm | tp-norm | loudmax-norm | lsp-norm | x42-norm
Scripts to batch normalize files to integrated or true peak targets

Prerequisites: `ebur128`, `sox`

Fully supported file inputs: wav, aiff (or aif), flac, ogg.  
Mp3, opus and wavpack are first converted to wav before normalizing.

### Process
Files are analyzed by `ebur128` with the required gain passed to `SoX`. 
+/− gain is calculated by the target level minus the analyzed integrated or peak value.
In the case of `ebu-norm`, this takes place post-limiting to ensure that the exact integrated value is reached.
Files are written to a sub-folder with suffix added to filename.

N.B. `loudmax-norm`, `lsp-norm` and `x42-norm` are essentially `ebu-norm` but using Loudmax, LSP limiter or x42 limiter (aka dpl.lv2) respectively as the sole limiter instead of the compand chain. A major benefit is being able to get very close to, or precisely hit, -1 dBTP. It probably goes without saying that `loudmax-norm`, `lsp-norm` and `x42-norm` require Loudmax, LSP Plugins LADSPA to be installed in `/usr/lib/ladspa`; `x42-norm` requires dpl.lv2 (https://x42-plugins.com/x42/x42-limiter) and lv2file (https://github.com/jeremysalwen/lv2file). A user-friendly variable near top of each script is present to allow for the more transparent / less aggressive Loudmax, LSP and x42 limiting algorithms. Essentially, if you find you have true peak overage with problematic files, try setting the variables to -1.2 or even -1.5.

With all these scripts, limiting—true peak or otherwise—is no substitute for correctly mastered files in terms of dynamics. If you find yourself applying more than a couple of dB of peak limiting, perhaps it is a sign to return to the original file and re-mix/master.

### Usage: 
```shell
ebu-norm [-t target_value] infiles
```
where ```-t``` allows for an integrated target other than -23 LUFS.

As of v.0.3, if true peaks rise above -1 dBTP, a `SoX` limiter chain is engaged.

```shell
tp-norm [-t target_value] infiles
```
where ```-t``` allows for an true peak target other than -1 dBTP.

#### Examples

```shell
ebu-norm AudioFolder/*.wav
```
will create an `ebu-norm` sub-folder and create -23 LUFS integrated WAV files (default).

```shell
ebu-norm -t -20 AudioFolder/*.flac 
```
will create an `ebu-norm` sub-folder and create -20 LUFS integrated FLAC files. 

```shell
tp-norm AudioFolder/*.wav
```
will create a `tp-norm` sub-folder and -1 dBTP WAV files (default). 


```shell
tp-norm -t -2 AudioFolder/*.flac
```
will create a `tp-norm` sub-folder and -2 dBTP FLAC files. 

# ebu-scan
Script to batch analyze audio files and print true peak and various loudness values to screen and text file.

Prerequisites: `ebur128`

Fully supported file inputs: wav, aiff (or aif), flac, ogg, opus.  
Mp3 and wavpack are first converted to wav before normalizing.

#### Example

```shell
ebu-scan AudioFolder/*.wav
```
Sample formatted terminal output (and also written to analysis.txt):
```shell
File                     True Peak  Integrated  Short-term  Momentary  LRA
                         (dBTP)     (LUFS)      (LUFS)      (LUFS)     (LU)
02 Ubi Caritas.wav       -3.2       -21.7       -14.4       -13.1      15.8
07 Bring Him Home.wav    -9.0       -25.0       -18.6       -17.5      11.5
08 Steal Away.wav        -7.2       -22.4       -16.7       -15.4      12.9
09 Lullaby.wav           -2.5       -22.4       -15.0       -11.8      14.7
11 Over the Rainbow.wav  -6.8       -21.9       -16.6       -14.8      11.9

```

# ebu-plot (alpha)
Wrapper script to batch produce PNG loudness graphs using PowerShell. Will move to native bash script for v2.0

Prerequisites: `pwsh` PowerShell, `ffmpeg`, `gnuplot` 

Fully supported file inputs: wav, aiff (or aif), flac, ogg, opus, mp3 and wavpack 

#### Example

```shell
ebu-scan AudioFolder/Sympathique.flac
```

