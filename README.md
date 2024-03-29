# Master_PS
Repository for python codes and projects
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Importing Modules"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import numpy as np\n",
    "%matplotlib inline\n",
    "import seaborn as sns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Importing Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "D:\\SAROHA\\DSP\\DSP_Class 25 & 26 Clustering_Segmentation_PCA\\Segmentation Case Study- Banking\n"
     ]
    }
   ],
   "source": [
    "%cd \"D:\\SAROHA\\DSP\\DSP_Class 25 & 26 Clustering_Segmentation_PCA\\Segmentation Case Study- Banking\""
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data = pd.read_csv(\"CC_GENERAL.csv\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Missing Value Treatment"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data['CREDIT_LIMIT'].fillna(credit_card_data['CREDIT_LIMIT'].median(),inplace=True)\n",
    "credit_card_data['MINIMUM_PAYMENTS'].median()\n",
    "credit_card_data['MINIMUM_PAYMENTS'].fillna(credit_card_data['MINIMUM_PAYMENTS'].median(),inplace=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Deriving New KPI"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1. Monthly Average Purchase"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data['Monthly_ap']=credit_card_data['PURCHASES']/credit_card_data['TENURE']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 2. Monthly Cash Advance"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data['Monthly_ca']=credit_card_data['CASH_ADVANCE']/credit_card_data['TENURE']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3. Limit Usage Ratio"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data['limit_usage_ratio']=credit_card_data.apply(lambda x: x['BALANCE']/x['CREDIT_LIMIT'],axis=1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 4. Payment:Min Payment"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data['payment_minpayment_ratio']=credit_card_data.apply(lambda x:x['PAYMENTS']/x['MINIMUM_PAYMENTS'],axis=1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Outlier Treatment"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "#log transformatopn\n",
    "credit_card_data_log=credit_card_data.drop(['CUST_ID'],axis=1).applymap(lambda x: np.log(x+1))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Standardzing data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.preprocessing import StandardScaler"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "sc=StandardScaler()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "credit_card_data_log_scaled=sc.fit_transform(credit_card_data_log)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Dimesion Reduction"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.decomposition import PCA"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "var_ratio={}\n",
    "for n in range (4,15):\n",
    "    pc=PCA(n_components=n)\n",
    "    cr_pca=pc.fit(credit_card_data_log_scaled)\n",
    "    var_ratio[n]=sum(cr_pca.explained_variance_ratio_)"
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
       "<matplotlib.axes._subplots.AxesSubplot at 0xd68ff28>"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "pd.Series(var_ratio).plot()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Since 6 Components are explaining about 85% variance so we select 6 components"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "pc_final=PCA(n_components=6).fit(credit_card_data_log_scaled)\n",
    "\n",
    "reduced_cr=pc_final.fit_transform(credit_card_data_log_scaled)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [],
   "source": [
    "col_list=credit_card_data_log.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
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
       "      <th>PC_0</th>\n",
       "      <th>PC_1</th>\n",
       "      <th>PC_2</th>\n",
       "      <th>PC_3</th>\n",
       "      <th>PC_4</th>\n",
       "      <th>PC_5</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>BALANCE</th>\n",
       "      <td>-0.144662</td>\n",
       "      <td>0.387557</td>\n",
       "      <td>-0.119601</td>\n",
       "      <td>-0.111047</td>\n",
       "      <td>-0.123326</td>\n",
       "      <td>0.116233</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <td>-0.016479</td>\n",
       "      <td>0.298761</td>\n",
       "      <td>-0.178288</td>\n",
       "      <td>0.014098</td>\n",
       "      <td>-0.211666</td>\n",
       "      <td>0.436921</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES</th>\n",
       "      <td>0.312377</td>\n",
       "      <td>0.173386</td>\n",
       "      <td>-0.015731</td>\n",
       "      <td>-0.048341</td>\n",
       "      <td>0.157348</td>\n",
       "      <td>-0.060155</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <td>0.178951</td>\n",
       "      <td>0.229773</td>\n",
       "      <td>0.156819</td>\n",
       "      <td>-0.435435</td>\n",
       "      <td>0.253131</td>\n",
       "      <td>0.017885</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <td>0.267605</td>\n",
       "      <td>0.121040</td>\n",
       "      <td>-0.132085</td>\n",
       "      <td>0.395632</td>\n",
       "      <td>-0.038459</td>\n",
       "      <td>-0.122547</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE</th>\n",
       "      <td>-0.282725</td>\n",
       "      <td>0.197232</td>\n",
       "      <td>0.178242</td>\n",
       "      <td>0.180715</td>\n",
       "      <td>0.138206</td>\n",
       "      <td>-0.071072</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <td>0.304867</td>\n",
       "      <td>0.162305</td>\n",
       "      <td>-0.097524</td>\n",
       "      <td>0.213353</td>\n",
       "      <td>0.099591</td>\n",
       "      <td>0.002414</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <td>0.183314</td>\n",
       "      <td>0.227807</td>\n",
       "      <td>0.170135</td>\n",
       "      <td>-0.364433</td>\n",
       "      <td>0.219648</td>\n",
       "      <td>0.082384</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <td>0.254453</td>\n",
       "      <td>0.114009</td>\n",
       "      <td>-0.170659</td>\n",
       "      <td>0.448480</td>\n",
       "      <td>-0.032153</td>\n",
       "      <td>-0.075939</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <td>-0.246611</td>\n",
       "      <td>0.213931</td>\n",
       "      <td>0.181451</td>\n",
       "      <td>0.210783</td>\n",
       "      <td>0.203431</td>\n",
       "      <td>-0.055693</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <td>-0.264110</td>\n",
       "      <td>0.221337</td>\n",
       "      <td>0.178522</td>\n",
       "      <td>0.207784</td>\n",
       "      <td>0.169353</td>\n",
       "      <td>-0.081620</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <td>0.309090</td>\n",
       "      <td>0.212162</td>\n",
       "      <td>-0.048191</td>\n",
       "      <td>0.083519</td>\n",
       "      <td>0.098504</td>\n",
       "      <td>-0.049534</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CREDIT_LIMIT</th>\n",
       "      <td>0.035595</td>\n",
       "      <td>0.203215</td>\n",
       "      <td>0.261724</td>\n",
       "      <td>-0.102357</td>\n",
       "      <td>-0.165378</td>\n",
       "      <td>-0.586613</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PAYMENTS</th>\n",
       "      <td>0.035862</td>\n",
       "      <td>0.280426</td>\n",
       "      <td>0.316873</td>\n",
       "      <td>0.048139</td>\n",
       "      <td>-0.399746</td>\n",
       "      <td>0.261667</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>MINIMUM_PAYMENTS</th>\n",
       "      <td>-0.129179</td>\n",
       "      <td>0.330027</td>\n",
       "      <td>-0.266649</td>\n",
       "      <td>-0.065138</td>\n",
       "      <td>-0.127607</td>\n",
       "      <td>-0.171647</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <td>0.171670</td>\n",
       "      <td>-0.099260</td>\n",
       "      <td>0.279142</td>\n",
       "      <td>0.206422</td>\n",
       "      <td>-0.130293</td>\n",
       "      <td>0.219508</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>TENURE</th>\n",
       "      <td>0.044348</td>\n",
       "      <td>0.072668</td>\n",
       "      <td>-0.018541</td>\n",
       "      <td>-0.151180</td>\n",
       "      <td>-0.617022</td>\n",
       "      <td>-0.349503</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ap</th>\n",
       "      <td>0.307448</td>\n",
       "      <td>0.203673</td>\n",
       "      <td>0.014769</td>\n",
       "      <td>-0.044128</td>\n",
       "      <td>0.166727</td>\n",
       "      <td>-0.021188</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ca</th>\n",
       "      <td>-0.277518</td>\n",
       "      <td>0.202738</td>\n",
       "      <td>0.193319</td>\n",
       "      <td>0.194495</td>\n",
       "      <td>0.158734</td>\n",
       "      <td>-0.084296</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <td>-0.194318</td>\n",
       "      <td>0.257264</td>\n",
       "      <td>-0.315933</td>\n",
       "      <td>-0.064156</td>\n",
       "      <td>-0.044664</td>\n",
       "      <td>0.254373</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <td>0.142383</td>\n",
       "      <td>-0.019231</td>\n",
       "      <td>0.538031</td>\n",
       "      <td>0.076870</td>\n",
       "      <td>-0.188407</td>\n",
       "      <td>0.249324</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                      PC_0      PC_1      PC_2      PC_3  \\\n",
       "BALANCE                          -0.144662  0.387557 -0.119601 -0.111047   \n",
       "BALANCE_FREQUENCY                -0.016479  0.298761 -0.178288  0.014098   \n",
       "PURCHASES                         0.312377  0.173386 -0.015731 -0.048341   \n",
       "ONEOFF_PURCHASES                  0.178951  0.229773  0.156819 -0.435435   \n",
       "INSTALLMENTS_PURCHASES            0.267605  0.121040 -0.132085  0.395632   \n",
       "CASH_ADVANCE                     -0.282725  0.197232  0.178242  0.180715   \n",
       "PURCHASES_FREQUENCY               0.304867  0.162305 -0.097524  0.213353   \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.183314  0.227807  0.170135 -0.364433   \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.254453  0.114009 -0.170659  0.448480   \n",
       "CASH_ADVANCE_FREQUENCY           -0.246611  0.213931  0.181451  0.210783   \n",
       "CASH_ADVANCE_TRX                 -0.264110  0.221337  0.178522  0.207784   \n",
       "PURCHASES_TRX                     0.309090  0.212162 -0.048191  0.083519   \n",
       "CREDIT_LIMIT                      0.035595  0.203215  0.261724 -0.102357   \n",
       "PAYMENTS                          0.035862  0.280426  0.316873  0.048139   \n",
       "MINIMUM_PAYMENTS                 -0.129179  0.330027 -0.266649 -0.065138   \n",
       "PRC_FULL_PAYMENT                  0.171670 -0.099260  0.279142  0.206422   \n",
       "TENURE                            0.044348  0.072668 -0.018541 -0.151180   \n",
       "Monthly_ap                        0.307448  0.203673  0.014769 -0.044128   \n",
       "Monthly_ca                       -0.277518  0.202738  0.193319  0.194495   \n",
       "limit_usage_ratio                -0.194318  0.257264 -0.315933 -0.064156   \n",
       "payment_minpayment_ratio          0.142383 -0.019231  0.538031  0.076870   \n",
       "\n",
       "                                      PC_4      PC_5  \n",
       "BALANCE                          -0.123326  0.116233  \n",
       "BALANCE_FREQUENCY                -0.211666  0.436921  \n",
       "PURCHASES                         0.157348 -0.060155  \n",
       "ONEOFF_PURCHASES                  0.253131  0.017885  \n",
       "INSTALLMENTS_PURCHASES           -0.038459 -0.122547  \n",
       "CASH_ADVANCE                      0.138206 -0.071072  \n",
       "PURCHASES_FREQUENCY               0.099591  0.002414  \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.219648  0.082384  \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY -0.032153 -0.075939  \n",
       "CASH_ADVANCE_FREQUENCY            0.203431 -0.055693  \n",
       "CASH_ADVANCE_TRX                  0.169353 -0.081620  \n",
       "PURCHASES_TRX                     0.098504 -0.049534  \n",
       "CREDIT_LIMIT                     -0.165378 -0.586613  \n",
       "PAYMENTS                         -0.399746  0.261667  \n",
       "MINIMUM_PAYMENTS                 -0.127607 -0.171647  \n",
       "PRC_FULL_PAYMENT                 -0.130293  0.219508  \n",
       "TENURE                           -0.617022 -0.349503  \n",
       "Monthly_ap                        0.166727 -0.021188  \n",
       "Monthly_ca                        0.158734 -0.084296  \n",
       "limit_usage_ratio                -0.044664  0.254373  \n",
       "payment_minpayment_ratio         -0.188407  0.249324  "
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.DataFrame(pc_final.components_.T,columns=['PC_' +str(i) for i in range(6)],index=col_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index([u'BALANCE', u'BALANCE_FREQUENCY', u'PURCHASES', u'ONEOFF_PURCHASES',\n",
       "       u'INSTALLMENTS_PURCHASES', u'CASH_ADVANCE', u'PURCHASES_FREQUENCY',\n",
       "       u'ONEOFF_PURCHASES_FREQUENCY', u'PURCHASES_INSTALLMENTS_FREQUENCY',\n",
       "       u'CASH_ADVANCE_FREQUENCY', u'CASH_ADVANCE_TRX', u'PURCHASES_TRX',\n",
       "       u'CREDIT_LIMIT', u'PAYMENTS', u'MINIMUM_PAYMENTS', u'PRC_FULL_PAYMENT',\n",
       "       u'TENURE', u'Monthly_ap', u'Monthly_ca', u'limit_usage_ratio',\n",
       "       u'payment_minpayment_ratio'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "credit_card_data_log.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Removing the variables used for deriving KPIs\n",
    "my_vars=['BALANCE_FREQUENCY','ONEOFF_PURCHASES','INSTALLMENTS_PURCHASES','PURCHASES_FREQUENCY','ONEOFF_PURCHASES_FREQUENCY', 'PURCHASES_INSTALLMENTS_FREQUENCY','CASH_ADVANCE_FREQUENCY', 'CASH_ADVANCE_TRX', 'PURCHASES_TRX',\n",
    "        'PRC_FULL_PAYMENT','Monthly_ap', 'Monthly_ca', 'limit_usage_ratio','payment_minpayment_ratio']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Clustering using K-means"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.cluster import KMeans"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 4 Cluster Solution"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "km_4=KMeans(n_clusters=4,random_state=123)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,\n",
       "    n_clusters=4, n_init=10, n_jobs=None, precompute_distances='auto',\n",
       "    random_state=123, tol=0.0001, verbose=0)"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_4.fit(reduced_cr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([3, 1, 0, ..., 3, 1, 2])"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_4.labels_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3    2474\n",
       "1    2458\n",
       "0    2436\n",
       "2    1582\n",
       "dtype: int64"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.Series(km_4.labels_).value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "C4=pd.concat([credit_card_data_log[my_vars],pd.Series(km_4.labels_,name='cluster_4')],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
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
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <th>Monthly_ap</th>\n",
       "      <th>Monthly_ca</th>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <th>cluster_4</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0.597837</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.568506</td>\n",
       "      <td>0.154151</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.098612</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.191654</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.040086</td>\n",
       "      <td>0.894662</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.646627</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.223144</td>\n",
       "      <td>1.609438</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6.287695</td>\n",
       "      <td>0.376719</td>\n",
       "      <td>1.574068</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>6.651791</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.564949</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.180994</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.287197</td>\n",
       "      <td>0.688979</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>0.492477</td>\n",
       "      <td>7.313220</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.835620</td>\n",
       "      <td>2.898616</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>2.833213</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.847298</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.519644</td>\n",
       "      <td>1.327360</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   BALANCE_FREQUENCY  ONEOFF_PURCHASES  INSTALLMENTS_PURCHASES  \\\n",
       "0           0.597837          0.000000                4.568506   \n",
       "1           0.646627          0.000000                0.000000   \n",
       "2           0.693147          6.651791                0.000000   \n",
       "3           0.492477          7.313220                0.000000   \n",
       "4           0.693147          2.833213                0.000000   \n",
       "\n",
       "   PURCHASES_FREQUENCY  ONEOFF_PURCHASES_FREQUENCY  \\\n",
       "0             0.154151                    0.000000   \n",
       "1             0.000000                    0.000000   \n",
       "2             0.693147                    0.693147   \n",
       "3             0.080042                    0.080042   \n",
       "4             0.080042                    0.080042   \n",
       "\n",
       "   PURCHASES_INSTALLMENTS_FREQUENCY  CASH_ADVANCE_FREQUENCY  CASH_ADVANCE_TRX  \\\n",
       "0                          0.080042                0.000000          0.000000   \n",
       "1                          0.000000                0.223144          1.609438   \n",
       "2                          0.000000                0.000000          0.000000   \n",
       "3                          0.000000                0.080042          0.693147   \n",
       "4                          0.000000                0.000000          0.000000   \n",
       "\n",
       "   PURCHASES_TRX  PRC_FULL_PAYMENT  Monthly_ap  Monthly_ca  limit_usage_ratio  \\\n",
       "0       1.098612          0.000000    2.191654    0.000000           0.040086   \n",
       "1       0.000000          0.200671    0.000000    6.287695           0.376719   \n",
       "2       2.564949          0.000000    4.180994    0.000000           0.287197   \n",
       "3       0.693147          0.000000    4.835620    2.898616           0.200671   \n",
       "4       0.693147          0.000000    0.847298    0.000000           0.519644   \n",
       "\n",
       "   payment_minpayment_ratio  cluster_4  \n",
       "0                  0.894662          3  \n",
       "1                  1.574068          1  \n",
       "2                  0.688979          0  \n",
       "3                  0.000000          3  \n",
       "4                  1.327360          1  "
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "C4.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Mean value of each variable group by Cluster-4\n",
    "cluster_4_output=C4.groupby('cluster_4').apply(lambda x: x[my_vars].mean()).T"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
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
       "      <th>cluster_4</th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "      <th>2</th>\n",
       "      <th>3</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <td>0.676489</td>\n",
       "      <td>0.639905</td>\n",
       "      <td>0.679667</td>\n",
       "      <td>0.506229</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <td>5.968092</td>\n",
       "      <td>0.722225</td>\n",
       "      <td>4.891764</td>\n",
       "      <td>1.869839</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <td>5.110243</td>\n",
       "      <td>0.136660</td>\n",
       "      <td>4.569818</td>\n",
       "      <td>4.038034</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <td>0.581066</td>\n",
       "      <td>0.021842</td>\n",
       "      <td>0.497312</td>\n",
       "      <td>0.395083</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <td>0.357382</td>\n",
       "      <td>0.016614</td>\n",
       "      <td>0.237140</td>\n",
       "      <td>0.054074</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <td>0.414913</td>\n",
       "      <td>0.004695</td>\n",
       "      <td>0.362575</td>\n",
       "      <td>0.331963</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <td>0.007542</td>\n",
       "      <td>0.225882</td>\n",
       "      <td>0.268864</td>\n",
       "      <td>0.006873</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <td>0.067279</td>\n",
       "      <td>1.629820</td>\n",
       "      <td>1.901061</td>\n",
       "      <td>0.056500</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <td>3.118831</td>\n",
       "      <td>0.179148</td>\n",
       "      <td>2.681810</td>\n",
       "      <td>1.890622</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <td>0.172785</td>\n",
       "      <td>0.028833</td>\n",
       "      <td>0.030556</td>\n",
       "      <td>0.207587</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ap</th>\n",
       "      <td>4.836604</td>\n",
       "      <td>0.416937</td>\n",
       "      <td>4.264198</td>\n",
       "      <td>3.133625</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ca</th>\n",
       "      <td>0.225737</td>\n",
       "      <td>4.359313</td>\n",
       "      <td>4.813015</td>\n",
       "      <td>0.197355</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <td>0.264062</td>\n",
       "      <td>0.446963</td>\n",
       "      <td>0.460037</td>\n",
       "      <td>0.072860</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <td>1.721630</td>\n",
       "      <td>1.075376</td>\n",
       "      <td>1.129546</td>\n",
       "      <td>1.425390</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "cluster_4                                0         1         2         3\n",
       "BALANCE_FREQUENCY                 0.676489  0.639905  0.679667  0.506229\n",
       "ONEOFF_PURCHASES                  5.968092  0.722225  4.891764  1.869839\n",
       "INSTALLMENTS_PURCHASES            5.110243  0.136660  4.569818  4.038034\n",
       "PURCHASES_FREQUENCY               0.581066  0.021842  0.497312  0.395083\n",
       "ONEOFF_PURCHASES_FREQUENCY        0.357382  0.016614  0.237140  0.054074\n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.414913  0.004695  0.362575  0.331963\n",
       "CASH_ADVANCE_FREQUENCY            0.007542  0.225882  0.268864  0.006873\n",
       "CASH_ADVANCE_TRX                  0.067279  1.629820  1.901061  0.056500\n",
       "PURCHASES_TRX                     3.118831  0.179148  2.681810  1.890622\n",
       "PRC_FULL_PAYMENT                  0.172785  0.028833  0.030556  0.207587\n",
       "Monthly_ap                        4.836604  0.416937  4.264198  3.133625\n",
       "Monthly_ca                        0.225737  4.359313  4.813015  0.197355\n",
       "limit_usage_ratio                 0.264062  0.446963  0.460037  0.072860\n",
       "payment_minpayment_ratio          1.721630  1.075376  1.129546  1.425390"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cluster_4_output"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Cluster 4\n",
      "   Size  Percentage\n",
      "0  2436   27.217877\n",
      "1  2458   27.463687\n",
      "2  1582   17.675978\n",
      "3  2474   27.642458\n"
     ]
    }
   ],
   "source": [
    "# Share of each cluster\n",
    "\n",
    "s=C4.groupby('cluster_4').apply(lambda x: x['cluster_4'].value_counts())\n",
    "per=pd.Series((s.values.astype('float')/ C4['cluster_4'].shape[0])*100,name='Percentage')\n",
    "print (\"Cluster 4\")\n",
    "print (pd.concat ([pd.Series(s.values,name='Size'),per],axis=1))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.legend.Legend at 0x10087278>"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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TIwGDx6s/Px87w8iSoXoDAAAAJTR2rVr1apVqxK3rVixQuPHj9e6deu0fv16paWlaenSpcrOzlZgYKBSU1O1fft2jRs3TkuXLtXcuXO1bt3pVxQlJCQoNjZWqampys7OVs2aNU9rs3v3bk2fPl05OTlas2aNfvvb30qSHnroIXXu3FmrV6/WypUrFRYWpqysLE2ePFlffvmlli9frokTJ2rVqlWSpG+++UZ33nmnVq1apdq1a+u5557TvHnztHLlSsXGxurll1/28OihLLw6JRIAAACApLi4OIWEhEiS5s+fr6ysLLVu3VqSdPDgQV199dX68ssv1aVLFx1fPX3IkCHndf1bvXr1FBQUpBEjRqhv377q16+fJGnBggX+6+ACAwN1+eWXa8mSJRo4cKBq1y6+A9egQYO0ePFi9e/fX7/85S8VHx8vSVq+fLnWrVun9u3bS5IOHz6stm3bluOIoDwIbAAAAEAZhYWFKT09vcRtxwORVHxN2F133aX//u//PqnNJ598ImPKv/J1tWrVtGLFCs2fP18ffPCBXn/9dS1YsKDEttaeeVXsU8d84403aurUqeUeH8qPwAYAAIBKrSKW4e/WrZueeuopTZw4USNHjpQkffXVV/r8889Pate9e3fdcsstevTRR3X11Vdr9+7d2rdvn9q0aaOHH35Yu3btUr169fTRRx8pMjLytH7q1q2rffv2nXEc+fn5OnDggPr06aP4+Hj96le/8vf7xhtv6JFHHlFRUZH279+vTp06KSkpSWPHjpW1VtOnT9d77713Ws34+Hg98MAD2rRpk371q1/pwIED2rZtm38xFVxcXMMGAAAAlJExRtOnT9fcuXPVpEkThYWFKTk5+bTVHFu2bKnnnntOPXv2lM/n04033qjt27fr2muvVXJystq2basePXooJiamxH6SkpI0atSoMy46sm/fPvXr108+n0+dO3fWK6+8Ikn685//rIULFyoiIkKtWrVSTk6OYmJilJSUpLi4OLVp00YjRoxQdHT0aTWDg4OVkpKiYcOGyefzKT4+Xhs2bPDgqOF8mLNNjV4osbGxNjMz86L3i0sDN84GAKBqW79+vUJDQyt6GECplfSbNcZkWWtjz7UvM2wAAAAA4CiuYQMAAAAqgYEDB+r7778/6b0//vGP6tWrVwWNCBcDgQ0AAACoBKZPn17RQ0AF4JRI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fLNCQkL0+uuv6+WXX1Z0dLTi4+O1e/duSdLmzZvVu3dvtWrVSh07dtSGDRv843jooYfUrl07NW7cWOnp6f5jsXjxYkVFRemVV1456ThkZGSoa9euuv322xUREXHG76+k41mnTh1JkrVWY8aMUXh4uCIiIpSWllbKX03pEdgAAACA83TkyBF99tln/v/gnzRpkrKyspSZmakJEyZo165dGjp0qGbOnOkPQpMnT9bdd98tSdq0aZMefvhhrVmzRhs2bNCUKVO0ZMkSvfTSS/5ZnfHjx6tbt2766quvtHDhQo0ZM0b79++XJGVnZystLU1ff/210tLStHXrVr3wwgv+GcDU1NQSx71582bNnj1bM2bM0B133KGuXbvq66+/Vs2aNTV79uzT2u/fv1/x8fFavXq1OnXqpIkTJ/q35ebm6vPPP9fs2bM1atQoFRQU6Oqrr9bcuXO1cuVKpaWl6aGHHpJUfJrl5MmTJUl79+7VsmXL1KdPH0nS2rVrNWXKFK1YsUJPP/20atWqpVWrVqlt27Z69913JUn33XefXnvtNWVlZemll17S/fff7x/H9u3btWTJEs2aNUtjx46VJL3wwgvq2LGjsrOzTwrNx61YsULjx4/XunXrzvj9ne14fvzxx8rOztbq1as1b948jRkzRtu3bz/Dr+X8cA0bAAAAUEbHZ1yk4hm2e++9V5I0YcIETZ8+XZK0detWbdy4UfHx8erWrZtmzZql0NBQFRYWKiIiQrm5uQoJCfGHvbCwMHXv3l3GGP92SZozZ45mzpypl156SZJUUFCgf/3rX5Kk7t276/LLL5cktWzZUlu2bNH1119/zvHfdNNNql69uiIiIlRUVKTevXtL0kn9nuiyyy5Tv379JEmtWrXS3Llz/dtuu+02BQQEqGnTpmrcuLE2bNigkJAQjR49WtnZ2QoMDNS3334rSercubMeeOAB7dy5Ux9//LFuvfVWVatWHEm6du2qunXrqm7durr88st18803+8e0Zs0a5efna9myZRo8eLC/70OHDvmfDxgwQAEBAWrZsqV/BvBc4uLiFBIS4n9d0vd31VVXnXH/JUuWaNiwYQoMDNQ111yjzp0766uvvlL//v1L1X9pENgAAACAMjo+43KijIwMzZs3T1988YVq1aqlLl26qKCgQFLxzNLzzz+vFi1a+GfXJKlGjRr+5wEBAf7XAQEBOnLkiKTi0+6mTZum5s2bn9Tfl19+edL+gYGB/n1O9Je//MU/I/bpp5+e1G9AQICqV68uY8xp/Z7oxDan9nP8/RNfv/LKK7rmmmu0evVqHT16VEFBQf7tw4cPV2pqqj744ANNmjSp1Mfi6NGjql+//mnHvaT9rbUltjlV7dq1/c/P9v2dSWn7KQ9OiQQAAAA8sHfvXl1xxRWqVauWNmzYoOXLl/u3tWnTRlu3btWUKVM0bNiwMtXt1auXXnvtNX84WLVq1Tn3qV69uv8UzAceeEDZ2dnKzs7WddddV6a+S+Ojjz7S0aNHtXnzZn333Xdq3ry59u7dq2uvvVYBAQF67733TlrUIykpSa+++qqk4lnF0qpXr55CQkL00UcfSSoOS6tXrz7rPnXr1tW+fftKVf9s39+Jx/NEnTp1UlpamoqKipSXl6dFixYpLi6u1J+pNJhhAwAAQKVWmmX4L4bevXvrzTfflM/nU/PmzRUfH3/S9ttuu03Z2dm64oorylT3mWee0SOPPCKfzydrrRo1auRf3ONM7rvvPvl8PsXExJzxOjavNG/eXJ07d9aOHTv05ptvKigoSPfff79uvfVWffTRR+ratetJM1nXXHONQkNDNWDAgDL3lZqaqv/8z//Uc889p8LCQg0dOlSRkZFnbO/z+VStWjVFRkYqKSmpxOvYjjvb93em4zlw4EB98cUXioyMlDFGf/rTn/Qf//EfZf5cZ2MuxjTeqWJjY+3xFWeAsjLPmnM3KiM77uL/HQAAgPOzfv16hYaGVvQwyqxfv3569NFH1b1794oeimeSkpLUr18/JSQklHqfAwcOKCIiQitXrvRff3epK+k3a4zJstbGnmtfTokEAAAALqA9e/aoWbNmqlmz5iUV1s7HvHnz1KJFCz344INVJqyVF6dEAgAAABdQ/fr1/askXmpSUlLK1L5Hjx7+FS5ROsywAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAKByM8bbRym0a9fuvIb6ySefaN26dedsl5ycrJdeeklS8dL56enp59VfaaWkpOjHH3+8oH3g/BDYAAAAgDJatmzZee1X2sB2sRHY3EVgAwAAAMqoTp06kqSMjAx16dJFCQkJatGihRITE2WtlSSNHTtWLVu2lM/n0+OPP65ly5Zp5syZGjNmjKKiorR582ZNnDhRrVu3VmRkpG699VYdOHDgrP02atRITz31lNq2bavY2FitXLlSvXr1UpMmTfTmm2/627344otq3bq1fD6fxo0bJ0nKzc1VaGioRo4cqbCwMPXs2VMHDx5Uenq6MjMzlZiYqKioKB08ePACHTWcDwIbAAAAUA6rVq3Sq6++qnXr1um7777T0qVLtXv3bk2fPl05OTlas2aNfvvb36pdu3bq37+/XnzxRWVnZ6tJkyYaNGiQvvrqK61evVqhoaF6++23z9nf9ddfry+++EIdO3b0ny65fPly/e53v5MkzZkzRxs3btSKFSuUnZ2trKwsLVqcxrAkAAAgAElEQVS0SJK0ceNGPfDAA8rJyVH9+vU1bdo0JSQkKDY2VqmpqcrOzlbNmjUv6PFC2XDjbAAAAKAc4uLi1LBhQ0lSVFSUcnNzFR8fr6CgII0YMUJ9+/ZVv379Stx37dq1+u1vf6s9e/YoPz9fvXr1Omd//fv3lyRFREQoPz9fdevWVd26dRUUFKQ9e/Zozpw5mjNnjqKjoyVJ+fn52rhxo2644QaFhIQoKipKktSqVSvl5uZ6cARwITHDBgAAAJRDjRo1/M8DAwN15MgRVatWTStWrNCtt96qTz75RL179y5x36SkJL3++uv6+uuvNW7cOBUUFJS6v4CAgJP6DggI0JEjR2St1ZNPPqns7GxlZ2dr06ZNuvfee884VriNwAYAAAB4LD8/X3v37lWfPn306quvKjs7W5JUt25d7du3z99u3759uvbaa1VYWKjU1FRP+u7Vq5cmTZqk/Px8SdIPP/ygnTt3nnWfU8cFd3BKJAAAACq3Y4t8uGTfvn265ZZbVFBQIGutXnnlFUnS0KFDNXLkSE2YMEHp6en6wx/+oDZt2uiXv/ylIiIiPAlNPXv21Pr169W2bVtJxQukvP/++woMDDzjPklJSRo1apRq1qypL774guvYHGJsBfzAY2NjbWZm5kXvF5cG82zp7o9SFnace/9DDwAASrZ+/XqFhoZW9DCAUivpN2uMybLWxp5rX06JBAAAAABHEdhwYRnj/QMAAACoIghsAAAAAOAoAhsAAAAAOIrABgAAAACO8mRZf2NMrqR9kookHSnNaicAAAAAgLPz8j5sXa21P3tYDwAAADgnr2/5w+1+4BJOiQQAAADKaNu2bbrlllvUtGlTNWnSRA8//LAOHz58UfpevHixwsLCFBUVpYMHD2rMmDEKCwvTmDFjLkr/GRkZ6tev30XpC94FNitpjjEmyxhzX0kNjDH3GWMyjTGZeXl5HnULAAAAXFzWWg0aNEgDBgzQxo0b9e233yo/P19PP/30Rek/NTVVjz/+uLKzs1WzZk399a9/1cqVK/Xiiy962k9RUZGn9XB+vAps7a21MZJukvSAMabTqQ2stW9Za2OttbHBwcEedQsAAABcXAsWLFBQUJDuvvtuSVJgYKBeeeUVTZo0SQcOHFBKSooGDRqk3r17q2nTpnriiSf8+86ZM0dt27ZVTEyMBg8erPz8/DP2M3/+fEVHRysiIkL33HOPDh06pL/97W/68MMP9fvf/16JiYnq37+/9u/frzZt2igtLe2k/ZOTkzV8+HB169ZNTZs21cSJEyWdPkM2evRopaSkSJIaNWqk3//+9+rQoYM++ugjbdq0ST169FBkZKRiYmK0efNmSVJ+fr4SEhLUokULJSYmytri00h///vfq3Xr1goPD9d9993nf3/ChAlq2bKlfD6fhg4dKknav3+/7rnnHrVu3VrR0dGaMWNGeb6WS5Yn17BZa3889s+dxpjpkuIkLfKiNgAAAOCSnJwctWrV6qT36tWrpxtuuEGbNm2SJGVnZ2vVqlWqUaOGmjdvrgcffFA1a9bUc889p3nz5ql27dr64x//qJdfflm/+93vTuujoKBASUlJmj9/vpo1a6Y777xTb7zxhh555BEtWbJE/fr1U0JCgiSpTp06ys7OLnGsa9as0fLly7V//35FR0erb9++5/x8QUFBWrJkiSSpTZs2Gjt2rAYOHKiCggIdPXpUW7du1apVq5STk6PrrrtO7du319KlS9WhQweNHj3a/3mGDx+uWbNm6eabb9YLL7yg77//XjVq1NCePXskSePHj1e3bt00adIk7dmzR3FxcerRo4dq165dym+iaij3DJsxprYxpu7x55J6Slpb3roAAACAi6y1Mub0hU5OfL979+66/PLLFRQUpJYtW2rLli1avny51q1bp/bt2ysqKkrvvPOOtmzZUmIf33zzjUJCQtSsWTNJ0l133aVFi8o+H3LLLbeoZs2aatCggbp27aoVK1acc58hQ4ZIkvbt26cffvhBAwcOlFQc5GrVqiVJiouLU8OGDRUQEKCoqCjl5uZKkhYuXKg2bdooIiJCCxYsUE5OjiTJ5/MpMTFR77//vqpVK54zmjNnjl544QVFRUWpS5cuKigo0L/+9a8yf8ZLnRczbNdImn7sx1lN0hRr7T88qAsAAAA4JywsTNOmTTvpvf/7v//T1q1b1aRJE2VlZalGjRr+bYGBgTpy5Iistbrxxhs1derUc/Zx/FTC8jo1WBpjVK1aNR09etT/XkFBwUltjs9wnW0MJX2+goIC3X///crMzNT111+v5ORkf+3Zs2dr0aJFmjlzpv7whz8oJydH1lpNmzZNzZs3L/fnvJSVO7BZa7+TFOnBWAAAAIAyu9jL8Hfv3l1jx47Vu+++qzvvvFNFRUX6r//6LyUlJflnoEoSHx+vBx54QJs2bdKvfvUrHThwQNu2bfPPop2oRYsWys3N9bd977331Llz5zKPdcaMGXryySe1f/9+ZWRk6IUXXlBRUZHWrVunQ4cOqaCgQPPnz1eHDh1O27devXpq2LChPvnkEw0YMECHDh0660Ikx8NZgwYNlJ+fr/T0dCUkJPhPo+zatas6dOigKVOmKD8/X7169dJrr72m1157TcYYrVq1StHR0WX+jJc6lvUHAAAAysAYo+nTp+ujjz5S06ZN1axZMwUFBen5558/637BwcFKSUnRsGHD5PP5FB8frw0bNpTYNigoSJMnT9bgwYMVERGhgIAAjRo1qsxjjYuLU9++fRUfH69nnnlG1113na6//nrddttt/tMUzxaS3nvvPU2YMEE+n0/t2rXTTz/9dMa29evX18iRIxUREaEBAwaodevWkopXm7zjjjsUERGh6OhoPfroo6pfv76eeeYZFRYWyufzKTw8XM8880yZP19VYLyabi2L2NhYm5mZedH7RQUo4fzucpdM9rwkN8gEAJw3r2/afBz/bjqz9evXKzQ0tKKH4bzk5GTVqVNHjz/+eEUPpcor6TdrjMmy1saea19m2AAAAADAUZ4s6w8AAADg/AwcOFDff//9Se/98Y9/VK9evcpVNzk5uVz7ww0ENgAAAFQ6Z1pavzKaPn16RQ8BF1B5L0HjlEgAAABUKkFBQdq1a5dnS98DF4q1Vrt27VJQUNB512CGDQAAAJVKw4YNtW3bNuXl5VX0UIBzCgoKUsOGDc97fwIbAAAAKpXq1asrJCSkoocBXBScEgkAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAKDqMMb7BwBcQAQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcBSBDQAAAAAcRWADAAAAAEcR2AAAAADAUQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcBSBDQAAAAAcRWADAAAAAEcR2AAAAADAUQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcJRngc0YE2iMWWWMmeVVTQAAAACoyqp5WOthSesl1fOwJgAAAAAXGeN9TWu9r1nJeTLDZoxpKKmvpL95UQ8AAAAA4N0pka9KekLS0TM1MMbcZ4zJNMZk5uXledQtAAAAAFy6yh3YjDH9JO201madrZ219i1rbay1NjY4OLi83QIAAADAJc+LGbb2kvobY3IlfSCpmzHmfQ/qAgAAAECVVu7AZq190lrb0FrbSNJQSQustXeUe2QAAAAAUMVxHzYAAAAAcJSXy/rLWpshKcPLmgAAAABQVTHDBgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4qtyBzRgTZIxZYYxZbYzJMcY868XAAAAAAKCqq+ZBjUOSullr840x1SUtMcZ8Zq1d7kFtAAAAAKiyyh3YrLVWUv6xl9WPPWx56wIAAABAVefJNWzGmEBjTLaknZLmWmu/LKHNfcaYTGNMZl5enhfdAgAAAMAlzZPAZq0tstZGSWooKc4YE15Cm7estbHW2tjg4GAvugUAAACAS5qnq0Raa/dIypDU28u6AAAAAFAVebFKZLAxpv6x5zUl9ZC0obx1AQAAAKCq82KVyGslvWOMCVRxAPzQWjvLg7oAAAAAUKV5sUrkGknRHowFAAAAAHACT69hAwAAAAB4h8AGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgqHIHNmPM9caYhcaY9caYHGPMw14MDAAAAACqumoe1Dgi6b+stSuNMXUlZRlj5lpr13lQGwAAAACqrHLPsFlrt1trVx57vk/Sekm/KG9dAAAAAKjqPL2GzRjTSFK0pC9L2HafMSbTGJOZl5fnZbcAAAAAcEnyLLAZY+pImibpEWvt/5263Vr7lrU21lobGxwc7FW3AAAAAHDJ8iSwGWOqqzispVprP/aiJgAAAABUdV6sEmkkvS1pvbX25fIPCQAAAAAgeTPD1l7ScEndjDHZxx59PKgLAAAAAFVauZf1t9YukWQ8GAsAAAAA4ASerhIJAAAAAPAOgQ0AAAAAHEVgAwAAAABHlfsaNgAAAADwgnnW+6Ux7Djrec2LiRk2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBR3DgbkiSTkXFB6lbu2xQCAAAAFYsZNgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcBSBDQAAAAAcRWADAAAAAEcR2AAAAADAUQQ2AAAAAHAUga0SMsZ4/gAAuMkY7x8AgMqDwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAo6pV9AAAXHouxLLh1npfEwAAwHUENgAAUG4X4p6elv+nBgA4JRIAAAAAXMUMGwAAAHAJMxkZF6Quc+AXBzNsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4CgCGwAAAAA4isAGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAA4AhjjOcPVG7VKnoAAAAAJTEZGZ7XtJ5XBIALixk2AAAAAHAUgQ0AAAAAHEVgAwAAAABHcQ0bAHjsglx306WL5zUBAID7PJlhM8ZMMsbsNMas9aIeAAAAAMC7UyJTJPX2qBYAAAAAQB4FNmvtIkm7vagFAAAAACh20RYdMcbcZ4zJNMZk5uXlXaxuAQAAAKDSumiBzVr7lrU21lobGxwcfLG6BQAAAIBKi2X9AQAAAMBRBDYAAAAAcJRXy/pPlfSFpObGmG3GmHu9qAsAAAAAVZknN8621g7zog4AAAAA4P/jlEgAAAAAcBSBDQAAAAAcRWADAAAAAEcR2AAAAADAUQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcFS1ih4AAKBimGeN5zXtOOt5TQAAqjJm2AAAAADAUQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcBSBDQAAAAAcRWADAAAAAEcR2AAAAADAUQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHVavoAQBAaWSYjAtSt4vtckHqAi67UH9PAADvMcMGAAAAAI4isAEAAACAowhsAAAAAOAoAhsAAAAAOIrABgAAAACOIrABAAAAgKMIbAAAAADgKAIbAAAAADiKwAYAAAAAjiKwAQAAAICjCGwAAAAA4KhqFT2AS12GyajoIQAAAACopJhhAwAAAABHEdgAAAAAwFEENgAAAABwFIENAAAAABxFYAMAAAAARxHYAAAAAMBRBDYAAAAAcBSBDQAAAAAcRWADgMrAGO8fAADAeQQ2AAAAAHAUgQ0AAAAAHEVgAwAAAABHVavoAQAAAACVUYbJqOghoArwZIbNGNPbGPONMWaTMWasFzUBAAAAoKord2AzxgRK+oukmyS1lDTMGNOyvHUBAAAAoKrzYoYtTtIma+131trDkj6QdIsHdQEAAACgSvMisP1C0tYTXm879h4AAAAAoBy8WHSkpLuv2tMaGXOfpPsk6YYbbvCgW+9diPvIWtvF+5qnH153We/HWok+fZV1Ab52SV0uRFGZC/CHby/EAeBvqcqqLH9PlebfTfwtVVn8d94FwN/TReHFDNs2Sdef8LqhpB9PbWStfctaG2utjQ0ODvagWwAAAAC4tHkR2L6S1NQYE2KMuUzSUEkzPagLAAAAAFVauU+JtNYeMcaMlvRPSYGSJllrc8o9MgAAAACo4jy5cba19lNJn3pRCwAAAABQzJMbZwMA/l979xci+1nfcfzz0Wi0NG0iitEkBNoSUaMNzdESSxsl1nghlIjVy0TBKFRoCwW1qVBaWijthagXITe1LaIiJQhGGo+VRGmreCIxOUeNohYSLDSlBtpqreLTi53VVc6fnTNzZp455/WC5ezM/Hb2u3Aedt/7/PY3AADrJ9gAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmJdgAAAAmddG2BwDYpjHGtkcAADglO2wAAACTEmwAAACTEmwAAACTEmwAAACTEmwAAACTWinY2v522xNtf9j2yLqGAgAAYPUdtuNJXpvk02uYBQAAgANWeh22McaXk6TteqYBAADgRzb2N2xtb297rO2xxx9/fFOfFgAAYGedcYet7SeTXH6Sh+4YY3z0sJ9ojHFXkruS5MiRI+PQEwIAAFygzhhsY4xXbmIQAAAAfpLL+gMAAExq1cv639L2sSQ3JLmn7b3rGQsAAIBVrxJ5d5K71zQLAAAABzglEgAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFKCDQAAYFIXbXuAmYyx7QkAAAB+zA4bAADApAQbAADApAQbAADApAQbAADApAQbAADApAQbAADApAQbAADApAQbAADApLxwNgAA570xtj0BnB07bAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJMSbAAAAJNaKdja/mXbr7R9qO3dbS9d12AAAAAXulV32I4muXaM8eIkX03yztVHAgAAIFkx2MYYnxhj/GBx87NJrlx9JAAAAJLkojU+15uSfPhUD7a9Pcnti5v/3faRNX7u88Uzk/zHtoeA84C1BOthLcH6WE/8tKsPc1DHGKc/oP1kkstP8tAdY4yPLo65I8mRJK8dZ3pCTqntsTHGkW3PAbvOWoL1sJZgfawnztYZd9jGGK883eNtb03ymiQ3iTUAAID1WemUyLavTvL2JDeOMb6znpEAAABIVr9K5PuSXJLkaNsH2965hpkuZHdtewA4T1hLsB7WEqyP9cRZOePfsAEAALAdq+6wAQAAcI4INgAAgEkJNgAAgEkJtg1qe3nbD7X9etsvtf1422tOceytbb+2eLt107PCzJZcS//Q9om2H9v0nLALDrue2l7X9l/anmj7UNs3bGNemNUSa+nqtg8sLth3ou1btzEvu8NFRzakbZP8c5K/GWPcubjvuiSXjDE+81PHPiPJsey9GPlI8kCS68cY397s1DCfZdbS4rGbkvxMkreMMV6z0WFhckt+b7omyRhjfK3tc7P3ven5Y4wnNj03zGbJtfTU7P0M/r22P5vkeJKXjTG+tem52Q0rvQ4bS3lFku/vL+IkGWM8eIpjb05ydIzxn0nS9miSVyf54DmfEua3zFrKGOMf2758E4PBDjr0ehpjfPXA+99q++9JnpVEsMFya+n/Dty8OM544wz8B9mca7P328jDuCLJowduP7a4D1huLQGnd1brqe1Lkzw1ydfXPhHspqXWUtur2j6UvZ/3/sLuGqcj2ObUk9zn3FUAtq7tc5L8XZI3jjF+uO15YBeNMR4dY7w4yS8lubXts7c9E/MSbJtzIsn1hzz2sSRXHbh9ZRK/eYE9y6wl4PSWWk9tfy7JPUn+aIzx2XM2Feyes/retNhZO5Hk19c+EecNwbY5n0pycds379/R9iVtbzzJsfcmeVXby9peluRVi/uA5dYScHqHXk+LCyXcneRvxxgf2eCMsAuWWUtXtn364v3Lkvxakkc2Nik7R7BtyNi7HOctSX5zcbnXE0n+OCfZOVtcbORPk3x+8fYn+xcggQvdMmspSdp+JslHktzU9rG2N29sWJjckuvp9Ul+I8lti8uRP7i4Ch5c8JZcS89P8rm2X0xyf5K/GmM8vLFh2Tku6w8AADApO2wAAACT8jpsW9T2Rdm70tZB3xtj/Oo25oFdZS3B+lhPsB7WEuvilEgAAIBJOSUSAABgUoINAABgUoINgJ3T9vK2H1pcPvtLbT/e9pq2x8/y+W5r+9x1zwkAqxJsAOyUts3eCzjfN8b4xTHGC5L8YZJnr/C0tyVZKtjaunAXAOecYANg17wiyffHGHfu3zHGeDDJo/u3Fztm7ztw+2NtX972yW3f3/Z424fb/n7b1yU5kuQDixeDfnrb69ve3/aBtve2fc7iee5r++dt70/yuxv7igG4YPntIAC75tokD5zlx16X5IoxxrVJ0vbSMcYTbd+W5A/GGMfaPiXJe5P81hjj8bZvSPJnSd60eI5Lxxg3rvg1AMChCDYALiTfSPILbd+b5J4knzjJMc/LXhQe3Tv7Mk9O8m8HHv/wuR4SAPYJNgB2zYkkrzvDMT/IT572/7QkGWN8u+0vJ7k5ye8keX1+vHO2r0lOjDFuOMVz/8/SEwPAWfI3bADsmk8lubjtm/fvaPuSJFcfOOZfk1zX9kltr0ry0sVxz0zypDHG3yd5V5JfWRz/X0kuWbz/SJJntb1h8TFPafvCc/j1AMAp2WEDYKeMMUbbW5K8u+07kvxv9gLt9w4c9k9Jvpnk4STHk3xhcf8VSU12vG0AAABfSURBVP667f4vLN+5+Pf9Se5s+90kN2RvB+89bX8+e98r3529nT0A2KiOMbY9AwAAACfhlEgAAIBJCTYAAIBJCTYAAIBJCTYAAIBJCTYAAIBJCTYAAIBJCTYAAIBJ/T9yXNj/EJSL/QAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 1080x720 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "fig,ax=plt.subplots(figsize=(15,10))\n",
    "index=np.arange(len(cluster_4_output.columns))\n",
    "\n",
    "cash_advance=np.log(cluster_4_output.loc['Monthly_ca',:].values)\n",
    "credit_score=(cluster_4_output.loc['limit_usage_ratio',:].values)\n",
    "purchase= np.log(cluster_4_output.loc['Monthly_ap',:].values)\n",
    "payment=cluster_4_output.loc['payment_minpayment_ratio',:].values\n",
    "installment=cluster_4_output.loc['INSTALLMENTS_PURCHASES',:].values\n",
    "one_off=cluster_4_output.loc['ONEOFF_PURCHASES',:].values\n",
    "\n",
    "\n",
    "bar_width=.10\n",
    "b1=plt.bar(index,cash_advance,color='b',label='Monthly cash advance',width=bar_width)\n",
    "b2=plt.bar(index+bar_width,credit_score,color='m',label='Credit_score',width=bar_width)\n",
    "b3=plt.bar(index+2*bar_width,purchase,color='k',label='Avg purchase',width=bar_width)\n",
    "b4=plt.bar(index+3*bar_width,payment,color='c',label='Payment-minpayment ratio',width=bar_width)\n",
    "b5=plt.bar(index+4*bar_width,installment,color='r',label='installment',width=bar_width)\n",
    "b6=plt.bar(index+5*bar_width,one_off,color='g',label='One_off purchase',width=bar_width)\n",
    "\n",
    "plt.xlabel(\"Cluster\")\n",
    "plt.title(\"Insights\")\n",
    "plt.xticks(index + bar_width, ('C_0', 'C_1', 'C_2', 'C_3'))\n",
    "plt.legend()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "** Insights**\n",
    "\n",
    "\n",
    "\n",
    "- Cluster 0 is the group of customers who have highest Monthly_avg purchases, no hefty cash advances and doing both installment as well as one_off purchases, have marginally good credit score. *** This group is about 27% of the total customer base ***\n",
    " \n",
    "\n",
    " \n",
    "- cluster 1 is the group of customers who are not making efficient average purchase transactions, taking maximum advance_cash  and  is paying comparatively less minimum payment and credit_score is on better side & doing no purchase transaction. *** This group is about 27% of the total customer base ***\n",
    "\n",
    "\n",
    "\n",
    "- Cluster 2 customers are doing considerable One_Off transactions,installment purchases,credit score is good are maintaining least payment ratio. *** This group is about 18% of the total customer base ***\n",
    "\n",
    "\n",
    "\n",
    "- Cluster 3 customers have least credit score and  are maximum cash advances dues and are doing considerable installment purchases with good min payment ratio. *** This group is about 28% of the total customer base ***\n",
    "\n",
    "\n",
    "---\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 5 Cluster Solution"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [],
   "source": [
    "km_5=KMeans(n_clusters=5,random_state=123)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,\n",
       "    n_clusters=5, n_init=10, n_jobs=None, precompute_distances='auto',\n",
       "    random_state=123, tol=0.0001, verbose=0)"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_5.fit(reduced_cr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([0, 4, 2, ..., 0, 4, 1])"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_5.labels_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "4    2344\n",
       "0    2040\n",
       "3    1577\n",
       "2    1533\n",
       "1    1456\n",
       "dtype: int64"
      ]
     },
     "execution_count": 42,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.Series(km_5.labels_).value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [],
   "source": [
    "C5=pd.concat([credit_card_data_log[my_vars],pd.Series(km_5.labels_,name='cluster_5')],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
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
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <th>Monthly_ap</th>\n",
       "      <th>Monthly_ca</th>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <th>cluster_5</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0.597837</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.568506</td>\n",
       "      <td>0.154151</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.098612</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.191654</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.040086</td>\n",
       "      <td>0.894662</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.646627</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.223144</td>\n",
       "      <td>1.609438</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6.287695</td>\n",
       "      <td>0.376719</td>\n",
       "      <td>1.574068</td>\n",
       "      <td>4</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>6.651791</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.564949</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.180994</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.287197</td>\n",
       "      <td>0.688979</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>0.492477</td>\n",
       "      <td>7.313220</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.835620</td>\n",
       "      <td>2.898616</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>2.833213</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.847298</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.519644</td>\n",
       "      <td>1.327360</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   BALANCE_FREQUENCY  ONEOFF_PURCHASES  INSTALLMENTS_PURCHASES  \\\n",
       "0           0.597837          0.000000                4.568506   \n",
       "1           0.646627          0.000000                0.000000   \n",
       "2           0.693147          6.651791                0.000000   \n",
       "3           0.492477          7.313220                0.000000   \n",
       "4           0.693147          2.833213                0.000000   \n",
       "\n",
       "   PURCHASES_FREQUENCY  ONEOFF_PURCHASES_FREQUENCY  \\\n",
       "0             0.154151                    0.000000   \n",
       "1             0.000000                    0.000000   \n",
       "2             0.693147                    0.693147   \n",
       "3             0.080042                    0.080042   \n",
       "4             0.080042                    0.080042   \n",
       "\n",
       "   PURCHASES_INSTALLMENTS_FREQUENCY  CASH_ADVANCE_FREQUENCY  CASH_ADVANCE_TRX  \\\n",
       "0                          0.080042                0.000000          0.000000   \n",
       "1                          0.000000                0.223144          1.609438   \n",
       "2                          0.000000                0.000000          0.000000   \n",
       "3                          0.000000                0.080042          0.693147   \n",
       "4                          0.000000                0.000000          0.000000   \n",
       "\n",
       "   PURCHASES_TRX  PRC_FULL_PAYMENT  Monthly_ap  Monthly_ca  limit_usage_ratio  \\\n",
       "0       1.098612          0.000000    2.191654    0.000000           0.040086   \n",
       "1       0.000000          0.200671    0.000000    6.287695           0.376719   \n",
       "2       2.564949          0.000000    4.180994    0.000000           0.287197   \n",
       "3       0.693147          0.000000    4.835620    2.898616           0.200671   \n",
       "4       0.693147          0.000000    0.847298    0.000000           0.519644   \n",
       "\n",
       "   payment_minpayment_ratio  cluster_5  \n",
       "0                  0.894662          0  \n",
       "1                  1.574068          4  \n",
       "2                  0.688979          2  \n",
       "3                  0.000000          2  \n",
       "4                  1.327360          2  "
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "C5.head(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Mean value of each variable group by Cluster-5\n",
    "cluster_5_output=C5.groupby('cluster_5').apply(lambda x: x[my_vars].mean()).T"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
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
       "      <th>cluster_5</th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "      <th>2</th>\n",
       "      <th>3</th>\n",
       "      <th>4</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <td>0.471667</td>\n",
       "      <td>0.678981</td>\n",
       "      <td>0.684304</td>\n",
       "      <td>0.667514</td>\n",
       "      <td>0.638206</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <td>1.607545</td>\n",
       "      <td>5.002104</td>\n",
       "      <td>3.939956</td>\n",
       "      <td>6.798783</td>\n",
       "      <td>0.577715</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <td>4.286509</td>\n",
       "      <td>4.596027</td>\n",
       "      <td>3.508009</td>\n",
       "      <td>5.659006</td>\n",
       "      <td>0.113348</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <td>0.415385</td>\n",
       "      <td>0.500833</td>\n",
       "      <td>0.408998</td>\n",
       "      <td>0.627541</td>\n",
       "      <td>0.017117</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <td>0.045891</td>\n",
       "      <td>0.243639</td>\n",
       "      <td>0.162571</td>\n",
       "      <td>0.439020</td>\n",
       "      <td>0.012988</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <td>0.358504</td>\n",
       "      <td>0.364993</td>\n",
       "      <td>0.276736</td>\n",
       "      <td>0.457608</td>\n",
       "      <td>0.003618</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <td>0.007512</td>\n",
       "      <td>0.282348</td>\n",
       "      <td>0.013599</td>\n",
       "      <td>0.011279</td>\n",
       "      <td>0.235016</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <td>0.061128</td>\n",
       "      <td>1.979969</td>\n",
       "      <td>0.124474</td>\n",
       "      <td>0.097832</td>\n",
       "      <td>1.691389</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <td>1.934768</td>\n",
       "      <td>2.703849</td>\n",
       "      <td>2.180463</td>\n",
       "      <td>3.422792</td>\n",
       "      <td>0.142370</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <td>0.246229</td>\n",
       "      <td>0.031699</td>\n",
       "      <td>0.010536</td>\n",
       "      <td>0.266333</td>\n",
       "      <td>0.029465</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ap</th>\n",
       "      <td>3.138795</td>\n",
       "      <td>4.313432</td>\n",
       "      <td>3.631110</td>\n",
       "      <td>5.240514</td>\n",
       "      <td>0.337485</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ca</th>\n",
       "      <td>0.217827</td>\n",
       "      <td>4.986686</td>\n",
       "      <td>0.354654</td>\n",
       "      <td>0.335501</td>\n",
       "      <td>4.517826</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <td>0.025604</td>\n",
       "      <td>0.453417</td>\n",
       "      <td>0.439501</td>\n",
       "      <td>0.145872</td>\n",
       "      <td>0.441008</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <td>1.538033</td>\n",
       "      <td>1.157878</td>\n",
       "      <td>0.807034</td>\n",
       "      <td>2.239053</td>\n",
       "      <td>1.091678</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "cluster_5                                0         1         2         3  \\\n",
       "BALANCE_FREQUENCY                 0.471667  0.678981  0.684304  0.667514   \n",
       "ONEOFF_PURCHASES                  1.607545  5.002104  3.939956  6.798783   \n",
       "INSTALLMENTS_PURCHASES            4.286509  4.596027  3.508009  5.659006   \n",
       "PURCHASES_FREQUENCY               0.415385  0.500833  0.408998  0.627541   \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.045891  0.243639  0.162571  0.439020   \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.358504  0.364993  0.276736  0.457608   \n",
       "CASH_ADVANCE_FREQUENCY            0.007512  0.282348  0.013599  0.011279   \n",
       "CASH_ADVANCE_TRX                  0.061128  1.979969  0.124474  0.097832   \n",
       "PURCHASES_TRX                     1.934768  2.703849  2.180463  3.422792   \n",
       "PRC_FULL_PAYMENT                  0.246229  0.031699  0.010536  0.266333   \n",
       "Monthly_ap                        3.138795  4.313432  3.631110  5.240514   \n",
       "Monthly_ca                        0.217827  4.986686  0.354654  0.335501   \n",
       "limit_usage_ratio                 0.025604  0.453417  0.439501  0.145872   \n",
       "payment_minpayment_ratio          1.538033  1.157878  0.807034  2.239053   \n",
       "\n",
       "cluster_5                                4  \n",
       "BALANCE_FREQUENCY                 0.638206  \n",
       "ONEOFF_PURCHASES                  0.577715  \n",
       "INSTALLMENTS_PURCHASES            0.113348  \n",
       "PURCHASES_FREQUENCY               0.017117  \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.012988  \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.003618  \n",
       "CASH_ADVANCE_FREQUENCY            0.235016  \n",
       "CASH_ADVANCE_TRX                  1.691389  \n",
       "PURCHASES_TRX                     0.142370  \n",
       "PRC_FULL_PAYMENT                  0.029465  \n",
       "Monthly_ap                        0.337485  \n",
       "Monthly_ca                        4.517826  \n",
       "limit_usage_ratio                 0.441008  \n",
       "payment_minpayment_ratio          1.091678  "
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cluster_5_output"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('\\n', cluster_5   \n",
      "0          0    2040\n",
      "1          1    1456\n",
      "2          2    1533\n",
      "3          3    1577\n",
      "4          4    2344\n",
      "Name: cluster_5, dtype: int64)\n"
     ]
    }
   ],
   "source": [
    "s1=C5.groupby('cluster_5').apply(lambda x: x['cluster_5'].value_counts())\n",
    "print ('\\n',s1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('Cluster-5', '\\n')\n",
      "   size  Percentage\n",
      "0  2040   22.793296\n",
      "1  1456   16.268156\n",
      "2  1533   17.128492\n",
      "3  1577   17.620112\n",
      "4  2344   26.189944\n"
     ]
    }
   ],
   "source": [
    "# percentage of each cluster\n",
    "print (\"Cluster-5\",'\\n')\n",
    "per_5=pd.Series((s1.values.astype('float')/C5.shape[0])*100,name='Percentage')\n",
    "print (pd.concat((pd.Series(s1.values,name='size'),per_5),axis=1))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 6 Cluster Solution"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [],
   "source": [
    "km_6=KMeans(n_clusters=6,random_state=123)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,\n",
       "    n_clusters=6, n_init=10, n_jobs=None, precompute_distances='auto',\n",
       "    random_state=123, tol=0.0001, verbose=0)"
      ]
     },
     "execution_count": 51,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_6.fit(reduced_cr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([2, 1, 3, ..., 5, 2, 4])"
      ]
     },
     "execution_count": 52,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_6.labels_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1    2233\n",
       "0    1484\n",
       "5    1455\n",
       "4    1431\n",
       "3    1428\n",
       "2     919\n",
       "dtype: int64"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.Series(km_6.labels_).value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [],
   "source": [
    "C6=pd.concat([credit_card_data_log[my_vars],pd.Series(km_6.labels_,name='cluster_6')],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
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
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <th>Monthly_ap</th>\n",
       "      <th>Monthly_ca</th>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <th>cluster_6</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0.597837</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.568506</td>\n",
       "      <td>0.154151</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.098612</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.191654</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.040086</td>\n",
       "      <td>0.894662</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.646627</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.223144</td>\n",
       "      <td>1.609438</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6.287695</td>\n",
       "      <td>0.376719</td>\n",
       "      <td>1.574068</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>6.651791</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.564949</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.180994</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.287197</td>\n",
       "      <td>0.688979</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>0.492477</td>\n",
       "      <td>7.313220</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.835620</td>\n",
       "      <td>2.898616</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>2.833213</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.847298</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.519644</td>\n",
       "      <td>1.327360</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   BALANCE_FREQUENCY  ONEOFF_PURCHASES  INSTALLMENTS_PURCHASES  \\\n",
       "0           0.597837          0.000000                4.568506   \n",
       "1           0.646627          0.000000                0.000000   \n",
       "2           0.693147          6.651791                0.000000   \n",
       "3           0.492477          7.313220                0.000000   \n",
       "4           0.693147          2.833213                0.000000   \n",
       "\n",
       "   PURCHASES_FREQUENCY  ONEOFF_PURCHASES_FREQUENCY  \\\n",
       "0             0.154151                    0.000000   \n",
       "1             0.000000                    0.000000   \n",
       "2             0.693147                    0.693147   \n",
       "3             0.080042                    0.080042   \n",
       "4             0.080042                    0.080042   \n",
       "\n",
       "   PURCHASES_INSTALLMENTS_FREQUENCY  CASH_ADVANCE_FREQUENCY  CASH_ADVANCE_TRX  \\\n",
       "0                          0.080042                0.000000          0.000000   \n",
       "1                          0.000000                0.223144          1.609438   \n",
       "2                          0.000000                0.000000          0.000000   \n",
       "3                          0.000000                0.080042          0.693147   \n",
       "4                          0.000000                0.000000          0.000000   \n",
       "\n",
       "   PURCHASES_TRX  PRC_FULL_PAYMENT  Monthly_ap  Monthly_ca  limit_usage_ratio  \\\n",
       "0       1.098612          0.000000    2.191654    0.000000           0.040086   \n",
       "1       0.000000          0.200671    0.000000    6.287695           0.376719   \n",
       "2       2.564949          0.000000    4.180994    0.000000           0.287197   \n",
       "3       0.693147          0.000000    4.835620    2.898616           0.200671   \n",
       "4       0.693147          0.000000    0.847298    0.000000           0.519644   \n",
       "\n",
       "   payment_minpayment_ratio  cluster_6  \n",
       "0                  0.894662          2  \n",
       "1                  1.574068          1  \n",
       "2                  0.688979          3  \n",
       "3                  0.000000          2  \n",
       "4                  1.327360          3  "
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "C6.head(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Mean value of each variable group by Cluster-6\n",
    "cluster_6_output=C6.groupby('cluster_6').apply(lambda x: x[my_vars].mean()).T"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {},
   "outputs": [
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
       "      <th>cluster_6</th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "      <th>2</th>\n",
       "      <th>3</th>\n",
       "      <th>4</th>\n",
       "      <th>5</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <td>0.665510</td>\n",
       "      <td>0.654564</td>\n",
       "      <td>0.314868</td>\n",
       "      <td>0.685061</td>\n",
       "      <td>0.679115</td>\n",
       "      <td>0.590899</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <td>7.166017</td>\n",
       "      <td>0.573791</td>\n",
       "      <td>2.862660</td>\n",
       "      <td>4.047577</td>\n",
       "      <td>5.022481</td>\n",
       "      <td>0.800491</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <td>5.425212</td>\n",
       "      <td>0.123932</td>\n",
       "      <td>1.507995</td>\n",
       "      <td>3.465724</td>\n",
       "      <td>4.632959</td>\n",
       "      <td>5.987346</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <td>0.619456</td>\n",
       "      <td>0.017033</td>\n",
       "      <td>0.155670</td>\n",
       "      <td>0.400030</td>\n",
       "      <td>0.504359</td>\n",
       "      <td>0.577317</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <td>0.471360</td>\n",
       "      <td>0.012527</td>\n",
       "      <td>0.080748</td>\n",
       "      <td>0.165575</td>\n",
       "      <td>0.245871</td>\n",
       "      <td>0.020890</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <td>0.430045</td>\n",
       "      <td>0.003974</td>\n",
       "      <td>0.070495</td>\n",
       "      <td>0.267793</td>\n",
       "      <td>0.368029</td>\n",
       "      <td>0.547240</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <td>0.011925</td>\n",
       "      <td>0.241612</td>\n",
       "      <td>0.029353</td>\n",
       "      <td>0.014481</td>\n",
       "      <td>0.283223</td>\n",
       "      <td>0.003968</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <td>0.104862</td>\n",
       "      <td>1.725704</td>\n",
       "      <td>0.254887</td>\n",
       "      <td>0.132116</td>\n",
       "      <td>1.984231</td>\n",
       "      <td>0.031479</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <td>3.429305</td>\n",
       "      <td>0.142937</td>\n",
       "      <td>1.029240</td>\n",
       "      <td>2.163542</td>\n",
       "      <td>2.723147</td>\n",
       "      <td>2.486144</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <td>0.255163</td>\n",
       "      <td>0.027556</td>\n",
       "      <td>0.104212</td>\n",
       "      <td>0.008044</td>\n",
       "      <td>0.031550</td>\n",
       "      <td>0.316898</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ap</th>\n",
       "      <td>5.263442</td>\n",
       "      <td>0.337495</td>\n",
       "      <td>2.242451</td>\n",
       "      <td>3.603938</td>\n",
       "      <td>4.328982</td>\n",
       "      <td>3.669253</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ca</th>\n",
       "      <td>0.353933</td>\n",
       "      <td>4.549386</td>\n",
       "      <td>0.905437</td>\n",
       "      <td>0.380094</td>\n",
       "      <td>4.992796</td>\n",
       "      <td>0.112685</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <td>0.151783</td>\n",
       "      <td>0.460958</td>\n",
       "      <td>0.023920</td>\n",
       "      <td>0.461174</td>\n",
       "      <td>0.454022</td>\n",
       "      <td>0.044752</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <td>2.237347</td>\n",
       "      <td>1.089607</td>\n",
       "      <td>1.406371</td>\n",
       "      <td>0.785189</td>\n",
       "      <td>1.151311</td>\n",
       "      <td>1.605480</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "cluster_6                                0         1         2         3  \\\n",
       "BALANCE_FREQUENCY                 0.665510  0.654564  0.314868  0.685061   \n",
       "ONEOFF_PURCHASES                  7.166017  0.573791  2.862660  4.047577   \n",
       "INSTALLMENTS_PURCHASES            5.425212  0.123932  1.507995  3.465724   \n",
       "PURCHASES_FREQUENCY               0.619456  0.017033  0.155670  0.400030   \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.471360  0.012527  0.080748  0.165575   \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.430045  0.003974  0.070495  0.267793   \n",
       "CASH_ADVANCE_FREQUENCY            0.011925  0.241612  0.029353  0.014481   \n",
       "CASH_ADVANCE_TRX                  0.104862  1.725704  0.254887  0.132116   \n",
       "PURCHASES_TRX                     3.429305  0.142937  1.029240  2.163542   \n",
       "PRC_FULL_PAYMENT                  0.255163  0.027556  0.104212  0.008044   \n",
       "Monthly_ap                        5.263442  0.337495  2.242451  3.603938   \n",
       "Monthly_ca                        0.353933  4.549386  0.905437  0.380094   \n",
       "limit_usage_ratio                 0.151783  0.460958  0.023920  0.461174   \n",
       "payment_minpayment_ratio          2.237347  1.089607  1.406371  0.785189   \n",
       "\n",
       "cluster_6                                4         5  \n",
       "BALANCE_FREQUENCY                 0.679115  0.590899  \n",
       "ONEOFF_PURCHASES                  5.022481  0.800491  \n",
       "INSTALLMENTS_PURCHASES            4.632959  5.987346  \n",
       "PURCHASES_FREQUENCY               0.504359  0.577317  \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.245871  0.020890  \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.368029  0.547240  \n",
       "CASH_ADVANCE_FREQUENCY            0.283223  0.003968  \n",
       "CASH_ADVANCE_TRX                  1.984231  0.031479  \n",
       "PURCHASES_TRX                     2.723147  2.486144  \n",
       "PRC_FULL_PAYMENT                  0.031550  0.316898  \n",
       "Monthly_ap                        4.328982  3.669253  \n",
       "Monthly_ca                        4.992796  0.112685  \n",
       "limit_usage_ratio                 0.454022  0.044752  \n",
       "payment_minpayment_ratio          1.151311  1.605480  "
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cluster_6_output"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('\\n', cluster_6   \n",
      "0          0    1484\n",
      "1          1    2233\n",
      "2          2     919\n",
      "3          3    1428\n",
      "4          4    1431\n",
      "5          5    1455\n",
      "Name: cluster_6, dtype: int64)\n"
     ]
    }
   ],
   "source": [
    "s1=C6.groupby('cluster_6').apply(lambda x: x['cluster_6'].value_counts())\n",
    "print ('\\n',s1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('Cluster-6', '\\n')\n",
      "   size  Percentage\n",
      "0  1484   16.581006\n",
      "1  2233   24.949721\n",
      "2   919   10.268156\n",
      "3  1428   15.955307\n",
      "4  1431   15.988827\n",
      "5  1455   16.256983\n"
     ]
    }
   ],
   "source": [
    "# percentage of each cluster\n",
    "print (\"Cluster-6\",'\\n')\n",
    "per_6=pd.Series((s1.values.astype('float')/C6.shape[0])*100,name='Percentage')\n",
    "print (pd.concat((pd.Series(s1.values,name='size'),per_6),axis=1))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 7 Cluster Solution"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {},
   "outputs": [],
   "source": [
    "km_7=KMeans(n_clusters=7,random_state=123)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "KMeans(algorithm='auto', copy_x=True, init='k-means++', max_iter=300,\n",
       "    n_clusters=7, n_init=10, n_jobs=None, precompute_distances='auto',\n",
       "    random_state=123, tol=0.0001, verbose=0)"
      ]
     },
     "execution_count": 60,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_7.fit(reduced_cr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 61,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([5, 0, 6, ..., 1, 5, 6])"
      ]
     },
     "execution_count": 61,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "km_7.labels_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0    2196\n",
       "1    1425\n",
       "3    1316\n",
       "2    1303\n",
       "6    1047\n",
       "4     904\n",
       "5     759\n",
       "dtype: int64"
      ]
     },
     "execution_count": 62,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.Series(km_7.labels_).value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "metadata": {},
   "outputs": [],
   "source": [
    "C7=pd.concat([credit_card_data_log[my_vars],pd.Series(km_7.labels_,name='cluster_7')],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [
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
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <th>Monthly_ap</th>\n",
       "      <th>Monthly_ca</th>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <th>cluster_7</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0.597837</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.568506</td>\n",
       "      <td>0.154151</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.098612</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.191654</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.040086</td>\n",
       "      <td>0.894662</td>\n",
       "      <td>5</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>0.646627</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.223144</td>\n",
       "      <td>1.609438</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6.287695</td>\n",
       "      <td>0.376719</td>\n",
       "      <td>1.574068</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>6.651791</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>2.564949</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.180994</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.287197</td>\n",
       "      <td>0.688979</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>0.492477</td>\n",
       "      <td>7.313220</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.835620</td>\n",
       "      <td>2.898616</td>\n",
       "      <td>0.200671</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>0.693147</td>\n",
       "      <td>2.833213</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.080042</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.693147</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.847298</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.519644</td>\n",
       "      <td>1.327360</td>\n",
       "      <td>6</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   BALANCE_FREQUENCY  ONEOFF_PURCHASES  INSTALLMENTS_PURCHASES  \\\n",
       "0           0.597837          0.000000                4.568506   \n",
       "1           0.646627          0.000000                0.000000   \n",
       "2           0.693147          6.651791                0.000000   \n",
       "3           0.492477          7.313220                0.000000   \n",
       "4           0.693147          2.833213                0.000000   \n",
       "\n",
       "   PURCHASES_FREQUENCY  ONEOFF_PURCHASES_FREQUENCY  \\\n",
       "0             0.154151                    0.000000   \n",
       "1             0.000000                    0.000000   \n",
       "2             0.693147                    0.693147   \n",
       "3             0.080042                    0.080042   \n",
       "4             0.080042                    0.080042   \n",
       "\n",
       "   PURCHASES_INSTALLMENTS_FREQUENCY  CASH_ADVANCE_FREQUENCY  CASH_ADVANCE_TRX  \\\n",
       "0                          0.080042                0.000000          0.000000   \n",
       "1                          0.000000                0.223144          1.609438   \n",
       "2                          0.000000                0.000000          0.000000   \n",
       "3                          0.000000                0.080042          0.693147   \n",
       "4                          0.000000                0.000000          0.000000   \n",
       "\n",
       "   PURCHASES_TRX  PRC_FULL_PAYMENT  Monthly_ap  Monthly_ca  limit_usage_ratio  \\\n",
       "0       1.098612          0.000000    2.191654    0.000000           0.040086   \n",
       "1       0.000000          0.200671    0.000000    6.287695           0.376719   \n",
       "2       2.564949          0.000000    4.180994    0.000000           0.287197   \n",
       "3       0.693147          0.000000    4.835620    2.898616           0.200671   \n",
       "4       0.693147          0.000000    0.847298    0.000000           0.519644   \n",
       "\n",
       "   payment_minpayment_ratio  cluster_7  \n",
       "0                  0.894662          5  \n",
       "1                  1.574068          0  \n",
       "2                  0.688979          6  \n",
       "3                  0.000000          6  \n",
       "4                  1.327360          6  "
      ]
     },
     "execution_count": 64,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "C7.head(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 65,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Mean value of each variable group by Cluster-4\n",
    "cluster_7_output=C7.groupby('cluster_7').apply(lambda x: x[my_vars].mean()).T"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 66,
   "metadata": {},
   "outputs": [
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
       "      <th>cluster_7</th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "      <th>2</th>\n",
       "      <th>3</th>\n",
       "      <th>4</th>\n",
       "      <th>5</th>\n",
       "      <th>6</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>BALANCE_FREQUENCY</th>\n",
       "      <td>0.655009</td>\n",
       "      <td>0.582874</td>\n",
       "      <td>0.678995</td>\n",
       "      <td>0.666088</td>\n",
       "      <td>0.688297</td>\n",
       "      <td>0.277226</td>\n",
       "      <td>0.654753</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES</th>\n",
       "      <td>0.507211</td>\n",
       "      <td>0.872220</td>\n",
       "      <td>5.202949</td>\n",
       "      <td>7.226742</td>\n",
       "      <td>2.484707</td>\n",
       "      <td>2.294150</td>\n",
       "      <td>5.772909</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>INSTALLMENTS_PURCHASES</th>\n",
       "      <td>0.129408</td>\n",
       "      <td>5.932527</td>\n",
       "      <td>4.602336</td>\n",
       "      <td>5.757093</td>\n",
       "      <td>6.070871</td>\n",
       "      <td>1.609902</td>\n",
       "      <td>0.938701</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_FREQUENCY</th>\n",
       "      <td>0.015880</td>\n",
       "      <td>0.565942</td>\n",
       "      <td>0.497251</td>\n",
       "      <td>0.631018</td>\n",
       "      <td>0.565932</td>\n",
       "      <td>0.143854</td>\n",
       "      <td>0.279732</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>ONEOFF_PURCHASES_FREQUENCY</th>\n",
       "      <td>0.011168</td>\n",
       "      <td>0.022605</td>\n",
       "      <td>0.255508</td>\n",
       "      <td>0.481171</td>\n",
       "      <td>0.093494</td>\n",
       "      <td>0.059716</td>\n",
       "      <td>0.255615</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_INSTALLMENTS_FREQUENCY</th>\n",
       "      <td>0.004148</td>\n",
       "      <td>0.533998</td>\n",
       "      <td>0.358984</td>\n",
       "      <td>0.456898</td>\n",
       "      <td>0.533746</td>\n",
       "      <td>0.079292</td>\n",
       "      <td>0.033774</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_FREQUENCY</th>\n",
       "      <td>0.242652</td>\n",
       "      <td>0.004117</td>\n",
       "      <td>0.299306</td>\n",
       "      <td>0.012163</td>\n",
       "      <td>0.024736</td>\n",
       "      <td>0.033200</td>\n",
       "      <td>0.022582</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>CASH_ADVANCE_TRX</th>\n",
       "      <td>1.731255</td>\n",
       "      <td>0.031610</td>\n",
       "      <td>2.070526</td>\n",
       "      <td>0.106531</td>\n",
       "      <td>0.231991</td>\n",
       "      <td>0.294633</td>\n",
       "      <td>0.190006</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PURCHASES_TRX</th>\n",
       "      <td>0.131629</td>\n",
       "      <td>2.449660</td>\n",
       "      <td>2.720461</td>\n",
       "      <td>3.514846</td>\n",
       "      <td>2.738717</td>\n",
       "      <td>0.938928</td>\n",
       "      <td>1.737606</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>PRC_FULL_PAYMENT</th>\n",
       "      <td>0.028199</td>\n",
       "      <td>0.324695</td>\n",
       "      <td>0.032468</td>\n",
       "      <td>0.276667</td>\n",
       "      <td>0.013443</td>\n",
       "      <td>0.102077</td>\n",
       "      <td>0.031557</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ap</th>\n",
       "      <td>0.302525</td>\n",
       "      <td>3.633456</td>\n",
       "      <td>4.342171</td>\n",
       "      <td>5.344516</td>\n",
       "      <td>3.926585</td>\n",
       "      <td>1.951755</td>\n",
       "      <td>3.573141</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Monthly_ca</th>\n",
       "      <td>4.554609</td>\n",
       "      <td>0.111266</td>\n",
       "      <td>5.110474</td>\n",
       "      <td>0.356297</td>\n",
       "      <td>0.725745</td>\n",
       "      <td>1.052081</td>\n",
       "      <td>0.596583</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>limit_usage_ratio</th>\n",
       "      <td>0.462587</td>\n",
       "      <td>0.036481</td>\n",
       "      <td>0.449942</td>\n",
       "      <td>0.146421</td>\n",
       "      <td>0.497390</td>\n",
       "      <td>0.018992</td>\n",
       "      <td>0.323857</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>payment_minpayment_ratio</th>\n",
       "      <td>1.091331</td>\n",
       "      <td>1.638806</td>\n",
       "      <td>1.177907</td>\n",
       "      <td>2.320123</td>\n",
       "      <td>0.715518</td>\n",
       "      <td>1.291923</td>\n",
       "      <td>1.149156</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "cluster_7                                0         1         2         3  \\\n",
       "BALANCE_FREQUENCY                 0.655009  0.582874  0.678995  0.666088   \n",
       "ONEOFF_PURCHASES                  0.507211  0.872220  5.202949  7.226742   \n",
       "INSTALLMENTS_PURCHASES            0.129408  5.932527  4.602336  5.757093   \n",
       "PURCHASES_FREQUENCY               0.015880  0.565942  0.497251  0.631018   \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.011168  0.022605  0.255508  0.481171   \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.004148  0.533998  0.358984  0.456898   \n",
       "CASH_ADVANCE_FREQUENCY            0.242652  0.004117  0.299306  0.012163   \n",
       "CASH_ADVANCE_TRX                  1.731255  0.031610  2.070526  0.106531   \n",
       "PURCHASES_TRX                     0.131629  2.449660  2.720461  3.514846   \n",
       "PRC_FULL_PAYMENT                  0.028199  0.324695  0.032468  0.276667   \n",
       "Monthly_ap                        0.302525  3.633456  4.342171  5.344516   \n",
       "Monthly_ca                        4.554609  0.111266  5.110474  0.356297   \n",
       "limit_usage_ratio                 0.462587  0.036481  0.449942  0.146421   \n",
       "payment_minpayment_ratio          1.091331  1.638806  1.177907  2.320123   \n",
       "\n",
       "cluster_7                                4         5         6  \n",
       "BALANCE_FREQUENCY                 0.688297  0.277226  0.654753  \n",
       "ONEOFF_PURCHASES                  2.484707  2.294150  5.772909  \n",
       "INSTALLMENTS_PURCHASES            6.070871  1.609902  0.938701  \n",
       "PURCHASES_FREQUENCY               0.565932  0.143854  0.279732  \n",
       "ONEOFF_PURCHASES_FREQUENCY        0.093494  0.059716  0.255615  \n",
       "PURCHASES_INSTALLMENTS_FREQUENCY  0.533746  0.079292  0.033774  \n",
       "CASH_ADVANCE_FREQUENCY            0.024736  0.033200  0.022582  \n",
       "CASH_ADVANCE_TRX                  0.231991  0.294633  0.190006  \n",
       "PURCHASES_TRX                     2.738717  0.938928  1.737606  \n",
       "PRC_FULL_PAYMENT                  0.013443  0.102077  0.031557  \n",
       "Monthly_ap                        3.926585  1.951755  3.573141  \n",
       "Monthly_ca                        0.725745  1.052081  0.596583  \n",
       "limit_usage_ratio                 0.497390  0.018992  0.323857  \n",
       "payment_minpayment_ratio          0.715518  1.291923  1.149156  "
      ]
     },
     "execution_count": 66,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cluster_7_output"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 67,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('\\n', cluster_7   \n",
      "0          0    2196\n",
      "1          1    1425\n",
      "2          2    1303\n",
      "3          3    1316\n",
      "4          4     904\n",
      "5          5     759\n",
      "6          6    1047\n",
      "Name: cluster_7, dtype: int64)\n"
     ]
    }
   ],
   "source": [
    "s1=C7.groupby('cluster_7').apply(lambda x: x['cluster_7'].value_counts())\n",
    "print ('\\n',s1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 68,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "('Cluster-7', '\\n')\n",
      "   size  Percentage\n",
      "0  2196   24.536313\n",
      "1  1425   15.921788\n",
      "2  1303   14.558659\n",
      "3  1316   14.703911\n",
      "4   904   10.100559\n",
      "5   759    8.480447\n",
      "6  1047   11.698324\n"
     ]
    }
   ],
   "source": [
    "# percentage of each cluster\n",
    "print (\"Cluster-7\",'\\n')\n",
    "per_7=pd.Series((s1.values.astype('float')/C7.shape[0])*100,name='Percentage')\n",
    "print (pd.concat((pd.Series(s1.values,name='size'),per_7),axis=1))"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 2",
   "language": "python",
   "name": "python2"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 2
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython2",
   "version": "2.7.15"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
