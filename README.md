# Customer Segmentation by RFM Analysis w/ K-Means Clustering

Pls find the code and dynamic output here: https://nbviewer.jupyter.org/github/chanchanngann/RFM/blob/master/RFM.ipynb

### Background
From the Pareto Principle, 80% of sales come from 20% of customers. Usually your business is largely supported by a fraction of your customer base - your best customers.

From a marketing perspective, it is important to understand the characteristics and preferences of your best customers for at least 2 reasons:

1. to continue to provide the group with what they are looking for and keep them as customers
2. to target your marketing efforts towards prospects who resemble your best customers

### Objective
To identify the best customers who will be loyal from the start 

### Approach
Perform customer segmentation using RFM clustering to identify the best customers

  > Idea behind RFM:
  >
  > __R - Recency__ : Customers who have purchased from your store recently are more likely to convert again than those who haven't visit your store for a while
  > 
  > __F - Frequency__ : Customers who buy from your store more often are more likely to buy again than those who buy infrequenctly
  > 
  > __M - Monetary__ : Customers who spend more are more likely to buy again than those who spend less

### Steps
1. Load the dataset and proceed data cleaning

2. K-means Clustering to create the Recency, Frequency and Monetary scores for each customer
    > Find out the optimal no of clusters to be applied for each attribute.
    > Aim to assign same no of clusters for each attribute, a higher cluster no (score) should reflect a better result,i.e. cluster 3 should be better than cluster 1
    > - Recency: number of days since most recent purchase date (more #days would have lower cluster#)
    > - Frequency: number of transactions within the period (higher freq. would have higher cluster#)
    > - Monetary: total sales attributed to the customer (higher sales would have higher cluster#)

3. Compare the RFM clusters (observe the relationship against each other) and calculate the total score
4. Segment the customers into different categories based on the RFM scores and total score:
    > - Champions: our best customers!
    > - Loyalists
    > - New Customers
    > - Can???t Lose Them
    > - Slipping
5. Conclusion on RFM analysis

***About the dataset*** 
_This Online Retail II data set contains all the transactions occurring for a UK-based and registered, non-store online retail between 01/12/2009 and 09/12/2011.The company mainly sells unique all-occasion gift-ware. Many customers of the company are wholesalers. Data Source: https://www.kaggle.com/mathchi/online-retail-ii-data-set-from-ml-repository_


***
### Details

#### Recency
We need to find out the most recent purchase date of each customer and check out how many days since that purchase. Then, we can apply K-means clustering to assign each customer a recency score. 

![](https://github.com/chanchanngann/RFM/blob/master/image/01_recency_hist.png)

Average Recency is 90 days and median Recency is 51 days.

#### Frequency
We need to find out no of transaction for each customer which would be the frequency. Then, we can apply K-means clustering to assign each customer a frequency score.

![](https://github.com/chanchanngann/RFM/blob/master/image/03_frequency_hist.png)

Average Frequency is 95 transactions per customer and median Frequency is 44 transactions.

#### Monetary
We will calculate the total sales generated by each customer. Then, we will apply K-means clustering to assign each customer a Monetary score.

![](https://github.com/chanchanngann/RFM/blob/master/image/04_monetary_hist.png)

Average monetary value is 1905 per customer and median monetary value is 656. Some customers carried negative transaction value too.

#### K-means Clustering to create the Recency, Frequency and Monetary scores
The goal of K-means clustering is to group data points into distinct non-overlapping subgroups. The mathematics behind K-means clustering, involves minimizing the sum of square of distances sse between the cluster centroid and its associated data points. K-means clustering could be used to examine multiple variables, but in this exercise I just focus on 1 variable (R/F/M) each time.

![](https://github.com/chanchanngann/RFM/blob/master/image/02_elbow_method.png)
_The Elbow Method_

The sse(sum of squares) value decreases as number of clusters increases, we aim to get an optimal point after which the reduction of sse is only minimal. In this case, number of clusters = 3 would be the optimal point or we can also choose 4 which resulted in even lower sse. I have applied #clusters = 3 to all RFM variables.

#### Compare the RFM clusters
After performing K-means clustering, I assigned cluster # (0,1,2,3) to each customer. We can examine if any pattern obeserved by comparing different clusters.

![](https://github.com/chanchanngann/RFM/blob/master/image/05_RFM_cluster_count.png)

![](https://github.com/chanchanngann/RFM/blob/master/image/06_recency_vs_monetary_per_freq_cluster.png)

High spenders (cluster=3) usually has recency score of 3, which means those visit recently, while frequency score varies from 1 to 2.
Moderate spenders (cluster>=2) got high recency score of 3, and frequency at least 1.
Lower spender group usually visit less frequently (concentrated in frequency score 0 to 1).

#### Overall RFM Score
We have created Recency, Frequency and Monetary scores for each customer, we can sum up them to give a total score. The higher the score, the more valuable the customer is.

![](https://github.com/chanchanngann/RFM/blob/master/image/07_totalScore_hist.png)

#### RFM segmentation

The individual RFM distribution actually skewed towards right for the given dataset, which means most customers visited less frequent and consumed less amount but quite a lot are recent visitors. We have most customers filled in the low Frequency & Monetary clusters, while recency clusters are rather evenly distributed and total score of most customers is no more than 3.

With reference of above summary, we can group the customers into 5 segments based on the individual RFM scores and total score:

- __Champions__ (R>0,total score>=5) are your best customers, who bought most recently, most often, and are heavy spenders. Reward these customers. They can become early adopters for new products and will help promote your brand.

- __Loyalists__ (R>0,F>=1,3<=total score<5) are these recent & frequenct visitors and who spent a good amount. Offer membership or loyalty programs or recommend related products to upsell them and help them become your Champions.

- __New Customers__ (R=3,F=0) are your customers who visited recently but are not frequent shoppers. Start building relationships with these customers by providing onboarding support and special offers to increase their visits.

- __Can???t Lose Them__ (R<3,total score>=2) are customers who used to visit and purchase quite often, but haven???t been visiting recently. Bring them back with relevant promotions, and run surveys to find out what went wrong and avoid losing them to a competitor.

- __Slipping__ Great past customers who haven't bought in awhile.Apply retention strategies to get them back into your business.



Refer to the segment distribution below, most of the customers are newly boarding ones.

![](https://github.com/chanchanngann/RFM/blob/master/image/08_segment_hist.png)

Champions visited recently, most frequently and were high spenders. The slipping group had not visited the store for long, more than 200 days (median), and spent a little.

![](https://github.com/chanchanngann/RFM/blob/master/image/09_RFM_segment_count.png)

Bubble size represents frequency, the champions group climb along monetary axis with almost 0 recency and big bubble size.

![](https://github.com/chanchanngann/RFM/blob/master/image/10_segmentation_M_vs_R.png)

3-D plot shows the distribution of customer segment more clearly.

![](https://github.com/chanchanngann/RFM/blob/master/image/11_RFM_3Dplot1.png)
![](https://github.com/chanchanngann/RFM/blob/master/image/12_RFM_3Dplot2.png)


### Conclusion

Once we are done calculating the RFM scores, we are able to segment the customer pool and identify the best customers - the champion segment. Then we can start to analyse the characteristics and purchasing behavior of this group to figure out why they perceive more value in our business than the folks who seldom visit and consume less amount.

This will help us sharpen the understanding of the target market and be more precise in communicating with actual and potential customers.

*Reference*
- *https://towardsdatascience.com/data-driven-growth-with-python-part-2-customer-segmentation-5c019d150444*
- *https://www.eightleaves.com/2011/01/using-rfm-to-identify-your-best-customers#:~:text=RFM%20in%20a%20nutshell&text=The%20idea%20behind%20RFM%20is,than%20customers%20who%20buy%20infrequently.*
- *https://www.barilliance.com/rfm-analysis/*
- *https://clevertap.com/blog/rfm-analysis/*
- *https://cran.r-project.org/web/packages/rfm/vignettes/rfm-customer-level-data.html*

