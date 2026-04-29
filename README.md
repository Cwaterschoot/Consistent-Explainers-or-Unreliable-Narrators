# Consistent Explainers or Unreliable Narrators
Repository for the paper: Consistent Explainers or Unreliable Narrators: Systematic Differences in Consistency and Sensitivity Across Large Language Models for Group Recommendations


### Content

```text
├── 📁 datasets                   #
│   ├── groups.csv                # group dataset used in the study
│   ├── Tourist_destinations.csv  # Tourist destination dataset
│   └── movielens_dataset 
│       ├── movies.csv            # movie titles and documentation (movielens)
│       ├── ratings.csv           # ratings (movielens)
├── 📄 strats.py                  # Utils: Social choice-based aggregation functions
├── 📄 config_gen.py              # Utils: group configuration generation functions 
├── 📁 output.zip                 # Compressed file containing both LLM output file (results.csv) and final results containing explanation categories (full_results.csv)
└── 📄 README.md                  # documentation
```

### Prompts

# System Message
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
