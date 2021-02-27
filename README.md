# ebu-norm & tp-norm
Scripts to batch normalize files to integrated or true peak targets

Prerequisites: ebur128, sox

### Process
Files are analyzed by ebur128 with the required gain passed to SoX. 
+/− gain is calculated by the target level minus the analyzed integrated or peak value.
In the case of ebu-norm, this takes place post-limiting to ensure that the exact integrated value is reached.
Files are written to a sub-folder with suffix added to filename.

### Usage: 
```shell
ebu-norm [-t target_value] infiles
```
where ```-t``` allows for an integrated target other than -23 LUFS.

As of v.0.3, if true peaks rise above -1 dBTP, a SoX limiter chain is engaged.

```shell
tp-norm [-t target_value] infiles
```
where ```-t``` allows for an true peak target other than -1 dBTP.

#### Examples

```shell
ebu-norm AudioFolder/*.wav
```
will create an ebu-norm sub-folder and create -23 LUFS integrated WAV files (default).

```shell
ebu-norm -t -20 AudioFolder/*.flac 
```
will create an ebu-norm sub-folder and create -20 LUFS integrated FLAC files. 

```shell
tp-norm AudioFolder/*.wav
```
will create a tp-norm sub-folder and -1 dBTP WAV files (default). 


```shell
tp-norm -t -2 AudioFolder/*
```
will create a tp-norm sub-folder and -2 dBTP for all media files present. 
