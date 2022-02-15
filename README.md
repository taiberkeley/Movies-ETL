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
    
    
Wikipedia doesn't have strict standards on how movie data is presented, so it needs a lot of work to clean up the data and make it usable.
The iterative process for cleaning data can be broken down as follows:
First, we need to inspect our data and identify a problem.
Once we've identified the problem, we need to make a plan and decide whether it is worth the time and effort to fix it.
Finally, we execute the repair.

Early iterations focus on making the data easier to investigate: deleting obviously bad data, removing superfluous columns (e.g., columns with only one value or missing an overwhelming amount of data), removing duplicate rows, consolidating columns, and reshaping the data if necessary.

     wiki_movies_df = pd.DataFrame(wiki_movies_raw)
     wiki_movies_df.head(10)
   - Converting columns into a list
    wiki_movies_df.columns.to_list()
    
With a list of columns I was able to pick columns that were necessary and unecessary to the analysis. 

I have created a filter expression for only movies with a director and an IMDb link. I need to check if either "Director" or "Directed by" are keys in the current dict. If there is a director listed, I also want to check that the dict has an IMDb link. 

        wiki_movies = [movie for movie in wiki_movies_raw
               if ('Director' in movie or 'Directed by' in movie)
                   and 'imdb_link' in movie]
        len(wiki_movies)
        
Now that I've filtered out bad data, I will need to clean up each movie entry so it's in a standard format.

    def clean_movie(movie):
    movie = dict(movie) #create a non-destructive copy
    alt_titles = {}
    for key in ['Also known as','Arabic','Cantonese','Chinese','French',
                'Hangul','Hebrew','Hepburn','Japanese','Literally',
                'Mandarin','McCune–Reischauer','Original title','Polish',
                'Revised Romanization','Romanized','Russian',
                'Simplified','Traditional','Yiddish']:
    if key in movie:
            alt_titles[key] = movie[key]
            movie.pop(key)
    if len(alt_titles) > 0:
        movie['alt_titles'] = alt_titles
    return movie
    
Now I can make a list of cleaned movies with a list comprehension:

        clean_movies = [clean_movie(movie) for movie in wiki_movies]
        wiki_movies_df = pd.DataFrame(clean_movies)
        sorted(wiki_movies_df.columns.tolist())
    
    
        
