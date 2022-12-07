# How to increase house value via rennovation

## Project Overview
In this project, I attmepted to answer following question: What kind of house rennovations can home owners do to efficiently increase the predicted market price of their house?\
In order to answer this question, I have analysed data relating to houses in King County in order to investigate the relationshop between various feature of a house-like the size of the living space, the architecture, and the qaultiy of the material used to build the house- and the price of the house. Through the methods of multiple linear regression, I have isloated three features that significantly impact the price, namely the living area, building condition, and building grade.\
My three recommendations for home owners to maximize the predicted value of their houses through rennovations would be:\
**1. Try to increase living area by removing walls or adding rooms**\
**2. Make sure the facitlies are well-maintained to increase building condition**\
**3. Try to include custom designs or luxurious additions to the house to increase building grade**

### Business Problem
As briefly stated above, the goal of this project is to do research for a real estate agency that helps homeowners buy or sell homes, and to provide insight on what advice to give to homeowners regarding how home renovations might increase the estimated value of their homes, and by what amount.

### The Data

This project uses the King County House Sales dataset, which can be found in  `kc_house_data.csv` in the data folder of this GitHub repository. For the pruposes of this project, I decided to focus on select information that would be the most practical and useful for homeowners, such as the number of rooms, the area of the living space, and the classification for the house in terms of grade and conditon, as these are characteristics that could easily be altered via rennovating the house.

### Methods

The main tool used in this project is multiple linear regression, which takes in multiple independent variables and identifies how each one impacts the dependent varible, which is the price of the house in this scenario. The goal of doing so is to identify the top three variables that have the most significant impact on the price, and the extent to which they increase the price.\
Linear regression is appropriate for this analysis because it finds the average increase in price per unit increase of an independent variable, assuming the relationship between the variables are linear, which seems to be the case upon examination of the scatter matrix.\
To get to the model that best fits the data and thus has the highest predictive power, I used R squared- which measures the proportion of the variance of the dpenedent variable that is explained by the model-as a metirc to guage the fitness of the model, and I developed a simple baseline model to compare the more elaborate models to.
#### The Baseline Model
For the baseline model, I identified the variable that is the most strongly correlated to the price, whcih was squarefoot living area, and plotted a simple linear regression with just this variable. I obtained the R2 score by a 5-fold cross validation.\
![Baseline](baseline.png)

Then, I divided the data into continuous variables and categorical variables. For the categorical variables, waterfront has binary values while the others (grade, condition, number of rooms) have multiple discrete numbers as values. I also observed that waterfront has null values. Upon close examination, I noticed the vast ,majority of the non-null values are 'NO', and decided to fill in the null values with the mode value for hihgher probability of accuracy without losing valuable indformation, which would happen if I were to drop the null rows.\
Moreover, I converted the rest of the categorical values into the dtype float, then fitted a multiple linear regression model to the data. Using the same cross validation method, I took note of the R2 score, which was higher than that of the baseline, indicating an improvement. Moreover, according to the OLS model, all the coefficients were statistically significant, meaning they are very likely correlated due to cause and effect instead of by chance.
I then log transformed and normalised the continuous data so that the residuals would be more normally distributed and the data distribution more homoscedastic to better fulfill the assumptions of linear regression, then ran a multiple linear regression with the transformed data, but the R2 score was signifcantly worse than that of the untransformed data, therefore I chose the previous model over this one.\
Then, I tried one-hot encoding the categorical variables instead of converting them into numberical data to see if the resulting model would yield a better R2 score, but it did not.\
Finally, I looked at the correlation coefficients of all the variables to see if there is any strong colinearity, defined as having the correlation coefficient higher than or equal to 0.75, but there were not, so no further modifying was needed in that regard.\
After experimenting with different models by transforming the data in various ways, I settled for the second model wih all the data as numerical values and untransformed, with an R2 score of 0.587.\
Using the untransformed data, I then used train-test split 5 times to run a linear regression model, and the coefficients and final R2 score was obtained from the avergae of the 5 results. This was done as opposed to using the OLS model, which uses all the data as input, in order to avoid overfitting. 

## Results
The most significant variables were building grade, building conditon, and squarefoot living, their respective coefficient can be interpreted as:

**For every unit increase in building condition, the price increases by an average of 59472.8 dollars.**

![condition](cond_ind.png)

**For every unit increase in building grade, the price increases by an average of 106323.2 dollars.**

![grade](grade_ind.png)

**For every unit increase in squarefoot living area, the price increases by an average of 208.3 dollars.**

![living_area](sqft_living.png)

<p>Keeping this in mind, homeowners may aim to increase these variables accordingly to maximize the predicted value of their houses.</p>

### Conclusion

With these three coefficients in mind, the three recommendations one can give to home owners are the following.\
**1. improve building condition**\
Building condition has to do with the maintenance of the house,when doing rennovations, aim to increasing the life expectancy of the hosue upgrading old or deteriorating structures.\
**2. improve building grade**\
Building grade is related to the architecture/ design of the house, both interior and exterior.This includes the aesthetic of the architecture, and the quality of the material used in the building.The basic requirement is to meet the building code, and the grade can be further increased by including custom designs and added amenities such as solid woods, bathroom fixtures, marble entry ways, wood trim, high quality cabinet work etc.\
**3. increase living area**\
Living area is the area of the house exlcuding the walls. When rennovating, living area could be increased by removing walls or adding extra rooms.

### Caveats
According to the OLS model, the data has a significant degree of positive skewness and kurtosis, the former meaning the data is not normally distributed, with the median and mode higher than the mean, and the latter meaning there are many outliers that cannot be explained by the linear regression model.\
Moreover, the data violates the two assumptions of linear regression- homoscedasticity and normality, which negatively impacts the reliability of the predictions.\
The Next step would be to investigate the outliers of the data set to see why there is such high kurtosis and skewedness,
and potentially discover more confounding variables that would help the model make better predictions. A good place to start would be to include the variables that were disregarded in this project to see if they can explain the ouliers better.

### For More Information 
Please review our full analysis in [the github repository](./dsc-phase-2-project-v2-3) or my [presentation](./DS_Project2_Presentation.pdf).\
For any additional questions, please contact **Beatrix Wong, beatrix.wmh@hotmail.com**

## Repository Structure
```
├── README.md                           <- The top-level README for reviewers of this project
├── student.ipynb                       <- Narrative documentation of analysis in Jupyter notebook
├── DS_Project_Presentation.pdf         <- PDF version of project presentation
├── data                                <- Both sourced externally and generated from code
└── images (.png)                       <- Both sourced externally and generated from code
```