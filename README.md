# Inductive Graph Representation Learning for Fraud Detection
This repository contains the code used in the experimental setup of the paper 'Inductive Graph Representation Learning for Fraud Detection. 

# Abstract
Graphs can be seen as a universal language to describe and model a diverse set of complex systems and data structures. However, efficiently extracting topological information from dynamic graphs is not a straightforward task. Previous works have explored a variety of inductive graph representation learning frameworks, but despite the surge in development, little research deployed these techniques for real-life applications. Most earlier studies are restricted to a set of benchmark experiments, rendering their practical generalisability questionable. Our paper evaluates the proclaimed predictive performance of state-of-the-art inductive graph representation learning algorithms on highly imbalanced credit card transaction networks. More specifically, we assess the inductive capability of GraphSAGE and Fast Inductive Graph Representation Learning in a fraud detection setting. Credit card transaction fraud networks pose two crucial challenges for graph representation learners: First, these networks are highly dynamic, continuously encountering new transactions. Second, they are heavily imbalanced, with only a small fraction of transactions labeled as fraudulent. Our paper contributes to the literature by (i) proving how inductive graph representation learning techniques can be leveraged to enhance predictive performance for fraud detection and (ii) demonstrating the benefit of graph-level undersampling for representation learning in imbalanced networks.

# Experimental Pipeline
<img src="https://github.com/Charlesvandamme/Inductive-Graph-Representation-Learning-for-Fraud-Detection/blob/master/Figures/experimental_pipeline.JPG?raw=true"/>

## Transaction Data 
The real-life dataset used to construct our credit card transaction networks is provided by a credit card issuer and contains 3,724,348 transactions. This dataset includes information on the following features: anonymized identification of clients and merchants, merchant category code, country, monetary amount, time, acceptance, and fraud label. The aforementioned number of transactions resulted from the interaction between 1,325,070 clients and 144,439 merchants gathered over five weeks during October-November 2013. This real-life
dataset is highly imbalanced and contains only 0.65% fraudulent transactions.

## Pre-Processing

The second step in the pipeline is transforming the input data into a compatible format for the downstream graph representation learning algorithms. Specifically, the features needed to be transformed into a numeric format. The timestamp attribute was reshaped into the separate year, month, day, hour, minute and second attributes. Also, the acceptance and fraud label attributes were converted into binary values. An additional transformation was the creation of dummy variables for the country attribute. After these pre-processing steps, the number of attributes increased from 8 to 214 numeric variables. To improve the robustness of our results, we devised a manual 5-fold out-of-time validation, by dividing the dataset based on a rolling window approach. Each window has a size of 17 days, with the start date of each window five days apart. Within particular windows, a train-test split was made: the first twelve days were used to train the representation learners, leaving the last five days to test the inductive capability of the algorithms. 

## Graph Construction
The next step in the pipeline is constructing the graphs that will be used by FI-GRL and GraphSAGE to learn node embeddings. We designed the credit card transaction networks as heterogeneous tripartite graphs containing client, merchant and transaction nodes, following the tripartite graph construction approach of \cite{PreviousWork} and \cite{APATE2015}. Because of this tripartite setup, representations can be learned for the transaction nodes. Client and merchant nodes are determined by their anonymized identification. The 212 remaining attributes, hereafter referred to as the original transaction features, reside with the corresponding transaction nodes. A graph was constructed for each timeframe and used as input to the graph representation learning algorithms. Table \ref{timeframes} illustrates the number of transactions and fraud distribution in the training and test sets for each timeframe. The percentage fraud fluctuates substantially between timeframes, characterizing an additional difficulty to obtain stable results. Given the highly imbalanced nature of our dataset, we hypothesized that without sampling, the graph representation learning algorithms will have a blurred view on the structure of fraudulent transactions. To test this hypothesis, we fed graphs with varying sampling rates to the representation learners. 

## GraphSAGE

This notebook contains a supervised, heterogeneous implementation of the GraphSAGE framework. 
