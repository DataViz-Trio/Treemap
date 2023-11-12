# Treemap

This github repo has the code for the treemap implementation for Assignment 2 of the Data Visualization Course.

## Results 

- `Treemap` : Contains the screenshots of the treemaps generated with different spatial partitioning.
  - [AreaWiseCrimesSliceDice.png](Images/AreaWiseCrimesSliceDice.png) : Treemap on Area Wise Crimes in Los Angeles with SliceDice partitioning
  - [AreaWiseCrimesSquarify.png](Images/AreaWiseCrimesSquarify.png) : Treemap on Area Wise Crimes in Los Angeles with Squarify partitioning
  - [DateWiseCrimesSliceDice.png](Images/DateWiseCrimesSliceDice.png) : Treemap on Date wise crimes in Los Angeles with SliceDice partitioning
  - [DateWiseCrimesSquarify.png](Images/DateWiseCrimesSquarify.png) : Treemap on Date wise crimes in Los Angeles with Squarify partitioning
  - [TypeofCrimeSliceDice.png](Images/TypeofCrimeSliceDice.png) : Treemap on Type of Crimes in Los Angeles with SliceDice partitioning
  - [TypeofCrimeSquarify.png](Images/TypeofCrimeSquarify.png) : Treemap on Type of Crimes in Los Angeles with Squarify partitioning
 
## How to Generate the Treemaps

1. Make sure that the `Crime_Data_from_2020_to_Present.csv` file is present in the TreeMap folder.
It can be downloaded from `https://catalog.data.gov/dataset/crime-data-from-2020-to-present`

2. Run the `TreemapPreprocessing.ipynb` file to do data processing and generate 3 json files. Each json file is in the form of a tree like structure which will be used in the web application to generate the treemaps.

3. Download the `live server` extension of VS Code and open the `index.html` page. Start the live server, a web application will open in the browser with the 3 different treemaps drawn one below another

## Dataset Description

Crime Data of Los Angeles dataset, i.e the dataset assigned in A1. The given dataset has details of crimes that have taken place in Los Angeles from the January 2020
to August 2023. There are a total of 28 data fields where each field covers different aspects of the crime that took place. The major data fields that were used for the visualizations are :

- Date OCC : The date of occurrence of the crime.
- Area Name : The 21 Geographic areas of Los Angeles.
- Crm Cd Desc : Describes the crime.
- LAT, LON : Latitude and Longitude values of the crime.

## Dataset Processing

The dataset was loaded into a python notebook using the pandas library. Then the null value percentages in each column was measured. The columns with more than 40% null values
were removed from the dataset (Weapon Desc had 65% null values but it wasn’t removed as the remaining columns had useful information about crimes with weapons, which helped in generating insightful visualizations) and a new csv file was generated, which was used for the visualizations.

## Implementation 

- The treemaps are made using the d3 library in javascript and to run the website, the live server extension needs to be used. In the web application, the user has the option to change the spatial partitioning of the treemap, i.e Squarify ,Snake, etc allowing for a better understanding of the data. The level can be found out by the thickness of the margin where it decreases with the increase in level.

- Once the data is processed, we remodel the tabular data to a tree like structure which will help in drawing the treemap visualization. Three different experiments have been done on the given dataset, i.e 3 different columns are used to make the treemap visualizations : 

  - Date OCC column where the hierarchy is year -> month. This gives a 2 level hierarchy where the value of the leaf nodes is the number of crimes that took place in that month of the year.
  - Area Name column where the hierarchy is North or South LA -> Area names. This gives a 2 level hierarchy where the parents are North LA and South LA , with the children being the area names that belong to these regions. The values of the leaf nodes are the number of crimes that have taken place in the area name.
  - Crm cd Desc column where the crimes that are similar are grouped , which becomes the first level of hierarchy, and the leaf nodes being the different crimes in the group. The value of the leaf node is the number of crimes that fall under the given crime code description.

Once the hierarchies are determined, a dictionary structure was initialized which had the name and children attributes where children is an array of dictionaries that come under the parent. The dictionaries corresponding to the last level have a value attribute instead of children. These dictionaries are converted to a json file that is used by the d3 library to make the visualization.
