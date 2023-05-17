# A Preliminary EDA on Recipes from food.com
Authors: Linda Zhang (xiz115@ucsd.edu), Kay Qu(kqu@ucsd.edu)

# About this project
this is a paired EDA project which is originally assigned by UCSD's DSC80 offering during SP23. 
we will be focusing on  cleaning, exploreing, and visualizing data scraped from [food.com](food.com). 
More information about the project's guidelines can be found [here](https://dsc80.com/project3/recipes-and-ratings/).

# Part I: Introduction

## Research Question

We would like to investigate the relationship between Time (in minutues) to prepare a recipe and whether it correlates with the average rating that it receives. We hypothesize that ... (TBD). We will be implementing permutation testing techniques to investigate this question. 

## About our data
our data is derived from [food.com](https://www.food.com), originally scraped and used by them. The raw csv files can be found [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

Below are the breif description of the raw data that we are going to perform cleaning on: 

### Recipes (83782 rows, 12 columns)

| Column           | Description                                          |
|------------------|------------------------------------------------------|
| 'name'           | Name of the recipe                                          |
| 'id'             | Recipe's ID                                            |
| 'minutes'        | Time (in minutues) to prepare recipe                            |
| 'contributor_id' | Recipe contributor's ID                     |
| 'submitted'      | Date when the recipe was submitted                            |
| 'tags'           | Categorical tags for recipe                             |
| 'nutrition'      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps'        | Number of steps in recipe                            |
| 'steps'          | Description for 'n_steps', in order                       |
| 'description'    | additional description provided by user                             |

### Ratings (731927 rows, 5 columns)

| Column       | Description           |
|--------------|-----------------------|
| 'user_id'    | User's ID               |
| 'recipe_id'  | Recipe's ID             |
| 'date'       | Date when the rating was posted   |
| 'rating'     | rating, scaling from 1-5          |
| 'review'     | user's review message about the recipe           |

# Part II: Cleaning and EDA (Exploratory Data Analysis)

We begin our data cleaning by merging the two dataframes: `raw_recipes` and `raw_iteractions`together. We perform a left merge on both dataframe's `recipe_id` (formatted as `id` in `raw_recipe`) so that the resulting `merged` dataframe will have all recipes from the `raw_recipes` dataframe with information from the `raw_interactions`. A more in-depth guide about merging can be found under [this Stack Overflow post](https://stackoverflow.com/questions/53645882/pandas-merging-101).

Then we replace all illegal ratings (e.g. 0) with `Nan`. This step is to prevent numerical missing data from affecting certain aggregation methods, such as when looking for average rating using `.mean()`. Since aggregation methods ignores `NaN` as default, we can safely proceed to the next step. 