# Kepler challenge
Webpage for the challenge: https://www.kaggle.com/keplersmachines/kepler-labelled-time-series-data/home

I attempted this challenge as my project for a python course held at Uppsala University in 2018 (https://github.com/uu-python). I show my workings on this project through three jupyter notebook.

## step 1: preprocessing
Here I explore the data a little, putting it in the necessary format for tsfresh (which I used for feature extraction/selection), and plot the time series data passed through a few filters. I've decided to give tsfresh two time series for each star:
1. 5 step minimum filter of the detrended time series
2. 5 step mean filter of the raw time series

I used robust scaling to standardise these time series. As we only have 37 positives and over 5000 negatives (!), I augment the data by reversing the 37 posiitve time series (as done by Peter Greenholm in his kernel "Mystery Planet (99.8% CNN)").

## step 2: modelling
1. extract and select features from the preprocessed time series (based on the 37 actual + 37 augmented positives and 200 randomly drawn negatives). Positive = with an exoplanet, negative = without
2. apply PCA to the selected features
3. fit Random Forest models based on the top 20 PCs
4. perform transductive conformal prediction (TCP) 100 times by repeating steps 1-3
5. save the modelling output

## step 3: evaluation
1. plot the median p-values from the 100 replicates of the TCP algorithm
2. show TCP confusion matrices for two different significance levels (80% and 95%). 

At both the 95% and 80% significance level I correctly classified 3/5 of the planets that actually had exoplanets. At the 95% significance level the two remaining stars are classified as potentially belonging to either category. If I take an 80% significance level I do quite well with the nonexoplanet stars, but at a more stringent significance level a significant proportion of these become somewhat difficult to correctly classify. Although very few are put in the positive category.
