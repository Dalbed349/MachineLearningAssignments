Dallas Alberti 11/15/2020
## Documentation Final Assignment 3 
###  K means clustering of Artwork from the MET 

#### The following outlines a general process to cluster images based on metadata properties


1. 404 images from the Metropolitan Museum of Art and various metadata properties were used as the data for this project. The goal was to arrive at a clustering of images into groups that bear abstract visual similarities to one another. A proposed use for this project is to prepare a digital gallery view of the artwork created by machine learning techniques. 
2. The properties focused on for this final iteration were 
		```X = data[['primary_medium','te','ar','or','li','si']]```
		
a. primary_medium = the medium of the artwork. Originally this was a string variable which had to be converted into an np array.  This was done in the following code segment:
```mediums = {}
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
   b.  'te' =  variation in the fineness or coarseness of an area having a given value; includes blur.
   c.  'ar' =  An AREA signifies something on the plane that has a measurable size. This signification 		  applies to the entire area covered by the visible mark.
   d. 'or' = various orientations, ranging from the vertical to the horizontal in a distinct direction.
   e. 'li' = A LINE signifies a phenomenon on the plane which has measurable length but no area. This signification is independent of the width and characteristics of the mark which renders it visible.
   f. 'si' = variations in height, width, area. 

3. In order to determine the proper number of clusters for the model, a combination of an inertia graph and silhouette plots were used. The inertia graph was used to try to find an 'elbow' point for clusters 1 through 40 and the silhouette plots were examined to see the spread of the values for each cluster. 9 clusters was determined to be an appropriate number of clusters and images of these two graphs are found below. 
		
![InertiaPlotElbow.PNG](https://github.com/Dalbed349/MachineLearningAssignments/blob/main/FinalAssignment3/InertiaPlotElbow.PNG "InertiaPlotElbow.PNG")

![SilhouettePlotClusters_9.PNG](https://github.com/Dalbed349/MachineLearningAssignments/blob/main/FinalAssignment3/SilhouettePlotClusters_9.PNG "SilhouettePlotClusters_9.PNG")

It is worth mentioning that 17 clusters also seemed viable for this algorithm however for design and practicality I chose to stick with a smaller number of groupings. When 17 clusters were used images that seemed like they should be together were separated and it proved distracting. 

![SilhouettePlotClusters_17.PNG](https://github.com/Dalbed349/MachineLearningAssignments/blob/main/FinalAssignment3/SilhouettePlotClusters_17.PNG "SilhouettePlotClusters_17.PNG")

4. Setting my_n_clusters = 9 provided image clusters that can be viewed in the Final Assignment 3 ipynb document. I am currently working on loading these into p5js to create a better visualization. 
5. The process for getting these clusters into a format that can be read in by p5js begins with creating copies of the image files for each cluster and placing them in their own new directories. This was done with the shutil module. 
```
	from shutil import copyfile
import os 
for i in range(0, max(km.labels_)+1):
    os.mkdir('cluster' + str(i))# mode=0o777, *, dir_fd=None
    print(" ")
    print("* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * ")
    print("Images in cluster: " + str(i))
    print("* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * ")
    for j in range(0, len(km.labels_)):
         if km.labels_[j] == i:


            from IPython.display import Image, display, HTML
            from matplotlib.pyplot import figure, imshow, axis
            from matplotlib.image import imread
            
            
            print('<img src ="./img_small/'+ str(j+2) +'_small.jpg">')  
        
            copyfile('./img_small/'+ str(j+2) +'_small.jpg','./'+'cluster'+str(i)+'/'+str(j+2) +'_small.jpg')
  ```
  6. The result of the code above is found in the /data folder. 9 separate folders, one for each cluster. 
  7. A note on the final product of the clustering: This was an attempt to cluster images based on their medium, texture, orientations, lines, and presence of sizes and areas. Although the exact nature of the clustering is hard to pinpoint, the clusters do provide some unique insight into what may have caused their groupings. A few observations from the clusters: primary medium might not have been the best characteristic to group by or my quantification of the string variable might not have been ideal. There were over 70 mediums listed so rather than getting clusters of each medium I think other metadata properties took more weight. 'te' texture, 'or' orientations, and 'li' lines proved to be strong clustering traits in my opinion. For example, the first cluster (0) includes many artworks that have an element of blue and curvature to them. I am happy with the second cluster containing mostly sculptural works or photographs of sculptures. These works all appear among some sort of distinct background hinting towards a separation based on relative measurable size. The non sculptural works here are very geometric in composition and have an almost sculptural appearance to them. Cluster 5 demonstrates a solid grouping of mixed media that contain aggregations of several small lines. I think this cluster would serve as a nice exhibit. Cluster 6 is one of my favorites as we see a pretty clear grouping of earth-like, almost natural, formations such as rocks and clouds. Cluster 8 is an ideal grouping, in my opinion, because of the combination of the textured 'chaos' that can be seen. These artworks appear to be very busy and combine lines and orientations of many varieties. Overall this project was incredibly entertaining and I think the possibilities for appropriate groupings reach way beyond where I have arrived. It is even harder to talk about these clusters as the more I review them the more I question their plausibility. The final touches to this project will be trying to tie various parts of this semesters curriculum together by display the clusters in a web browser using JavaScript. 