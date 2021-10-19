# Efficient product segmentation with Kmeans

The dataset (Source: https://www.kaggle.com/felixzhao/productdemandforecasting) contains historical product demand for a manufacturing company with 4 warehouses that are responsible for their shipments. 

## Project Intro/Objective

This project aims to use classify over 1000 product types based on their historical demands in order to efficiently plan for future supply, demand and warehousing management. 

## Methods Used

* Data Munging
* EDA
* Feature Engineering
* Clustering using K-Means

## Technologies

* Python
* pandas, jupyter
* sklearn
* tslearn

## Project Description

1. Data cleaning/ preprocessing: 
    * Null values for 'Date' observed and dropped from the DataFrame
    * Parse string in 'Order_demand' into float datatype
    * Parse 'Date' into datetime
    * Preprocessing for time series clustering:\
        a. Data points are required to cover every month. Using native python function *.resample* sum daily dataset by month. Zero values will be assigned for the month with no data points\
        b. Retain only data in 2016 for the scope of this project
        c. Reshape the dataframe as required by ts learn to get (no of products, no of months, 1)

2. Feature Engineering:
    * Create Month and Year period columns

3. Clustering models:
    * Clustering performed on yearly demand levels by product_code\
        a. PCA performed on its StandardScaler transformed features, based on the explained_variance_ratio, optimal number of components chosen = 4  which is sufficient in explaining close to 100% of variance across products\
        b. In determining no of clusters, plot the intertia_ at each number of clusters. The optimal no of clusters, k, is the minimum beyond which there is no significant reduction in inertia
        c. Fit the model at k=5 and PCA_components = 4
        d. Evaluation of the result shows model is unable to differentiate product types with almost 99% in the same cluster
    * Clustering performed on monthly demand levels by product_code\
        This follows the same steps as above, with no significant improvements in the model
    * Time Series Clustering using DTW (Dynamic Time Wrap) based on daily demand\
        a. install and import tslearn
        b. using pre-processed dataset transform the features of dataset using TimeSeriesScalerMeanVariance
        c. Plot the elbow graph to select the optimal no of clusters (k) 
        d. Evaluation of the result shows significant improvement with products split reasonably into 5 clusters 
        e. Visualise the demand level across time and the *cluster_centers_* of each k, this provides indications of seasonality of each cluster

4. Business application
    * Differences in product life cycle can be observed across different clusters
    * Time series clustering provides insights on seasonality of products which allows cluster based planning for future demands
    * Visualise the level of products fulfilled by each cluster and consider the need to readjust based on similarity in seasonality of each cluster

Note that some improvements can be made to current project:
* Further evaluate the accuracy of the model using the silhouette plot
