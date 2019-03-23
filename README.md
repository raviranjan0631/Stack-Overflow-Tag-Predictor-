# Stack-Overflow-Tag-predictor
 Problem Statemtent Suggest the tags based on the content that was there in the question posted on Stackoverflow.
<h2> Source / useful links </h2>

Data Source : https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data <br>
Youtube : https://youtu.be/nNDqbUhtIRg <br>
Research paper : https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tagging-1.pdf <br>
Research paper : https://dl.acm.org/citation.cfm?id=2660970&dl=ACM&coll=DL

<h2> Real World / Business Objectives and Constraints </h2>

1. Predict as many tags as possible with high precision and recall.
2. Incorrect tags could impact customer experience on StackOverflow.
3. No strict latency constraints

<h3> Data Overview </h3>
Refer: https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data
<br>
All of the data is in 2 files: Train and Test.<br />
<pre>
<b>Train.csv</b> contains 4 columns: Id,Title,Body,Tags.<br />
<b>Test.csv</b> contains the same columns but without the Tags, which you are to predict.<br />
<b>Size of Train.csv</b> - 6.75GB<br />
<b>Size of Test.csv</b> - 2GB<br />
<b>Number of rows in Train.csv</b> = 6034195<br />
</pre>
The questions are randomized and contains a mix of verbose text sites as well as sites related to math and programming. The number of questions from each site may vary, and no filtering has been performed on the questions (such as closed questions).<br />
<br />
<h2>Example Data Point</h2>

<pre>
<b>Title</b>:  Implementing Boundary Value Analysis of Software Testing in a C++ program?
<b>Body </b>: <pre><code>
        #include&lt;
        iostream&gt;\n
        #include&lt;
        stdlib.h&gt;\n\n
        using namespace std;\n\n
        int main()\n
        {\n
                 int n,a[n],x,c,u[n],m[n],e[n][4];\n         
                 cout&lt;&lt;"Enter the number of variables";\n         cin&gt;&gt;n;\n\n         
                 cout&lt;&lt;"Enter the Lower, and Upper Limits of the variables";\n         
                 for(int y=1; y&lt;n+1; y++)\n         
                 {\n                 
                    cin&gt;&gt;m[y];\n                 
                    cin&gt;&gt;u[y];\n         
                 }\n         
                 for(x=1; x&lt;n+1; x++)\n         
                 {\n                 
                    a[x] = (m[x] + u[x])/2;\n         
                 }\n         
                 c=(n*4)-4;\n         
                 for(int a1=1; a1&lt;n+1; a1++)\n         
                 {\n\n             
                    e[a1][0] = m[a1];\n             
                    e[a1][1] = m[a1]+1;\n             
                    e[a1][2] = u[a1]-1;\n             
                    e[a1][3] = u[a1];\n         
                 }\n         
                 for(int i=1; i&lt;n+1; i++)\n         
                 {\n            
                    for(int l=1; l&lt;=i; l++)\n            
                    {\n                 
                        if(l!=1)\n                 
                        {\n                    
                            cout&lt;&lt;a[l]&lt;&lt;"\\t";\n                 
                        }\n            
                    }\n            
                    for(int j=0; j&lt;4; j++)\n            
                    {\n                
                        cout&lt;&lt;e[i][j];\n                
                        for(int k=0; k&lt;n-(i+1); k++)\n                
                        {\n                    
                            cout&lt;&lt;a[k]&lt;&lt;"\\t";\n               
                        }\n                
                        cout&lt;&lt;"\\n";\n            
                    }\n        
                 }    \n\n        
                 system("PAUSE");\n        
                 return 0;    \n
        }\n
        </code></pre>\n\n
        <p>The answer should come in the form of a table like</p>\n\n
        <pre><code>       
        1            50              50\n       
        2            50              50\n       
        99           50              50\n       
        100          50              50\n       
        50           1               50\n       
        50           2               50\n       
        50           99              50\n       
        50           100             50\n       
        50           50              1\n       
        50           50              2\n       
        50           50              99\n       
        50           50              100\n
        </code></pre>\n\n
        <p>if the no of inputs is 3 and their ranges are\n
        1,100\n
        1,100\n
        1,100\n
        (could be varied too)</p>\n\n
        <p>The output is not coming,can anyone correct the code or tell me what\'s wrong?</p>\n'
<b>Tags </b>: 'c++ c'
</pre>

<h1>Our Approach</h1>

1. We first downloaded the train and the test data from : https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data
2. As the dataset was too big(around 8Gb) we decided to take 100k data points to work with.
3. We saved the 100k points as a csv file. We did the EDA
4. Most frequent tag (i.e. c#) is used 331505 times.
5. Since some tags occur much more frequenctly than others, Micro-averaged F1-score is the appropriate metric for this probelm.
6. 384 Tags are used more than 100 times
7. 1 Tags are used more than 7000 times
8. Maximum number of tags per question: 5
9. Minimum number of tags per question: 1
10. Avg. number of tags per question: 2.883030
11. Most of the questions are having 2 or 3 tags

<h1>Summary</h1>
<p>Here is a Comparison of our models.</p>


<table style="width:100%">
  <tr>
    <th>No of Tags</th>
    <th>Model</th>
    <th>Precision</th>
     <th>Recall</th>
      <th>F1-measure</th>
      <th>Hyperparameter</th>
      
      
  </tr>
  <tr>
    <td>500</td>
    <td>Logistic Regression</td>
    <td>0.6464</td>
    <td>0.3602</td>
    <td>0.4626</td>
    <td>100</td>
  </tr>
   <tr>
    <td>500</td>
    <td>Linear SVM</td>
    <td>0.8033</td>
    <td>0.2022</td>
    <td>0.3231</td>
    <td>0.00001</td>
  </tr>
  </tr>
   <tr>
    <td>500</td>
    <td>Linear SVM</td>
    <td>0.8046</td>
    <td>0.2027</td>
    <td>0.3239</td>
    <td>0.0001</td>
  </tr>
  <tr>
    <td>5500</td>
    <td>Logistic Regression</td>
    <td>0.7205</td>
    <td>0.2298</td>
    <td> 0.3485</td>
    <td>0.00001</td>
  </tr>
</table> 
