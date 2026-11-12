JET-Piano
This repository contains Python source code, configuration files, command-line scripts, tests, and documentation for automatic piano transcription and performance velocity prediction with JET-Piano.
Package Contents
Path	Contents
src/jet_piano/	Model components, data processing, training utilities, inference, and evaluation modules.
scripts/	Command-line scripts for data preparation, training, calibration, evaluation, analysis, and package validation.
configs/	Base settings, five fold-specific configuration files, and an ablation configuration matrix.
docs/	Data preparation instructions, reproduction documentation, baseline integration notes, and output-to-manuscript mapping.
examples/	Example MAESTRO and MusicNet manifest CSV files illustrating the expected metadata format.
manifests/	A README describing the manifest directory and locally generated metadata files.
logs/simulated_reference/	Labelled simulated reference files illustrating log, table, and plotting schemas. These files are not empirical experiment outputs.
logs/synthetic_smoke/	Synthetic smoke-test documentation and a package validation record.
tests/	Tests for grouped folds, Hann fusion, decoder constraints, and calibration.
.github/workflows/	A continuous integration workflow.
requirements.txt	Python dependency specifications.
environment.yml	Conda environment specification.
pyproject.toml	Package metadata, dependencies, command-line entry point, and development tool settings.
Makefile	Convenience commands.
.gitignore	Git exclusion rules.
LICENSE	MIT license for the repository code.
CITATION.cff	Citation metadata.
MANIFEST.sha256	File checksum manifest supplied with the package.


Source Code
Data Processing: src/jet_piano/data/
File	Contents
manifests.py	MAESTRO and MusicNet manifest construction, text normalization, and audio/MIDI metadata extraction.
folds.py	Grouped fold assignment and checks for overlap across folds.
features.py	Log-Mel and Bark-based acoustic feature extraction.
normalization.py	Robust feature normalization statistics.
datasets.py	Audio loading, segment construction, MIDI target preparation, a MAESTRO dataset class, and batch collation.


Model Components: src/jet_piano/models/
File	Contents
jet_piano.py	The integrated JETPiano model and its encoding, forward, and decoding interfaces.
front_end.py	Convolutional acoustic front-end blocks.
dtat.py	The dual-timescale acoustic tokenizer and its layer implementation.
pooling.py	Onset-guided event pooling.
paces.py	The PACES structured decoder and Viterbi-related data structures.
cerd.py	The CERD module, residual projection, time embeddings, and cross-covariance penalty.
crnn.py	A CRNN backbone.


Training: src/jet_piano/training/
File	Contents
trainer.py	Training steps, candidate construction, attribute targets, audio augmentation, and gradient accumulation utilities.
losses.py	Training loss functions.
group_dro.py	Group distributionally robust optimization utilities.


Inference: src/jet_piano/inference/
File	Contents
fusion.py	Hann-weighted fusion of overlapping window outputs.
ensemble.py	State-dictionary averaging and external checkpoint aggregation.
events.py	Event records and conversion of decoded state paths to note events.
calibration.py	Temperature scaling, split-conformal velocity intervals, calibration error, and selective-risk utilities.
pipeline.py	Recording encoding, fused-output aggregation, decoder parameter averaging, decoding, and checkpoint loading.


Evaluation: src/jet_piano/evaluation/
File	Contents
matching.py	Note-event matching utilities.
metrics.py	Recording-level transcription and attribute metrics, and summary functions.
statistics.py	Cluster bootstrap comparisons, paired Wilcoxon tests, Holm adjustment, and rank-biserial effects.
robustness.py	Noise, equalization, and contiguous frame-mask transformations.
interpretability.py	Pitch and velocity probing utilities.
efficiency.py	Benchmarking utilities.
io.py	MAESTRO and MusicNet reference annotation readers.


The package also includes config.py for configuration handling, utils.py for shared utilities, and cli.py for the jet-piano command-line entry point.
Command-Line Scripts
Script	Purpose
build_manifests.py	Construct dataset metadata manifests.
make_folds.py	Create grouped fold assignments.
train_fold.py	Train a model for a specified fold.
calibrate_fold.py	Run fold-specific calibration.
evaluate_internal.py	Evaluate internal out-of-fold predictions.
evaluate_external.py	Evaluate external MusicNet recordings.
run_baselines.py	Import and rescore baseline note-event exports through a common interface.
run_ablation.py	Run variants defined in the ablation configuration.
run_robustness.py	Run evaluation under the implemented input perturbations.
run_statistics.py	Perform paired statistical comparisons.
run_interpretability.py	Run model representation probes.
run_efficiency.py	Run computational benchmarking.
make_figures.py	Generate figures from supplied output files.
synthetic_smoke.py	Exercise code paths using synthetic inputs.
validate_package.py	Check package structure, reference-log labels, anonymity, and optional manifests.


All scripts listed above are located in scripts/. The baseline script provides an event-export rescoring interface; third-party baseline implementations are not bundled in the package.
Configuration Files
- configs/base.yaml: data processing, model, training, validation, external inference, and efficiency settings.
- configs/folds/fold_1.yaml through fold_5.yaml: fold-specific settings.
- configs/ablations.yaml: ablation variant settings.
Documentation
File	Contents
docs/DATA_PREPARATION.md	Dataset organization, manifest schemas, and preparation procedures.
docs/REPRODUCIBILITY_CHECKLIST.md	Reproduction workflow checklist.
docs/AI_REPRODUCTION_GUIDE.md	Guidance for AI-assisted code reproduction and inspection.
docs/BASELINES.md	Baseline event-export format and integration requirements.
docs/RESULT_MAPPING.md	Mapping between output artifacts and manuscript tables and figures.


Included Tests
- tests/test_grouped_folds.py: grouped fold separation.
- tests/test_hann_fusion.py: overlapping-window fusion.
- tests/test_paces_constraints.py: structured decoder constraints.
- tests/test_calibration.py: calibration utilities.
Data and Supporting Files
The package contains example metadata schemas and code for working with MAESTRO and MusicNet. Raw audio, MIDI files, MusicNet annotations, trained model weights, and third-party baseline checkpoints are not included.

The supplied log directories contain labelled simulated reference artifacts and synthetic smoke-test materials. This README describes their contents without reporting experimental measurements.
License
The repository code is distributed under the MIT License in LICENSE. External datasets and third-party software retain their respective licenses.
