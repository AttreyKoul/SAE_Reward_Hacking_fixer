# **Matroshka SAEs for Backdoor Correction to Mittigate Reward HAcking**

This is an attempt to reduce reward hacking in LLMs using Matryoshka SAEs

Keywords: Activation Patching, SAES, Matryoshka SAEs(also known as Nested SAEs), Sycophancy, Reward hacking, Activations. Gemma-2-2b, Gemma-2-2b-it, Mistral-7b-instruct

Important Note: We use Gemma-2-2b as a negative control for all testing. This is because Gemma-2-2b is a base model, and is not capable of reward hacking. Testing on Gemma-2-2b determines if our method targets features specifcially caused by reawrd hacking

## **How does this methodology work**

There are 5 steps to this methodology:

### Activation Generation
First, we generate activations that could be either sycophantic or reward hacked. In orfer to do this, we use prompts from Meg-tong's sycophancy datset and Allenai's Reward Bench dataset respectively. 

We use the first 13650 prompts from Megtong's sycophancy dataset and all prompts from Reward Bench. We save each response to a csv and save the all activations from each response in pytorch files, each resopnse having it's own pytorch file.

### Model Response Judging
To determine if a response is either eward hacked or sycophantic, we use ArmoRM and GPT-4o-mini together. We compute thresholds based on previous model responses to deetermine the margins for reward hacking/sycophancy. If the response's reward score falls in between the margins, it get's escalated to GPT-4o-mini. If not, the response is either logged as not reward hacked or sycohpnatic, or it is logged as sycophantic or reward hacked, depending on which responses we are analyzing.

### SAE Creation
To create the Nested SAEs. we train each SAE on around 4.1 million activations. We give each SAE different widths, with similar widths as the original Matryoshka SAE paper. We use activations from 4 datasets for training the SAEs. We use The Pile, Reward Bench, Meg-Tong's sycophancy dataset, and ultrafeedback. For Reawrd Bench and Meg-tong's sychphnancy dataset, we use the responses already contained in the dataset and use the tokenizer for the model we are creating an SAE for to generate the activations needed for the SAE. 


### SAE Feature Analysis

