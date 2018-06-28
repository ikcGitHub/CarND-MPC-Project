# CarND-Term2-P4-Model-Predictive-Control  
## Overview  
(Need to change)
In this project you'll implement Model Predictive Control to drive the car around the track. This time however you're not given the cross track error, you'll have to calculate that yourself! Additionally, there's a 100 millisecond latency between actuations commands on top of the connection latency.

## Prerequisites/Dependencies  
* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
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

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.
## Setup Instructions (abbreviated)  
1. Meet the `Prerequisites/Dependencies`  
2. Intall `uWebSocketIO ` on your system  
  2.1 Windows Installation  
  2.1.1 Use latest version of Ubuntu Bash 16.04 on Windows 10, here is the [step-by-step guide](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) for setting up the utility.  
  2.1.2 (Optional) Check your version of Ubuntu Bash [here](https://www.howtogeek.com/278152/how-to-update-the-windows-bash-shell/).  
3. Open Ubuntu Bash and clone the project repository  
4. On the command line execute `./install-ubuntu.sh`  
5. Build and run your code.  
Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)  
## Project Description  
- [main.cpp](./src/main.cpp):Reads in data, calls a function to initialize and run the model predictive controller on steering angle,  acceleration pedal and brake. Besides that, it will retrieve the predicted trajectory from the model predictive controller and propagate the line in front of the vehicle.  
- [MPC.cpp](./src/ukf.cpp): Initializes the model predictive controller, define the cost calculation, set the contraints, process data and return actuator commands and predicted trajectory. Defines `FG_eval()` and `Solve()`.  
- [README.md](./README.md): Writeup for this project, including setup, running instructions and project rubric addressing.  
- [CMakeLists.txt](./CMakeLists.txt): `CMakeLists.txt` file that will be used when compiling your code (you do not need to change this file)
## Run the project  
* Clone this respository
* At the top level of the project repository, create a build directory: `mkdir build && cd build`
* In `/build` directory, compile yoru code with `cmake .. && make`
* Launch the simulator from Windows
* Execute the run command for the project `./mpc` (Make sure you also run the simulator on the Windows host machine) If you see * * this message, it is working `Listening to port 4567 Connected!!!`

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Rubric  
### 1. Compilation  
#### 1.1 Your code should compile.  
Yes, it does.  
### 2. Implementation  
#### 2.1 The Model  
Please see my notes/comments/documentation in [`MPC.cpp`](./src/MPC.cpp).  
#### 2.2 Timestep Length and Elapsed Duration (N & dt)  
Start with N = 25, dt = 0.05 then stawy with N = 10, dt = 0.1 at [`MPC.cpp` Line8-10](./src/MPC.cpp#L8-L10).  
Because the latency is around 100 millisecond, setting dt = 0.1 can synchronize the laging.  
As dt = 0.1, then N = 25 makes the prediction a little bit further but cause more calculation consumption.  
Based on the speed (~55 mph) I have, 10 is good enough and 25 is not that bad.  
#### 2.3 Polynomial Fitting and MPC Preprocessing  
Polynomial fitting was called at [`main.cpp` Line120-124](./src/main.cpp#L120-L124).  
Polynomial fitting was implemented at [`main.cpp` Line44-66](./src/main.cpp#L44-L66).  
MPC preprocessing was implemented at [`main.cpp` Line104-118](./src/main.cpp#L104-L118).  
#### 2.4 Model Predictive Control with Latency  
The latency was handled at [`main.cpp` Line126-151](./src/main.cpp#L126-L151).  
Also, one line changed at [`main.cpp` Line228](./src/main.cpp#L228).  
### 3. Simulation  
#### 3.1 The vehicle must successfully drive a lap around the track.  
Yes, it does. Please see the videos below.  
## Videos
Video recordings for success cases.  
Success case for smooth driving.  
![Smooth_Driving](./videos/CarND-Term2-P5-Smooth-Driving_self_driving_car_nanodegree_program_6_27_2018_11_28_12_PM.gif)  
Success case for highest speed driving with around 57 mph.  
![Highest_Speed_Driving](./videos/CarND-Term2-P5-Highest-Speed_self_driving_car_nanodegree_program_6_27_2018_11_21_04_PM.gif)  
