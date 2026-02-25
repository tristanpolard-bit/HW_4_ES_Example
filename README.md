tristanpolard@Tristans-MacBook-Air ~ % cd ~
tristanpolard@Tristans-MacBook-Air ~ % mdfind "HW4_EchoSurveyData.xlsx"
2026-02-24 10:48:42.817 mdfind[71263:10032728] [UserQueryParser] Loading keywords and predicates for locale "en_GB"
/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx
tristanpolard@Tristans-MacBook-Air ~ % python3
Python 3.14.3 (v3.14.3:323c59a5e34, Feb  3 2026, 11:41:37) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import pandas as pd
... 
... df = pd.read_excel("/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"\
)
... print(df.head())
... print(df.columns)
... 

  Demographic Questions  ...                                         Unnamed: 3
0                   NaN  ...                                             Coding
1                   NaN  ...  1 - 18 - 25\n2 - 26 - 35\n3 - 36 - 45\n4 - 46 ...
2                   NaN  ...                    0 = male\n1 = female\n2 = other
3                   NaN  ...  1 - $25K or less\n2 - $26K - $50K\n3 - $56 - $...
4                   NaN  ...                                                NaN

[5 rows x 4 columns]
Index(['Demographic Questions', 'Unnamed: 1', 'Unnamed: 2', 'Unnamed: 3'], dtype='str')
>>> 
>>> 
>>> import pandas as pd
... 
... path = "/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"
... xl = pd.ExcelFile(path)
... print(xl.sheet_names)
... 
['Variables', 'Data']
>>> 
>>> 
>>> df_raw = pd.read_excel(path, sheet_name=xl.sheet_names[0], header=None)
... print(df_raw.shape)
... print(df_raw.head(15))
... 
(25, 4)
                         0  ...                                                  3
0    Demographic Questions  ...                                                NaN
1                      NaN  ...                                             Coding
2                      NaN  ...  1 - 18 - 25\n2 - 26 - 35\n3 - 36 - 45\n4 - 46 ...
3                      NaN  ...                    0 = male\n1 = female\n2 = other
4                      NaN  ...  1 - $25K or less\n2 - $26K - $50K\n3 - $56 - $...
5                      NaN  ...                                                NaN
6   Satisfaction Questions  ...                                                NaN
7                      NaN  ...                                             Coding
8                      NaN  ...           1 (very unlikely) -10 (extremely likely)
9                      NaN  ...  1 - poor\n2 - fair\n3 - okay\n4 - good\n5 - ex...
10                     NaN  ...  1 - very difficult\n2 - somewhat difficult\n3 ...
11                     NaN  ...  1 - very dissatisfied\n2 - somewhat dissatisfi...
12                     NaN  ...                                                NaN
13         Usage Questions  ...                                                NaN
14                     NaN  ...                                             Coding

[15 rows x 4 columns]
>>> header_row = 6  # <- change this to whatever row you see has real headers
... df = pd.read_excel(path, sheet_name=xl.sheet_names[0], header=header_row)
... 
... print(df.shape)
... print(df.head())
... print(df.columns)
... 
(18, 4)
  Satisfaction Questions  ...                                         Unnamed: 3
0                    NaN  ...                                             Coding
1                    NaN  ...           1 (very unlikely) -10 (extremely likely)
2                    NaN  ...  1 - poor\n2 - fair\n3 - okay\n4 - good\n5 - ex...
3                    NaN  ...  1 - very difficult\n2 - somewhat difficult\n3 ...
4                    NaN  ...  1 - very dissatisfied\n2 - somewhat dissatisfi...

[5 rows x 4 columns]
Index(['Satisfaction Questions', 'Unnamed: 1', 'Unnamed: 2', 'Unnamed: 3'], dtype='str')
>>> print(df.columns)
Index(['Satisfaction Questions', 'Unnamed: 1', 'Unnamed: 2', 'Unnamed: 3'], dtype='str')
>>> df = pd.read_excel(path, sheet_name="Data")
... 
... print(df.head())
... print(df.columns)
... 
   id  age  gender  hhi  nps  ...  intercom  shopping  skills  users  satisfaction
0   1    1       1    2    1  ...         1         0       1      0             5
1   2    2       1    2    5  ...         0         0       1      0             1
2   3    2       0    4    1  ...         0         0       2      0             1
3   4    4       0    3    9  ...         0         0       8      1             5
4   5    1       0    2    5  ...         0         0       1      0             1

[5 rows x 18 columns]
Index(['id', 'age', 'gender', 'hhi', 'nps', 'app', 'alexa', 'bluetooth',
       'music', 'alarm', 'search', 'flash', 'smart', 'intercom', 'shopping',
       'skills', 'users', 'satisfaction'],
      dtype='str')
>>> from scipy import stats
... 
... # create groups
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 


>>> 
>>> 
>>> from scipy import stats
... 
... # create groups
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
>>> 
>>> t_stat, p_value = stats.ttest_ind(advanced, basic, equal_var=False)
... 
... print(t_stat)
... print(p_value)
... 
5.973807895694745
6.73013120107126e-09
>>> import numpy as np
... 
... def ci(data):
...     mean = np.mean(data)
...     se = stats.sem(data)
...     ci = stats.t.interval(0.95, len(data)-1, loc=mean, scale=se)
...     return mean, ci
... 
... basic_mean, basic_ci = ci(basic)
... adv_mean, adv_ci = ci(advanced)
... 
... print("Basic:", basic_mean, basic_ci)
... print("Advanced:", adv_mean, adv_ci)
... 
Basic: 2.881188118811881 (np.float64(2.629567223771613), np.float64(3.132809013852149))
Advanced: 4.0 (np.float64(3.728625209253733), np.float64(4.2713747907462665))
>>> print(t_stat, p_value)
... print(basic_mean, basic_ci)
... print(adv_mean, adv_ci)
... 
5.973807895694745 6.73013120107126e-09
2.881188118811881 (np.float64(2.629567223771613), np.float64(3.132809013852149))
4.0 (np.float64(3.728625209253733), np.float64(4.2713747907462665))
>>> import statsmodels.api as sm
... 
... # select variables
... X = df[['skills', 'smart', 'music', 'age']]
... y = df['satisfaction']
... 
... # add constant (required)
... X = sm.add_constant(X)
... 
... # run model
... model = sm.OLS(y, X).fit()
... 
... print(model.summary())
... 
                            OLS Regression Results                            
==============================================================================
Dep. Variable:           satisfaction   R-squared:                       0.108
Model:                            OLS   Adj. R-squared:                  0.097
Method:                 Least Squares   F-statistic:                     9.678
Date:                Tue, 24 Feb 2026   Prob (F-statistic):           2.12e-07
Time:                        10:58:36   Log-Likelihood:                -631.43
No. Observations:                 325   AIC:                             1273.
Df Residuals:                     320   BIC:                             1292.
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.8047      0.387      4.667      0.000       1.044       2.565
skills         0.3456      0.073      4.713      0.000       0.201       0.490
smart          0.2686      0.270      0.994      0.321      -0.263       0.800
music          0.2718      0.321      0.846      0.398      -0.360       0.904
age            0.2551      0.086      2.971      0.003       0.086       0.424
==============================================================================
Omnibus:                     2019.796   Durbin-Watson:                   1.862
Prob(Omnibus):                  0.000   Jarque-Bera (JB):               38.961
Skew:                          -0.028   Prob(JB):                     3.47e-09
Kurtosis:                       1.305   Cond. No.                         18.0
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
>>> print(model.summary())
                            OLS Regression Results                            
==============================================================================
Dep. Variable:           satisfaction   R-squared:                       0.108
Model:                            OLS   Adj. R-squared:                  0.097
Method:                 Least Squares   F-statistic:                     9.678
Date:                Tue, 24 Feb 2026   Prob (F-statistic):           2.12e-07
Time:                        10:58:48   Log-Likelihood:                -631.43
No. Observations:                 325   AIC:                             1273.
Df Residuals:                     320   BIC:                             1292.
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.8047      0.387      4.667      0.000       1.044       2.565
skills         0.3456      0.073      4.713      0.000       0.201       0.490
smart          0.2686      0.270      0.994      0.321      -0.263       0.800
music          0.2718      0.321      0.846      0.398      -0.360       0.904
age            0.2551      0.086      2.971      0.003       0.086       0.424
==============================================================================
Omnibus:                     2019.796   Durbin-Watson:                   1.862
Prob(Omnibus):                  0.000   Jarque-Bera (JB):               38.961
Skew:                          -0.028   Prob(JB):                     3.47e-09
Kurtosis:                       1.305   Cond. No.                         18.0
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
>>> print(model.summary())
                            OLS Regression Results                            
==============================================================================
Dep. Variable:           satisfaction   R-squared:                       0.108
Model:                            OLS   Adj. R-squared:                  0.097
Method:                 Least Squares   F-statistic:                     9.678
Date:                Tue, 24 Feb 2026   Prob (F-statistic):           2.12e-07
Time:                        10:58:50   Log-Likelihood:                -631.43
No. Observations:                 325   AIC:                             1273.
Df Residuals:                     320   BIC:                             1292.
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.8047      0.387      4.667      0.000       1.044       2.565
skills         0.3456      0.073      4.713      0.000       0.201       0.490
smart          0.2686      0.270      0.994      0.321      -0.263       0.800
music          0.2718      0.321      0.846      0.398      -0.360       0.904
age            0.2551      0.086      2.971      0.003       0.086       0.424
==============================================================================
Omnibus:                     2019.796   Durbin-Watson:                   1.862
Prob(Omnibus):                  0.000   Jarque-Bera (JB):               38.961
Skew:                          -0.028   Prob(JB):                     3.47e-09
Kurtosis:                       1.305   Cond. No.                         18.0
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
>>> print(df[['satisfaction', 'skills', 'age']].corr())
              satisfaction    skills       age
satisfaction      1.000000  0.282483  0.202602
skills            0.282483  1.000000  0.172358
age               0.202602  0.172358  1.000000
>>> corr = df[['satisfaction', 'skills', 'age', 'smart', 'music']].corr()
... print(corr)
... 
              satisfaction    skills       age     smart     music
satisfaction      1.000000  0.282483  0.202602  0.041253  0.020328
skills            0.282483  1.000000  0.172358  0.077729 -0.096108
age               0.202602  0.172358  1.000000 -0.122554  0.090153
smart             0.041253  0.077729 -0.122554  1.000000 -0.303397
music             0.020328 -0.096108  0.090153 -0.303397  1.000000
>>> 
>>> print(df[['satisfaction', 'skills', 'age', 'smart', 'music']].corr())
              satisfaction    skills       age     smart     music
satisfaction      1.000000  0.282483  0.202602  0.041253  0.020328
skills            0.282483  1.000000  0.172358  0.077729 -0.096108
age               0.202602  0.172358  1.000000 -0.122554  0.090153
smart             0.041253  0.077729 -0.122554  1.000000 -0.303397
music             0.020328 -0.096108  0.090153 -0.303397  1.000000
>>> corr.round(2)
              satisfaction  skills   age  smart  music
satisfaction          1.00    0.28  0.20   0.04   0.02
skills                0.28    1.00  0.17   0.08  -0.10
age                   0.20    0.17  1.00  -0.12   0.09
smart                 0.04    0.08 -0.12   1.00  -0.30
music                 0.02   -0.10  0.09  -0.30   1.00
>>> corr.round(2).to_csv("correlation_matrix.csv")
>>> 
>>> 
>>> corr.round(2).to_csv("correlation_matrix.csv")
>>> correlation_matrix.csv
Traceback (most recent call last):
  File "<python-input-30>", line 1, in <module>
    correlation_matrix.csv
    ^^^^^^^^^^^^^^^^^^
NameError: name 'correlation_matrix' is not defined
>>> correlation_matrix.csv
Traceback (most recent call last):
  File "<python-input-31>", line 1, in <module>
    correlation_matrix.csv
    ^^^^^^^^^^^^^^^^^^
NameError: name 'correlation_matrix' is not defined
>>> import os
... print(os.listdir())
... 
['.Rhistory', '.config', 'Music', '.cursor', '.zprofile.pysave', 'quiz #1 (16th)', 'PSY151. test 1. back up.docx', 'review for week 3', '.DS_Store', 'source_library.csv', '.CFUserTextEncoding', 'Self-evaluation. word doc.pdf', 'this is it.docx', '.local', 'Self-evaluation. word doc.pdf...pdf', 'Creative Cloud Files', 'Pictures', '.zprofile', 'Self-evaluation. word doc.final.pdf', '.zsh_history', 'Desktop', 'Library', '.zprofileecho', 'HW 2: Qualitative Data Analysis', 'source_library', 'tutor work (1.0)', '.cups', 'week 6 (2.0)', 'Public', 'Back up, 2.docx', 'HW 2: Qualitative Report, first cycle coding', 'VR_data1.csv', '.idlerc', 'review for quiz #1', '.RData', '.sh_history', 'questions for (quiz #1)', 'Users', 'Movies', 'Applications', 'tristanpolard.Rproj', '.Trash', 'PSY151_Exam1_S24.pdf.#1.pdf', 'Documents', 'week 7, hw', 'Self-evaluation. word doc.final copy.pdf', '.vscode', 'Self-evaluation.pdf.final.pdf', 'PSY151_Exam1_S24.pdf.#3.pdf', 'Self-evaluation. word doc.pdf..pdf', 'Self-evaluation. final pdf .pdf', 'homework week 3', 'Downloads', '.python_history', '.Rproj.user', 'Week 7', '.zsh_sessions', 'week 6', 'tutor work', 'week 8', 'correlation_matrix.csv', 'work show (week 5)']
>>> correlation_matrix.csv
Traceback (most recent call last):
  File "<python-input-33>", line 1, in <module>
    correlation_matrix.csv
    ^^^^^^^^^^^^^^^^^^
NameError: name 'correlation_matrix' is not defined
>>> exit()
tristanpolard@Tristans-MacBook-Air ~ % correlation_matrix.csv
zsh: command not found: correlation_matrix.csv
tristanpolard@Tristans-MacBook-Air ~ % HW_3_UX_March 6
zsh: command not found: HW_3_UX_March
tristanpolard@Tristans-MacBook-Air ~ % correlation_matrix.csv
zsh: command not found: correlation_matrix.csv
tristanpolard@Tristans-MacBook-Air ~ % correlation_matrix.csv
zsh: command not found: correlation_matrix.csv
tristanpolard@Tristans-MacBook-Air ~ % open correlation_matrix.csv
tristanpolard@Tristans-MacBook-Air ~ % open correlation_matrix.csv
tristanpolard@Tristans-MacBook-Air ~ % import matplotlib.pyplot as plt

# create groups
basic = df[df['skills'] == 1]['satisfaction']
advanced = df[df['skills'] > 1]['satisfaction']

# means
means = [basic.mean(), advanced.mean()]

# labels
labels = ['Basic (1 skill)', 'Advanced (>1 skills)']

# plot
plt.bar(labels, means)
plt.ylabel("Average Satisfaction")
plt.title("Satisfaction by Skill Usage")
plt.show()
function> 
function> 
function> 
function> 
function> import matplotlib.pyplot as plt
zsh: command not found: import
zsh: command not found: #
zsh: = not found
tristanpolard@Tristans-MacBook-Air ~ % import matplotlib.pyplot as plt
zsh: command not found: import
tristanpolard@Tristans-MacBook-Air ~ % python3
Python 3.14.3 (v3.14.3:323c59a5e34, Feb  3 2026, 11:41:37) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib.pyplot as plt
... 
... # create groups
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
... # means
... means = [basic.mean(), advanced.mean()]
... 
... # labels
... labels = ['Basic (1 skill)', 'Advanced (>1 skills)']
... 
... # plot
... plt.bar(labels, means)
... plt.ylabel("Average Satisfaction")
... plt.title("Satisfaction by Skill Usage")
... plt.show()
... 
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    import matplotlib.pyplot as plt
ModuleNotFoundError: No module named 'matplotlib'
>>> exit()
tristanpolard@Tristans-MacBook-Air ~ % pip3 install matplotlib
Collecting matplotlib
  Downloading matplotlib-3.10.8-cp314-cp314-macosx_11_0_arm64.whl.metadata (52 kB)
Collecting contourpy>=1.0.1 (from matplotlib)
  Downloading contourpy-1.3.3-cp314-cp314-macosx_11_0_arm64.whl.metadata (5.5 kB)
Collecting cycler>=0.10 (from matplotlib)
  Downloading cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
Collecting fonttools>=4.22.0 (from matplotlib)
  Downloading fonttools-4.61.1-cp314-cp314-macosx_10_15_universal2.whl.metadata (114 kB)
Collecting kiwisolver>=1.3.1 (from matplotlib)
  Downloading kiwisolver-1.4.9-cp314-cp314-macosx_11_0_arm64.whl.metadata (6.3 kB)
Requirement already satisfied: numpy>=1.23 in /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages (from matplotlib) (2.4.2)
Requirement already satisfied: packaging>=20.0 in /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages (from matplotlib) (26.0)
Collecting pillow>=8 (from matplotlib)
  Downloading pillow-12.1.1-cp314-cp314-macosx_11_0_arm64.whl.metadata (8.8 kB)
Collecting pyparsing>=3 (from matplotlib)
  Downloading pyparsing-3.3.2-py3-none-any.whl.metadata (5.8 kB)
Requirement already satisfied: python-dateutil>=2.7 in /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages (from matplotlib) (2.9.0.post0)
Requirement already satisfied: six>=1.5 in /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages (from python-dateutil>=2.7->matplotlib) (1.17.0)
Downloading matplotlib-3.10.8-cp314-cp314-macosx_11_0_arm64.whl (8.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 MB 19.7 MB/s  0:00:00
Downloading contourpy-1.3.3-cp314-cp314-macosx_11_0_arm64.whl (273 kB)
Downloading cycler-0.12.1-py3-none-any.whl (8.3 kB)
Downloading fonttools-4.61.1-cp314-cp314-macosx_10_15_universal2.whl (2.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.8/2.8 MB 25.6 MB/s  0:00:00
Downloading kiwisolver-1.4.9-cp314-cp314-macosx_11_0_arm64.whl (64 kB)
Downloading pillow-12.1.1-cp314-cp314-macosx_11_0_arm64.whl (4.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.7/4.7 MB 17.1 MB/s  0:00:00
Downloading pyparsing-3.3.2-py3-none-any.whl (122 kB)
Installing collected packages: pyparsing, pillow, kiwisolver, fonttools, cycler, contourpy, matplotlib
Successfully installed contourpy-1.3.3 cycler-0.12.1 fonttools-4.61.1 kiwisolver-1.4.9 matplotlib-3.10.8 pillow-12.1.1 pyparsing-3.3.2
tristanpolard@Tristans-MacBook-Air ~ % python3
Python 3.14.3 (v3.14.3:323c59a5e34, Feb  3 2026, 11:41:37) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import matplotlib.pyplot as plt
... 
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
... means = [basic.mean(), advanced.mean()]
... labels = ['Basic (1 skill)', 'Advanced (>1 skills)']
... 
... plt.bar(labels, means)
... plt.ylabel("Average Satisfaction")
... plt.title("Satisfaction by Skill Usage")
... plt.show()
... 

Matplotlib is building the font cache; this may take a moment.
Traceback (most recent call last):
  File "<python-input-0>", line 3, in <module>
    basic = df[df['skills'] == 1]['satisfaction']
            ^^
NameError: name 'df' is not defined
>>> 
>>> import pandas as pd
... 
... df = pd.read_excel("/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"\
)
... 
>>> import matplotlib.pyplot as plt
>>> basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
... means = [basic.mean(), advanced.mean()]
... labels = ['Basic (1 skill)', 'Advanced (>1 skills)']
... 
... plt.bar(labels, means)
... plt.ylabel("Average Satisfaction")
... plt.title("Satisfaction by Skill Usage")
... plt.show()
... 
Traceback (most recent call last):
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/core/indexes/base.py", line 3641, in get_loc
    return self._engine.get_loc(casted_key)
           ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
  File "pandas/_libs/index.pyx", line 168, in pandas._libs.index.IndexEngine.get_loc
  File "pandas/_libs/index.pyx", line 197, in pandas._libs.index.IndexEngine.get_loc
  File "pandas/_libs/hashtable_class_helper.pxi", line 7668, in pandas._libs.hashtable.PyObjectHashTable.get_item
  File "pandas/_libs/hashtable_class_helper.pxi", line 7676, in pandas._libs.hashtable.PyObjectHashTable.get_item
KeyError: 'skills'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    basic = df[df['skills'] == 1]['satisfaction']
               ~~^^^^^^^^^^
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/core/frame.py", line 4378, in __getitem__
    indexer = self.columns.get_loc(key)
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/core/indexes/base.py", line 3648, in get_loc
    raise KeyError(key) from err
KeyError: 'skills'
>>> mport pandas as pd
... 
... path = "/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"
... df = pd.read_excel(path, sheet_name="Data")
... 
... print(df.columns.tolist())
... 
  File "<python-input-5>", line 1
    mport pandas as pd
    ^^^^^
SyntaxError: invalid syntax. Did you mean 'import'?
>>> import pandas as pd
>>> import pandas as pd
... 
... path = "/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"
... df = pd.read_excel(path, sheet_name="Data")
... 
... print(df.columns.tolist())
... 
['id', 'age', 'gender', 'hhi', 'nps', 'app', 'alexa', 'bluetooth', 'music', 'alarm', 'search', 'flash', 'smart', 'intercom', 'shopping', 'skills', 'users', 'satisfaction']
>>> ['id', 'age', 'skills', 'satisfaction', ...]
['id', 'age', 'skills', 'satisfaction', Ellipsis]
>>> import matplotlib.pyplot as plt
... 
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
... means = [basic.mean(), advanced.mean()]
... labels = ['Basic (1 skill)', 'Advanced (>1 skills)']
... 
... plt.bar(labels, means)
... plt.ylabel("Average Satisfaction")
... plt.title("Satisfaction by Skill Usage")
... plt.show()
... 

>>> 
>>> smart_users = df[df['smart'] == 1]['satisfaction']
... non_smart = df[df['smart'] == 0]['satisfaction']
... 
... means2 = [smart_users.mean(), non_smart.mean()]
... labels2 = ['Smart users', 'Non-smart users']
... 
... plt.bar(labels2, means2)
... plt.ylabel("Average Satisfaction")
... plt.title("Satisfaction by Smart Home Usage")
... plt.show()
... 
>>> plt.hist(df['satisfaction'])
... plt.xlabel("Satisfaction")
... plt.ylabel("Count")
... plt.title("Distribution of Satisfaction")
... plt.show()
... 
>>> import pandas as pd
... 
... path = "/Users/tristanpolard/Downloads/HW4_EchoSurveyData.xlsx"
... df = pd.read_excel(path, sheet_name="Data")
... 
>>> 
>>> from scipy import stats
... 
... # create groups
... basic = df[df['skills'] == 1]['satisfaction']
... advanced = df[df['skills'] > 1]['satisfaction']
... 
... # t-test
... t_stat, p_value = stats.ttest_ind(advanced, basic, equal_var=False)
... 
... print("t-stat:", t_stat)
... print("p-value:", p_value)
... 


t-stat: 5.973807895694745
p-value: 6.73013120107126e-09
>>> 
>>> 
>>> import numpy as np
... 
... def ci(data):
...     mean = np.mean(data)
...     se = stats.sem(data)
...     ci = stats.t.interval(0.95, len(data)-1, loc=mean, scale=se)
...     return mean, ci
... 
... basic_mean, basic_ci = ci(basic)
... adv_mean, adv_ci = ci(advanced)
... 
... print("Basic:", basic_mean, basic_ci)
... print("Advanced:", adv_mean, adv_ci)
... 
Basic: 2.881188118811881 (np.float64(2.629567223771613), np.float64(3.132809013852149))
Advanced: 4.0 (np.float64(3.728625209253733), np.float64(4.2713747907462665))
>>> smart_users = df[df['smart'] == 1]['satisfaction']
... non_smart = df[df['smart'] == 0]['satisfaction']
... 
... t_stat2, p_value2 = stats.ttest_ind(smart_users, non_smart, equal_var=False\
)
... 
... print("Smart vs Non-smart p-value:", p_value2)
... 
Smart vs Non-smart p-value: 0.4606944852571955
>>> import statsmodels.api as sm
... 
... # predictors
... X = df[['skills', 'smart', 'music', 'age']]
... y = df['satisfaction']
... 
... # add constant
... X = sm.add_constant(X)
... 
... # run model
... model = sm.OLS(y, X).fit()
... 
... print(model.summary())
... 

                            OLS Regression Results                            
==============================================================================
Dep. Variable:           satisfaction   R-squared:                       0.108
Model:                            OLS   Adj. R-squared:                  0.097
Method:                 Least Squares   F-statistic:                     9.678
Date:                Tue, 24 Feb 2026   Prob (F-statistic):           2.12e-07
Time:                        11:55:40   Log-Likelihood:                -631.43
No. Observations:                 325   AIC:                             1273.
Df Residuals:                     320   BIC:                             1292.
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.8047      0.387      4.667      0.000       1.044       2.565
skills         0.3456      0.073      4.713      0.000       0.201       0.490
smart          0.2686      0.270      0.994      0.321      -0.263       0.800
music          0.2718      0.321      0.846      0.398      -0.360       0.904
age            0.2551      0.086      2.971      0.003       0.086       0.424
==============================================================================
Omnibus:                     2019.796   Durbin-Watson:                   1.862
Prob(Omnibus):                  0.000   Jarque-Bera (JB):               38.961
Skew:                          -0.028   Prob(JB):                     3.47e-09
Kurtosis:                       1.305   Cond. No.                         18.0
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
>>> 
>>> corr = df[['satisfaction', 'skills', 'age', 'smart', 'music']].corr()
... print(corr.round(2))
... 
              satisfaction  skills   age  smart  music
satisfaction          1.00    0.28  0.20   0.04   0.02
skills                0.28    1.00  0.17   0.08  -0.10
age                   0.20    0.17  1.00  -0.12   0.09
smart                 0.04    0.08 -0.12   1.00  -0.30
music                 0.02   -0.10  0.09  -0.30   1.00
>>> smart vs non-smart
  File "<python-input-23>", line 1
    smart vs non-smart
          ^^
SyntaxError: invalid syntax. Did you mean 'is'?
>>> smart_users = df[df['smart'] == 1]['satisfaction']
... non_smart = df[df['smart'] == 0]['satisfaction']
... 
... t_stat2, p_value2 = stats.ttest_ind(smart_users, non_smart, equal_var=False\
)
... 
... print("t-stat:", t_stat2)
... print("p-value:", p_value2)
... 
t-stat: 0.7415886661651645
p-value: 0.4606944852571955
>>> from scipy.stats import f_oneway
... 
... # group by age categories
... group1 = df[df['age'] == 1]['satisfaction']
... group2 = df[df['age'] == 2]['satisfaction']
... group3 = df[df['age'] == 3]['satisfaction']
... group4 = df[df['age'] == 4]['satisfaction']
... 
... f_stat, p_value = f_oneway(group1, group2, group3, group4)
... 
... print("F-stat:", f_stat)
... print("p-value:", p_value)
... 
F-stat: 12.546528867902868
p-value: 9.3820600591633e-08
>>> [ ]
[]
>>> import pandas as pd
... 
... # load your file (you need to upload it first)
... df = pd.read_excel("/content/HW4_EchoSurveyData.xlsx")
... 
... print(df.head())
... 
Traceback (most recent call last):
  File "<python-input-27>", line 4, in <module>
    df = pd.read_excel("/content/HW4_EchoSurveyData.xlsx")
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/io/excel/_base.py", line 481, in read_excel
    io = ExcelFile(
        io,
    ...<2 lines>...
        engine_kwargs=engine_kwargs,
    )
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/io/excel/_base.py", line 1604, in __init__
    ext = inspect_excel_format(
        content_or_path=path_or_buffer, storage_options=storage_options
    )
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/io/excel/_base.py", line 1452, in inspect_excel_format
    with get_handle(
         ~~~~~~~~~~^
        content_or_path, "rb", storage_options=storage_options, is_text=False
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    ) as handle:
    ^
  File "/Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/pandas/io/common.py", line 935, in get_handle
    handle = open(handle, ioargs.mode)
FileNotFoundError: [Errno 2] No such file or directory: '/content/HW4_EchoSurveyData.xlsx'
>>> import os
... print(os.listdir('/content'))
... 
Traceback (most recent call last):
  File "<python-input-28>", line 2, in <module>
    print(os.listdir('/content'))
          ~~~~~~~~~~^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: '/content'
>>> echo "# HW_4_ES" >> README.md
... git init
... git add README.md
... git commit -m "first commit"
... git branch -M main
... git remote add origin https://github.com/tristanpolard-bit/HW_4_ES.git
... git push -u origin main
... 
  File "<python-input-29>", line 1
    echo "# HW_4_ES" >> README.md
         ^^^^^^^^^^^
SyntaxError: invalid syntax
