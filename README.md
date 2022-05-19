# Modeling Earthquake Damage

### Collaborators:
* Emily Gama
* Jack Roach
* Tamara Frances
* Marva Loyfer

## Background

In 2015, a 7.8 magnitude earthquake struck the Nepalese capital of Kathmandu that killed almost 9,000 people. For an extended length of time following the disaster, parts of the city were without power, water, and resources. Large avalanches were set off on nearby Mt Everest as well due to this tragic event. This earthquake is often referred to as the Gorkha Earthquake as the epicenter was in the Gorkha District, a historic region whose namesake is attributed to soldiers who aided in creating modern Nepal. 

Towers, homes, and other structures were destroyed. In fact, it is estimated that over 600,000 structures were destroyed in the aftermath. From news reports and pictures, it appeared that older buildings were more impacted by this earthquake. At the UNESCO World Heritage Site in Kathmandu Valley, buildings centuries-old were destroyed, including temples, courtyards, monuments, and houses of meditation. The earthquake also triggered several aftershocks that continued to destroy structures and leave Nepal people homeless. 

These reports of structural loss following the earthquake are consistent with loss from other similar events. Structures, buildings, and homes can be damaged when the seismic motion of the tectonic plates causes waves significant enough to wreck anything on the ground surface. This can include bridges, towers, dams, residential homes, power lines, and resource stations (like water treatment centers). Because of this significant loss, larger earthquakes are often devastating. Earthquakes can also cause other natural disasters like floods, tsunamis, avalanches, sinkholes, landslides, and fires. Again, all of which can contribute to further structural loss. 

* [*Source: Driven Data*](https://www.drivendata.org/competitions/57/nepal-earthquake/page/135/)
* [Source: *2015 Nepal Earthquake Portal*](http://eq2015.npc.gov.np/#/)
* [Source: *Raw Data Download*](http://eq2015.npc.gov.np/#/download)
* [*Source: Gorkha Earthquake*](https://en.wikipedia.org/wiki/April_2015_Nepal_earthquake)
* [*Source: Gorkha District*](https://en.wikipedia.org/wiki/Gorkha_District)
* [*Source: Britannica, Nepal Earthquake*](https://www.britannica.com/topic/Nepal-earthquake-of-2015)


## Problem Statement
This project will aim to predict the damage level of a structure following an earthquake using data from the 2015 Gorkha earthquake in Nepal that includes structural information, socio-economic data, and disaster impact. 


## Data


The data was collected by Kathmandu Living Labs and Central Bureau of Statistics (National Planning Commission Secretariat of Nepal), and it is considered one of the “largest post-disaster datasets ever collected” (DrivenData website). It includes information on over 260,000 damaged structures impacted by the earthquake. The dataset’s 39 columns range from age of the building to type of roof or flooring to the configuration makeup of the building prior to damage. Our target variable was the damage level quantified into 3 levels: low damage (1), medium damage (2), and almost complete destruction (3)

We put together a Tableau dashboard to explore this differentiation: [*Tableau*](‘https://public.tableau.com/views/Book1_16527329823420/Dashboard?:language=en-US&:display_count=n&:origin=viz_share_link’) 

Our biggest struggle with this dataset is that the categorical features were coded randomly by DrivenData. This means that we could not decipher which letter attributed to which category so as to make inferences from their meaning or encode them in any meaningful way. This can be seen with the data dictionary below. 

|Feature|Type|Dataset|Description|
|---|---|---|---|
|damage_level (target)|*ordinal*|level of destruction to structure. Possible values: 1, 2, 3|
|geo_level_1_id, geo_level_2_id, geo_level_3_id|*int*|geographic region in which building exists, from largest (level 1) to most specific sub-region (level 3). Possible values: level 1: 0-30, level 2: 0-1427, level 3: 0-12567|
|count_floors_pre_eq|*int*|number of floors in the building before the earthquake|
|age|*int*|age of the building in years|
|area_percentage|*int*|normalized area of the building footprint|
|height_percentage|*int*|normalized height of the building footprint|
|land_surface_condition|*categorical*|surface condition of the land where the building was built. Possible values: n, o, t|
|foundation_type|*categorical*|type of foundation used while building. Possible values: h, i, r, u, w|
|roof_type|*categorical*|type of roof used while building. Possible values: n, q, x|
|ground_floor_type|*categorical*|type of the ground floor. Possible values: f, m, v, x, z|
|other_floor_type|*categorical*|type of constructions used in higher than the ground floors (except of roof). Possible values: j, q, s, x|
|position|*categorical*|position of the building. Possible values: j, o, s, t|
|plan_configuration|*categorical*|building plan configuration. Possible values: a, c, d, f, m, n, o, q, s, u|
|legal_ownership_status|*categorical*|legal ownership status of the land where building was built. Possible values: a, r, v, w|
|count_families|*int*|number of families that live in the building|
|has_superstructure_ ...|*binary*|flag variable that indicates if the superstructure was made of a certain material|
|has_secondary_use|*binary*|flag variable that indicates if the building was used for any secondary purpose|
|has_secondary_use_ ...|*binary*|specifies the secondary use of the building|


## Model and Findings

Because our dataset had over 260,000 datapoints, we decided to first work with smaller sample size of 1,000 and 5,000 data points. This allowed us to build and test models succinctly without the lengthy model run times on such a large data set. The models we built on these smaller datasets were Logistic Regression, SVC, Random Forest Classifier, Decision Trees Classifier, GradientBoost Classifier, and KNearestNeighbors Classifier. We chose these as we wanted to test a broad range of classifier model types in order to see which could produce the best F1 score. 
As for our metric, we are choosing the F1 score as the best evaluation metric for our model because it is that balanced score between precision and recall, and it helps explain the relationship around all positive predictions and false negatives. It also is beneficial for us because we have moderately imbalanced data. It is more transparent about model performance, for example, if our model was heavily predicting one class over another. By evaluating the F1 score, we can get a more clear picture of model performance on our data.

So, we found that K Nearest Neighbors Classifier and Gradient Boost Classifier fit best on our sample datasets. Although these models fit well on the training set, we noted overfitting on the sample test sets. However, we decided to move forward with using these models on our big dataset as they showed promise for a larger scale model. 

Our KNN model fit the big dataset well with a 0.71 test F1 score. The GBC fit the big dataset slightly worse with a 0.68 for both train and test F1 scores. Between these and because we chose F1 to be our evaluation metric, our final model choice is KNN. This intuitively makes sense as we can see structures built in clusters like cities and neighborhoods; it makes sense that a model looking around a certain data point would find similarities. While the GradientBoost performed well on the data, the KNN just slightly outperformed and that was the ultimate decider on our final model. While we were mostly focused on the F1 score, I will also note that the accuracy score for our model was 72%. 

Our model also outperformed the baseline model of 56% predicting all medium level damage. While the model still heavily predicted medium level damage, it predicted more accurately at 72% on the test data. One key part of building our model was tuning hyperparameters. We found that using a neighbor count of 8 was the best hyperparameter. Since our dataset was so large at over 260,000 data points, having a larger neighbor count is beneficial to working through the entirety of the data. Finally, even though KNN over fit on the smaller datasets, it did not show major overfitting on the larger dataset. We attributed this to the larger number of datapoints to train on as compared to the smaller dataset.


Conclusion and Recommendations
 
The KNN model provided the most accurate predictions, but we could not determine which features affected performance the most, a known limitation of the KNN model. Consequently, it was useful for the competition, but to make more meaningful recommendations, we used the logistic regression model.
 
According to our analysis, the following five elements had the biggest impact on the extent of damage:
 
|Feature|Type|Dataset|Description|
|---|---|---|---|
|area_percentage|*int*|normalized area of the building footprint|
|foundation_type|*categorical*|type of foundation used while building. Possible values: h, i, r, u, w|
|roof_type|*categorical*|type of roof used while building. Possible values: n, q, x|
|ground_floor_type|*categorical*|type of the ground floor. Possible values: f, m, v, x, z|
|has_ superstructure_cement_mortar_brick|*binary*|flag variable that indicates if the superstructure was made of a certain material|

 
Cities and regions may find this data helpful for preparing their storm readiness programs for possible naturaldisasters. 

Thank you!
