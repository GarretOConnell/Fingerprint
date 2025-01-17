# Minodes Fingerprint to Zone Classification Challenge

## Introduction

At Minodes, we provide insights to retailers using WiFi Analytics. Insights are derived by analysing the location of people's smartphones during their visits.

How does it work? We install a set of WiFi routers (so called "nodes") with a custom firmware inside or close to the areas to be monitored. Nodes collect the signal strength (`RSSI`) of WiFi probe requests sent regularly by smartphones (so called "observations"). Observations are then used to estimate the position of smartphones: Let `RSSI(N,X)` be a function that returns the signal strength of probe request `X` from node `N` (measured in dBm: `0` is strongest signal strength, `-100` is weakest signal strength, extremes are seldomly reached). The set of observations `RSSI(Ni,X)` for all nodes `i` is called "fingerprint" and constitutes a radio signature of the smartphone correlated (non linearly) to its position in space (a distance of few meters translates to a signal strength within `[0,-30]`). Stores are manually partitioned into regions (so called "zones"), e.g., "entrance" and "checkout area". Fingerprints and zones are used as features and prediction classes, respectively.

Example: Given a store with zones `{A,B}`, nodes `{N1 (inside A), N2 (inside B), N3 (between A and B)}`, we want to estimate the most likely zone where the device that generated probe request `X` is located at that point of time. Intuitively, if `RSSI(N1,X) > RSSI(N3,X)`, the device is probably located in zone `A`. If `RSSI(N3,X)` is the highest, the answer is uncertain and we must look at the relative difference of the values of `RSSI(N1,X), RSSI(N3,X)`. If `RSSI(N2,X) > RSSI(N1,X)`, then most likely the correct zone is `B`.

## Dataset

This archive contains a dataset in CSV format located at `data/fingerprints_gt_ver3.csv`. The dataset represents a large deployment in a mall (three floors), and has been generated by walking inside each zone with a set of phones, labeling fingerprints manually with their corresponding zone. Nodes are located at the exterior of the entrance of the stores, and not inside the stores. Zones map to shops, aisles, and surrounding areas. The dataset is structured as follows:

| fr_observation_time  | fr_values  | fr_mac_address_id | zo_name  |
| -------------------- | ---------- | ----------------- | ---------|  
| 2015-12-08 10:00:13  | {'9': '-83', '13': '-67', '33': '-62', '101': ...  | 3192369 | Zone 355  |
| 2015-12-08 10:00:13  | {'12': '-69', '33': '-61', '128': '-68', '276'...  | 2002427 | Zone 355  |

Description of the fields:

* `fr_observation_time`: first timestamp in ascending order of the observations aggregated into the fingerprint (aggregation spans 1 second by default, to accomodate time desynchronisation issues on the nodes)
* `fr_values`: fingerprint, represented as a dictionary  {`node_id`: `signal_strength`, ...}. If a node ID is not present,  you can assume that its associated signal strength is `-100`
* `fr_mac_address_id`: unique id of the mac address of the phone that emitted the corresponding probe request
* `zo_name`: zone name (class to be predicted). Zone names are strings containing numbers, and do not have a direct semantic meaning.

Some statistics about the dataset:

* `343449` unique fingerprints (rows in the CSV file, excluding header line)
* `19` unique mac address ids. That means that 19 different phones where used to collect the dataset
* `449` unique zones
* `261` unique nodes
* On average, each node is present in `23224` fingerprints

## Problem

Given a fingerprint, predict its corresponding zone with a classifier.
The dataset can be used for training and testing the classifier.
Assess your solution with cross-validation by reporting results for confusion matrix, precision, recall and F1 score.

## Submission

Submit your solution by emailing us the link to a public GitHub repository, containing the solution as a Jupyter notebook and any additional files you deem necessary.
Do not include the files contained in this archive in the public repository.
You must use Python 3.x for your solution.
We evaluate your submission by scoring it on these questions:

* Is the candidate following the provided instructions?
* IIs there an accompanying README.md file, that describes the contents of the submission?
* Is it well written using Markdown title(s), lists, and punctuation? any typos?
* Is it well structured, with a title, introduction, conclusions, sections, and explanatory comments?
* Is it easy to follow, with a clear and logical flow? Can I comprehend it without spending lots of energy?
* Last but not least: How good are results?

If you have any questions, feel free to approach us directly.
