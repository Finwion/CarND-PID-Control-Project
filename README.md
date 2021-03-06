# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

# Describe the effect each of the P, I, D components had in your implementation.
## P
The P term is proportional to the CTE (cross-track error). This will allow the controller to respond to directly to changes in the CTE.

With a large P value, the steering angle will start oscillating between negative and positive steering angles. Basically, switching between driving on the left and right sides of the road. Eventually, it will oscillate between the highest and lowest steering angles and drive off the road. 

With a small P value, the steering angle changes are very small. For curves, this will cause the car not to respond with a sufficient steering angle to clear the curve.

## D
The D term is proportional to the change in the CTE. This allows the controller to dampen the oscillations. It does this by reducing the overshoot of the P value. 
 
With a sufficiently large D value, the car was able to stay in the lanes. 

With only P&D parameters, the controller was able to keep the car in the lanes. 

## I
The I term is proportional to the integral of the CTE. This term will help eliminate steering biases.  


# Describe how the final hyperparameters were chosen.
For manually tuning the P parameter, the I and D parameters were set to 0. With P=1.0, the car had too many oscillations at the beginning of the track. With P=0.1, the car was able to clear the first part of the track before the turn. With P=0.2, the car responded better to steeper curves.

For tuning the D parameter, P was set to 0.2 and I was set to 0. With D=0.1, this was insufficient to keep the oscillations from driving the car off the road. With D=1.0, this dampened the oscillations slightly but was not able to keep the car on the road at the end of the track. With D=10.0, the car was able to keep on the road. 

For tuning the I parameter, the previous P and D parameters found were used. A very large I value of 10 or 1 seemed to negatively impact the start of the track, where it just seemed to want to turn unnecessarily. A smaller value, I=0.001, seemed to be best for the implementation. 

The final parameters were picked manually were P=0.2, I=0.001, D=10.0. 

While the parameters above kept the car in the lanes as required by the project specification, I know that these values could have been fined tuned according to actual error values. A possible enhancement would be to implement the twiddle algorithm to find a more optimal set of PID parameters.