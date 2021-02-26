# ebu-norm & dbtp-norm
Scripts to batch normalize files to integrated or true peak targets

Prerequisites: ebur128, sox

### Process
Files are analyzed by ebur128 with the required gain passed to SoX. 
+/− gain is calculated by the target level minus the analyzed integrated value.
Files are written to a sub-folder with suffix added to filename.

### Usage: 
```shell
ebu-norm [-t target_value] infiles
```
where ```-t``` allows for an integrated target other than -23 LUFS.

```shell
dbtp-norm [-t target_value] infiles
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
dbtp-norm AudioFolder/*.wav
```
will create a dbtp-norm sub-folder and -1 dBTP WAV files (default). 


```shell
dbtp-norm -t -2 AudioFolder/*
```
will create a dbtp-norm sub-folder and -2 dBTP WAV files. 

### To-do List

- [ ] Add SoX limiter function for positive gain instances
- [x] Add verbose output showing filename and applied gain
- [x] Add error message if no target value added with `-t`
- [x] Add dBTP target option
