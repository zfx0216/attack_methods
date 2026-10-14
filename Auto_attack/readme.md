# AutoAttack

This folder runs an untargeted adversarial attack using ConvNeXt Base.

## Files

- `untargeted attack.py`: image-folder processing, model loading, attack execution, and output saving.
- `Autoattack.py`: Ensemble orchestration for standard, plus, and rand variants.
- `apgd.py`: Auto-PGD with cross-entropy or DLR loss.
- `apgdt.py`: Targeted Auto-PGD.
- `fab.py`: Fast Adaptive Boundary attack.
- `square.py`: Square black-box attack.
- `multiattack.py`: Sequential attack composition.
- `attack.py`: Shared attack interface and data utilities.

## Configuration

Replace every `r"......"` with the required local absolute path before running. The adjacent English comment identifies its purpose. Python raw strings preserve Windows backslashes. Input images and checkpoints are external to this project. Create the output folder before running. For PGD, MI-FGSM, C&W, and FSA, configure matching checkpoint and class-index JSON paths in both the entry script and attack implementation.

The maximum number of attack iterations is 1000 per run. Successful attacks may stop earlier. The perturbation budget is `8 / 255` in [0, 1] RGB units, equivalent to at most 8 intensity levels per channel in an 8-bit image. For the custom normalized attacks, channel bounds are calculated as `eps / std`, using ImageNet standard deviations `(0.229, 0.224, 0.225)`. Conversion back to RGB also clips pixel changes to 8 levels.

The entry scripts use ImageNet normalization. PGD, MI-FGSM, FSA, and AutoAttack resize images to 224 x 224; the C&W script expects input images already sized 224 x 224. Image filenames should contain digits because the scripts sort them by their numeric portions. Output filenames follow the input filenames. Use PNG for lossless output; JPEG compression can change pixel values after saving. The custom PGD, MI-FGSM, C&W, and FSA entry scripts use CUDA directly and require a CUDA-capable PyTorch installation.

AutoAttack components APGD, APGDT, and FAB default to 1000 iterations per restart and target. Square uses 1000 search iterations per restart (`n_queries=1000`); its initialization and diagnostic model evaluations are additional queries. Restart counts, target counts, and EOT repetitions are separate from this limit. Component `eps` values remain `8/255` in the supplied tensor coordinates; the existing entry script supplies ImageNet-normalized tensors, so component-space limits differ from RGB-space limits. The final RGB conversion enforces an 8-level pixel limit. This is the project's customized implementation, not an unmodified official AutoAttack benchmark. The entry script also saves a preview image; replace its annotated `r"......"` with a path ending in `.png`.

## Run

Install PyTorch, torchvision, NumPy, Pillow, and matplotlib in your Python environment. The AutoAttack entry script also imports torchattacks. From the project root:

```powershell
python "Auto_attack/untargeted attack.py"
```

Set the paths first; the placeholder value is intentionally not a usable dataset or checkpoint location.
