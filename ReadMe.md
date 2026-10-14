# Adversarial Attack Supplementary Code

This project contains five independent attack folders. All Python source text and comments are in English. Each folder contains an English `readme.md` with configuration and execution instructions.

| Folder | Method | Entry script | Model |
| --- | --- | --- | --- |
| [Auto_attack](Auto_attack/readme.md) | AutoAttack | `untargeted attack.py` |
| [C&W](C&W/readme.md) | Carlini-Wagner | `C&W untargeted attack Li.py` |
| [FSA](FSA/readme.md) | Frequency-based attack | `FSA untargeted attack Li.py` |
| [MI-FGSM](MI-FGSM/readme.md) | Momentum Iterative FGSM | `MI FGSM untargeted attack Li.py` |
| [PGD](PGD/readme.md) | Projected Gradient Descent | `PGD untargeted attack Li.py` |

## Configuration

Replace every `r"......"` with the required local absolute path before running. The adjacent English comment identifies its purpose. Python raw strings preserve Windows backslashes. Input images and checkpoints are external to this project. Create the output folder before running. For PGD, MI-FGSM, C&W, and FSA, configure matching checkpoint and class-index JSON paths in both the entry script and attack implementation.

The maximum number of attack iterations is 1000 per run. Successful attacks may stop earlier. The perturbation budget is `8 / 255` in [0, 1] RGB units, equivalent to at most 8 intensity levels per channel in an 8-bit image. For the custom normalized attacks, channel bounds are calculated as `eps / std`, using ImageNet standard deviations `(0.229, 0.224, 0.225)`. Conversion back to RGB also clips pixel changes to 8 levels.

The entry scripts use ImageNet normalization. PGD, MI-FGSM, FSA, and AutoAttack resize images to 224 x 224; the C&W script expects input images already sized 224 x 224. Image filenames should contain digits because the scripts sort them by their numeric portions. Output filenames follow the input filenames. Use PNG for lossless output; JPEG compression can change pixel values after saving. The custom PGD, MI-FGSM, C&W, and FSA entry scripts use CUDA directly and require a CUDA-capable PyTorch installation.

## Environment

Use Python with PyTorch, torchvision, NumPy, Pillow, and matplotlib. AutoAttack's entry script additionally imports torchattacks. Model checkpoints, class-index JSON files, and datasets must be supplied locally. Keep each folder's `attack.py` beside its entry script: imports are local to that folder.

## AutoAttack limits

APGD, APGDT, and FAB default to 1000 iterations per run; Square defaults to 1000 search iterations per restart. The ensemble can perform multiple runs, targets, restarts, and EOT evaluations. These limits do not imply a total ensemble budget of 1000 model calls. The existing AutoAttack entry script works in normalized tensor coordinates with component `eps=8/255`, then enforces an 8-level bound on exported RGB arrays. This customized pipeline should not be described as an unmodified official AutoAttack evaluation.

## Documentation filename

This root file is named `ReadMe.ma` exactly as requested. Its content uses Markdown syntax.
