# (n)ASAP: the (note-)Aligned Scores And Performances dataset

(n)ASAP is a dataset of aligned musical scores and performances built by extending the [ASAP dataset](https://github.com/fosfrancesco/asap-dataset) with note-level annotations.

It contains data and annotations in the following domains:
- Musical scores (both MIDI and MusicXML)
- Performances (performance MIDI and audio)
- MusicXML score to performance note alignments that align score notes (identified by added note IDs in the musicXML file) to performed notes (= note alignments)
- Performance downbeat, beat, time signature, and key signature annotations. 
- MIDI score beat and downbeat annotations that align to the performance beat and downbeat annotations (= beat alignments)

#### Dataset Uses:

With the variety of annotations and alignments available (n)ASAP is suitable for several MIR tasks. The following is an incomplete list to help you navigate the dataset contents:

- MIDI to Score transcription (beat-based ASAP annotations: see annotations.json and *_annotations.txt files. Or note-based (n)ASAP annotations: see note_alignment.tsv file)
- Performance generation or analysis ((n)ASAP annotations: see note_alignment.tsv file)
- Audio to MIDI transcription (aligned MIDI and audio: see Maestro data)
- Audio or MIDI beat/downbeat tracking, key and time signature estimation (ASAP annotations per file: see annotations.json and *_annotations.txt files) 


This repository contains all of the annotations, as well as all of the MIDI and MusicXML files. To get the audio files, follow the instructions below.

#### Getting the Audio:
The audio files are not distributed in this repository. You can obtain them from the maestro dataset:
- Install dependencies:
  - python 3
  - librosa
  - pandas
  - numpy
- If you use conda, you can install the dependencies with:
  - `conda env create -f environment.yml`
- download the Maestro dataset v2.0.0 https://magenta.tensorflow.org/datasets/maestro
- unzip the data
- run `python initialize_dataset.py -m [maestro location]`
- The maestro directory and zip file can now be safely deleted

The script has been tested in Windows, Linux and Mac OS with python 3.6, and the libraries librosa v0.7.2 and pandas v1.0.3.



## Dataset Content
ASAP  contains  **236  distinct  musical  scores**  and  **1067  performances**  of  Western  classical  piano  music from 15 different composers (see Table below for a breakdown).

| Composer     	| MIDI Performance | Audio Performance	| MIDI/XML Score 	|
|--------------	|--------------	| ----------------------|-------------	|
| Bach         	| 169         	| 152       | 59    	|
| Balakirev    	| 10           	| 3        |1     	|
| Beethoven    	| 271          	| 120       |57   	  |
| Brahms       	| 1            	| 0        |1     	|
| Chopin       	| 289         	| 108       |34    	|
| Debussy      	| 3            	| 3        |2     	|
| Glinka       	| 2            	| 2        |1     	|
| Haydn        	| 44           	| 16        |11    	|
| Liszt        	| 121          	| 48        |16    	|
| Mozart       	| 16           	| 5        |6     	|
| Prokofiev    	| 8            	| 0        |1     	|
| Rachmaninoff 	| 8           	| 4        |4     	|
| Ravel        	| 22           	| 0        |4     	|
| Schubert     	| 62           	| 44        |13    	|
| Schumann     	| 28           	| 7        |10    	|
| Scriabin     	| 13           	| 7        |2     	|
| **Total**     | 1067           |519      | 222    |

<!--548 of the recordings are available as MIDI only, and others (520) are provided as MIDI and audio recordings  aligned  with  approximately  3  ms  precision.    Each score corresponds with at least one performance (and usually more). Each  musical  score  is  provided  in  both  MusicXML and MIDI formats.  In the MIDI score, the position of all MIDI events are quantized to a metrical grid according to their position in the MusicXML score.  The aligned time signature and tempo change events ensure that the metrical grid of the MIDI score is identical to that of the corresponding  MusicXML  score,  aligning  with  abrupt  time417signature  and  tempo  changes,  as  well  as  gradual  tempo changes such as ritardando and accelerando.  Grace notes are represented in MIDI as notes of very short duration. Repetitions  in  the  score  are  “unfolded”  in  the  MIDI  file such that some sections of the MusicXML score may be duplicated  in  the  MIDI  score.    Except  for  performance mistakes,  there  is  a  one-to-one  correspondence  between424the notes in a MIDI performance and its associated MIDI score.-->


Scores and performances are distributed in a folder system structured as `composer/subgroup/piece` where the `piece` directory contains the XML and MIDI score, plus all of the performances of a specific piece. `subgroup` contains additional hierarchies (e.g., `Bach` contains subgroups `Fugues` and `Preludes`).

A metadata CSV file is available in the main folder with  information about file correspondances, and the title and composer of each performance.

Annotations are given in tab-separated-values (TSV) files in the same folder as each performances, named as `{basename(file)}_annotations.txt`. These TSV  files can be read by Audacity (File->Import->Labels) to view the annotations on a corresponding audio perfomance. Paired MIDI and audio performances correspond to the same annotation file since they are exactly aligned.

A single json in the main folder also contains all of the annotation information for every file in ASAP.

It is possible that 2 versions of the same piece are in different folders if they refer to slighlty different scores (e.g. different repetitions in the performance). Even in this case, the composer and title in the metadata table will be the same for both performances. For the applications where unique pieces are needed (e.g., to create a training/test dataset with not overlapping) look for the unique couple `(title,composer)`. Note that this is not the same as considering all the unique xml-score names, since in order to deal with different repetitions in performances, 2 different xml-scores may refer to the same piece (e.g., "Beethoven/Piano_sonatas/17_1/xml_score.musicxml" and "Beethoven/Piano_sonatas/17_1_no_repeat/xml_score.musicxml").


### Metadata table
Each row in `metadata.csv` file contains the following information:

- **composer**: a common name for the composer of a piece
- **title**: a title that (along with composer) uniquely identifies each piece in the dataset
- **folder**: the path of the directory containing all scores and performances of the piece
- **xml_score**: the path of the MusicXML score of a piece
- **midi_score**: the path of the score in MIDI format 
- **midi_performance** : the path of the MIDI performance
- **performance_anotations** : the path of the TSV annotations on the performances (audio and MIDI) 
- **midi_score_annotations** : the path of the TSV annotations of the MIDI score
- **maestro_midi_performance** : the path of the MIDI performance in the MAESTRO dataset (if it exists)
- **maestro_audio_performance** : the path of the audio performance in the MAESTRO dataset (if it exists)
- **start**: If not blank, specifies a time in seconds where the original MAESTRO performance (audio and MIDI) has been cut to match the annotations. The ASAP performance at time 0 will match the original MAESTRO performance at this time
- **end**: The same as start, but for the end of the original MAESTRO performance. The end of the ASAP performance is at this time in the original MAESTRO performance
- **audio_performance**: the path of the (properly cut) audio file in the ASAP dataset (if one exists)
- **robust_note_alignment**: the robustness of the note alignment(1 = robust, 0 = not robust)
- **note_alignments**: the path of the TSV with alignments between score and performance notes 
- **parangonada_data_folder**: the path of the directory containing the files for visualization and checking with Parangonada
- **match_file**: the path of the match file with alignments between score and performance notes 


### Annotation json
In `asap_annotations.json` the first group of keys are the path of the MIDI performance files in ASAP.
Every performance has the following keys:
- **performance_beats** : a list of beat positions in seconds
- **performance_downbeats** : a list of downbeat positions in seconds
- **performance_beats_type** : a dictionary where the annotation time is the key and the value is either "db"(downbeat), "b" (beat) or "bR" (in cases where standard notation rules are not followed---e.g. rubato or pitckup measures in the middle of the score---and we cannot determine the exact beat position)
- **perf_time_signatures** : a dictionary where the annotation time is the key and the value is a couple [`time_signature_string`,`number_of_beats`]. `number_of_beats`  is an integer specifying the number of beats per measure. For compound meters, there is not a universal agreement on the number of beats; here we refer to  https://dictionary.onmusic.org/appendix/topics/meters, and annotate e.g., 6/8 with 2 beats per measure.} 
- **perf_key_signatures** :  a dictionary where the annotation time is the key and the value is a couple [`key_signature_number`,`number_of_sharps`]. The first value is an integer in [0,11] marking the tonic of the key signature (if it is major) with C = 0. The second is an integer in [-11,11] where negative numbers mark the number of flats and positive numbers mark the number of sharps. Note that we only annotate the key \emph{signature}, and not the key, since modes are not annotated in scores, so we cannot distinguish between, e.g., a major key and it's relative minor.
- **midi_score_beats** : same as perf_beats, but on the MIDI score
- **midi_score_downbeats** :  same as perf_downbeats, but on the MIDI score
- **midi_score_beats_type** : same as perf_beats_type, but on the MIDI score
- **midi_score_time_signatures** : same as perf_time_signatures, but on the MIDI score
- **midi_score_key_signatures** : same as perf_key_signatures, but on the MIDI score
- **downbeats_score_map** : a list of measure numbers in the score (starting from 0, including any pickup measures) corresponding to each downbeat in the MIDI score. For example, if the first number in this list is 1, that means that the first annotated downbeat corresponds to measure number 1 in the score. In case multiple score measures correspond to the same downbeat (e.g., for a measure that is split in the score for some reason), the values in the list are a string in the form "number1-number2-number3-..."
- **score_and_performance_aligned** : True if the MIDI score and the performance have the same number of annotations. If this is false, the performance can still be used for beat tracking tasks, but not for full transcription. It is False for only 29 performances where some beats are skipped by the performer due to a mistake or an incomplete performance.


### TSV annotation
For each performance and MIDI score we provide a tab-separated value (TSV) file of the position (in seconds) of each beat, downbeat, time signature change, and key signature change.  These files can be read by Audacity to view the annotations on a corresponding audio perfomance. Each line in the TSV file begins with 2 identical columns containing the time in seconds corresponding to the annotation, followed by a 3rd column containing the label:
- **Beats and downbeats**: annotated with either `b` (for beats), `db` (for downbeats) or `bR` (in cases where standard notation rules are not followed---e.g. rubato or pitckup measures in the middle of the score---and we cannot determine the exact beat position)
- **time signature changes**: the time signature as a string (e.g., `4/4`).
- **key changes** (k in [-11,11]): the number of accidentals in the key signature, where 0 is none, positive numbers count sharps and negative numbers count flats.


### Note alignment TSV annotation
For each MIDI performance and MusicXML score we provide a tab-separated value (TSV) file encoding a note-wise alignment of these files.

- **xml_id**: The note ID of the score note in the MusicXML file. To uniquely identify notes that might be repeated (but only notated once in the score), the ID is suffixed with "-REPEAT_NUMBER", most commonly "-1" and "-2".
- **midi_id**: The unique note ID in the MIDI performance. Note IDs are directly inferred if loading using [partitura](https://github.com/CPJKU/partitura) using the `load_performance_midi` method. MIDI notes are also uniquely identified by the information given in the next 4 columns:
- **track**: The track number of the MIDI note.
- **channel**: The channel number of the MIDI note.
- **pitch**: The pitch of the MIDI note.
- **onset**: The onset time of the MIDI note.


## Note Alignments Usage example


We use [Partitura](https://github.com/CPJKU/partitura) as file I/O utility. Note alignments (tsv files) can be parsed into python lists.

```python
import partitura as pt

# load note alignments of the (n)ASAP dataset: 
alignment = pt.io.importparangonada.load_alignment_from_ASAP(filename= 'path/to/note_alignment.tsv')
# load scores of the (n)ASAP dataset: 
score = pt.load_score(filename= 'path/to/xml_score.musicxml')
# load performance of the (n)ASAP dataset: 
performance = pt.load_performance_midi(filename= 'path/to/performance_name.mid')

# sometimes scores contain repeats that need to unfolded to make the alignment make sense
part = pt.score.merge_parts(score)
unfolded_part = pt.score.unfold_part_maximal(part)

# to get numpy arrays of the score and performance for downstream processing without partitura:
score_array = part.note_array()
performance_array = performance.note_array()
```

There is a [tutorial](https://cpjku.github.io/partitura_tutorial/) and [documentation](https://partitura.readthedocs.io/en/latest/) for Partitura that can help you get started!

Besides the tsv alignments, (n)ASAP contains files ready for visualization and checking with the web tool [Parangonda](https://sildater.github.io/parangonada/). Go to the website and upload the csv files to visualize a note alignment. Partitura handles import and export for files created for and by Parangonada:


```python
import partitura as pt
# export a note alignment for visualization with parangonada:
pt.io.save_parangonada_csv(alignment, 
                            performance_data, # this is a note array or Performance object as created above
                            score_data, # this is a note array or Score object as created above
                            outdir="path/to/dir")

# import a corrected note alignment from parangonada:
alignment = pt.io.importparangonada.load_parangonada_alignment(filename= 'path/to/note_alignment.csv')
```


## Information

#### Dataset History:

(n)ASAP is the product of several iterative improvements:
- performance MIDI and audio recordings from the [Yamaha piano-e-competition](http://piano-e-competition.com)
- assembled and curated by Hawthorne et al. as [Maestro dataset](https://magenta.tensorflow.org/datasets/maestro#dataset)
- augmented with scores and by preliminary alignments by Jeong et al. and on [here on github](https://github.com/mac-marg-pianist/chopin_cleaned)
- curated, beat-aligned, and annotated by Foscarin et al. as [ASAP dataset](https://github.com/fosfrancesco/asap-dataset)
- note-aligned by Peter et al. and published as (n)ASAP dataset.

#### Citing
If you use the (n)ASAP dataset in any research, please cite: 

```
@article{Peter-2023,
 title = {Automatic Note-Level Score-to-Performance Alignments in the ASAP Dataset},
 author = {Peter, Silvan David and Cancino-Chacón, Carlos Eduardo and Foscarin, Francesco and McLeod, Andrew Philip and Henkel, Florian and Karystinaios, Emmanouil and Widmer, Gerhard},
 doi = {10.5334/tismir.149},
 journal = {Transactions of the International Society for Music Information Retrieval {(TISMIR)}},
 year = {2023}
}
```
If you use any of the datasets on which we built (n)ASAP, please cite the relevant publications.

#### Limits of the dataset
- The scores were written by non-professionals, and although we manually corrected them to filter out most of the incorrect notation, they still present some problems. Our suggestion is to use the corrected annotations provided in the TSV and in the JSON files rather than extracting them again from the score.
- In certain cases (e.g., pickup measures in the middle of the score, rubato measures, complex embellishments) it is not possible to estabilish the position of the beat from the score. In our annotations we choose to mark those beats as "bR".
- 31 performances are not aligned with the score. The cause is an incomplete performance or the player missing some beats due to a mistake. Altough not good for AMT and score production, those performances were manually checked and are still usable for beat/downbeat tracking. This information is in the json file.

#### License
The dataset is made available under a [Creative Commons Attribution Non-Commercial Share-Alike 4.0 (CC BY-NC-SA 4.0) license](https://creativecommons.org/licenses/by-nc-sa/4.0/).

#### Versions
- The initial version of (n)ASAP v2.0 (as it was released for TISMIR in 2023) is based on ASAP [v1.1](https://github.com/fosfrancesco/asap-dataset/releases/tag/v1.1).
- The second version of (n)ASAP v2.1 (May 2025) merges the changes made in ASAP [v1.2](https://github.com/fosfrancesco/asap-dataset/releases/tag/v1.2). It also fixes 7 scores, and 18 alignments.
