# Google-Research-Kaggle-Competition

Google Research - Identify Contrails to Reduce Global Warming
Train ML models to identify contrails in satellite images and help prevent their formation

About this Competition
In this competition you will be using geostationary satellite images to identify aviation contrails. The original satellite images were obtained from the GOES-16 Advanced Baseline Imager (ABI), which is publicly available on Google Cloud Storage. The original full-disk images were reprojected using bilinear resampling to generate a local scene image. Because contrails are easier to identify with temporal context, a sequence of images at 10-minute intervals are provided. Each example (record_id) contains exactly one labeled frame.

Learn more about the dataset from the preprint: OpenContrails: Benchmarking Contrail Detection on GOES-16 ABI. Labeling instructions can be found at in this supplementary material. Some key labeling guidance:

Contrails must contain at least 10 pixels
At some time in their life, Contrails must be at least 3x longer than they are wide
Contrails must either appear suddenly or enter from the sides of the image
Contrails should be visible in at least two image
Ground truth was determined by (generally) 4+ different labelers annotating each image. Pixels were considered a contrail when >50% of the labelers annotated it as such. Individual annotations (human_individual_masks.npy) as well as the aggregated ground truth annotations (human_pixel_masks.npy) are included in the training data. The validation data only includes the aggregated ground truth annotations.

This is an example of labeled contrails. Code to produce images like this can be found in this notebook.



Files

train/ - the training set; each folder represents a record_id and contains the following data:
band_{08-16}.npy: array with size of H x W x T, where T = n_times_before + n_times_after + 1, representing the number of images in the sequence. There are n_times_before and n_times_after images before and after the labeled frame respectively. In our dataset all examples have n_times_before=4 and n_times_after=3. Each band represents an infrared channel at different wavelengths and is converted to brightness temperatures based on the calibration parameters. The number in the filename corresponds to the GOES-16 ABI band number. Details of the ABI bands can be found here.
human_individual_masks.npy: array with size of H x W x 1 x R. Each example is labeled by R individual human labelers. R is not the same for all samples. The labeled masks have value either 0 or 1 and correspond to the (n_times_before+1)-th image in band_{08-16}.npy. They are available only in the training set.
human_pixel_masks.npy: array with size of H x W x 1 containing the binary ground truth. A pixel is regarded as contrail pixel in evaluation if it is labeled as contrail by more than half of the labelers.
validation/ - the same as the training set, without the individual label annotations; it is permitted to use this as training data if desired
test/ - the test set; your objective is to identify contrails found in these records. Note: Since this is a Code competition, you do not have access to the actual test set that your notebook is rerun against. The records shown here are copies of the first two records of the validation data (without the labels). The hidden test set is approximately the same size (± 5%) as the validation set. IMPORTANT: Submissions should use run-length encoding with empty predictions (e.g., no contrails) should be marked by '-' in the submission. (See this notebook for details.)
{train|validation}_metadata.json - metadata information for each record; contains the timestamps and the projection parameters to reproduce the satellite images.
sample_submission.csv - a sample submission file in the correct format
