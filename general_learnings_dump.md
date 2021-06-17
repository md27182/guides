# Processing pipeline
Shuffling the dataset using tf.data.Dataset.shuffle causes a 0.09 s / iteration slowdown, doubling the total training time. For this reason, the data needs to be preshuffled using numpy.

We need to use the rough bg subtraction to throw away rays that don't hit the object to reduce the size of the datasets in the preprocessing steps. Subtraction can reduce dataset sizes from 500GB-1TB to 100-200GB.

We also need to do the processing on the cloud because it costs too much money (~$50) to transfer an entire dataset from S3 or Google Cloud to the lambda computer. Since the datasets are so large, we should delete them after final approval of the scan. Using a fixed random seed would allow the multifile dataset to be reproduced. Don't delete last_samp_info since that can't be recovered without retraining.

