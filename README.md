# ShroomNet
Data Preprocessing and ML Models to Predict Psilocybin Intoxication Based on Brainwaves

A binary classifier that predicts psilocybin intoxication based on brainwaves. 

Data: 
Data comes from a Swiss study where subjects were administered high doses, low doses, and placebo doses of psilocybin and 
presented a range auditory and visual stimulus while their EEG was recorded. There are only 22 subjects, posing a significant
challenge in creating a model that can generalize to new unseen subjects. At this point in time, I do not have permission to 
release the dataset. 

Pre-processing: 
The data was preprocessed in EEGLAB, where it was downsampled significantly to 125 HZ with low-pass filtering to 
prevent aliasing. For now, each EEG channel is treated as its own separate 1-D time series, rather than the true multivariate 
spatial time series represented by many channels placed at various positions across the scalp. While treating each channel 
as an independent time series neglects the spatial relationships between channels, it simplifies the model significantly and 
also provides much more training examples. Since there are 64 channels, treating each channel separately increases the number 
of training examples available 64 fold. 

The data is split into train/test sets by designating a subset of the research participants for the training set, and making
the rest of the subjects the test set. To perform well on the test set, the model must generalize to subjects it has never
seen during training. 

Input sequences are fixed length 1-D sequences of Voltage over time at an electrode. Each sequence is standardized using the
rule standardVec = (vec - mean)/stdev. 

Models:
Models in consideration include LSTM and 1-D convolutional neural networks

Results:
I am still in the process of tuning model architecture using a validation set. Validation set performance reached 70% using 
a simple LSTM with one dense hidden layer. I was curious if generalization across subjects was possible, so admittedly I 
evaluated this model on the test set. To my surprise, generalization seems possible even with only 22 subjects, as the
model scored 63% accuracy on the test set. This motivates me to continue tuning the architecture and trying new things, which I
will post here as I go. I promise not to evaluate on the test set again though before I'm done tuning :) 

Discussion:
Even simple LSTM models seem to be able to determine state of psilocybin intoxication based on Voltage at the scalp. Is this
due to some deep understanding of brain activity as it is altered by magic mushrooms? Maybe not... EEG data has a 
notoriously 