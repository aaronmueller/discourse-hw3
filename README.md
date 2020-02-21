Names: Lisa Li, Aaron Mueller, Alexandra DeLucia

Emails: {xli150, amueller, aadelucia}@jhu.edu
-------------------------------------------------------------

# Reproduction Instructions

0. Copy the files from this repository into your installation of ParlAI. A few files will be replaced, so we recommend creating backups beforehand.

1. Now, we need to preprocess the Movie Triples dataset. 
Download the Movie Triple Data to ParlAI/data, unzip it and rename the file to MovieTriples_Dataset.tar. Inside the MovieTriple_Dataset file, we also need to rename the triple txt files to {train, valid, test}.txt, respectively. Our code will take care of the rest.

2.  Training: 
We provide training scripts for easy reproduction of our training setup. We roughly follow the hyperparameters presented in the original HRED paper---with a few modifications, given the constraints of the assignment. Note that this script is set up to work on the CLSP grid, and you will most certainly need to make some modifications to make it work for your machine or installation.
'''
train.sh
'''
It (i) sources a ParlAI-enabled conda environment w/ CUDA-enabled PyTorch, (2) sets the proper environment variables, and (3) calls the training function with all the appropriate hyperparameters. We have also included `train_2.sh`, which is similar to `train.sh` but with some (unstable) multi-threading behaviors. We recommend using `train.sh`. 

3. Evaluation:
From the ParlAI/ directory, run the following command:
'''
python parlai/scripts/eval_model.py -mf parlai_internal/zoo/movie_hred/hred_model.ckpt.checkpoint -m internal:hred -t internal:dailydialog
'''
Because we have replaced dailydialog with our MovieTriples dataset in our internal implementations, this will actually evaluate on the validation and test sets of MovieTriples.


4. Interactive:
From the ParlAI/ directory, run the following command:
'''
python examples/interactive.py -mf parlai_internal/zoo/movie_hred/hred_model.ckpt.checkpoint -m internal:hred
'''

5. Integrating with Alexa: 
_Alexandra_


# Qualitative Evaluations
Similar to other groups, we note that our model has low variance but very high bias. In other words, the model always outputs the same response regardless of what the user input is---and this response is useless in most situations.

The chatbot responds with "robotic legalistic robotic", followed by a long and repetitive series of "nehru" tokens. We tried a variety of inputs, but this seems to be the only response that the bot is capable of producing. It is unclear why it is unable to output more sensible responses, though this does yield plenty of inspiration for different types of evaluation metrics than have previously been proposed.

For example, consider BLEU: it has a brevity penalty and is essentially just a modified form of n-gram precision. With our chatbot's uniformly long response, it will never be subject to the brevity penalty and may sometimes demonstrate very small n-gram overlap in certain specialized domains. Perhaps we could define a new metric that encourages chatbot responses to be similar in length to a reference response, or a metric which discourages repetitive sequences of the same token.

There are a variety of metrics that could be used to qualitatively and automatically judge the performance of a chatbot system based on the flaws of our current system, but ultimately, we do not need any of these to see that it does not produce naturalistic responses.


# Issues and Potential Improvement
1. From our qualitative evaluation, our trained model is outputting with little variance -- essentially, it outputs the same sentence despite the various input we tried. 

2. Some easy improvement includes training the system for longer time; Carefully tune and search over the hyper-parameters; Initialize the model with some pre-trained language model, and then fine-tune. We could initialize our model with GPT-2 to obtain a language-modeling aware starting point. 

3. At decoding time, we could use the mutual information objective (A Diversity-Promoting Objective Function for Neural Conversation Models) or the Nucleus sampling techniques (The Curious Case of Neural Text Degeneration) to promote model diversity. 
