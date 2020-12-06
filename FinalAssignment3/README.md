Dallas Alberti 11/15/2020

Documentation Final Assignment 3 Iterations 

  
1. For the first iteration I worked on quantifying the country of origin for each of the artworks. 
This yielded 74 distinct locations which made clustering complicated. 

2. For the second iteration I used this same counting method to quantify the artwork medium and then paired it with texture ('te') to create clusters. 
```
mediums = {}
int = 0

for medium in data['primary_medium'].values:   
    if medium in mediums:
        medium = mediums[medium]
    else:
        mediums[medium] = int
        int += 1

data['primary_medium'] = [mediums[item] for item in data['primary_medium']]
data['primary_medium'].replace(np.nan, 0)
```
3. Clusters were selected based on the highest silhouette_score which was a value of .99 before returning no more increase in silhouette_score values. This was 37 clusters.  Perhaps this means that there are too few variables but I will slowly be introducing more. 
`![](silhouette_score_values.PNG)`



