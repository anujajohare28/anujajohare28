- import streamlit as st
import pickle
import pandas as pd
import requests



def fetch_poster(movie_id):
   response = requests.get('https://api.themoviedb.org/3/movie/{}?api_key=8265bd1679663a7ea12ac168da84d2e8&language=en-US'.format(movie_id))
   data =response.json()
   #st.text(data)
   #st.data('https://api.themoviedb.org/3/movie/{}?api_key=06a10ee878f7a0628208ab6bf71f8e21&language=en-US'.format(movie_id))
   return "https://image.tmdb.org/t/p/w500/" +data['poster_path']

def recommend(movie):
    index = movies[movies['title'] == movie].index[0]
    distances = sorted(list(enumerate(similarity[index])),reverse=True,key = lambda x: x[1])

    recommended_movies=[]
    recommended_movies_posters =[]
    for i in distances[1:6]:
        movie_id = movies.iloc[i[0]].movie_id
        recommended_movies.append(movies.iloc[i[0]].title)
        recommended_movies_posters.append(fetch_poster(movie_id))

        return recommended_movies,recommended_movies_posters




movie_dict= pickle.load(open('movie_dict.pkl','rb'))
movies = pd.DataFrame(movie_dict)

similarity = pickle.load(open('similarity.pkl','rb'))

st.title('AU2FLIX')


Selected_movie_name = st.selectbox('HELLOOO YOUR  WELCOME  HERE   What Would You Like To Watch?',
                      (movies['title'].values))
recommended_movies=[];
if st.button('Show Recommendation'):
  recommended_movies,recommended_movies_posters= recommend(Selected_movie_name)
  recommended_movies,recommended_movies_posters= recommend(Selected_movie_name)


print(len(recommended_movies))

j1=len(recommended_movies)
col1,col2,col3 = st.columns(3)
for j in range(j1):
       try:
           with col1:
                st.text(recommended_movies[j])
                st.image(recommended_movies_posters[j])
                j=j+1
           with col2:
                st.text(recommended_movies[j])
                st.image(recommended_movies_posters[j])
                j=j+1
           with col3:
                st.text(recommended_movies[j])
                st.image(recommended_movies_posters[j])

       except Exception :
                print()
