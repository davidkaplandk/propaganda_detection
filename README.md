# propaganda_detection

/API Calls

Contains how we prompted the model.
We're not sure, if this will work with the models, that you are going to test.

Most important consideration:
The validation.ipynb expects a certain structured json output in order to work. 
So scripts should monitor if the output of the LLMs are valid or replace them with placeholders / -1 or something like that.
The validation notebook requires 200 items in order to work.


Our pipeline to prompt models was:

Extract a jsonl line including its prompt from the jsonl 
-> Extract the prompt ({"messages": [{"role": "system", "content": <EXTRACT_THIS> ... )
-> Replace the actual tweet after the "Text:" with "{text}"
-> Use this prompt to prompt the given model


======================================================== = = = = = = = = = = = = = =========================================

/Training and Validation files

Contains Training and validation files for Our Schema and Sahitaj et al.'s Schema, split into high only and main-and-high / low-main-and-high prompting.

High only files are appended with *_highonly.jsonl

Main-and-high schema files are appended with *_high_and_main.jsonl OR *_high_and_main_updated.jsonl


Example Jsonl line to understand its structure:


{"messages": [{"role": "system", "content": "You are an expert in propaganda analysis. Always keep in mind that you are only looking for Russian propaganda, not any other type."}, {"role": "user", "content": "You are an expert in propaganda analysis. Analyze the following text. \nAlways keep in mind that you are only looking for russian propaganda, not any other type of propaganda. Any other type of propaganda should be ignored and labeled as \"No Propaganda\".\n\nText:\n\"@USER HTTPURL all you need to hear about Ukraine propaganda\"\n\n1.⁠ ⁠Identify the high-level category that best describes the text.\n\nHigh-level categories:\n\n1 — Patriotic & Catchy Appeals: Techniques like flag-waving or catchy slogans appealing to patriotism or identity.  \n2 — Popularity Appeals: Techniques like bandwagon arguments, claiming something is good/true because it’s popular.  \n3 — Deflections & Distractions: Includes whataboutism, repetition, causal oversimplification, or red herrings that shift focus.  \n4 — Emotional & Loaded Persuasion: Techniques using loaded language, fear, name-calling, exaggeration, Nazi comparisons, or appeals to authority.  \n5 — Argument Manipulations: Techniques like straw man, clichés that shut down debate, doubt, or black-and-white framing.  \n6 — No Propaganda: No relevant Russian propaganda detected.\n\nNow return the labels in a JSON format with the following structure:\n{{\"high\": integer}}\n\nFollow the json format strictly. Do not add any additional text or explanations. Just use the integers for labeling, no strings.\nRemember to always return Integers only! Only return one integer that best describes the text. Never return multiple integers or a list of integers!"}, {"role": "assistant", "content": "{\"high\": 5}"}]}


======================================================== = = = = = = = = = = = = = =========================================

golden_test_sets

Contains both golden test sets as python pickle files.

golden_test_set_our_schema.pkl 
golden_test_set_sahitaj_schema.pkl

Each can be used for both prompting strategies, since they contain both (low), main and high level labels.

======================================================== = = = = = = = = = = = = = =========================================

validation.ipynb

Requires a json files with 200 objects that match the json schema from our API calls.

