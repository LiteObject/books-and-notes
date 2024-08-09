# Activation Function

Activation functions are a crucial component of neural networks, and they play a significant role in determining the output of each node (or neuron) in the network.

## Without Activation Functions:
Suppose we have a neural network with two inputs (x1 and x2) that multiply together to produce an output.

```
output = x1 * x2
```
If both x1 and x2 are large numbers, the output will be extremely large. But if either x1 or x2 is small (even zero), the output will be zero.

## With Activation Functions:
Now, let’s add a ReLU activation function to the output:

```
output = max(0, x1 * x2)
```

This ensures that even if one of the inputs is small (or zero), the output won’t be zero. The ReLU activation function "turns on" the output only when both inputs are positive.

In this simple example, the activation function helps prevent the network from producing extreme or unhelpful outputs. This is just a tiny glimpse into why activation functions are important in neural networks!

## Here are some common activation functions, along with simple examples:

### Sigmoid Function
The sigmoid function maps any real-valued number to a value between 0 and 1.

**Example**: If we input x = 2, the sigmoid function would output approximately 0.88.

Mathematically, the sigmoid function is defined as:

```
sigmoid(x) = 1 / (1 + exp(-x))
```

### ReLU (Rectified Linear Unit)
ReLU is a simple activation function that outputs 0 for negative inputs and the input value itself for positive inputs.

**Example**: If we input x = -3, the ReLU function would output 0. If we input x = 4, the ReLU function would output 4.

Mathematically, the ReLU function is defined as:

```
ReLU(x) = max(0, x)
```

### Leaky ReLU
Leaky ReLU is a variation of the ReLU activation function that allows a small fraction of the input value to pass through even when it’s negative.

**Example**: If we input x = -3, the leaky ReLU function might output -0.1. If we input x = 4, the leaky ReLU function would output 4.

Mathematically, the leaky ReLU function is defined as:

```
leakyReLU(x) = max(0.01x, x)
```

### Tanh (Hyperbolic Tangent)
The tanh function maps any real-valued number to a value between -1 and 1.

**Example**: If we input x = 2, the tanh function would output approximately 0.96.

Mathematically, the tanh function is defined as:

```
tanh(x) = (exp(x) - exp(-x)) / (exp(x) + exp(-x))
```

### Softmax
Softmax is an activation function that maps any real-valued vector to a probability distribution over a set of possible classes.

**Example**: If we input x = [2, 3], the softmax function would output [0.73, 0.27], which represents a probability distribution over two classes.

Mathematically, the softmax function is defined as:

```
softmax(x) = exp(x) / sum(exp(x))
```

These are just a few examples of activation functions, and there are many more out there! Each activation function has its own strengths and weaknesses, and choosing the right one depends on the specific problem you’re trying to solve.