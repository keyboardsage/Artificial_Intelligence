# What is it?

# Types?
Convolutional Neural Networks - Good for image recognition
Long Short-term Memory Networks - Good for speech recognition

# Important Parts
A neuron can be thought of a just a space to store a value
The **activation** is what is stored inside the neuron. Think of it as a float value ranging from 0 to 1.

The hidden layers are composed of 1 or more sets of neurons. The exact number of sets, or their cardinality is somewhat arbitrary and worth experimenting with to find the ideal structure.

So the process looks like this: Input Layer -> Hidden Layers -> Output Layer 

The idea is that some neurons firing/activated in one layer, will cause other neurons to fire/activate in the next layer if a **threshold** is met. So basically, how activiations in one layer produce activations in the next layer is what ultimately will bring about information processing.

**Learning** is getting the computer to set the ideal numbers to the weights and biases to solve the problem at hand.

# Example of Layers in Action
Suppose the goal is to identify a PNG file that is 28 x 28 pixels.

## Input
You can convert it to BMP, grayscale it, so the background is darked and pixels associated with the number itself are closer to white.  
Assign a float to each pixel. The whiter the pixel, the closer it is to 1.  
That is 784 pixels, the value of each can be stored in a neuron, resulting in 768 neurons as input.

## Output
The final set of neurons will be 10 neurons. Why 10? Because there are 10 potential answers.
The value stored, or activation, in each neuron represents the level of confidence that the image corresponds to the given number.
The "brightest" neuron, or one with the highest activation, is the answer.

## Hidden
By applying **weights** to the values of the first Layer, the second layer can be influenced. For example, every pixel with a low activation cost is viewed as a 0, and with a high activation cost it is a 1, then those values can be put into a polynomial, and your weights can act as a coefficients.  
- Negative coefficient emphasizes that a value is less important
- Positive coefficients emphasizes that it is important

Adding the polynomial terms together will result in a final value. That final value will be stored in the second layer's neuron.

### The question now is how it should be stored in the second layer?
Ideally, you want to squish the real number line into a value between 0 and 1, and a common function that does this is the **sigmoid function**, a.k.a. **logistic curve**.

### Also, do you always want to always store the value or only under certain conditions?
This is a **bias**, you might only want to use the sigmoid function and store the result in the second layer iff the weighted value before applying the sigmoid function is > 10 for instance. (**Question:** Technically, you could store it based on the image of the sigmoid function too right?)  
This is easy enough, we can introduce a bias for the next neuron to be inactive by simply subtracting the bias from the polynomial; that would be `-10` added to the end of the polynomial in this case.

In practice, the **Rectified Linear Unit (ReLU)** algorithm is used instead these days, which is a `max(0, a)` so it ignores all values <= 0 and then uses `a`, the resulting activation. This results in a faster learning/training process.

# Linear Concepts
First lets think about something. Suppose you have a scatter plot on a cartesian coordinate system and you want to fit a line to those points in a way that represents the data. This activity is known as **linear regression**, which you might know from a numerical analysis class in college.  
Now you could plot a line, but the data points will always been slightly off. Points will deviate some positive or negative value relative to the point on the line.  
So the goal should be to identify the line with the smallest sum of squares of deviations. This is known as the **Least Squares Method** $S = \sum_{i=1}^{n} (y_i - (mx_i + b))^2$. There are other ways of denoting the algebra as well. But the main takeaway here is that by squaring we remove any concern regarding signs (whether the deviation is above or below our line) and deviations that are further away get penalized more.  
If you complicate the model further by increasing the degree of the polynomial, it originally will fit the points better and better, but eventually the curve hits a point where it is massively incorrect for a subset of values. This is what the phenomenon of **overfitting** is in AI. A model is **underfitting**, or inaccurately modeling answers in the beginning. Over time as the model's values get better and better, the best line that fits the points is found, but then if you keep improving the model it becomes inaccurate again. From a calculus perspective, underfitting is a decreasing slope before the answer is perfect, the minima is the desired perfect answer, and overfitting is an increasing slope as the answer becomes less perfect.  
Put a pin in the ideas of underfitting and overfitting for now, we will come back to it.

# Important Functions for Evaluation
## Loss Function
A **loss function** measures the error **for a single training example**. It tells **how wrong** the model's prediction is for **one** data point.

$$\text{Loss} = \mathcal{L}(y_{\text{true}}, y_{\text{predicted}})$$
or more specifically
$$\mathcal{L}(y, \hat{y}) = (y - \hat{y})^2$$

## Cost Function
A **cost function** is the **average loss** over the entire dataset. It aggregates the loss across **all training samples**.

$$\text{Cost} = J(\theta) = \frac{1}{N} \sum_{i=1}^{N} \mathcal{L}(y_i, \hat{y}_i)$$

$$J(\theta) = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2$$

Put a pin in the ideas of loss function and cost function, we will come back to it.

# Training
Now armed with the knowledge of underfitting, overfitting, loss function, and cost function lets talk about how training is done.

You have to define a **loss function**/**cost function**, which provides a value that indicates how correct the neural network's answer was. A simple one could be the summation of `(resulting neuron value - expected neuron value)^2`. This provides a way to compare one result to another result. Moreover, and more importantly, we can divide the summation by some number of neurons to reach an average then compare all of the results as a group to other runs of the neural network.

Now take that average, imagine it plotted on a curve on a graph, the x-axis can be weight and y-axis the result of ReLU for instance. The goal is to find a result that is the lowest possible value.
You take a step in the direction of the slope (or gradient), and the step size is relative to the slope.
Finding a local minima is doable, but finding the absolute minima is hard.

In a nutshell, what we mean when we talk about a network learning, is simply the minimization of a cost function.

# Backpropogation
Backpropagation computes the "nudges" that the cost function needs efficiently. This gets it to approach a minima.

# Sources
- [3Blue1Brown Neural Networks](https://www.youtube.com/watch?v=aircAruvnKk&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=1)  
- [What OVERFITTING Teaches You About Life | Machine Learning and Statistics](https://www.youtube.com/watch?v=Nhsw8x9vyc0)  
- [Mathematical Origins of Machine Learning | Teaching Computers to Learn, Part 2](https://www.youtube.com/watch?v=_GkNhKqsgVQ)  
- [Perceptrons: The First Trainable Neural Networks | Teaching Computers to Learn, Part 3](https://www.youtube.com/watch?v=Ip6RIHwi21c)  
