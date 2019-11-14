# My Approach to AWS Deep Racer

There is lot out there about AWS Deepracer, but in the pursuit of exploring and investigating myself, I decided to try a few things without any deep research to what has already been done, and to come up with some finding of my own.

### Variables

The first thing I wanted to understand are the sensitivity and effect of the input variables.

Firstly the action space comprises of:
* steering anlge (0-30 degrees)
* steering angle granularity (3,5,7)
* max speed (m/s) (0.8-12)
* speed granularity (1-3)

The combination of these gives an action space. After the CNN is trained in the cloud simulation environment, essentially a lookup table is loaded into the deepracer car to say "based on the current image, then this is the action (steering angle and speed) that should be used".

My thinking was that a reduced action space may allow for potentially a faster "driver", but also likely more erratic".

### Initial Results

So let's look at some of the initial results:

![table](/first_tests.png)

hmmmm.

#### Issues:
* Low track completion rates (in simulation and evaluation)
* There did not seem to be a clear advantage to the reduced action space vs max action space
* Initial faster speeds of 4m/s for initial training seem too aggressive

#### Think about it more:

* A reduced action space limits exploration of the entire environment - one of the strnegths of reinforcement learning is that the AI exlpores the environment and the reward function determines how to improve the model
* When you run a simulation and gather results, you can then clone the model, which retains all of the previous training and then can continue to develop your reward function. However once a model is started and then cloned, you cannot change the action space. This makes sense as the CNN has assigned weight to the nodes within each layer that at least in the last layer of the CNN, as directly related to the lookup table and max possible actions.

So at this point I kind of went back to thr drawing board, but had a better idea of what not to do.






