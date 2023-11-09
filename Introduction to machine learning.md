# Introduction to machine learning

Imperial Introductory course - Nov 2023

## What is machine learning?

Some common ideas of what machine learning are include:
- artifical intelligence
linear regressions
- "black box modeling" for example neural networks 
    though on this point there was that articale that coame about about using non black box methods
- predictions
-relationships between varibales
- teaching a machine to code (learning to code)-> LLMs for example Chat CPT, Generative AI for exampel to create art music, text, video games etc
- robotics

Three main areas:
LLMs, Generative AI, robotos -> complex AI, (Deep learning)
not in the scope of this lesson at the moment

Relationship between variable (more to do with unsupervised learning) 

Predications, linear regressions, "black box" (supervised machine learning)

but these are all under the umbrella of AI. Machine learning is often to do with making predictions, closer to the data, while AI is more generalised idea of thinking. Data science is an associated field with leads into more statistical methods, and more modern ideas we apply ML to make predictive models, inside machine learning, there is a specialised field called deep learning that uses specialised software etc often used with big data and trying to come out with more complex modeling. 

- statistical learning theory
introdycted in the late 1990s, became applied science in the 1990s with the invention and distribution of computers. 
this allowed us to learn structures and relationshuips in data, assign observstions to different clases, and make predictions based on current knowledge. 

Terms to be familiar with
- vector: a quantity in a multidimensinoal space. for example a point that has coordinates in a 3D space. 
- function: a mapping from one vector space (input) to another(output). for example a linear regression f(x)=2^x. the function is basically like a machine that you give it input and it produces some output. 
-optimisatoin: a procedure that attempts to find the minimum or maximum of a function. So given all inputs (x), what are the maximum and minimum outputs (y). So this is part of optimisation of trying to maximize and minimize outputs. figuring out where an input-output position is is the processes of optimisatoin. 
traingin models requires alot of potimisation 

- A machine has inputs and outputs
the input is known as the *feature vector (x)*
we are going to use that informatoin in order to produce an *output (y)*
the machine is like a function that maps the vector data
what we want to do with ML is to train with machine with data to provide good output mapping. 

need to think about types of data. four key types we need to be aware of:

***Categorical data*** 

- **Nominal data**: no obvious ordering of categories. 
Basically there is no numerical relationship between the values. 

for example classes of vertebrate: amphibian, bird, fish, mammal, reptile. 

When there are only two possible categories, the data is called dichotomous, or binary. 

- **Ordinal data**: There is a natural order for the categories
for example opinions on a Likert scale: strongly disagree, disagree, neutral, agree, strongly agree. 

***Quantitative data***
(numerical data from counts or measurements)
- **Discrete data**: can only take specified values (eg. number of children in a family - it is an integer)
-**Continuous data**: can take any value in an interval (eg. blood pressure)

Orange data mining: really great software to use learn machine learning.

Once installed, select New to create a new workflow
- on the left had side you have a pallet containing widgets.
- start with data: datasets
Drag that onto the page. When you add it they have built in datasets that you can work with. Click the dataset called *iris*. it is a 1936 dataset about anatomical measurements of flowers. 
very classical machine learning dataset, so a great place to start. 
- after selecting iris, you can close the popup. 
then drag a data table onto the page. now we want to connect hte datasets to the datatable. What we can do is select on of the nodes around the dataests and link it to the datatable. Now when you click the data table you can see the data. 
- there are 150 rows, where each row represents an individual observation. adn 5 columns, each column represents a variable. 
first of all what type of data is it - nominal and continous (can i include this in my github with  a hoover function that shows the answer when you click a button?) that would be pretty cool to include. 
 
 also to include for ewarlier, the orange program is great to learn concepts of machine learning, and then do the specific course in python after which is more detailed. there are also resources to do machine learning in other programs. 

 Inside the datatabel, the first column is shown in grey - this is something from the orange setup, and it has been identified that it is the *target*. so it is a prediction about what the iris label is. 
 feature vs varibale - slight difference, but most of the time its the same. 
 
 **unsupervised learning**

 unsuperivsed learning, you through away the iris (feature) category - so imaginge thta that category is hidden from us, so that each measruement is unlabelled. 
 what we want ot do is discover some kind of structure in that dataset often used to discvoer something new about the data that we have. for example, do we have multiple species of iris hidding in that data?
 some examples include:
 - dimensionality reduction eg principal component analysis
 - self organising maps: what is different between different datapoint
 - clustering: can we put this into subdata points

- latent class analysis: unsupervised strategy that lets you discover groupings based on the analysis (similar to clustering but the way its done is differet)

An example:

we have different nodes of different colour (x). We then give this as an input to the "machine" which produces an output that clusters the labers (y). here it has grouped the colours by similarities and has given me an output of how many there colours are in each group.  This is something known as **flat** clustering where it just reports the labels to me. 

can also be hierarchical (reporting a dendrogram of nested clusters)

feature-based clustering : takes as input the set of input feature vectors
distanace-based clusters: takes as input a matrix of distances that are calculated between each pair of input feature vectors eg. Euclidean distance. 
for example, what is the distance between the colours 

k-means clustering:
A feature-based technique for flat clustering. So it just reports cluster labels as an output. 
Requires a prior decision of the numnber of clusters (k) - in practice a good k for a given data set can sometimes be found by post-hoc analysis (e.g. silhouette score)

k-means clustering aims to partition n observations into k clusters, in which each observation belongs to the cluster with the nearest mean. 

In orange, educational and image analytics -> install them. 
this gives you acces to interactive k-means -> connect this to datasets. 

to change the data to only look at two clusters -> click on the square

k-means clustering algorithm:
1. initialise positions for k lcuster centroids (at random)
2. assignment step
3. update step 
4. check for convergence 

k-means is often fast in practice, but is heuristic method (rather than deterministic) so it is not guaranteed to find the global optimum . therefore advisable to re-run several times with different starting points. 
this is an example of an expectation maximisation approach

k-means excerise:
look at housing dataset
considering only numerical features, perdorm lk-means clustering. use the solhouette score to determine how many clusters the data appear to fall into.

**Supervised learning**
Here, labelled data are used to train the machine 
there are two types of predictions that we can make: 
y is a quantitative value: regression (estimate the response to a given input)
y is a categorical variable: classification (identify the class of a given input)

the first one is very similar to a linear regression - predict y from the features of x by fitting a linear function. 
fitting is an optimisation procedure e.g minimise the sum of squared errors.
x(input) -> y(output): the purpose is to make a prediction about the y value if i know what x is. 

linear regression example:
with the iris dataset, considering only iris virginica -> split the data into trainging and testing sets. 
use linerar regression to predict sepal length from petal length. 

in data sampler: replicable sampling -> chose the same random data. good for testing and replication. also choose stratify sample: trying to choose a ranodm sample that reflects the composition of the overall population -> good for unbalanced data. eg. males and females, try and sample in that proportion so that my training and testing data represent each other. 

polynomical regression: polunomical degree:1 straight line model. petal length -> sepal length there is a linear model between the two. we want to try to minimise the residuals (so the root mean square error -> reduce error bars)


## Day 2
Supervised learning: **classification**
the labels that we are trying to predict are classes
eg. animals in a group

Logistic regression
- it is an algorithm for classificatoin: considered binary classification with classess labelled 0 or 1. 
this is not a regression, but see above!

for our trainign data we can plot the probability that a particular value of x is labelled as class 1. 

orange: consusion matrix: send preidicts to it, cnSEE MORE clearly the corroect and incorrect predictions

adding more feeatures doesnt always make predictions better -~> it depends how consistent features are across different groups. 

Using a new datagroup called Kickstarter

- the first four columns are metadata, so we cannot use that to make predictions -> but the rest of the projects could be used to make predictions. 

taks 1: can you make predictions only using numerical categories. 
1. split the data into training and testing sets
2. predict fundded from numerical features
3. use a contingency table to examine te resilts
4. how could we make use of the type deatures which is a nominal data type. 

so we would first connect the datasets to select columns

If you number groups -> there is no relationship between them. so you are now giving them an order and a relationship

One-hot encoding is useful for converting a categorical variable into multiple binary featyres, which can be used in algorithsm that require numerical inputs. 

## What about non-linear classifications
Eg. measure blood pressure of patients -> Q: which will come back within three months will come back. We obviously think people with very low or very high BP will come back. 

How can we make use of a non-linear classification model?

Eg. you have a box full of shapes. one box is classified as yes, one box is classified as no. If we are now given shapes not in the box or different colours, how to we classify them?
- for example we can look at the link between the shape and colour and size
so in our example:
-  blue shapes are a yes. 
- if it is a small ellipse -> yes
this is constructing a decision tree if we think about probabilities and a step by step algorithm. but finding the optimal tree is hard. but its good to have a greedy algorithm which builds the tree step by step, optimising the resutls after each step. 

Summary here

# Evaluating performance and improving performance

How can we compare different ML methods for the same taks?
- we need performance metrics that measure the sinmilarity between the model's predictions and the ground truth
- these will be different depending on the type of model we are assessing

## Evaluating regression models
- you are probably familiar with the metric R^2. it is a way of assessing the fit between a linear model and the data. A good model has an R^2 value close to 1.
- You can also have a negative value if the model is very bad. 
- An example of R^2 to compare different regression models
- overfitting: it finds a position for every input, but it is not good for the training data. 

## Working with training data and validating
- hyper parameters: parameters of the algorithm itself: min number of instances in leaves -> the minimum number of end leaves, or for example limit the maximal tree depth 

- cross validation:
-small value k is used (5 maybe 10). and tarin on (k-1) subsets. 
- coefficient of determination: linear regression is better than the tree 98% of the time. 
- constant model : expect it to perform worse. expect ~0

https://davemcg.github.io/post/are-you-in-genomics-stop-using-roc-use-pr/
