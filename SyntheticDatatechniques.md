Short analysis of LLMs versus specialised models for generating synthetic data (as of July 2023)


## LLMs for semantically rich textual data
- LLMs are very effective for creating small amounts of semantically rich textually-biased data containing "real world" content that can be fitted to a domain or topic. This has potential application for 1) Small size "seed data" sets that used to produce larger data sets 2) scenarios needing high volumes of "real world"-like data (e.g. generation of realistic multi page textual medical narratives. Additional tooling is needed to generate multi table data sets. For low volume use in (e.g.) early MVP testing the ease of use could be a helpful developer and test experience   

## LLMs for numerically or statistically rich data
- Numerically simple data is possible to produce with LLMs, but numerical sophistication in the data sets is expensive (in terms of tokens, compute power and time)

## Specialist models & tools that can generate realistic data 
-- Numerically and statistically sophisticated is not efficient to produce with LLMs due to the lack of specificity in the natural language or sample data input approach, plus complex requirements become unreliable in their interpretation by the LLM. Instead a more aligned approach is to use specialist tooling, such as the Synthetic Data Vault which packages together multiple neural nets that are aligned to specialist data synthesis. Good examples include [Synthetic data generation with CTGAN](https://www.youtube.com/watch?v=Ei0klF38CNs) This approach provides more sophistication but does however come at the expense of learning a multi-use toolkit (and loses the immediacy of the LLM model). 

-- Tools like [Faker](https://faker.readthedocs.io/en/master/) have a potential role in supporting creation of synthetic data sets (and are used by SDV). Faker itself provides an extensibility model and it's that for specialist or "verbose"  textual data generation needs, the faker framework could utilise LLM generated to produce realistic or domain specific data sets as part of a wider synthetic data capabilty 

## Research into tabular preditctions data using LLMs
 There is research underway using embeddings to query LLMs for real world statistical data, but while interesting, at this stage this approach seems potentially a lot more effort and uncertainty than approach 3. See [Applying Large Language Models To Tabular Data: A New Approach - Arize AI](https://arize.com/blog-course/applying-large-language-models-to-tabular-data/)