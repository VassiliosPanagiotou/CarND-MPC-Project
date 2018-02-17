# **Model Predicitve Control** 

**Model Predicitve Control Project**
The goals / steps of this project are the following:
* Implement the MPC controller as taught in lesson 19 "Model Predicitve Control"
* Tune the MPC controller parameters such that the vehicle safely drives through the simulated race track
* Summarize the results with a written report

## Rubric Points

## Implementation of Model Predictive Controller

#### The model

The kinematic model from the chapter 12 "Motion Models" was used. The state variables are the (x, y)-coordinates, the vehicle orientation theta and the vehicle longitudinal speed v. The actuators steering angle delta and acceleration a were used and as were the equations learned in chapter 18 "Vehicle Models".

For delta != 0, the system model update equations are:
x_f	= x_0 + (L / delta_0) * [sin(theta_0 + (v_0 * delta_0 / L) * dt) - sin(theta_0)]
y_f = y_0 + (L/delta_0) * [cos(theta_0) - cos(theta_0 + (v_0 * delta_0 / L) * dt)]
theta_f = theta_0 + (v_0 * delta_0 / L) * dt
v_f = v_0 + a_0 * dt

For delta = 0, the system model update equations are:
x_f	= x_0 + v_0 * cos(theta_0) * dt
y_f = y_0 + v_0 * sin(theta_0) * dt
theta_f = theta_0
v_f = v_0 + a_0 * dt

The trajectory error update equations from chapter 18 "Vehicle Models" were used (cross track error cte and orientation error thetae):
cte_f = cte_0 + v_0 * sin(thetae) * dt
thetae_f = thetae_0 + (v_0 * delta_0 / L) * dt

#### Timestep Length and Elapsed Duration (N & dt)
In order to cope with the latency of the system - as proposed in the lesson - the dt was chosen equal to the latency, dt = 0.1 seconds.
The proposed value was used and worked fine.
The number of timestamps N was set to 10. Playing with the number and setting it higher revealed instabilities.
Anyhow, I can imagine that an existing map could influence dynamicly this value.

#### Model Predictive Control with Latency
For handling the latency the model predictive control time was set according to the latency. The time frame for which the control values are applied to the actuators is equal to the time frame used in computation by the optimization algorithm. Besides that and logically (and as mentioned in the lesson), the vehicle state was projected to the future, so that the optimization algorithm would optimize the actuator values for the vehicle state at which they will be applied. 