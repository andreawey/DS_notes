
>[!INFO] NLP definition
>- systems that understand and generate human language

Approach: deep learning techniques to capture context and address biases
Challenges: Human language is ambiguous and context-dependent, not propagating existing biases in the training data

## Fields of NLP
- ML and DL for training 
- Software engineering for development, processing and deployment
- LinAlg, Calculus, statistics and probability, to define learning objectives, metrics and optimization methods
- Linguistics to understand language structure, syntax, semantics and pragmatics
- Cognitive science insights into human language processing NLU and NLG

## What can It be used for ?
- Text and speech analysis
	- spelling & grammar check
	- Document retrieval
	- Text classification
	- Information extraction
	- Sentiment analysis
	- Named-entity recognition
	- Content-based recommendation
	- Speech recognition
- Generation
	- generate text
	- Synthesize speech
- Analysis and generation
	- machine translation
	- Question answering
	- Summarization
- Analysis & generation & interaction
	- Dialogue systems / chatbots

## Learning approaches in NLP
**[[Supervised Learning]]** Learning relations from labeled input-output data
**[[Unsupervised Learning]]** Self supervised learning: learning hidden patterns from unlabeled data by creating its own labels from input data
**Semi supervised Learning**: Leveraging information from labeled data to annotate unlabeled data

Self supervised learning example -> Deep NN using Transformers (BERT)
self supervised because trained on text with 12% masked tokens

**Fine tuning**: further training a model on a smaller domain/task specific dataset using self-supervised-learning
**Parameter-efficient fine-tuning (PEFT)**: Fine tuning LLMs by modifying only a small subset of parameters
**Preference alignment learning**: training models to align their outputs with human preferences using human feedback using RL or self-supervised learning
**In-context learning (ICL)**: Performing task based on examples in the prompt without parameter updates (no real ml)

>[!NOTE] Parameter Efficient Fine-Tuning (PEFT)
>Reduces inference time and energy consumption while maintaining good performance
>- Low-Rank adaptation (LoRA)
>	- Freeze pretrained model weights and inject trainable rank decomposition matrices whose low rank approximations retain the most important information
>- Pruning
>	- Identify and eliminate redundant or less important parameters based on criteria, such as magnitude-based pruning or structured pruning
>- Quantization
>	- Represent parameters with lower bit precision (32-bit-floats -> 8-bit-integers)
>	- can be implemented during training or after training (post-quantization)

>[!NOTE] Preference Alignment Learning
>Refining a model's behavior to get it close to human preferences requires model output samples ranked by humans
>- Reinforcement Learning from Human Feedback (RLHF)
>	- Trains an auxiliary model on human-provided evaluations
>	- adjusts main model to maximize rewards based on this model
>- Direct Preference Optimization
>	- Directly optimizing the model's output against human preference data, without explicit reward models
>- Odds Ratio Preference Optimization (ORPO)
>	- fine-tuning LM without requiring a reference model or a separate preference alignment phase


>[!NOTE] In-Context Learning (ICL)
>- ICL uses trained LLMs to solve tasks without retraining or fine-tuning
>- New tasks are solved by inference of prompted, clear, well structured and relevant examples
>- ICL works better in larger models which support generalization across various contexts
>- ICL improves by exposure to diverse data
>- ICL depends critically on prompting, but performance drops only marginally when labels in the examples are replaces by random ones

















