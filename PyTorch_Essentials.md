# PyTorch Essentials

Welcome to this short course on PyTorch! PyTorch is a popular open-source machine learning library used for applications such as natural language processing. It is known for its flexibility and ease of use. This course will cover the essential concepts you need to get started with PyTorch.

## Table of Contents

1. **Introduction to PyTorch**
2. **Tensors**
3. **Autograd**
4. **Neural Networks**
5. **Optimization**
6. **Data Loading**
7. **Saving and Loading Models**
8. **Conclusion**

## 1. Introduction to PyTorch

PyTorch is a library developed by Facebook's AI Research lab. It provides two high-level features:
- **Tensor computation (like NumPy)** with strong GPU acceleration.
- **Deep neural networks** built on a tape-based autograd system.

### Why PyTorch?

- **Dynamic Computation Graphs**: PyTorch builds the computation graph on-the-fly, making it more intuitive and easier to debug.
- **Ease of Use**: It has a simple and intuitive API.
- **Strong Community Support**: A large and active community means plenty of resources and support.

## 2. Tensors

Tensors are the core data structure in PyTorch, similar to NumPy arrays but with GPU support.

### Basic Operations

```python
import torch

# Creating a tensor
x = torch.tensor([1.0, 2.0, 3.0])

# Basic operations
y = x + 2
z = x * y

print(z)
```

### Tensor Types

- **FloatTensor**: Default type for floating-point tensors.
- **LongTensor**: Default type for integer tensors.
- **ByteTensor**: Default type for byte tensors.

### Moving Tensors to GPU

```python
# Check if GPU is available
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# Move tensor to GPU
x = x.to(device)
```

## 3. Autograd

Autograd is PyTorch's automatic differentiation engine. It records operations performed on tensors and automatically computes gradients.

### Example

```python
# Create a tensor with requires_grad=True
x = torch.tensor([1.0, 2.0, 3.0], requires_grad=True)

# Define a simple function
y = x + 2
z = y * y * 3
out = z.mean()

# Compute gradients
out.backward()

# Print gradients
print(x.grad)
```

## 4. Neural Networks

PyTorch provides a module called `torch.nn` for building neural networks.

### Defining a Simple Neural Network

```python
import torch.nn as nn
import torch.nn.functional as F

class SimpleNet(nn.Module):
    def __init__(self):
        super(SimpleNet, self).__init__()
        self.fc1 = nn.Linear(16, 128)
        self.fc2 = nn.Linear(128, 10)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
        return x

# Instantiate the network
net = SimpleNet()
```

## 5. Optimization

Optimization algorithms are used to update the weights of the neural network to minimize the loss function.

### Example with SGD

```python
import torch.optim as optim

# Define a loss function and optimizer
criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)

# Training loop
for epoch in range(100):
    optimizer.zero_grad()
    outputs = net(inputs)
    loss = criterion(outputs, targets)
    loss.backward()
    optimizer.step()
```

## 6. Data Loading

PyTorch provides `torch.utils.data.DataLoader` for loading data in batches.

### Example

```python
from torch.utils.data import DataLoader, TensorDataset

# Create a dataset
dataset = TensorDataset(inputs, targets)

# Create a data loader
dataloader = DataLoader(dataset, batch_size=32, shuffle=True)

# Iterate through the data loader
for batch_inputs, batch_targets in dataloader:
    # Training code here
    pass
```

## 7. Saving and Loading Models

You can save and load models to/from disk using `torch.save` and `torch.load`.

### Saving a Model

```python
torch.save(net.state_dict(), 'model.pth')
```

### Loading a Model

```python
net = SimpleNet()
net.load_state_dict(torch.load('model.pth'))
net.eval()
```

## 8. Conclusion

Congratulations! You've covered the essential concepts of PyTorch. From tensors to neural networks, optimization, data loading, and saving models, you now have the foundation to build and train your own machine learning models.

### Next Steps

- **Explore More**: Dive deeper into advanced topics like convolutional neural networks (CNNs), recurrent neural networks (RNNs), and transformers.
- **Practice**: Implement projects to reinforce your learning.
- **Join the Community**: Engage with the PyTorch community for support and collaboration.
