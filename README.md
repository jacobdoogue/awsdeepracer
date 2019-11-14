# My Approach to awsdeepracer

There is lot out there about AWS Deepracer, but in the pursuit of self exploration, I decided to try a few things without any deep research to what has already been done, and to come up with some finding myself.

### Variables

The first thing I wanted to understand are what input variables are.

Firstly the action space comprises of:
* steering anlge (0-30 degrees)
* steering angle granularity (3,5,7)
* max speed (m/s) (0.8-12)
* speed granularity (1-3)

The combination of these gives an action space. After the CNN is trained in the cloud simulation environment, essentially a lookup table is loaded into the deepracer car to say "based on the current image, then this is the action (steering angle and speed) that should be used".

My thinking was that a reduced action space may allow for potentially a faster "driver", but also likely more erratic".

The main issues that I found with this approach are:
* It limit exploration of the entire environment - one of the strnegths of reinforcement learning is that the AI exlpores the environment and the reward function determines how to improve the model
* When you run a simulation and gather results, you can then clone the model, which retains all of the previous training and then can continue to develop your reward function. However once a model is started and then cloned, you cannot change the action space. This makes sense as the CNN has assigned weight to the nodes within each layer that at least in the last layer of the CNN, as directly related to the lookup table and max possible actions.

### Initial Results

So let's look at some of the initial results:


