# Anomaly Detection in ISO 20022 Payment Transactions Using a Self-Supervised Masked Tabular Transformer Model
## Abstract
ISO 20022 payment messages are high-dimensional and mixed-type, and confirmed fraud
labels are very sparse, which limits supervised detection and can weaken classical detectors.
The study developed and evaluated a self-supervised Masked Tabular Transformer (MTT)
for anomaly detection in ISO 20022 payment transactions under a leakage-controlled
design. A publicly available synthetic dataset of 5,000 transactions (6.4% rejected) was
used, and post-outcome fields were removed through a leakage audit. The data was
split chronologically, and all models were trained on accepted transactions only. The
MTT was tuned with a 60-trial Optuna search on a label-free pseudo-anomaly set, and
benchmarked against conventional anomaly detection methods, including Z-score analysis,
Isolation Forest, and Local Outlier Factor. Evaluation focused on anomaly-detection
effectiveness and the model’s ability to reconstruct normal transaction patterns using
Mean Absolute Error (MAE) and Root Mean Square Error (RMSE) for numerical field
reconstruction, Cross-Entropy and Top-k Accuracy for categorical field reconstruction,
and Precision, Recall, F1-score, and AUC-PR for anomaly-detection ranking performance.
On the controlled structural-anomaly benchmark, the MTT achieved an AUPRC of
0.8612, however LOF, was significantly better than the MTT (difference = -0.031, 95% CI
[−0.043, −0.020]). On the held-out TxSts endpoint, no model demonstrated meaningful
discrimination, with the MTT achieving an AUROC of 0.465 and an AUPRC of 0.0635,
compared with 0.517 and 0.0714, respectively, for Isolation Forest. A supervised probe
achieved an AUROC of 0.497, indicating that the permitted ex-ante fields contained almost
no recoverable signal about rejection. The MTT is therefore capable of detecting controlled
structural anomalies but shows no demonstrable advantage over simpler detectors for the
weak proxy labels.
Key words: ISO 20022 Payment Transactions; Anomaly Detection; Self-Supervised
learning; Masked Tabular Transformer; Representation Learning.

## Summary: Chapter 1
The study set up the case for using a self-supervised Masked Tabular Transformer to
detect anomalies in ISO 20022 payment data. ISO 20022 is a global messaging standard
that produces large volumes of rich, high-dimensional tabular data, which makes manual
inspection impractical and automated anomaly detection necessary. Labelled fraud data
is scarce and costly, and that is where self-supervised learning comes in: a model learns
useful representations from unlabeled data by solving a pretext task, and for tabular
data that usually means masked modelling with transformers. The chapter highlights the
necessity to test self-supervised representations in specific payment context.
Classical methods degrade as dimensionality grows, because distance-based separation
stops being informative. Unsupervised reconstruction models can also generalise too well
and reconstruct anomalies as if they were normal, and little is known whether SSL actually
helps for tabular data. Therefore, a self-supervised model that can use large unlabeled
transaction datasets effectively is a necessity.

## Summary Chapter 2
This chapter reviewed the literature that bears on whether a model that learns the normal
structure of payment messages can flag unusual transactions better than conventional
detectors. ISO 20022 data is richly structured, but it is also high-dimensional and mixed
in type, which weakens classical detectors and makes manual review impractical. Financial
anomaly detection research relies mainly on labelled credit card data, whereas real payment
operations offer few confirmed labels, and outcomes such as rejection are weak proxies for
fraud.
The framework combined three ideas: one-class normality learning, masked reconstruction,
and attention-based representation learning, and it flags recurring difficulties: class
imbalance, high dimensionality, non-stationarity, reconstruction of anomalies as if they
were normal, and outcome leakage. It then reviewed self-supervised learning and masked
tabular transformers, and noted that the literature is divided on whether self-supervised
representations outperform raw features, so that the superiority of the proposed model
must be tested rather than assumed. The classical baselines were described, together with
their algorithms
Two gaps emerged: firstly, there is little evidence for masked self-supervised anomaly
detection on ISO 20022 data, because published work concentrates on labelled credit
card or retrospective fraud datasets. Second, label scarcity in real payment operations
leaves it uncertain how well methods evaluated in supervised settings transfer to one-class,
normality-learning settings. These gaps motivate the leakage-controlled evaluation of a
Masked Tabular Transformer reported in the chapters that follow.

## Summary Chapter 3
A quantitative research design was adopted, in which a self-supervised Masked Tabular
Transformer was developed and evaluated against alternative detectors. The study
used the synthetic ISO 20022 payments analytics dataset of 5,000 pacs.008 transactions
with their pacs.002 status reports, of which 320 (6.4%) were rejected. A leakage audit
verified algebraically that ProcessingTimeSecs was determined by StatusTimestamp and

TxDateTime, excluded it and the transaction identifiers from every predictive model, and
reserved TxSts and RejectionReason for evaluation only. Eighteen ex-ante features were
engineered across five semantic groups, and the data was split chronologically into FIT,
TUNE, REF, CAL and TEST sets, with all detectors fitted on accepted transactions only.
Because TxSts could not be used for tuning, hyperparameters were selected on a label-free
pseudo-anomaly set built from TUNE-accepted records with six controlled corruption
families, including a currency-shuffle null control.
The Masked Tabular Transformer was compared with three classical baselines, all tuned
on the same pseudo-anomaly set. The transformer was tuned with a 60-trial Optuna
search, which is reported as exploratory because it followed prior exposure to TEST-set
behaviour. Every model was evaluated on a controlled structural-anomaly benchmark,
and the external TxSts proxy outcome, using AUROC, AUPRC, alert-budget metrics,
split-conformal decision metrics, and reconstruction-quality metrics. Statistical inference
relied on a paired stratified bootstrap, the DeLong test, a paired permutation test and an
exact McNemar test, with a Holm correction across the family of comparisons. The next
chapter presents the results of this evaluation.

## Summary Chapter 4
This chapter presented the results of the evaluation described in Chapter 3. The widened
60-trial Optuna search consistently preferred mask ratios between 0.60 and 0.75, and the
Masked Tabular Transformer (MTT) trial reached a pseudo-AUPRC of 0.922. The three
classical baselines, tuned on the same pseudo-anomaly set, were led by LOF (0.897), while
Isolation Forest was clearly the weakest (0.549).
The MTT with learned-field weighting (learn_max) was the strongest transformer variant
(AUPRC 0.900) and the second-strongest detector overall. It improved most on the
amount-tail family, where the AUROC of the standard ensemble rose from 0.788 to 0.996.
Every MTT variant remained weak on the rare-but-coherent-route family.
On the external TxSts endpoint, every model performed at chance (AUROC close to
0.5, AUPRC lift close to 1). The MTT ranked eighth of ten by AUPRC (0.0635, lift
0.92), and the widened search did not help on this endpoint. This is consistent with
the supervised detectability probe, which showed that the permitted ex-ante fields carry
almost no recoverable signal about rejection. Reconstruction quality was strong only
for the near-deterministic location fields (Top-1 accuracy 1.00), while most numerical
fields were no better predicted than their own mean and PurposeCode remained close to
chance. Seed variation (AUROC 0.451 and 0.494 across the two seeds) was comparable
in size to some differences between models, which supports seed averaging and cautious
interpretation.
Considering the dataset used, the results show that MTT was competitive but not superior
detector.
## Summary Chapter 5
This chapter related the findings of Chapter 4 to the research objectives, stated the
limitations of the study and set out directions for future work. The study asked whether a
self-supervised Masked Tabular Transformer (MTT), trained only on accepted transactions
under strict leakage control, can improve anomaly detection in ISO 20022 payment
transactions relative to conventional baselines. All four objectives were met: the literature
was reviewed, the main model was evaluated with reconstruction-quality and anomalyranking metrics, and it was compared with nine comparator detectors using paired
statistical tests with Holm correction.

The answer to the research question differs between the two endpoints. On the controlled
structural-anomaly benchmark, LOF was significantly better than the MTT (difference =
−0.031, 95% CI [−0.043, −0.020]). And Masked Tabular Transformer model outperformed
two classical baselines (z-score and Isolation forest). On the external TxSts proxy endpoint,
the MTT showed no demonstrable advantage: every model performed at chance, which is
consistent with the supervised detectability probe showing that the permitted ex-ante fields
carry almost no recoverable signal about rejection. The MTT is therefore a competitive
but not superior detector of the structural dependencies it is exposed to.
The conclusions are bounded by six limitations: the data is synthetic, rejection is only
a weak and heterogeneous proxy for anomalous behaviour, the dataset is small (1,000
test transactions, 69 rejections), the ex-ante fields carry little signal about the proxy
outcome, the hyperparameter search was exploratory because it followed prior exposure
to TEST-set behaviour. Recommended future work is to repeat the evaluation on real,
larger-scale ISO 20022 data with richer ex-ante fields, and to add network graph features
and longer velocity windows.

## Conclusion
In conclusion, this research asked whether a self-supervised Masked Tabular Transformer
can improve anomaly detection in unlabeled ISO 20022 payment transactions relative
to conventional baselines. It was tested on a controlled structural-anomaly benchmark
and on a real but weak proxy outcome (TxSts), using statistical safeguards, a wider
hyperparameter search and two new scoring variants.
On the controlled benchmark, the Transformer, once corrected for fields known to carry
no separable signal, is a competent detector of the structural dependencies it is trained to
recognise. However LOF, was significantly better than the MTT (difference = −0.031,
95% CI [−0.043, −0.020]), While MTT outperformed two classical baselines.
These conclusions are bounded by the synthetic data, the small test set (1,000 transactions,
69 rejections) and the exploratory nature of the re-tuning, which followed prior exposure
to TEST-set behaviour. The Transformer is therefore a competitive but not superior
detector of structural anomalies, and the evidence does not yet justify its added complexity
over simpler methods.
As financial institutions adopt richer messaging standards, disciplined, endpoint-aware
evaluation will be essential. Such evaluation separates what a model can do from what
it has been shown to do on the target that matters, and reports a partial improvement
as partial rather than rounding it up to a clean win. This is what is needed to turn
self-supervised representation learning into trustworthy transaction monitoring.
