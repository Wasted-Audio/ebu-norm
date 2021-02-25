# ebu-norm
A script to batch normalize files to EBU R 128 (-23 LUFS Integrated) 
or another specified integrated target

Prerequisites: ebur128, sox

### Process
Files are analyzed by ebur128 with the required gain passed to SoX. 
+/− gain is calculated by the target level minus the analyzed integrated value.
Files are written to a "Normalized" sub-folder with `-normalized` added to filename.

### Usage: 
```shell
ebu-norm [-t target_value] infiles
```
where ```-t``` allows for an integrated target other than -23 LUFS.

#### Examples

```shell
ebu-norm AudioFolder/*.wav
```
will create a Normalized sub-folder and create -23 LUFS integrated WAV files.

```shell
ebu-norm -t -20 AudioFolder/*.flac 
```

will create a Normalized sub-folder and create -20 LUFS integrated FLAC files. 

### To-do List

- [ ] Add SoX limiter function for positive gain instances
- [x] Add verbose output showing filename and applied gain
- [ ] Add error message if no target value added with `-t`
- [ ] Add dBTP target option
