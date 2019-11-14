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

#### Thinking about it more:

* A reduced action space limits exploration of the entire environment - one of the strnegths of reinforcement learning is that the AI exlpores the environment and the reward function determines how to improve the model
* When you run a simulation and gather results, you can then clone the model, which retains all of the previous training and then can continue to develop your reward function. However once a model is started and then cloned, you cannot change the action space. This makes sense as the CNN has assigned weight to the nodes within each layer that at least in the last layer of the CNN, as directly related to the lookup table and max possible actions.

So at this point I kind of went back to thr drawing board, but had a better idea of what not to do.

### Inputs to Reward Function

I then decide to focus less on the action space, and more on the reward function optimisation.

It is easy to write a reward function that with training will increase to produce greater reward, but not necessarily a lower track time. The reward function must be directly tailored towards rewards action that clearly reduce time around the track (and to a less degree accuracy on the track).

During [Latency 2019](https://www.latencyconf.io/) it was mentioned that some people approaches were to compare the cars position to an "ideal racing line". 

I thought I'd attack this from a slightly different perspective.

#### Developing a new reward function from sratch

closest_waypoints gives the closes two waypoints to the car (this will always be one waypoint in front of the car and one waypoint behind the car, but not stricylt order). So let's take the max of the two waypoints [int, int] to ensure we have the one in front of the car current position ( as the car always travel Counter Clockwise of the 2018 Reinvent travel, with the waypoints in the same order).

Now using some known information from Latency:
* Optimal Speeds are around 4 m/s, faster tends to be erractic
* The images are processed at 15 fps
* The car will go fastest on the straight, so knowing the size of the track below, there are 168.72 inches (4.285m) for 7 waypoints (1-6). 

https://docs.aws.amazon.com/deepracer/latest/developerguide/images/deepracer-track-guideline.png
https://docs.aws.amazon.com/deepracer/latest/developerguide/images/deepracer-reward-function-input-waypoints.png

Therefore the car at max speed would go past ~7 waypoints in 1 seconds, therefore 15 frames for 7 waypoints, so 15/7 = 2 frames per waypoint to change the action space according to the image.

Now the at 4 m/s the car moves quite fast, and event if an steering angle and throttle are selected, they re not effective immedaitely.
So let's write function to looks at steering based on the next waypoint or two waypoints ahead or three waypoints ahead.














