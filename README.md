Names:
Emails:
-------------------------------------------------------------

# Quantitative Evaluations

# Qualitative Evaluations


# Reproduce

1. Some data pre-processing. 
Download the Movie Triple Data to parlai/data, unzip it and rename the file to MovieTriples_Dataset.tar. Inside the movieTriple_Dataset file, we also need to rename the triple txt file to {train, valid, test}.txt respectively. 

2.  Training: 
We run the following scripts for training, and follow the hyper-parameter mentioned in the HRED paper. 
'''
python examples/train_model.py -m internal:hred -t dailydialog -bs 32 -mf parlai_internal/zoo/movie_hred/hred_model.ckpt
'''

3. Interactive: 
'''
python examples/interactive.py -mf parlai_internal/zoo/movie_hred/hred_model.ckpt.checkpoint -m internal:hred
'''

4. Integrate with Alexa: 
IDK about this part... 


# Issues and Potential Improvement. 
1. From our qualitative Evaluation, our trained model is outputting with little variance -- essentially, it outputs the same sentence despite the input we tried. 

2. Some easy improvement includes training the system for longer time; Carefully tune and search over the hyper-parameters; Initialize the model with some pre-trained language model, and then fine-tune. We could initialize our model with GPT-2 to obtain a language-modeling aware starting point. 

3. At decoding time, we could use the mutual information objective (A Diversity-Promoting Objective Function for Neural Conversation Models) or the Nucleus sampling techniques (The Curious Case of Neural Text Degeneration) to promote model diversity. 