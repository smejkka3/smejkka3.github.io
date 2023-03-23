---
layout: post
title:  "Combining Machine Learning with Reasoning"
excerpt: "Notes from reference papers for project of implementing graph knowledge embeding systems which combine machine learning with reasoning as part of thesis project for DIMA group of TU Berlin"
---

In this post are described main ideas from several papers which helped me understand the topic of the project "Knowledge Graph Completion using Embeddings and Rule-based Reasoning". The purpose of reading all these appers is mainly to get idea what exactly knowledge graph embeding (KGE) is, logical reasoning and datasets created and evaluated on for (KGE).

# Description of the topic: 
In the recent years, we have seen many applications of knowledge graphs across various industries, including but not limited to product recommendation, question answering, sentence completion, chatbots, and network security. However, the state-of-the-art methods are highly dependent only on the information contained in the input of the knowledge graph. Inferring missing links in knowledge graphs (a.k.a knowledge graph completion) requires data. It, therefore, is very reliant on the quality of the incoming information into the knowledge graph, which can cause illogical and dire link predictions which are not intuitive to humans. This comes from the fact that knowledge graphs are created from web-scale knowledge bases and therefore miss much information making them incomplete. Knowledge graph completion tasks are, therefore, highly data-driven. Inferring information not explicitly contained in the knowledge graph but that holds true could enhance the accuracy of industries where it is impossible to create a complete representa- tion of all the information, such as the pharmaceutical industry. To enhance the performance of the knowledge graph completion methods, we propose combining data-driven knowledge graph embeddings with logical rules for inferring missing information from experts or entailment regimes. This way, we supplement the original knowledge graph with information from domain-specific knowledge not explicitly stated in the knowledge graph. On top of that, we make the knowledge graph embedding flexible to include any existing knowledge graph embedding and inference method. By evaluating the proposed framework against vanilla knowl- edge graph embeddings and current state-of-the-art hybrid approaches combining knowledge graph embeddings with rule mining, we prove that our method enhances accuracy up to threefold in terms of mean reciprocal rank accuracy.

#### Introduction
Knowledge graphs are widely used across many applications. A knowledge graph is a collection of connected entities in a graph-like structure. This helps to find meaningful and related relationships in-between entities. Knowledge graphs are the backbone for products such as chatbots, search, product recommendations, and many more. Among prominent examples of companies that would not be able to succeed without having knowledge graph algorithms used in their products are, but are not limited to, eBay, Airbnb, Microsoft, Google, and Comcast.

Predicting missing links automatically for large-scale knowledge graphs is known as a process of knowledge graph completion. Knowledge graph completion is a necessary task because knowledge graphs are created from web-scale knowledge bases and therefore miss much information making them incomplete. Knowledge graph completion is a task widely used across different industries, such as drug side effects, career path prediction, product recommendation, and flavor combinations. One of the possible ways to solve knowledge graph completion is to use embedding. Unlike traditional deep learning methods, which are domain-specific (convolutional neural network for grids, recurrent neural network for text), graphs can capture more complex, multimodal structures. Therefore, coming up with a solution that enhances the accuracy of knowledge graph embeddings compared to traditional solutions can have considerable implications across multiple industries.

Integrating rules and logic into knowledge graphs enhance prediction accuracy by using ontologies determined by domain experts and knowledge graph embeddings, allowing users to input their logical reasoning into knowledge graphs.

#### Research Problem
In this chapter, we introduce the overall problem we aim to solve in the domain of KG completion and the impact and expectations of such a solution compared to the state-of-art techniques described in Chapter 6.
Unlike the conventional deep learning approaches that are domain-specific (such as CNN for grids and RNN for texts), graphs can capture increasingly multifaceted and multimodal structures. Hence, developing a solution that improves the accu- racy of KG embeddings compared to conventional solutions can result in outstand- ing outcomes across various industries. Nonetheless, KGEs are data-driven and do not perform effectively for sparse entities.
A different approach involves applying rule-based methods that learn rules from a KG and subsequently use them in inferring the missing connections or links. Nevertheless, such an approach is also data-driven, and it is improbable to learn rules with inadequate evidence. Moreover, KGE and rule mining does not offer good prediction outcomes for arbitrary KGs since they depend on information and patterns in the input KG and hence cannot be generalized when there is insufficient data. For example, KGEs perform effectively in predicting links in popular relations and entities (dense regions of graphs). On the other hand, rule-based approaches work effectively when there is sufficient evidence to support their mining patterns. Nonetheless, in the real world, KGs comprise sparse and dense regions, and hence, the models fail to offer acceptable prediction outcomes in general cases and increasingly rely on input data.

- Dependence solely on the information in the input knowledge graph. Removing the dependency on a data-driven approach, where we can only in- fer information explicitly contained in the knowledge graph. To avoid the dependency solely on the input, our goal is to deploy logical rules while lever- aging specific entailment regimes such as OWL2 to enhance the accuracy of the KG completion.
- Deep embedding of rules with KGE. Existing approaches deeply embed rules in the training of KGEs. This approach is still reliant on the data contained only in the input of KG; therefore, it is data-driven. The second problem of deeply embedding the rules with KG is relying on specific scoring functions and losing the flexibility of using different KGE models. We aim to loosely couple rules and KGE as separate systems to prevent information loss during embedding rules with the KGE and preserve the flexibility of using any existing KGE model.

To loosely couple rules with KGEs efficiently and effectively, we aim to solve the following subproblems:
- We need to decide on the KGE module and inference engine.
- We aim to solve the problem of the connection and communication of these
two components.
- Some of the inferred triples contain a certain degree of uncertainty. There- fore, we want to determine which triples of the KGE will be passed to the reasoner and how to do it efficiently.

#### Conclusion

# Complex Embeddings for Simple Link Prediction

#### Introduction

#### TODO

#### TODO

#### Conclusion

# Popularity Agnostic Evaluation of Knowledge Graph Embeddings

#### Introduction

#### TODO

#### TODO

#### Conclusion

# AMIE: association rule mining under incomplete evidence in ontological knowledge bases

#### Introduction

#### TODO

#### TODO

#### Conclusion
