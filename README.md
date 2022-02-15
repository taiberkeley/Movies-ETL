# Movies-ETL
Getting Data ready for a Hackathon with ETL

Extract dataset on Kaggle that contains ratings data from MovieLens, a site run by the GroupLens research team, which has over 20 million ratings.
clean up the data and make it usable

  import json
  import pandas as pd
  import numpy as np
  file_dir = 'C:/Users/taina/Dropbox/1_BERKELEY_DATA\MODULE_8_ETL/Movies ETL'
  f'{file_dir}wikipedia-movies.json'
  with open(f'{file_dir}/wikipedia-movies.json', mode='r') as file:
    wiki_movies_raw = json.load(file)
    len(wiki_movies_raw)
  - First 5 records
  wiki_movies_raw[:5]
  - Last 5 records
  wiki_movies_raw[-5:]
  movies_metadata = pd.read_csv ('movies_metadata.csv', low_memory=False)
  ratings = pd.read_csv ('ratings.csv')
  movies_metadata.head(10)
