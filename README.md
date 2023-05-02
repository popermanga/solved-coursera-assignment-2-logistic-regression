Download Link: https://assignmentchef.com/product/solved-coursera-assignment-2-logistic-regression
<br>
In this exercise, you will implement logistic regression and apply it to two different datasets. Before starting on the programming exercise, we strongly recommend watching the video lectures and completing the review questions for the associated topics.

To get started with the exercise, you will need to download the starter code and unzip its contents to the directory where you wish to complete the exercise. If needed, use the cd command in Octave/MATLAB to change to this directory before starting this exercise.

You can also find instructions for installing Octave/MATLAB in the “Environment Setup Instructions” of the course website.

<h2>Files included in this exercise</h2>

ex2.m – Octave/MATLAB script that steps you through the exercise ex2 reg.m – Octave/MATLAB script for the later parts of the exercise ex2data1.txt – Training set for the first half of the exercise ex2data2.txt – Training set for the second half of the exercise submit.m – Submission script that sends your solutions to our servers mapFeature.m – Function to generate polynomial features plotDecisionBoundary.m – Function to plot classifier’s decision boundary

[<em>?</em>] plotData.m – Function to plot 2D classification data [<em>?</em>] sigmoid.m – Sigmoid Function

[<em>?</em>] costFunction.m – Logistic Regression Cost Function

[<em>?</em>] predict.m – Logistic Regression Prediction Function

[<em>?</em>] costFunctionReg.m – Regularized Logistic Regression Cost

<em>? </em>indicates files you will need to complete

Throughout the exercise, you will be using the scripts ex2.m and ex2 reg.m. These scripts set up the dataset for the problems and make calls to functions that you will write. You do not need to modify either of them. You are only required to modify functions in other files, by following the instructions in this assignment.

<h2>Where to get help</h2>

The exercises in this course use Octave<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> or MATLAB, a high-level programming language well-suited for numerical computations. If you do not have Octave or MATLAB installed, please refer to the installation instructions in the “Environment Setup Instructions” of the course website.

At the Octave/MATLAB command line, typing help followed by a function name displays documentation for a built-in function. For example, help plot will bring up help information for plotting. Further documentation for Octave functions can be found at the <a href="https://www.gnu.org/software/octave/doc/interpreter/">Octave documentation pages</a><a href="https://www.gnu.org/software/octave/doc/interpreter/">.</a> MATLAB documentation can be found at the <a href="https://www.mathworks.com/help/matlab/?refresh=true">MATLAB documentation pages</a><a href="https://www.mathworks.com/help/matlab/?refresh=true">.</a>

We also strongly encourage using the online <strong>Discussions </strong>to discuss exercises with other students. However, do not look at any source code written by others or share your source code with others.

<h1>1             Logistic Regression</h1>

In this part of the exercise, you will build a logistic regression model to predict whether a student gets admitted into a university.

Suppose that you are the administrator of a university department and you want to determine each applicant’s chance of admission based on their results on two exams. You have historical data from previous applicants that you can use as a training set for logistic regression. For each training example, you have the applicant’s scores on two exams and the admissions decision.

Your task is to build a classification model that estimates an applicant’s probability of admission based the scores from those two exams. This outline and the framework code in ex2.m will guide you through the exercise.

<h2>1.1            Visualizing the data</h2>

Before starting to implement any learning algorithm, it is always good to visualize the data if possible. In the first part of ex2.m, the code will load the data and display it on a 2-dimensional plot by calling the function plotData.

You will now complete the code in plotData so that it displays a figure like Figure 1, where the axes are the two exam scores, and the positive and negative examples are shown with different markers.

Figure 1: Scatter plot of training data

To help you get more familiar with plotting, we have left plotData.m empty so you can try to implement it yourself. However, this is an <em>optional (ungraded) exercise</em>. We also provide our implementation below so you can copy it or refer to it. If you choose to copy our example, make sure you learn what each of its commands is doing by consulting the Octave/MATLAB documentation.

<table width="0">

 <tbody>

  <tr>

   <td width="527">% Find Indices of Positive and Negative Examples pos = find(y==1); neg = find(y == 0);% Plot Examples plot(X(pos, 1), X(pos, 2), ‘k+’,’LineWidth’, 2, …‘MarkerSize’, 7); plot(X(neg, 1), X(neg, 2), ‘ko’, ‘MarkerFaceColor’, ‘y’, …‘MarkerSize’, 7);</td>

  </tr>

 </tbody>

</table>

<h2>1.2            Implementation</h2>

<h3>1.2.1         Warmup exercise: sigmoid function</h3>

Before you start with the actual cost function, recall that the logistic regression hypothesis is defined as:

<em>h<sub>θ</sub></em>(<em>x</em>) = <em>g</em>(<em>θ<sup>T</sup>x</em>)<em>,</em>

where function <em>g </em>is the sigmoid function. The sigmoid function is defined as:

<em>.</em>

Your first step is to implement this function in sigmoid.m so it can be called by the rest of your program. When you are finished, try testing a few values by calling sigmoid(x) at the Octave/MATLAB command line. For large positive values of x, the sigmoid should be close to 1, while for large negative values, the sigmoid should be close to 0. Evaluating sigmoid(0) should give you exactly 0.5. Your code should also work with vectors and matrices. <strong>For a matrix, your function should perform the sigmoid function on every element.</strong>

You can submit your solution for grading by typing submit at the Octave/MATLAB command line. The submission script will prompt you for your login e-mail and submission token and ask you which files you want to submit. You can obtain a submission token from the web page for the assignment.

<em>You should now submit your solutions.</em>

<h3>1.2.2         Cost function and gradient</h3>

Now you will implement the cost function and gradient for logistic regression.

Complete the code in costFunction.m to return the cost and gradient.

Recall that the cost function in logistic regression is

<em>,</em>

and the gradient of the cost is a vector of the same length as <em>θ </em>where the <em>j</em><sup>th </sup>element (for <em>j </em>= 0<em>,</em>1<em>,…,n</em>) is defined as follows:

Note that while this gradient looks identical to the linear regression gradient, the formula is actually different because linear and logistic regression have different definitions of <em>h<sub>θ</sub></em>(<em>x</em>).

Once you are done, ex2.m will call your costFunction using the initial parameters of <em>θ</em>. You should see that the cost is about 0.693.

<em>You should now submit your solutions.</em>

<h3>1.2.3         Learning parameters using fminunc</h3>

In the previous assignment, you found the optimal parameters of a linear regression model by implementing gradent descent. You wrote a cost function and calculated its gradient, then took a gradient descent step accordingly. This time, instead of taking gradient descent steps, you will use an Octave/MATLAB built-in function called fminunc.

Octave/MATLAB’s fminunc is an optimization solver that finds the minimum of an unconstrained<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> function. For logistic regression, you want to optimize the cost function <em>J</em>(<em>θ</em>) with parameters <em>θ</em>.

Concretely, you are going to use fminunc to find the best parameters <em>θ </em>for the logistic regression cost function, given a fixed dataset (of <em>X </em>and <em>y </em>values). You will pass to fminunc the following inputs:

<ul>

 <li>The initial values of the parameters we are trying to optimize.</li>

 <li>A function that, when given the training set and a particular <em>θ</em>, computes the logistic regression cost and gradient with respect to <em>θ </em>for the dataset (<em>X</em>, <em>y</em>)</li>

</ul>

In ex2.m, we already have code written to call fminunc with the correct arguments.

<table width="0">

 <tbody>

  <tr>

   <td width="527">% Set options for fminunc options = optimset(‘GradObj’, ‘on’, ‘MaxIter’, 400);% Run fminunc to obtain the optimal theta % This function will return theta and the cost [theta, cost] = …fminunc(@(t)(costFunction(t, X, y)), initialtheta, options);</td>

  </tr>

 </tbody>

</table>

In this code snippet, we first defined the options to be used with fminunc. Specifically, we set the GradObj option to on, which tells fminunc that our function returns both the cost and the gradient. This allows fminunc to use the gradient when minimizing the function. Furthermore, we set the MaxIter option to 400, so that fminunc will run for at most 400 steps before it terminates.

To specify the actual function we are minimizing, we use a “short-hand” for specifying functions with the @(t) ( costFunction(t, X, y) ) . This creates a function, with argument t, which calls your costFunction. This allows us to wrap the costFunction for use with fminunc.

If you have completed the costFunction correctly, fminunc will converge on the right optimization parameters and return the final values of the cost and <em>θ</em>. Notice that by using fminunc, you did not have to write any loops yourself, or set a learning rate like you did for gradient descent. This is all done by fminunc: you only needed to provide a function calculating the cost and the gradient.

Once fminunc completes, ex2.m will call your costFunction function using the optimal parameters of <em>θ</em>. You should see that the cost is about

0.203.

This final <em>θ </em>value will then be used to plot the decision boundary on the training data, resulting in a figure similar to Figure 2. We also encourage you to look at the code in plotDecisionBoundary.m to see how to plot such a boundary using the <em>θ </em>values.

<h3>1.2.4         Evaluating logistic regression</h3>

After learning the parameters, you can use the model to predict whether a particular student will be admitted. For a student with an Exam 1 score of 45 and an Exam 2 score of 85, you should expect to see an admission probability of 0.776.

Another way to evaluate the quality of the parameters we have found is to see how well the learned model predicts on our training set. In this




30             40             50             60             70             80             90             100

Exam 1 score

Figure 2: Training data with decision boundary

part, your task is to complete the code in predict.m. The predict function will produce “1” or “0” predictions given a dataset and a learned parameter vector <em>θ</em>.

After you have completed the code in predict.m, the ex2.m script will proceed to report the training accuracy of your classifier by computing the percentage of examples it got correct.

<em>You should now submit your solutions.</em>

<h1>2             Regularized logistic regression</h1>

In this part of the exercise, you will implement regularized logistic regression to predict whether microchips from a fabrication plant passes quality assurance (QA). During QA, each microchip goes through various tests to ensure it is functioning correctly.

Suppose you are the product manager of the factory and you have the test results for some microchips on two different tests. From these two tests, you would like to determine whether the microchips should be accepted or rejected. To help you make the decision, you have a dataset of test results on past microchips, from which you can build a logistic regression model.

You will use another script, ex2 reg.m to complete this portion of the exercise.

<h2>2.1            Visualizing the data</h2>

Similar to the previous parts of this exercise, plotData is used to generate a figure like Figure 3, where the axes are the two test scores, and the positive (<em>y </em>= 1, accepted) and negative (<em>y </em>= 0, rejected) examples are shown with different markers.

Figure 3: Plot of training data

Figure 3 shows that our dataset cannot be separated into positive and negative examples by a straight-line through the plot. Therefore, a straightforward application of logistic regression will not perform well on this dataset since logistic regression will only be able to find a linear decision boundary.

<h2>2.2            Feature mapping</h2>

One way to fit the data better is to create more features from each data point. In the provided function mapFeature.m, we will map the features into all polynomial terms of <em>x</em><sub>1 </sub>and <em>x</em><sub>2 </sub>up to the sixth power.

<table width="0">

 <tbody>

  <tr>

   <td width="25"></td>

   <td width="29">1 <em>x</em><sub>1 </sub><em>x</em><sub>2</sub><em>x</em>2<sub>1</sub></td>

   <td width="11"></td>

  </tr>

 </tbody>

</table>

           

mapFeature(

 3 

 <em>x</em><sub>1 </sub>

 … 





 <em>x</em>1<em>x</em>52  

<em>x</em>6<sub>2</sub>

As a result of this mapping, our vector of two features (the scores on two QA tests) has been transformed into a 28-dimensional vector. A logistic regression classifier trained on this higher-dimension feature vector will have a more complex decision boundary and will appear nonlinear when drawn in our 2-dimensional plot.

While the feature mapping allows us to build a more expressive classifier, it also more susceptible to overfitting. In the next parts of the exercise, you will implement regularized logistic regression to fit the data and also see for yourself how regularization can help combat the overfitting problem.

<h2>2.3            Cost function and gradient</h2>

Now you will implement code to compute the cost function and gradient for regularized logistic regression. Complete the code in costFunctionReg.m to return the cost and gradient.

Recall that the regularized cost function in logistic regression is

<em>.</em>

Note that you should not regularize the parameter <em>θ</em><sub>0</sub>. In <strong>Octave/MATLAB</strong>, recall that indexing starts from 1, hence, you should not be regularizing the theta(1) parameter (which corresponds to <em>θ</em><sub>0</sub>) in the code. The gradient of the cost function is a vector where the <em>j</em><sup>th </sup>element is defined as follows:

for <em>j </em>= 0

for <em>j </em>≥ 1

Once you are done, ex2 reg.m will call your costFunctionReg function using the initial value of <em>θ </em>(initialized to all zeros). You should see that the cost is about 0.693.

<em>You should now submit your solutions.</em>

<h3>2.3.1         Learning parameters using fminunc</h3>

Similar to the previous parts, you will use fminunc to learn the optimal parameters <em>θ</em>. If you have completed the cost and gradient for regularized logistic regression (costFunctionReg.m) correctly, you should be able to step through the next part of ex2 reg.m to learn the parameters <em>θ </em>using fminunc.

<h2>2.4            Plotting the decision boundary</h2>

To help you visualize the model learned by this classifier, we have provided the function plotDecisionBoundary.m which plots the (non-linear) decision boundary that separates the positive and negative examples. In plotDecisionBoundary.m, we plot the non-linear decision boundary by computing the classifier’s predictions on an evenly spaced grid and then and drew a contour plot of where the predictions change from <em>y </em>= 0 to <em>y </em>= 1.

After learning the parameters <em>θ</em>, the next step in ex reg.m will plot a decision boundary similar to Figure 4.

<h2>2.5            Optional (ungraded) exercises</h2>

In this part of the exercise, you will get to try out different regularization parameters for the dataset to understand how regularization prevents overfitting.

Notice the changes in the decision boundary as you vary <em>λ</em>. With a small <em>λ</em>, you should find that the classifier gets almost every training example correct, but draws a very complicated boundary, thus overfitting the data (Figure 5). This is not a good decision boundary: for example, it predicts that a point at <em>x </em>= (−0<em>.</em>25<em>,</em>1<em>.</em>5) is accepted (<em>y </em>= 1), which seems to be an incorrect decision given the training set.

With a larger <em>λ</em>, you should see a plot that shows an simpler decision boundary which still separates the positives and negatives fairly well. However, if <em>λ </em>is set to too high a value, you will not get a good fit and the decision boundary will not follow the data so well, thus underfitting the data (Figure

6).

<em>You do not need to submit any solutions for these optional (ungraded) exercises.</em>

lambda = 1

Figure 4: Training data with decision boundary (<em>λ </em>= 1)

lambda = 0

Figure 5: No regularization (Overfitting) (<em>λ </em>= 0)

lambda = 100

Figure 6: Too much regularization (Underfitting) (<em>λ </em>= 100)

<a href="#_ftnref1" name="_ftn1">[1]</a> Octave is a free alternative to MATLAB. For the programming exercises, you are free to use either Octave or MATLAB.

<a href="#_ftnref2" name="_ftn2">[2]</a> Constraints in optimization often refer to constraints on the parameters, for example, constraints that bound the possible values <em>θ </em>can take (e.g., <em>θ </em>≤ 1). Logistic regression does not have such constraints since <em>θ </em>is allowed to take any real value.