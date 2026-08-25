RFM Scoring 
rfm["R_score"] = pd.qcut( rfm["recency"], 4, labels=[4,3,2,1], duplicates="drop" 
).astype(int) 
rfm["F_score"] = pd.qcut( rfm["frequency"].rank(method="first"), 4, labels=[1,2,3,4] ).astype(int) 
rfm["M_score"] = pd.qcut( rfm["monetary"].rank(method="first"), 4, labels=[1,2,3,4] 
).astype(int) 
rfm["RFM_score"] 	= 	( rfm["R_score"].astype(str) 	+ rfm["F_score"].astype(str) + 
rfm["M_score"].astype(str)




Simple Item-Based Similarity Example 
from sklearn.metrics.pairwise import cosine_similarity import 
pandas as pd 
basket 	= 	( 	items.merge( customer_orders[["order_id","customer_unique_id"]], on="order_id", how="inner" 
) 
.assign(value=1) 
.pivot_table( index="customer_unique_id", columns="product_id", values="value", aggfunc="sum", fill_value=0 
) 
) 
item_matrix = basket.T similarity = cosine_similarity(item_matrix) similarity_df = pd.DataFrame( similarity, index=item_matrix.index, 
columns=item_matrix.index 
)




Example Using VADER 
from nltk.sentiment import SentimentIntensityAnalyzer import nltk nltk.download("vader_lexicon") sia = SentimentIntensityAnalyzer() 
reviews["comment"] = reviews["review_comment_message"].fillna("") reviews["compound"] = reviews["comment"].apply( lambda x: sia.polarity_scores(x)["compound"] 
) 
reviews["sentiment"] = reviews["compound"].apply( lambda x: 
"positive" if x >= 0.05 else ("negative" if x <= -0.05 else "neutral") )



Example Using VADER 
from nltk.sentiment import SentimentIntensityAnalyzer import nltk nltk.download("vader_lexicon") sia = SentimentIntensityAnalyzer() 
reviews["comment"] = reviews["review_comment_message"].fillna("") reviews["compound"] = reviews["comment"].apply( lambda x: sia.polarity_scores(x)["compound"] 
) 
reviews["sentiment"] = reviews["compound"].apply( lambda x: 
"positive" if x >= 0.05 else ("negative" if x <= -0.05 else "neutral") )




FastAPI Example 
from fastapi import FastAPI import joblib import pandas as pd app = FastAPI() churn_model = joblib.load("models/churn_model.joblib") 
@app.post("/churn") def churn_prediction(data: dict): X = pd.DataFrame([data]) 	probability 	= churn_model.predict_proba(X)[0, 	1] 	return 
{"churn_probability": float(probability)}
