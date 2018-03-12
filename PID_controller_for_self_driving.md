# PID Controller for self-driving

#### 1. Project Overview

In this project you'll revisit the lake race track from the Behavioral Cloning Project. This time, however, you'll implement a PID controller in C++ to maneuver the vehicle around the track!

The simulator will provide you the cross track error (CTE) and the velocity (mph) in order to compute the appropriate steering angle.

#### 2. Reflection

Classic PID algorithm is used to compute steering value to reduce cross track error (CTE) so that car stays in the center of racing track. 
P control is the dominant term to compensate the cross track error. It is proportional to CTE in the opposite direction with an amplification factor Kp. Kp determines how fast CTE converges. A large Kp helps the car move towards the center faster. However, excessive Kp will cause stability issues which makes the car oscillate around the center line. It can be easily observed when car makes sharp turns with high speed.
D control is normally used together with P control to improve transient behaviors. It compensates the change rate of CTE and effectively reduces the overshoot caused by P control.
I control uses a correction term propotional to integration of all CTEs to remove steady state error caused by DC bias in the system. Without it, the car may slightly drift away from the center line if there are any DC disturbances.

#### 3. Parameters tuning

PID parameters are tuned manually based on following procedures:
1). Gradually increase P gain to make car follow some sharp turns. Signifcant oscillation may observed during these turns.
2). Gradually increase D gain to reduce oscillation until the car is able to drive around the whole lap.
3). No obvious bias is observed in the system. Car performs Ok even without an integal term. Using Twiddle may help to further fine tune integration gain by optimizing overall CTE around the tracks.

For facilitating tuning, main.cpp is modified to accept passing in parameters.
./pid Kp_s, Ki_s, Kd_s, Kp_thr, Ki_thr, Kd_thr, throttle

The car demonstrated reasonable performance using the following parameters with constant throttle value 0.3 (no speed control)
./pid 0.15, 0.0001, 2, 0, 0, 0, 0.3

After the steering loop tuning with the fixed throttle 0.3, I tried to increase car's speed by using larger throttle value 0.6. Car start racing out of the track when making some sharp turns. A throttle loop is added to regulate car's speed, it uses CTE's magnitude as the feedback signal to speed up or slow down car around nominal throttle value 0.6. The abovmentioned tuning procedure is still applicable.  After tuning, the car was able to racing around the track again with faster average speed. It is interesting to see the braking lights turned on when it was making fast turns. It verified the throttle loop was in action together with steering loop to maintain the car on track.

./pid 0.15, 0.0001, 2, 0.1, 0, 5, 0.6


