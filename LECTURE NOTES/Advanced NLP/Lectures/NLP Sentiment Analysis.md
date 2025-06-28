Understanding the emotional state of the author of a text
- Also called opinion mining
- very lucrative applications ( marketing, voice of the customer)
- can be extremely hard

In principle, it's very different from text classification
- Text classification: what's this text about ? 
- Sentiment analysis: how does the author feel about the subject of this text?
But there are similarities:
- the text can reflect a positive or negative emotional state

It can be applied to anything that contains subjective opinions:
- movie reviews
- product reviews
- stock markets
- politics


## Formal definition
Commonly framed as attitude detection
- Attitude: enduring disposition toward someone or something with an emotional connotation
- The attitude can be captured as quintuple
	1. Opinion holder: whose opinion is this ?
	2. Target entity: what is this opinion about ?
	3. Aspect: specific features of the target that this opinion is about
	4. Type: most commonly positive, negative, neutral along with indication of strength
	5. time when this opinion was expressed
- Opinion target = target entity + aspect
- Knowing the opinion without knowing the opinion target is of limited use