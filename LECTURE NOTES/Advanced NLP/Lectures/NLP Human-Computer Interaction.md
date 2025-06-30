## Chatbots
- Systems that carry on extended conversations that mimic informal human-human interaction
- Modern chatbots: LLMs trained to do tasks within a conversation interface
	- Answering questions
	- Writing, summarizing or editing text or code
	- Carrying on discussions about any topic

## Pre-training: How LLMs Learn from Raw Text
- Language models predict the next word given prior words (causal modeling)
- Pre-training data comes from diverse sources (web, books, Wikipedia)
- No explicit instruction-following capability, just raw pattern recognition

## Pre-training data
- Text scraped from the web, augmented by more carefully curated data
- Colossal Clean Crawled Corpus
	- 156 billion tokens of English
	- Filtered, removed offensive language and non-natural language
	- Includes texts from patents, Wikipedia and news sites
- Dialogues & Pseudo-Dialogues
	- EMPATHETICDIALOGUES: 25k conversations
	- SaFeRDialogues: 8k conversations
	- Filtered pseudo-conversations from Reddit, Twitter and Weibo


## Instruction Fine-Tuning (IFT)
- Goal: Teach models to follow instructions, not just predict the next word
- Supervised fine-tuning on labeled instruction-response pairs
- Quality: producing responses that are sensible and interesting
- Safety: not suggesting harmful actions

## Fine-tuning for Quality: Adding Positive Data
After pretraining we improve dialogue quality and safety by incorporating human feedback in the instruction fine-tuning (IFT) stage: 
- Provide human speakers with an initial prompt and clear instructions to encourage high-quality, safe dialogues
- Gather interactions where users engage with an initial system, and their dialogues and responses are used to fine-tune the next-generation model
- Train the system by combining dialogue with other tasks, enabling it to:
	- Answer questions and follow instructions accurately
	- Engage in high-quality, safe conversations
- Achieve a unified multi-task learning approach for better generalization

## Fine-tuning for safety
create specific safe answers to instructions and add this safety data during the instruction-tuning stage
- If trained only on general-purpose instruction-following datasets, models occasionally respond to unsafe queries
- A small number of safety-focused examples can help a lot
- If you overdo it, the model will reject harmless queries

## Collecting Human-Labeled Ratings
It is possible to train models to predict safety and quality values using human-labeled ratings.
- Human annotators interact with the system to generate dialogues
- A second set of annotators label each system turn for:
	- Quality ( is the response relevant and well-formed ?)
	- Safety ( is the response safe and non-harmful ?)
- Each turn receives a binary label (1= good, 0=bad) for quality and safety

## Classification Model for Safety & Quality
- A language model is fine-tuned to classify response turns based on:
	- Quality labels
	- Safety labels
- The system learns to predict labels for generated responses, improving model reliability
- Instruction Fine-Tuning improves zero-shot generalization

## Zero-Shot Generalization
- Zero shot learning refers to a model's ability to perform a task without having seen specific examples of that task during training.
- Zero-Shot Generalization: instead of learning from direct examples, the model generalizes from related tasks or prior knowledge

### How Instruction Fine-Tuning Enhances ZSL
- Training on diverse Instruction-Response pairs
	- Model learn to follow instructions across different domains
	- Instead of just predicting text, they learn to respond to commands
- Generalization to New Tasks
	- Models understand how to process new instructions even if they were never explicitly trained on them
- No Task-specific fine-tuning needed
	- Traditional models require separate fine-tuning for each task
	- Instruction Fine-tuned models perform well without additional training, making them usable out-of-the-box

### Scaling Instruction Fine-Tuning with SELF-INSTRUCT
- Problem: manually collecting instruction-response pairs is expensive
- Solution: SELF-INSTRUCT use LLMs to generate new instructions iteratively
- How it works:
	- Start with a few human-written instructions
	- Let the model generate new instruction-response pairs
	- Filter out low-quality data

### Why instruction fine tuning is not enough
- Instruction fine tuning helps models follow commands
- However it does not optimize responses for human preference
- Solution: Reinforcement Learning from Human Feedback (RLHF) 

## RLHF
- Trains models to align with human preferences
- Human compare multiple model-generated responses and rank or score them
- A reward model learns to approximate human preferences
- The model is fine-tuned using reinforcement learning (RL) to maximize reward

Choosing the right model:
- RLHF requires a strong initial model that already understands natural language
- There is no definitive answer on which model is the best for RLHF
- Companies experiment with different architectures and scales
	- OpenAI: smaller gpt3 models fine tuned with RLHF, ....
- The key requirements is a model that responds well to diverse instructions

### reward model
Another LM in addition to the initial LM
- Input: sequence of text
- Output: a scalar reward representing human preference
How is it trained ? 
- training set generated by sampling a set of prompts from a predefined dataset
- The prompts are passed through the initial LM to generate new text
- Human annotators rank the generated text outputs form the LM
	- Different annotators may use different scales 
	- Instead of direct scoring, use pairwise comparison and rank outputs relative to eachother

Two model outputs are compared for the same prompt
- Annotators choose which response is better
- The system records a win/loss rather than an arbitrary score
- This method is more robust than direct scoring
Use an Elo system to assign a relative rating to each response
Example: 
- $y_1$ (output A) wins -> gains rating points
- $y_2$ (output B) loses -> loses rating points
Over time this rank responses on a continuous scale
The Elo scores are then normalized into a scalar reward signal

The reward model does not have to be as large as the LM
Intuition: the reward model only needs to evaluate text, not generate it


>[!INFO] Formal definition
>- Start with a Supervised Fine-Tuned (SFT) model $\pi^{SFT}$
>- Given a prompt x the model generates two possible responses: 
>  $$(y_1, y_2) \tilde \pi^{SFT}(y|x)$$
>- Human annotators are asked to compare $y_1$ and $y_2$ and select the preferred response
>- If human prefers $y_w$ over $y_l$ we write: $$y_w > w_l | x$$
>- We assume human preferences come from a latent reward function $r*(x,y)$ 
>- The Bradley-Terry (BT) Model is used to express the probability that response $y_1$ is preferred over $y_2$: $$p*(y_1>y_2|x)= \frac{expr(r*(x,y_1))}{exp(r*(x,y_1))+exp(r*(x, y_2))}$$
>- This model assumes:
>	- The higher the reward $r*(x,y)$ the more likely the response is preferred
>	- The probability distribution over responses is softmax-like
>Training the reward model
>- Since $r*(x,y)$ is unknown, we train an approximate reward model $r_φ(x,y)$ using human preference data
>- The dataset consists of labeled comparisons: $$D = {(x^{(i)}, y_w^{(i)}, y_l^{(i)})}_{i=1}^N$$
>- since humans provide pairwise rankings rather than absolute scores the training process is formulated as binary classification problem
>- The goal is to ensure that the model assigns higher reward scores to responses that humans prefer

- Instead of predicting a single reward score the model compares two responses and predicts which one is better
- Given two responses $y_w$ (preferred) and $y_l$(less preferred) we model $$P(y_w>y_l|x) = \sigma (r_φ(x,y_w) -r_φ(x, y_l))$$
- the logistic function $\sigma(.)$ converts the reward difference into a probability between 0 and 1
- This turns the ranking problem into a binary classification

## The negative Log-Likelihood (NLL) Loss Function
- To train the reward model, we use the negative log-likelihood loss: $$L_R(r_φ, D) = - E_{x, y_w, y_l~D}[log \sigma(r_φ(x,y_w)-r_φ(x,y_l))]$$
- Breaking down the components:
	- $r_φ(x,y)$ -> the reward model predicts the quality of a response
	- $\sigma(.)$ -> the logistic function converting reward differences into ranking probabilities
	- The model is trained to assign higher rewards to preferred responses
- This loss function maximizes the probability that the model ranks responses correctly


## NLL loss
The model learns by minimizing the loss, adjusting reward scores to match human preferences
- Lower loss occurs when the model correctly ranks preferred responses higher
- Higher loss occurs when the model assigns incorrect rankings
If $P(y_w>y_l)$ is already high, the log function assigns a low penalty, encouraging stability. if $P(y_w>y_l)$ is too low, the log function assigns a high penalty, forcing corrections.


>[!FAQ] Recap
>- The reward model is trained to rank responses, not predict absolute scores
>- Training is formulated as a binary classification problem using a pairwise ranking loss
>- The negative log-likelihood loss ensures the model assigns higher rewards to preferred responses
>- This approach avoids issues with inconsistent absolute scoring and provides a more stable reward signal


## From reward model to RL fine-tuning
- we now have 
	- A language model LM that generates responses
	- a reward model RM that predicts a scalar score based on human preference rankings
- Next step: Reinforcement Learning to fine-tune the LM by:
	- Generating new responses 
	- Assigning rewards using the reward model
	- Updating a policy to maximize expected reward while maintaining stability

Policy: A policy $\pi(a|s)$ is a function that defines how an agent chooses actions based on the current state
Formally it is a probability distribution
- s is the state (context, previous tokens)
- a is the action(next token to generate)
- $\pi(a|s)$ is the probability of choosing action a given state s
Policies can be 
- Deterministic : always picks the same action for a given state
- Stochastic: Outputs a probability distribution over actions

In RLHF the policy is the LLM
- Given a prompt x the policy selects a sequence of tokens
- Each action is a word or token chosen at step t
- The policy is updated using reinforcement learning to improve text generation

![[Kullback-Leibler (KL) Divergence]]


>[!SUMMARY] RLHF Overview
>Key Steps: 
>1. Gather and prepare prompt data
>   - pairs of (prompt, response) examples
>2. Supervised Fine-tuning
>   - Train or fine-tune a baseline model on these prompt/response pairs
>3. Collect Human Preference data
>  - Have a human annotator compare different model outputs
>4. Train a reward model 
>   - Convert pairwise rankings to scores (e.g. Elo), train a model to predict them
> 5. Optimize with RL (e.g. PPO)
>    - Use the reward model as the "reward" signal, fine-tune to get an aligned policy

![[RLHF as a fine-tuning process.png]]

- Prompt data -> supervised model: trains an initial policy on known good responses
- Human annotators: produce pairwise rankings of model outputs
- Ranking data: turned into a scalar reward signal (via Elo,....)
- Reward Model: predicts how humans would rank a given response
- Aligned model: optimized via RL (eg. PPO) with the reward model guiding the policy updates


>[!INFO] RLHF as a fine-tuning process
>- RLHF refines an existing pretrained model rather than training from scratch
>- The process:
>	- Step 1: Fine tune a pre trained language model using prompt/response pairs written/selected by humans to get a reasonable starting policy
>	- Step 2: Train a reward model from human-labeled rankings
>	- Step 3: Use RL (PPO) to further align the policy
>- This allows the model to align better with human preferences while retaining fluency

