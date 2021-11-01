# mini-project
Smart television remote
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "\n",
    "import random\n",
    "import re\n",
    "import pickle\n",
    "import nltk"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/anubhav/anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2785: DtypeWarning: Columns (7) have mixed types. Specify dtype option on import or set low_memory=False.\n",
      "  interactivity=interactivity, compiler=compiler, result=result)\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>marketplace</th>\n",
       "      <th>customer_id</th>\n",
       "      <th>review_id</th>\n",
       "      <th>product_id</th>\n",
       "      <th>product_parent</th>\n",
       "      <th>product_title</th>\n",
       "      <th>product_category</th>\n",
       "      <th>star_rating</th>\n",
       "      <th>helpful_votes</th>\n",
       "      <th>total_votes</th>\n",
       "      <th>vine</th>\n",
       "      <th>verified_purchase</th>\n",
       "      <th>review_headline</th>\n",
       "      <th>review_body</th>\n",
       "      <th>review_date</th>\n",
       "      <th>timestamp</th>\n",
       "      <th>IP Address</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>US</td>\n",
       "      <td>20422322</td>\n",
       "      <td>R8MEA6IGAHO0B</td>\n",
       "      <td>B00MC4CED8</td>\n",
       "      <td>82850235</td>\n",
       "      <td>BlackVue DR600GW-PMP</td>\n",
       "      <td>Mobile_Electronics</td>\n",
       "      <td>5</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>N</td>\n",
       "      <td>Y</td>\n",
       "      <td>Very Happy!</td>\n",
       "      <td>As advertised. Everything works perfectly, I'm...</td>\n",
       "      <td>2015-08-31</td>\n",
       "      <td>1.440988e+09</td>\n",
       "      <td>193.93.167.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>US</td>\n",
       "      <td>40835037</td>\n",
       "      <td>R31LOQ8JGLPRLK</td>\n",
       "      <td>B00OQMFG1Q</td>\n",
       "      <td>82850235</td>\n",
       "      <td>GENSSI GSM / GPS Two Way Smart Phone Car Alarm...</td>\n",
       "      <td>Mobile_Electronics</td>\n",
       "      <td>5</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>N</td>\n",
       "      <td>Y</td>\n",
       "      <td>five star</td>\n",
       "      <td>it's great</td>\n",
       "      <td>2015-08-31</td>\n",
       "      <td>1.441002e+09</td>\n",
       "      <td>193.93.167.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>US</td>\n",
       "      <td>51469641</td>\n",
       "      <td>R2Y0MM9YE6OP3P</td>\n",
       "      <td>B00QERR5CY</td>\n",
       "      <td>82850235</td>\n",
       "      <td>iXCC Multi pack Lightning cable</td>\n",
       "      <td>Mobile_Electronics</td>\n",
       "      <td>5</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>N</td>\n",
       "      <td>Y</td>\n",
       "      <td>great cables</td>\n",
       "      <td>These work great and fit my life proof case fo...</td>\n",
       "      <td>2015-08-31</td>\n",
       "      <td>1.440959e+09</td>\n",
       "      <td>193.93.167.87</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  marketplace  customer_id       review_id  product_id  product_parent  \\\n",
       "0          US     20422322   R8MEA6IGAHO0B  B00MC4CED8        82850235   \n",
       "1          US     40835037  R31LOQ8JGLPRLK  B00OQMFG1Q        82850235   \n",
       "2          US     51469641  R2Y0MM9YE6OP3P  B00QERR5CY        82850235   \n",
       "\n",
       "                                       product_title    product_category  \\\n",
       "0                               BlackVue DR600GW-PMP  Mobile_Electronics   \n",
       "1  GENSSI GSM / GPS Two Way Smart Phone Car Alarm...  Mobile_Electronics   \n",
       "2                    iXCC Multi pack Lightning cable  Mobile_Electronics   \n",
       "\n",
       "  star_rating  helpful_votes  total_votes vine verified_purchase  \\\n",
       "0           5            0.0          0.0    N                 Y   \n",
       "1           5            0.0          1.0    N                 Y   \n",
       "2           5            0.0          0.0    N                 Y   \n",
       "\n",
       "  review_headline                                        review_body  \\\n",
       "0     Very Happy!  As advertised. Everything works perfectly, I'm...   \n",
       "1       five star                                         it's great   \n",
       "2    great cables  These work great and fit my life proof case fo...   \n",
       "\n",
       "  review_date     timestamp     IP Address  \n",
       "0  2015-08-31  1.440988e+09  193.93.167.87  \n",
       "1  2015-08-31  1.441002e+09  193.93.167.87  \n",
       "2  2015-08-31  1.440959e+09  193.93.167.87  "
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "dataset = pd.read_csv(\"reviews.csv\",sep=\"\\t\")\n",
    "#dataset.drop(columns= \"Unnamed: 0\",inplace = True)\n",
    "dataset.head(3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "### WORKING ON TWITTER DATA FOR SENTIMENTAL ANALYSIS\n",
    ""\n",
    "You can download these pickle files from https://github.com/anubhavs11/Sentimental-Analysis-using-Logistic-Regression \n",
    "with open(\"Pickle Files/classifier.pickle\",\"rb\") as f:\n",
    "    clf = pickle.load(f)\n",
    "\n",
    "with open(\"Pickle Files/TfidfModel.pickle\",\"rb\") as f:\n",
    "    tfidf = pickle.load(f)\n",
    "\n",
    "def getSentiment(text):\n",
    "\n",
    "    # PREPROCESSING THE DATASET\n",
    "    text = str(text)\n",
    "    text = text.lower()\n",
    "    text = re.sub(r\"that's\",\"that is\",text)\n",
    "    text = re.sub(r\"there's\",\"there is\",text)\n",
    "    text = re.sub(r\"what's\",\"what is\",text)\n",
    "    text = re.sub(r\"where's\",\"where is\",text)\n",
    "    text = re.sub(r\"it's\",\"it is\",text)\n",
    "    text = re.sub(r\"who's\",\"who is\",text)\n",
    "    text = re.sub(r\"i'm\",\"i am\",text)\n",
    "    text = re.sub(r\"she's\",\"she is\",text)\n",
    "    text = re.sub(r\"he's\",\"he is\",text)\n",
    "    text = re.sub(r\"they're\",\"they are\",text)\n",
    "    text = re.sub(r\"who're\",\"who are\",text)\n",
    "    text = re.sub(r\"ain't\",\"am not\",text)\n",
    "    text = re.sub(r\"wouldn't\",\"would not\",text)\n",
    "    text = re.sub(r\"shouldn't\",\"should not\",text)\n",
    "    text = re.sub(r\"can't\",\"can not\",text)\n",
    "    text = re.sub(r\"couldn't\",\"could not\",text)\n",
    "    text = re.sub(r\"won't\",\"will not\",text)\n",
    "    \n",
    "    text = re.sub(r\"\\W\",\" \",text)\n",
    "    text = re.sub(r\"\\d\",\" \",text)\n",
    "    text = re.sub(r\"\\s+[a-z]\\s+\",\" \",text)\n",
    "    text = re.sub(r\"^[a-z]\\s+\",\" \",text)    \n",
    "    text = re.sub(r\"\\s+[a-z]$\",\" \",text)    \n",
    "    text = re.sub(r\"\\s+\",\" \",text)    \n",
    "    \n",
    "    sent = clf.predict(tfidf.transform([text]).toarray())\n",
    "    \n",
    "    return sent[0]\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 1. Reviews which have dual view"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "#1. different sentiment in review headline and review body\n",
    "\n",
    "remove_reviews = []\n",
    "# stores the list of review_id of fake reviews\n",
    "\n",
    "for i in range(len(dataset)):\n",
    "    #iterate through the whole dataset\n",
    "    \n",
    "        if( getSentiment( dataset[\"review_headline\"][i] ) != getSentiment( dataset[\"review_body\"][i] ) ):\n",
    "            # checking if the sentiment of the body and the headline are not same\n",
    "            \n",
    "            remove_reviews.append(dataset[\"review_id\"][i])\n",
    "            # append review_id to the list of fake reviews."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "32322"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 2. Reviews in which same user promoting or demoting a particular brand"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "#2. Users which are posting either all positive or negative reviews on different products of same brand\n",
    "\n",
    "customers = dataset.groupby(\"customer_id\")\n",
    "# groups dataset by customers\n",
    "\n",
    "customer_list = dataset[\"customer_id\"].unique()\n",
    "#list of unique customers\n",
    " \n",
    "size = len(customer_list.tolist())\n",
    "#size of total unique customers\n",
    "\n",
    "for i in range(size):\n",
    "    # iterate through all the customers\n",
    "    \n",
    "    brand_df = customers.get_group(customer_list[i])    \n",
    "    # Dataframe for each customers\n",
    "    \n",
    "    brands = brand_df.groupby(\"product_parent\")\n",
    "    # groups reviews of each customers by brand\n",
    "    \n",
    "    brands_list = brand_df[\"product_parent\"].unique()\n",
    "    # unique list of brands for each customers reviews\n",
    "    \n",
    "    no_of_brands = len(brands_list.tolist())\n",
    "    # no. of brands for which reviews had been written by the customer\n",
    "    \n",
    "    for j in range(no_of_brands):\n",
    "        # iterate through all the brands\n",
    "        \n",
    "        product_df = brands.get_group(brands_list[j])\n",
    "        # Dataframe of products for a brand for which a customer had written reviews\n",
    "        \n",
    "        no_of_products = len(product_df[\"product_id\"])\n",
    "        # no of products\n",
    "        \n",
    "        if no_of_products<=2:\n",
    "            # it will filter the products which are less than 2 for a brand\n",
    "            continue\n",
    "            \n",
    "        indices = product_df.index.values.tolist()\n",
    "        # index of the dataframe of the products of each brand for each customers\n",
    "        \n",
    "        sentiment = getSentiment(product_df[\"review_body\"][indices[0]])\n",
    "        # sentiment of the review of the first product\n",
    "        \n",
    "        isSameSentiment = True\n",
    "        \n",
    "        #discarding those cases in which we have only less than 3 reviews on same brand\n",
    "        if(no_of_products<4)\n",
    "            continue\n",
    "        \n",
    "        for k in range(1,no_of_products):\n",
    "            # iterate through all the products\n",
    "            \n",
    "            text = str(product_df[\"review_body\"][indices[k]])\n",
    "            # review of each product\n",
    "            \n",
    "            if getSentiment(text)!=sentiment :\n",
    "                # if sentiment is different than discard it\n",
    "                isSameSentiment = False\n",
    "                break;\n",
    "                \n",
    "        if(isSameSentiment):\n",
    "            # if sentiments of all the products of same brand by a customer is same, \n",
    "            #append customer_id to blocked users list\n",
    "            \n",
    "            remove_reviews.append(customer_list[i])\n",
    "            break\n",
    "        \n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "32327"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 3. Reviews in which person from same IP Address promoting or demoting a particular brand"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "#3.Reviews from same IP either all positive or negative reviews on different products of same brand\n",
    "\n",
    "\n",
    "ip = dataset.groupby(\"IP Address\")\n",
    "#grouping the dataset by ip address\n",
    "\n",
    "ip_list = dataset[\"IP Address\"].unique()\n",
    "#stores the list pf unique ip addresses\n",
    "\n",
    "remove_ip = []\n",
    "#stores the list of ip address from where reviews have been written.\n",
    "\n",
    "size = len(ip_list.tolist())\n",
    "#stores the size of the total unique ip addresses\n",
    "\n",
    "for i in range(size):\n",
    "    # iterate through all the ip addresses\n",
    "    \n",
    "    brand_df = ip.get_group(ip_list[i])\n",
    "    # Dataframe of brands for which reviews have been written from the same ip address\n",
    "    \n",
    "    brands = brand_df.groupby(\"product_parent\")\n",
    "    # grouping the products of the same brands for each ip addresses\n",
    "    \n",
    "    brands_list = brand_df[\"product_parent\"].unique()\n",
    "    #list of unique brands for each ip addresses\n",
    "    \n",
    "    no_of_brands = len(brands_list.tolist())\n",
    "    # total no. of brands\n",
    "    \n",
    "    for j in range(no_of_brands):\n",
    "        # iterate through all the brands\n",
    "        \n",
    "        product_df = brands.get_group(brands_list[j])\n",
    "        # Dataframe of the products of each brand of each products\n",
    "        \n",
    "        no_of_products = len(product_df[\"product_id\"])\n",
    "        # no of products of each brand for each ip addresses\n",
    "        \n",
    "        if no_of_products<=2:\n",
    "            # filter the reviews of the brandswith less than 3 reviews\n",
    "            break\n",
    "        \n",
    "        indices = product_df.index.tolist()\n",
    "        # indices of dataframe of products of each brand for each customers\n",
    "        \n",
    "        sentiment = getSentiment(product_df[\"review_body\"][ indices[0] ])\n",
    "        # sentiment of review of first product of each brand\n",
    "                \n",
    "        isSameSentiment = True\n",
    "        \n",
    "        for k in range(1,no_of_products):\n",
    "            # iterate through all the reviews\n",
    "            \n",
    "            text = str(product_df[\"review_body\"][indices[k]])\n",
    "            # reviews of each product\n",
    "            \n",
    "            if getSentiment(text)!=sentiment :\n",
    "                # if sentiment of 2 products of same brand are not same \n",
    "                # then check the next brand\n",
    "                isSameSentiment = False\n",
    "                break;\n",
    "                \n",
    "        if(isSameSentiment):\n",
    "            # if all the sentiments are same , append ip to blocked list\n",
    "            remove_ip.append(ip_list[i])\n",
    "            "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "['193.93.167.87', '205.10.168.66']"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "remove_ip"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 4. Reviews which are posted as flood by same user all the reviews are either positive or negative."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.sort_values(\"customer_id\",inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [],
   "source": [
    "#4. User posting (>3) reviews on the same day with all the reviews are either positive or negative.\n",
    "\n",
    "customer_group = dataset.groupby(\"customer_id\")\n",
    "#creates the group of the customers \n",
    "\n",
    "customer_group_list = dataset[\"customer_id\"].unique().tolist()\n",
    "# list of unique customers\n",
    "\n",
    "for i in range(len(customer_group_list)):\n",
    "    # iterate through all customers , starts with 1 as column could not be included\n",
    "    \n",
    "    customer_reviews = customer_group.get_group( customer_group_list[i] )\n",
    "    # Dataframe of data of each cutomers\n",
    "    \n",
    "    dates_list = customer_reviews[\"review_date\"].unique().tolist()\n",
    "    # list of dates of reviews written by each customers\n",
    "    \n",
    "    reviews_by_date = customer_reviews.groupby(\"review_date\");\n",
    "    # gouping reviews by date for each cutomers\n",
    "    \n",
    "    for j in range(len(dates_list)):\n",
    "        # iterating through all dates\n",
    "        \n",
    "        reviews_by_date_for_pos = []\n",
    "        reviews_by_date_for_neg = []\n",
    "\n",
    "        df = reviews_by_date.get_group(dates_list[j])\n",
    "        #dataframe storing the details for each details for each customers\n",
    "        \n",
    "        indices = df.index.tolist()\n",
    "        # indices of dataframe of each date\n",
    "        \n",
    "        for k in range(len(df)):\n",
    "            # iterating through dataframe of each day for each customers\n",
    "            \n",
    "            text = df[\"review_body\"][ indices[k] ]\n",
    "            #review on a single day\n",
    "            \n",
    "            if(getSentiment(text) == 0):\n",
    "                \n",
    "                #if sentiment is negative, append review_id to list of negative reviews\n",
    "                reviews_by_date_for_neg.append(df[\"review_id\"][ indices[k] ])\n",
    "                \n",
    "            else:\n",
    "                \n",
    "                #if sentiment is positive, append review_id to list of positive reviews\n",
    "                reviews_by_date_for_pos.append(df[\"review_id\"][ indices[k] ])\n",
    "                \n",
    "        # CONDITION FOR CONSIDERING THE FAKE REVIEW \n",
    "        \n",
    "        #removing postive reviews that are written by a reviewer that are > 3 on same day\n",
    "        if(len(reviews_by_date_for_pos)>3):\n",
    "            remove_reviews.extend(reviews_by_date_for_pos)\n",
    "        \n",
    "        #removing postive reviews that are written by a reviewer that are > 3 on same day\n",
    "        if(len(reviews_by_date_for_neg)>3):\n",
    "            remove_reviews.extend(reviews_by_date_for_neg)\n",
    "        "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "32348"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 5. Reviews which are posted as flood by same person from same IP Address"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [],
   "source": [
    "#5. Reviews(>3) from same IP on the same day with all the reviews are either positive or negative.\n",
    "\n",
    "ip_group = dataset.groupby(\"IP Address\")\n",
    "# grouping the dataset by ip addresses\n",
    "\n",
    "ip_list = dataset[\"IP Address\"].unique().tolist()\n",
    "# stores the list of unique ip addresses\n",
    "\n",
    "size = len(ip_list)\n",
    "# total no of unique ip addresses\n",
    "\n",
    "for i in range(size):\n",
    "    # iterate through all the ip addresses\n",
    "    \n",
    "    reviews = ip_group.get_group( ip_list[i] )\n",
    "    # dataframe of each ip\n",
    "    \n",
    "    dates_list = reviews[\"review_date\"].unique().tolist()\n",
    "    # list of dates of reviews by each ip addresses\n",
    "    \n",
    "    reviews_by_date = reviews.groupby(\"review_date\");\n",
    "    # grouping the dataframe by date\n",
    "    \n",
    "    for j in range(len(dates_list)):\n",
    "        # iterate through all the dates\n",
    "        \n",
    "        reviews_by_date_for_pos = []\n",
    "        reviews_by_date_for_neg = []\n",
    "\n",
    "        reviews_for_each_day = reviews_by_date.get_group(dates_list[j])\n",
    "        #dataframe of reviews for a day by each ip addresses\n",
    "        \n",
    "        indices = reviews_for_each_day.index.tolist()\n",
    "        # list of indices of the dataframe reviews_for_each_day\n",
    "        \n",
    "        for k in range(len(reviews_for_each_day)):\n",
    "            #iterate through all the reviews on a day by each ip addresses\n",
    "            \n",
    "            text = reviews_for_each_day[\"review_body\"][ indices[k] ]\n",
    "            # reviews on a day for an ip addresses\n",
    "            \n",
    "            if(getSentiment(text) == 0):\n",
    "                \n",
    "                #if sentiment is negative, append review_id to list of negative reviews\n",
    "                reviews_by_date_for_neg.append(reviews_for_each_day[\"review_id\"][ indices[k] ])\n",
    "            else:\n",
    "                \n",
    "                #if sentiment is positive, append review_id to list of positive reviews\n",
    "                reviews_by_date_for_pos.append(reviews_for_each_day[\"review_id\"][ indices[k] ])\n",
    "                \n",
    "        # CONDITION FOR CONSIDERING THE FAKE REVIEW        \n",
    "     \n",
    "        #removing postive reviews that are written by a reviewer that are > 3 on same day\n",
    "        if(len(reviews_by_date_for_pos)>3):\n",
    "            remove_reviews.extend(reviews_by_date_for_pos)\n",
    "        \n",
    "        #removing postive reviews that are written by a reviewer that are > 3 on same day\n",
    "        if(len(reviews_by_date_for_neg)>3):\n",
    "            remove_reviews.extend(reviews_by_date_for_neg)\n",
    "        "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "32356"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 6. Similar reviews posted in the same time interval"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "from nltk.corpus import stopwords\n",
    "from sklearn.feature_extraction.text import TfidfVectorizer\n",
    "from sklearn.metrics.pairwise import cosine_similarity"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "tfidf_vectorizer = TfidfVectorizer()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.reset_index()\n",
    "dataset.set_index(\"review_id\")\n",
    "dataset.sort_values(\"timestamp\",inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "def OnlyStopwords(str):\n",
    "    words = nltk.word_tokenize(str)\n",
    "    words = [word for word in words if word not in stopwords.words(\"english\")]\n",
    "    if(len(words)==0):\n",
    "        return True\n",
    "    return False"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "from nltk.corpus import wordnet\n",
    "\n",
    "remove_reviews = []\n",
    "indices = []\n",
    "for i in range(len(dataset)):\n",
    "    \n",
    "    reviews = [str(dataset[\"review_body\"][i])]\n",
    "    \n",
    "    try:\n",
    "        tfidf_vectorizer.fit_transform(reviews)\n",
    "    except:\n",
    "        # reviews with one word and with no dictionary meaning will be invalid\n",
    "        # e.g- [\"c\",\"O.K.\"]\n",
    "        remove_reviews.append(dataset[\"review_id\"][i]) \n",
    "        continue\n",
    "        \n",
    "    Time = dataset[\"timestamp\"][i]\n",
    "    # timestamp of the review that will be compared\n",
    "    \n",
    "    for j in range(i+1,len(dataset)):\n",
    "            \n",
    "        indices.append(dataset[\"review_id\"][j])\n",
    "        \n",
    "        if(dataset[\"timestamp\"][j]-Time <= 1800):\n",
    "            # reviews written in 30 min of intervals will be checked for same pattern\n",
    "            reviews.append(str(dataset[\"review_body\"][j]))\n",
    "        else:\n",
    "            break\n",
    "    \n",
    "    tfidf_matrix = tfidf_vectorizer.fit_transform(reviews)\n",
    "    \n",
    "    #creates TF-IDF Model\n",
    "    tfidf_list = cosine_similarity(tfidf_matrix[0:1], tfidf_matrix).tolist()\n",
    "    # Creates matrix based on document similarity\n",
    "         \n",
    "    # To check similarity b/w 2 reviews\n",
    "    i_appended = False\n",
    "    for k in range(1,len(tfidf_list[0])):\n",
    "        #print(tfidf_list[0][k],i+k)\n",
    "        \n",
    "        if(tfidf_list[0][k]>0.6):\n",
    "            # 0.6 is defind for the simmilarity level\n",
    "            \n",
    "            remove_reviews.append(dataset[\"review_id\"][i+k])\n",
    "            # i+k is to get the review id of the review\n",
    "            \n",
    "            if(not i_appended):\n",
    "                remove_reviews.append(dataset[\"review_id\"][i]) \n",
    "                i_appended = True\n",
    " "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "11283"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 7. Reviews in which Reviewer using arming tone to by the product (Action)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "#7. Removing reviews with no. of verbs > no. of nouns\n",
    "\n",
    "for i in range(len(dataset)):\n",
    "    #iterate the whole dataset\n",
    "    \n",
    "    words = nltk.word_tokenize(str(dataset[\"review_body\"][i]))\n",
    "    #storing the words from the reviews into the list\n",
    "    \n",
    "    tagged_words = nltk.pos_tag(words)\n",
    "    # returns list of tuples of words along with their parts of speech\n",
    "    \n",
    "    nouns_count = 0\n",
    "    verbs_count = 0\n",
    "    \n",
    "    for j in range(len(tagged_words)):\n",
    "        #iterate through all the words\n",
    "\n",
    "        if(tagged_words[j][1].startswith(\"NN\")):\n",
    "            nouns_count+=1\n",
    "            #counts the no. of nouns in the review\n",
    "\n",
    "        if(tagged_words[j][1].startswith(\"VB\")):\n",
    "            verbs_count+=1\n",
    "            #counts the no. of verbs in the review\n",
    "\n",
    "    if(verbs_count>nouns_count):\n",
    "        #comparing the no. of verbs and nouns\n",
    "        remove_reviews.append(dataset[\"review_id\"][i])\n",
    "        #storing the review to be removed "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "12214"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 8. Reviews in which reviewer is writing his own story"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/anubhav/anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: \n",
      "A value is trying to be set on a copy of a slice from a DataFrame\n",
      "\n",
      "See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy\n",
      "  \n"
     ]
    }
   ],
   "source": [
    "# 8. Removing reviews with includes more first person pronouns.\n",
    "\n",
    "for i in range(len(dataset)):\n",
    "    #iterate through all the reviews\n",
    "    \n",
    "    dataset[\"review_body\"][i] = str(dataset[\"review_body\"][i]).lower()\n",
    "    # converting each characters to its lower cases\n",
    "    \n",
    "    words = nltk.word_tokenize(dataset[\"review_body\"][i])\n",
    "    # storing the list of words for each reviews\n",
    "    \n",
    "    sentence = nltk.sent_tokenize(dataset[\"review_body\"][i])\n",
    "    # storing the list of sentences for each reviews\n",
    "    \n",
    "    count=0\n",
    "    if(len(sentence)>4):\n",
    "        # Checking only those reviews which have atleast 5 sentences.\n",
    "        \n",
    "        for j in range(len(words)):\n",
    "            #iterating through all the reviews\n",
    "            \n",
    "            if(words[j]==\"i\" or words[j]==\"we\"):\n",
    "                #counting personal pronouns\n",
    "                count+=1\n",
    "                \n",
    "        if(count > len(sentence)/2):\n",
    "            #reviews with number of personal pronouns used greater than half the no. of sentences.\n",
    "            remove_reviews.append(dataset[\"review_id\"][i])\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "45012"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 9. Meaningless Texts in reviews using LSA"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.feature_extraction.text import TfidfVectorizer\n",
    "from sklearn.decomposition import TruncatedSVD\n",
    "import nltk\n",
    "import re\n",
    "from nltk.corpus import stopwords \n",
    "import collections\n",
    "from nltk.corpus import wordnet"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.set_index(\"review_id\",inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Latent symantic analysis\n",
    "# it will analyse all reviews and determine all reviews belong to the same concept\n",
    "def LSA(text):\n",
    "    #text is list of reviews of same product\n",
    "    \n",
    "    \n",
    "    # Created TF-IDF Model\n",
    "    vectorizer = TfidfVectorizer()\n",
    "    X = vectorizer.fit_transform(text)\n",
    "    \n",
    "    # Created SVD(Singular Value Decomposition)\n",
    "    lsa = TruncatedSVD(n_components = 1,n_iter = 100)\n",
    "    lsa.fit(X)\n",
    "    \n",
    "    terms = vectorizer.get_feature_names()\n",
    "    concept_words={}\n",
    "\n",
    "    for j,comp in enumerate(lsa.components_):\n",
    "        componentTerms = zip(terms,comp)\n",
    "        sortedTerms = sorted(componentTerms,key=lambda x:x[1],reverse=True)\n",
    "        sortedTerms = sortedTerms[:10]\n",
    "        concept_words[str(j)] = sortedTerms\n",
    "     \n",
    "    sentence_scores = []\n",
    "    for key in concept_words.keys():\n",
    "        for sentence in text:\n",
    "            words = nltk.word_tokenize(sentence)\n",
    "            scores = 0\n",
    "            for word in words:\n",
    "                for word_with_scores in concept_words[key]:\n",
    "                    if word == word_with_scores[0]:\n",
    "                        scores += word_with_scores[1]\n",
    "            sentence_scores.append(scores)\n",
    "    return sentence_scores"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "/home/anubhav/anaconda3/lib/python3.6/site-packages/sklearn/decomposition/truncated_svd.py:192: RuntimeWarning: invalid value encountered in true_divide\n",
      "  self.explained_variance_ratio_ = exp_var / full_var\n"
     ]
    }
   ],
   "source": [
    "product_df = dataset.groupby(\"product_id\")\n",
    "#grouping dataset by product_id\n",
    "\n",
    "unique_products = dataset[\"product_id\"].unique()\n",
    "#stores list of all product id\n",
    "\n",
    "no_products = len(unique_products.tolist())\n",
    "#no. of products\n",
    "\n",
    "remove_reviews = []\n",
    "#store review_id that are fake \n",
    "    \n",
    "for i in range(no_products):\n",
    "    #iterate through all product reviews \n",
    "    \n",
    "    df = product_df.get_group(unique_products[i])\n",
    "    #dataframe of a single product\n",
    "    \n",
    "    unique_reviews = df.index.tolist()\n",
    "    #list of review_id of reviews of same product\n",
    "    \n",
    "    no_reviews = len(unique_reviews)   \n",
    "    #no. of reviews of same product\n",
    "    \n",
    "    count = no_reviews \n",
    "    #count is no. of reviews that can be analysed\n",
    "    \n",
    "    reviews = []\n",
    "    #list of review texts\n",
    "    \n",
    "    review_id = []\n",
    "    #list of review texts\n",
    "    \n",
    "    for j in range(no_reviews):\n",
    "        #iterate through all reviews\n",
    "        \n",
    "        text = str(df.loc[unique_reviews[j]][\"review_body\"])\n",
    "        # text of a review \n",
    "        \n",
    "        #cleaning the text\n",
    "        text = re.sub(r\"\\W\",\" \",text)             \n",
    "        text = re.sub(r\"\\d\",\" \",text)             \n",
    "        text = re.sub(r\"\\s+[a-z]\\s+\",\" \",text)    \n",
    "        text = re.sub(r\"^[a-z]\\s+\",\" \",text)    \n",
    "        text = re.sub(r\"\\s+[a-z]$\",\" \",text)    \n",
    "        text = re.sub(r\"\\s+\",\" \",text)\n",
    "        \n",
    "        words = nltk.word_tokenize(text)\n",
    "        #text into word list\n",
    "        \n",
    "        if(len(words)==1):\n",
    "        #if only one word in text review\n",
    "            \n",
    "            if(len(text) <=1 or not wordnet.synsets(text) ):\n",
    "            #if word is having only one character or invalid english word\n",
    "                \n",
    "                remove_reviews.append(unique_reviews[j])\n",
    "                #append this review as fake\n",
    "                \n",
    "                count-=1\n",
    "                #review left to be analysed will be decrease by 1\n",
    "                \n",
    "                continue\n",
    "                #check for next review \n",
    "                \n",
    "        elif(len(words)==0):\n",
    "        #if no words\n",
    "            \n",
    "            remove_reviews.append(unique_reviews[j])\n",
    "            #append this review as fake\n",
    "            \n",
    "            count-=1\n",
    "            #review left to be analysed will be decrease by 1\n",
    "            \n",
    "            continue\n",
    "            #check for next review\n",
    "        \n",
    "        review_id.append(unique_reviews[j])\n",
    "        #valid: append review_id to review_id list for analysis\n",
    "        \n",
    "        reviews.append(text)\n",
    "        #valid: append review_body to reviews list for analysis\n",
    "        \n",
    "    ###########################################################################################\n",
    "    if(count<=0):\n",
    "        #if no reviews left to analyse \n",
    "\n",
    "        continue\n",
    "        #check for next\n",
    "        \n",
    "    if(count==1): \n",
    "        #if only one review is left to analyse\n",
    "        \n",
    "        #check concept between product title and the review\n",
    "        \n",
    "        text = [text,str(df.loc[review_id[0]][\"product_title\"])] \n",
    "        #add product_title and review to the list \n",
    "        \n",
    "        sentence_scores = LSA(text) \n",
    "        #call LSA method to get the score\n",
    "        \n",
    "        if(sentence_scores[0]==0): \n",
    "        #if review score is 0, then it's fake\n",
    "            remove_reviews.append(review_id[0])\n",
    "        continue\n",
    "    \n",
    "    #list of scores of reviews of same product\n",
    "    sentence_scores = LSA(reviews)\n",
    "            \n",
    "    for j in range(len(sentence_scores)):\n",
    "        #iterating through all the scores\n",
    "        \n",
    "        if(sentence_scores[j]==0.00):\n",
    "            # if score is 0, it's fake\n",
    "            remove_reviews.append(review_id[j])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "5243"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(remove_reviews)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Removing Fake Reviews"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.drop(remove_reviews,inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset = dataset.set_index(\"IP Address\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.drop(remove_ip,inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [],
   "source": [
    "dataset.to_csv(\"real_reviews.csv\",sep=\"\\t\")"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
