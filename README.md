# mml_thing
A simple tool to automate repetitive and tedious 3mle tasks.

[download latest (v1.5.2)](https://github.com/firelight322/mml_thing/releases/latest)

### This is an old tool I wrote a long time ago for Maple Story 2 music. I am uploading it here for posterity and backup. It is not being developed anymore.

## Backup Documentation
This is a command prompt app. You cannot open it by double-clicking on the exe. You must launch it from your command prompt.

## Features
- **propagate_t** Propagate Tempo : Adds all tempos from track 1 to all other tracks. Splits notes if necessary.
- **propagate_p** Propagate Performance Tempo: Adds performance tempo to tempos on track 1.
- **propagate_v** Propagate Volume : Copies starting volume from track 1 to other tracks that don't have a start volume. If no volume is found, adds v15 to tracks without starting volume. Loud is good!
- **compress** When importing midi files with velocity, the velocity steps are often really wide and noticeable. Fixes this by narrowing the gap between velocity "steps" and makes everything louder.
- **max_velocity** Boosts or lowers velocities to match provided maximuim velocity. Keeps relative distance between velocities.
- **min_velocity** Boosts or lowers velocities to match provided minimum velocity. Keeps relative distance between velocities.
- **add_sustain_track** Adds a basic sustain track with sustain off/on every beat.
- **clean** Currently only removes endlines.
- **kick_track** Converts selected track to a kick track. All notes are converted to C2, so they work with new 3mle.
- **snare_track** Converts selected track to a snare track. All notes are converted to D2, so they work with new 3mle.
- **cymbal_track** Converts selected track to a cymbal track. All notes are converted to C3, so they work with new 3mle.
- **ms2mml** Outputs the file in Maple Story 2 format so you can "Open File" in the game's sequencer.
- **batch mode** By specifying a folder as input, the tool will process all '.mml' files in the specified folder non-recursively. All options currently work in batch mode.

## How to use
- Import your midi files in 3mle, optimize all tracks, do things, go outside and get some fresh air (lol), save when finished.
- Use this tool to post-process and convert your mml file to Maple Story 2 format (no copypasta required).

## Examples

The following applies all processes and outputs to "glou.ms2mml".
```bash
mml_thing.exe turkey.mml glou.ms2mml -e --ms2mml
```

The following converts track 1 to kick track.
```bash
mml_thing.exe harambe.mml dedmeme.ms2mml --kick_track 1 --ms2mml
```

The following will output the final file "C:\gud beats\sandstorm.out.ms2mml". You can import that file in Maple Story 2.
```bash
mml_thing.exe "C:\gud beats\sandstorm.mml" --propagate --compress --clean --ms2mml
```

You can also use short options to help your tendinitis.
```bash
mml_thing.exe "C:\gud beats\sandstorm.mml" -pvc --ms2mml
```

This will apply propagate and clean on all '.mml' files in folder "gud beats" and output the files as '.out.ms2mml' in "C:\gud beats\out\".
```bash
mml_thing.exe "C:\gud beats\" --propagate --clean --ms2mml
```

This will convert all '.mml' files in folder "my song" to '.ms2mml' format. The files will be output in the current working directory and named song1.ms2mml, song2.ms2mml, etc. If the current working folder is "my song", the ms2mml files will be output in "out/" subfolder.
```bash
mml_thing.exe "C:\my song\" song.ms2mml --ms2mml
```

## Setup
The tool doesn't require any special installation to be used, but if you use it often, the following might be nice.

- To be able to launch the tool from any folder you are working in, add the tool folder to your user 'Path' environment variable : 'System > Advanced System Settings > Environment Variables...' Edit or create user variable 'Path' and add the mml_thing folder to it. When downloading new versions, overwrite the exe in there.
- To launch a command prompt from your current directory in File Explorer, type 'cmd' in the top path bar.
- To easily copy the path of a file or folder, shift + right click a file and select 'Copy as path'.
- If you don't like command prompts gtfo scrub.﻿


## Updates
### v1.5.2
- Change tempo propagation order. Adds tempos to the end of other notes. Fixes issues like t100t110 becoming t110t100.
- Keep 3mle track ordering. This would also cause 3mle track names to be erased, which we don't like so much.

### v1.5.1
- Revert changes to ties, which broke propagate tempo. [queue sad violins] Thanks to ST for the bug report!

### v1.5.0
- Added 'max_velocity' and 'min_velocity' to give more control over velocity operations. Max/min velocity will boost or lower the maximum/minimum volume. Steps between velocities are kept as much as possible, which helps when you have tracks with big velocity gaps. You can think of this as an expander.
- Added comment support, though the output isn't always very pretty. At least we don't explode anymore.
- Optimize propagate_t to merge small denominations together (less verbose output). Shouldn't output 2 million r16& anymore.
- Changed how mml_thing handles ties and endlines to match 3mle behavior more closely.
- Fix a compress bug where it would skip some velocities.

### v1.4.0
- Added 'add_sustain_track' option, which creates a basic track with sustain toggling on beats. Only works when outputting to ms2mml.
- Added 'kick_track', 'snare_track' and 'cymbal_track' options. This will convert all notes of the selected track to the correct kick, snare, cymbal (C2, D2, C3). Nothing else is changed on the track. You will need to provide the track number. Thanks to Yasuno for the idea!
- Renamed a bunch of options again. Sorry about that, names will stop to change once I'm out of beta. 'Propagate' is split into 'propagate_t' and 'propagate_p', for tempo and performance tempo respectively. 'start_volume' renamed to 'propagate_v'. Remember you can use short-options to apply all of the propagates : '-pPv'.
- Sneakily add triplet support. Nobody seems to have noticed they were missing. Triplets are sad.
- Updated 3mle backend to new data model. Fixes some parsing issues like '&l4c' or out of range tempos.
- Removed deprecated 'propagate'.

### v1.3.0
- Added experimental 'propagate2', a rewrite of propagate. The new version will copy all tempos from the first track to the others. It will split notes appropriately. Volume propagation moved to its own option. Propagate2 will replace propagate in next major version.
- Moved propagate volume to new 'start_volume' option. Grabs the first volume from track 1 and copies it to other tracks.
- Deprecated 'propagate' (will be removed in next major version).
- Renamed 'normalize_velocity' to 'compress'. Changed its short option to '-C'.

### v1.2.0
- Added short options and "everything" option.
- Fix some path bugs.

### v1.1.0
- Added batch mode.


## Help
```
Usage: mml_thing.exe in_file out_file [options]
	Arguments:
 in_file     MML input file or folder.
             Using a folder enters batch mode, which will process all '.mml' files in the specified folder.
 out_file    MML output file.
             Optional, outputs to 'in_file.out.mml' if no argument is provided.
             Outputs to 'out/' subfolder if using batch mode and no argument is provided.
	Options:
 -p, --propagate_t           Propagate Tempo : Adds all tempos from track 1 to all other tracks.
 -P, --propagate_p           Propagate Performance Tempo : Adds performance tempos to all tempos on track 1.
 -v, --propagate_v           Propagate Volume : If track 1 has a starting volume, copies it to other tracks.
                             If not, adds v15 to beginning of tracks without volume.
 -C, --compress              Narrows the gap between velocity steps. Make everything louder.
 -M, --max_velocity <value>  Boosts or lowers velocities to match provided maximuim velocity.
                             Keeps relative distance between velocities.
 -m, --min_velocity <value>  Boosts or lowers velocities to match provided minimum velocity.
                             Keeps relative distance between velocities.
 -s, --add_sustain_track     Adds a basic sustain track with sustain on/off on beats.
                             Currently only works when exporting ms2mml.
 -c, --clean                 Additional optimizations. Currently, only removes line endings.
 -e, --everything            Apply almost all post-process operations :
                             propagate_t, propagate_p, propagate_v, compress, add_sustain_track, clean.
     --kick_track <value>    Converts selected track to a kick track. Must provide track number.
     --snare_track <value>   Converts selected track to a snare track. Must provide track number.
     --cymbal_track <value>  Converts selected track to a cymbal track. Must provide track number.
     --ms2mml                Outputs in 'ms2mml' format.
 -h, --help                  Print this help

mml_thing
version : 1.5.2 beta
```
