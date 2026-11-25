<p align="center">
  <img src="snapshot-wmc-figures.png" alt="White matter tracts overlaid on a T1-weighted image" width="750">
</p>

<h1 align="center">White matter tract figures</h1>

<p align="center">
  Axial, sagittal and coronal figures of every white matter tract in a classification (WMC), overlaid on a T1-weighted image, for visual quality control.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.607"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.607-blue.svg" alt="Run on brainlife.io"></a>
  <a href="https://github.com/brainlife/app-wmc_figures/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> 📍 **This branch, `1.1`, is the brainlife.io App Generate figures of white matter tracts overlaid on anatomical image** ([doi:10.25663/brainlife.app.607](https://doi.org/10.25663/brainlife.app.607)).

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Renders axial, sagittal and coronal images of each white matter tract in a WMC classification overlaid on a T1-weighted image, for visual quality control.**

For every tract in the classification (or the listed subset) and for all tracts together, draws the streamlines with FURY over T1 slices from six viewpoints and saves the images. Use it to check tract segmentation results visually. The tractogram and T1 must be in the same space. Visualisation only.

| | |
|---|---|
| 📥 **Inputs** | `neuro/wmc` · `neuro/anat/t1w` |
| 📤 **Outputs** | `generic/images` |
| 🐳 **Container** | `brainlife/dipy:1.4.1` |
| ⚙️ **Resources** | 1 CPUs · 1 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.607](https://doi.org/10.25663/brainlife.app.607) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `wmc` | `neuro/wmc` | yes | White Matter Classification structure |
| `t1` | `neuro/anat/t1w` | yes | T1 image |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `images` | `generic/images` | Images of the specified tracts, 6 figures each |

<details>
<summary><b>⚙️ Parameters</b> (6)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `coronal` | integer |  | If you want to specify which slice to use in the coronal plane, set that value here (ex. 128). Otherwise, the app will identify the mid-coronal slice. |
| `sagittal` | integer |  | If you want to specify which slice to use in the sagittal plane, set that value here (ex. 128). Otherwise, the app will identify the mid-sagittal slice. |
| `axial` | integer |  | If you want to specify which slice to use in the axial plane, set that value here (ex. 128). Otherwise, the app will identify the mid-axial slice. |
| `img_max` | integer |  | This field will set the maximum intensity brightness of the anatomical slice. The higher the value, the brighter the image. Warning: default value is img_max=3 for branch 1.0 and original T1 max value for branch 1.1. |
| `img_min` | integer |  | This field will set the minimum intensity brightness of the anatomical slice. The higher the value, the more saturated (i.e. less dynamic range) the image. Warning: default value is img_min=0.5 for branch 1.0 and original T1 min value for branch 1.1. |
| `tracts` | string |  | Only output images of specified set of tracts. Tract codes must be listed with a white space separating them, e.g. 'leftArcuate rightArcuate'. It is important that the codes listed here match EXACTLY with the names listed in the classification structure. If left blank, all the tracts will be considered (default). For tract codes derived from common segmentations available on brainlife, please refer to the README. |

</details>

<!-- brainlife-app:end -->

## 🚀 Running the App

For each tract, the App draws six figures: axial, axial flipped, sagittal left, sagittal right, coronal and coronal flipped. It also draws the same six figures with all the tracts together.

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.607](https://doi.org/10.25663/brainlife.app.607), choose a White Matter Classification (`neuro/wmc`) and a T1-weighted image in the same space, set the options and press **Execute**. The figures are archived to your project as a `generic/images` dataset and can be browsed on the task page.

- **`axial`, `sagittal`, `coronal`**: the T1 slice shown in each plane. Leave empty to use the middle slice. For HCP T1 images that have not been re-ACPC aligned, coronal = 85, sagittal = 74, axial = 48 work well; for T1 images aligned with the ACPC alignment App on brainlife.io, coronal = 105, sagittal = 89, axial = 65 are a good starting point.
- **`img_min`, `img_max`**: the displayed T1 intensity range, (mean − `img_min` × std, mean + `img_max` × std) over non-zero voxels. Set both or neither; when neither is set the full T1 range is used. Adjust them if the T1 looks too dark or too light.
- **`tracts`**: only draw these tracts, as space-separated codes that match the names in the classification exactly (for example `leftArcuate rightArcuate`). Leave empty to draw all tracts. Codes for common segmentations on brainlife.io:
  - AFQ (Yeatman, 2012): [TRACT_CODES_AFQ](TRACT_CODES_AFQ.md)
  - TractSeg (Wasserthal, 2018): [TRACT_CODES_TRACTSEG](TRACT_CODES_TRACTSEG.md)
  - White Matter Anatomy Segmentation (Bullock et al., 2019): [TRACT_CODES_WMASEG](TRACT_CODES_WMASEG.md)

### 💻 Locally

You need [Singularity/Apptainer](https://apptainer.org/); `main` runs `main.py` with FURY and nibabel in `brainlife/dipy:1.4.1`.

1. Clone this repository and check out the registered version (`git checkout 1.1`).
2. Create `config.json` with the paths to your inputs (`wmc` is the `tracts/` directory of the classification):

   ```json
   {
       "anat": "/input/t1/t1.nii.gz",
       "wmc": "/input/wmc/tracts",
       "axial": "",
       "sagittal": "",
       "coronal": "",
       "img_min": "",
       "img_max": "",
       "tracts": ""
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

### 🧪 Sample data

Download a classification and a T1-weighted image from brainlife.io with the [brainlife CLI](https://github.com/brainlife/cli).

## 📦 Outputs

```text
images/
├── <tract>_<view>.png           six views per tract (axial, sagittal_left, sagittal_right, coronal, and flipped axial and coronal)
└── alltracts_<view>.png         the same views with all tracts together
images.json                      list of the figures with names and descriptions, shown on brainlife.io
```

## 🩺 Troubleshooting

- **`error: both or none img_min and img_max should be defined.`**: set both intensity limits, or leave both empty.
- **Tracts misplaced on the anatomy**: the classification's tractogram and the T1 image must be in the same space.
- **No figures for a listed tract**: the codes in `tracts` must match the tract names in the classification exactly.

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Generate figures of white matter tracts overlaid on anatomical image. brainlife.io App. [doi:10.25663/brainlife.app.607](https://doi.org/10.25663/brainlife.app.607)
- Garyfallidis et al. Dipy, a library for the analysis of diffusion MRI data. *Frontiers in Neuroinformatics* 8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## 👥 Authors

- Lindsey Kitchell
- [Brad Caron](https://github.com/bacaron)
- [Giulia Bertò](https://github.com/giulia-berto)
- [Anibal Sólon](https://github.com/anibalsolon)
- [Franco Pestilli](https://github.com/francopestilli) (project director)

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

<sub>This material is based upon work supported by the National Science Foundation Graduate Research Fellowship under Grant No. 1342962. Any opinion, findings, and conclusions or recommendations expressed in this material are those of the authors(s) and do not necessarily reflect the views of the National Science Foundation.</sub>

## ⚖️ License

[MIT](https://github.com/brainlife/app-wmc_figures/blob/master/LICENSE).
