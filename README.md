# Consistent Explainers or Unreliable Narrators
Repository for the paper: Consistent Explainers or Unreliable Narrators: Systematic Differences in Consistency and Sensitivity Across Large Language Models for Group Recommendations


### Content

```text
├── Generation.ipynb              # Full code to generate groups and generate LLM output using Ollama
├── Analysis.ipynb                # Full code to analyze the data generated using the code found in Generation.ipynb. All visuals and regression from the paper are        included.
├── datasets                   
│   ├── groups.csv                # group dataset used in the study
│   ├── Tourist_destinations.csv  # Tourist destination dataset
│   └── movielens_dataset 
│       ├── movies.csv            # movie titles and documentation (movielens)
│       ├── ratings.csv           # ratings (movielens)
├── strats.py                     # Utils: Social choice-based aggregation functions
├── config_gen.py                 # Utils: group configuration generation functions 
├── output.zip                    # Compressed file containing both LLM output file (results.csv) and final results containing explanation categories (full_results.csv)
└── README.md                     # documentation
```


### Datasets
Domain datasets were only used to extract item names to assign within groups.
Included domains were movies (low risk) and tourism (high risk).
groups.csv includes the actual groups used in the study (user x item matrices) (with anonymized item identifiers)


### Prompts
Prompts were identical for each of the four LLMs. Prompts consist of two parts: system message (overall system/scenario instructions) and user message (the scenario itself).


#### System Message
```code
system_message = {
                                'role':'system',
                                'content': f"""
            You are tasked with making group recommendations based on the different preferences of the group members. 
            You need explain the process behind making the recommendation to the group in such a way that someone without recommender systems knowledge can understand. 
            The information you are provided contain the {domain} preferences of the group. Every candidate item for recommendation has a rating from each user listed in the order by user (first rating from user1, second from user2 etc). 
            The rating is a scale from 0 to 100. For the recommendation, you simply mention the {domain}.
            You make a recommendation to the group of users by providing a ranking of 10 {domain} based on the recommendation approach you came up with. 

            Provide your answer as VALID JSON ONLY.
            Do not use markdown or code fences.
            Do not include newlines inside string values.
            Use plain ASCII characters only.

            Format:
                {{
                "recommendation": ["item1","item2","item3","item4","item5","item6","item7","item8","item9","item10"],
                "explanation": Short explanation of how you made the recommendation with no line breaks"
                }}

            """}
```
#### User message (scenario itself)
```code
scenario = {
                    'role': 'user',
                    'content': f"""
                    The per-item ratings are presente below:
                    ### BEGIN TABLE ###
                    {result}
                    ### END TABLE ###

                    Think about the answer internally, but only output the final JSON object (containing recommendation ranking and explanation). Do not include any additional text or python code. 
                    Return STRICT JSON. Do not use markdown.
                    """

                }
```

### LLM inference parameters
Constant temperature across all runs in results.csv and full_results.csv

```code
temperature = 0.5
max_completion_tokens=1000
```
### Output
The compressed folder contains both the direct results file (results.csv) and the full file used for analysis (full_results.csv). The latter includes the full documentation of the explanation categories. These files are derived from the generation code.
