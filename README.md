# 25-2_ML_Yonsei_project
Modeling encoding/decoding/LFP-spike relationship in prefrontal cortex of interacting mice under social context

Medial PreFrontal Cortex(mPFC) is a region of brain that is involved in decision, execusion of behavior, and other higher cognitive functions. As a part of its role, it is involved in behavior encoding(recognization of social circumstances) and behavior decoding(generating social interaction behavior). While the underlying computational mechanism of link between neural signal and observed behavior remains ellusive. In this project, neuro-feasible, simple,shallow(so they can be easy to interpret) recursive neural network model(RNN) has been trained and tested to the real-word neural signal data. It provides the insights between the neural(Local Field Potential, spike) signal and observed behavior-related variables, and the generalized features of using real-word neuroscience data as an input of Neural Network.

Data aquistion
Dataset used is same dataset with https://doi.org/10.21203/rs.3.rs-5800769/v2, which is a paper under review in one of the nature journals from my previous lab. I participated in the experiment, data preprocessing, and almost all part of the data analysis of the paper. 

Experiment scheme
<img width="1755" height="881" alt="image" src="https://github.com/user-attachments/assets/5b040e43-69c0-4662-a35f-2655e62cd249" />
Two mice interacts in a box with no artificial restrainments of their behavior, while electrodes inserted to their prefrontal brain region is being recorded. The data used in this project was collected real-time, simultaneously, from the two mice respectively, after the 15 minutes of data confirmation recording.
Raw voltage traces were divided using bandpass filter and spike detection algorithm. LFP was achieved using 0.5Hz-200Hz bandpass filter and spikes are extracted using manuall clustering of fast transient spike-shaped band traces, consistent with the field's previous studies.

Genotype
In this experiment, WT(often regarded as normal mice), and KO(autism model mice) are used. Further analysis might be done to compare the difference between genotypes

Data preprocessing
LFP data are wavelet transformed using Morlet wavelet, and band power was extracted for theta[4-12Hz], beta[13-30Hz], low-gamma[31-80Hz], wide[4-100Hz] Spike times for each time bin was calculated to generate firing rate traces. position data was processed to extract velocity,components of the velocity, distance,derivative of distance, absolute position, and distance from the center. All datasets were finally downsampled to 30Hz, and succesive 10 datapoints were used as sequence data.

Model design
Input: spike firing rate data sequence
Output:elocity,components of the velocity, distance,derivative of distance, absolute position, and distance from the center, and LFP power trace of both mice in the final timestep of the sequence
rnn model is used from the torch.nn.RNN, tanh(normally used in neuroscience) activation function, Adam optimizer for batch optimization, one RNN layer and one fully connected output layer. Train/Val/Test was devided using nested 5-fold cross-validation. Early stopping using validation set were used during the train process. Result performance were evaluated using R2 score for all multivariate output targets.

Result
Without using nested cross validation, the R2 performed well, about mean of 0.72, for WT mice. However, using nested cross validation, the R2 value decreased and indicated the model has no generalized explainability for datasets. This spurious result must be due to the periodic relationship of given dataset, so no cross validation lead to test data leakage to train data. And the absence of explainability must be the result of complex mixed-selectivity relationship of neural data and observable behavior data, and graudal shift of low-dimensional neural computation structure during the 10-minute recording.

Further study
Extracting specific time bins may narrow down the neural computational structure that the model has to capture, and increase its performance level, and enable the further analysis of artificial social-brain system.


 
