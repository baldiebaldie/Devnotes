Affective computing
01-14-2026

### What is emotion (components)
- Cognitive - influenced by thinking
- Physiological - heart rate, blood pressure, respiration
- Expressive - facial expression
- Motivation - goal
	- Context!
- Feeling - awareness of being in an emotional state
- Different theories look at different 
	- appraisal - assessing something or someone
	- misappraisal - lack of valuation
Emotional phase
- Low-level : auto reflex
- High-level : deliberate decision making
- Behavior prep
- behavior execution
- Communication 

Valence - roughly how happy or sad you are
This is a look at dimensional emotion

(circumplex model)
![](../../../-images/Pasted%20image%2020260114171444.png)

This is a more accurate 3 dimensional model of potential emotions
![](../../../-images/Pasted%20image%2020260114171645.png)

Theories of emotion
1. Physiological - physical effects
	- James Lange theory of emotion - emotions are the result of interpreting our body's physiological responses to external stimuli
	![](../../../-images/Pasted%20image%2020260114173025.png)
2. Neurological - brain biology
	- Cannon Bard - when you encounter an emotional stimulus, your brain simultaneously triggers both the subjective feeling of the emotion (like fear) and the physical arousal (like a racing heart) independently, rather than one causing the other.
	![](../../../-images/Pasted%20image%2020260114173245.png)
3. Cognitive - what are you thinking
	- Schachter Singer - proposes that an emotion arises from two key components: physiological arousal (like a racing heart or sweating) and a cognitive label (interpreting the situation and physical response)
	![](../../../-images/Pasted%20image%2020260114172929.png)
4. Appraisal theory (Lazarus)
	![](../../../Pasted%20image%2020260114174141.png)
	- Due to the potential problem present due to the environment you illicit an emotion and then make a decision. A decision to act upon the environment or to act 
Facial Feedback theory of emotion (Darwin and James)
- There is a direct link between actions and emotions
- i.e (at a high level)
	- Smiling will make you happy 
- Strong vs Weak
	- Strong: Expression causes emotion
	- Weak: Expression modulates emotion
Constructive Theory 
- Emotions are NOT biologically hardwired and NOT universal
- They are constructed by:
	- Affect (arousal and valence)
	- Context (beliefs, culture, experience, etc.)
- Emotions are learned, culturally viable, and context dependent
Dual-process Theory of emotion
- BOTH your emotions and your cognition effect your behavior. 
	- i.e you want some chocolate but need to lose weight
		- In the event you eat the chocolate you are emotionally happy however, you do not follow your diet 
		- In the event you do not eat the chocolate you can emotionally sad, however you stick to your diet
	- Analytically decisions are slower when emotionally decisions are much quicker
### Emotion components and multimodal signals
Physiological data:
- Heart rate, respiration, EDA
- Can be gathered through a watch for instance
Cognitive data:
- EEG data 
- Very hard to get direct data from this. 
- Can be gathered through wet caps with gel and proper data collection or dry caps which is less accurate yet more comfy 
Expressive data:
- Images
Emotional data:
- Self-report on feelings
Motivational data:
- Context
#### Physiological-based model
- Heart rate, respiration, EDA
- Can be gathered through a watch for instance
- James-Lange is commonly implemented (even unknowingly)
	- High arousal = high emotion intensity 
- Cannon Bard favors multimodal models
	- Expressions can be masked
	- physiological can be noisy
	- feelings can be "faked"
	- Thus you need potentially multiple avenues of data to put anything together. This promotes robustness
### Machine learning 
Classical vs Deep Learning
- Hand crafted features - features you extract from a signal 
- Deep features - Features extracted from a model
Temporal modeling
- LSTM, transformers, windows-based
- Emotion as a process and depends on prior state (appraisal theory
Multi-modal fusion
- Early fusion - features are fused before modeling (component process model of emotion)
	- If we made a vector of all physiological, cognitive, expressive, motivational, or emotional data we can throw them all together
- Late fusion - decisions are  (Cannon-Bard)

So what does this all mean?
- **AFFECTIVE COMPUTING SYSTEMS CHOICES ARE NOT THEORY - NEUTRAL**

Supervised learning
- Fixed labels (giving images saying this person is happy, mad, etc)
- Human annotations (labeling the image and saying if its happy, mad, etc)
- Assumes ground truth labels are correct
	- Aligns with basic emotion theory
	- Contradicts constructivist theory
Unsupervised learning
- no fixed labels
- learn without assistance of humans
- Aligns with constructivist theory 

MODEALS DONT JUST DETECT EMOTION, THEY COMMIT TO A THEORY OF WHAT EMOTION IS



### Emotion elicitation and ground truth (how to maybe get the data)
Emotion elicitation
- Watch videos
- interviews
- games/different tasks
Ground truth
- self report 
- Observer annotation (most common)
- AI annotation (putting an image into ai and telling it that this is what it is)
Ground truth challenges
- Bias (maybe biased off culture or personal anecdotes)
- Annotator agreement 
- Constructivist theory
	- not everyone feels the same emotions, there is no true label
- Same stimulus, different emotions (appraisal theory)