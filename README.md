# Rat rsfMRI Preprocessing Toolbox
Preprocessing codes for the rat rs-fMRI database www.nitrc.org/projects/rat_rsfmri. 

## Prerequisites:
1. GIFT ICA toolbox. (https://www.nitrc.org/projects/gift)
2. jsonlab. (https://www.mathworks.com/matlabcentral/fileexchange/33381-jsonlab-a-toolbox-to-encode-decode-json-files)
3. Tools for NIfTI and ANALYZE image (https://www.mathworks.com/matlabcentral/fileexchange/8797-tools-for-nifti-and-analyze-image)
4. ANTS. (http://stnava.github.io/ANTs/)
5. export_fig. (https://github.com/altmany/export_fig)

## Preprocessing steps
1. despiking.m  
*Discard frames with excessive motion in data (aka motion scrubbing).*
2. alignment\_checking\_tool.m  
*A graphcial user interface (GUI) to manually coregister a rsfMRI scan to a template.*
3. motion\_correction.m
*Motion correction (save motion parameters);*  
4. ica_cleaning.m  
*Run independent component analysis (ICA) with the GIFT ICA toolbox to identify noisy independent components (ICs);*  
5. copy\_ica\_results\_4labeling.m  
*Copy ICA results to an temporary folder for the ease of manual labeling.*  
*Save spatial maps, time courses, and frequency spetrums of the ICs to figures.*  
6. ica_cleaning_view.m  
*A GUI to manually label the ICs.*  
7. copy\_labels.m  
*Copy IC labels to the database folder.*  
8. last_steps.m  
*Soft IC-regressing warped images, along with the motion parameters and the [CompCor](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2214855/) regressors;*  
*Spatial smoothing.*
*Temporal filtering.*  

## Database folder structure
```bash
./
├── rat001
│   ├── rat001_info.json  [Sequence names, acquisition dates, number of frames, and corresponding names inside folders]
│   ├── raw  [Nifti files converted from raw Bruker data using Bru2Nii (https://github.com/neurolabusc/Bru2Nii)]
│   │   ├── X2P1.nii
│   │   ├── X4P1.nii
│   │   ├── X7P1.nii
│   │   ├── X8P1.nii
│   │   └── ...
│   ├── rfmri_unprocessed  [rsfMRI scans from the folder 'raw']
│   │   ├── 01.nii
│   │   ├── 02.nii
│   │   ├── 03.nii
│   │   └── 04.nii
│   ├── rfmri_intermediate  [Intermediate files generated from preprocessing] 
│   │   ├── 01_despiked.json	[Contains framewise displacements, scrubbing criterion, and scrubbed frames]
│   │   ├── 01_despiked.nii.gz  [Not further processed since more than 10% of the frames were motion-scrubbed]
│   │   ├── 02_despiked.json
│   │   ├── 02_despiked.nii.gz  [Despiked image] 
│   │   ├── 02_registered.json	[Contains rigid-body registration matrix]
│   │   ├── 02_registered.nii.gz  [Manually coregistered image] 
│   │   ├── 02_motioncorrected.json
│   │   ├── 02_motioncorrected.nii.gz  [Motion corrected image]
│   │   ├── 02_warped.json
│   │   ├── 02_warped.nii.gz  [Warped image]
│   │   ├── 02_warp_field.nii.gz  [Deformation field]
│   │   ├── 02_warp_affine.txt  [Affine transformation applied with the deformation field]
│   │   ├── 02_motion.json
│   │   ├── 02_motion.txt  [Motion parameters]
│   │   ├── 02.gift_ica  [Results from single-scan ICA]
│   │   │   ├── ica__ica_br1.mat
│   │   │   ├── ica__ica_c1-1.mat
│   │   │   ├── ica__ica.mat
│   │   │   ├── ica__icasso_results.mat
│   │   │   ├── ica_Mask.hdr
│   │   │   ├── ica_Mask.img
│   │   │   ├── ica__pca_r1-1.mat
│   │   │   ├── ica__postprocess_results.mat
│   │   │   ├── ica__results.log
│   │   │   ├── ica__sub01\_component\_ica\_s1_.mat
│   │   │   ├── ica__sub01\_component\_ica\_s1_.nii
│   │   │   ├── ica__sub01\_timecourses\_ica\_s1_.nii
│   │   │   ├── ica_Subject.mat
│   │   │   └── labels.csv  [IC labels: only the ones labeled with 'noise' were soft-regressed] 
│   │   ├── 02\_WMCSF_timeseries.json
│   │   ├── 02\_WMCSF_timeseries.txt  [Averaged signal and PCs from white matter and ventricle voxels]
│   │   ├── ...
│   └── rfmri_processed  [Preprocessed images]
│       ├── 02.json
│       ├── 02.nii
│       ├── 04.json
│       └── 04.nii
├── rat002
│   ├── ...
```
