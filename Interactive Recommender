
import csv
from ps5a import User, Movie, UserToUserRecommender, ItemToItemRecommender # Importing your work from Part A

RATINGS_FILENAME = 'ratings.csv'
MOVIES_FILENAME = 'movies.csv'
GENRES = ['Action', 'Adventure', 'Animation', 'Children', 'Comedy', 'Crime', 'Documentary', 'Drama', 'Fantasy', 'Film-Noir', 'Horror', 'Musical', 'Mystery', 'Romance', 'Sci-Fi', 'Thriller', 'War', 'Western']

def load_recommender(recommender, movies_file=MOVIES_FILENAME, ratings_file=RATINGS_FILENAME):
    """
    Loads the recommender with data from the movies_file and ratings_file.

    recommender (UserToUserRecommender or ItemToItemRecommender): the recommender to be loaded
    movies_file (string): the filename of a csv file containing information about movies, RATINGS_FILENAME by default
    ratings_file (string): the filename of a csv file containing user ratings of movies, MOVIES_FILENAME by default

    Returns: None
    """
    users = {}
    movies = {}
    processed_users = set()

    with open(movies_file, mode='r', encoding='utf-8') as file:
        reader = csv.DictReader(file)
        for row in reader:
            movie_id = int(row['movieId'])
            name = row['title']
            genres = row['genres'].split('|') if row['genres'] != '(no genres listed)' else []
            movie = Movie(movie_id, name, genres)
            movies[movie_id] = movie

    with open(ratings_file, mode='r', encoding='utf-8') as file:
        reader = csv.DictReader(file)
        for row in reader:
            user_id = int(row['userId'])
            name = row['firstName']

            if user_id not in processed_users:
                users[user_id] = User(user_id, name)
                processed_users.add(user_id)

            movie_id = int(row['movieId'])
            rating = float(row['rating'])

            movie = movies[movie_id]
            if movie:
                recommender.add_rating(users[user_id], movie, rating)

def interactive_recommender(recommender_type, user_ids=list(range(1, 285)), genres=GENRES):
    """
    Initializes and loads the specified recommender type.
    Interacts with a user by asking for their ID,
                             genre of movie they would like recommendations for, and
                             number of recommendations to provide.
    Prints recommendations for the user based on their input.

    recommender_type (str): the type of recommender to use, either 'user' or 'item'
    user_ids (list[int]): the list of existing user IDs
    genres (list[str]): the list of available movie genres

    Returns: None
    """
    while True: #getting user id and making sure the value is valid
        try:
            user_id = int(input("Please type your user ID: "))
            if user_id in user_ids:
                break
            else:
                print(f"Invalid ID. Please enter a valid ID from the following range: {min(user_ids)} - {max(user_ids)}")
        except ValueError:
            print("Invalid input. Please enter a valid numerical user ID.")

    
    while True: # getting genre and making sure value is valid
        genre = input("please state the genre you would like to consider or say NONE ")
        if genre in GENRES or genre == "NONE":
                break
        else:
            print("Invalid genre. Please enter a valid genre from the list provided.")
    
    while True: #getting recomenndation number
        try:
            recNumber = int(input("Please state the number of recommendations you would like: "))
            break
        except ValueError:
            print("Invalid input. Please enter a number.")
              
    #initializing recommender objects   
    if recommender_type == "item": 
       recommender =ItemToItemRecommender() 
    elif recommender_type == "user":
        recommender =UserToUserRecommender()
    if genre == "NONE":
        genre = None  # Set to None to indicate no filtering by genre
     # Assuming the User class exists and takes an ID as a parameter

    load_recommender(recommender)
    target_user =recommender.find_user(user_id)
    recommendations  = recommender.recommend(target_user,recNumber, genre)
    if recommendations: # if there are receommendations prints the ranking and the recommendation for each line 
        print(f"Recommendations:")
        for rank, movie in enumerate(recommendations, 1):
            print(f"{rank}. {movie}") 
    else: 
        print("no recommendations") #if there isnt recommendations



    

if __name__ == '__main__':
    # # Uncomment these lines to try running interactive_recommender()
    #interactive_recommender('user')
    interactive_recommender('item')
    pass
