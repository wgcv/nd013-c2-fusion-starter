# Writeup: Track 3D-Objects Over Time

Please use this starter template to answer the following questions:

### 1. Write a short recap of the four tracking steps and what you implemented there (filter, track management, association, camera fusion). Which results did you achieve? Which part of the project was most difficult for you to complete, and why?

#### Step 1: Extended Kalman Filter
I start implementing EKF for single objective1.
1. Implement [predict()](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L68)
  1. Create the [F Matrix](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L29) and [Q Matrix](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L46)
  2. Calculate a [System matrix for constant velocity process and corresponding process noise covariance](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L74)
2.   [update()](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#LL82C9-L82C15)
  1. Calculate [Gamma / Residual](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L88)
  2. Get the [Covariance of residual](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#LL89C38-L89C60)
  3. Compute the Kalman gains for the update
  4. Calculate the [Update state](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#L91)
  5. Calculate the [Covariance Update](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/filter.py#LL93C37-L94C1)

<img width="1572" alt="Screenshot 2023-05-25 at 8 44 49 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/4b81f3a5-89d0-410e-b2a3-c142fcaa2e5c">
<img width="1586" alt="Screenshot 2023-05-25 at 8 44 31 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/3eacbec8-6806-4856-babe-196eed335ab1">

We can calculate the position for a single track with a RMSE around 0.35 or lower (My result was around 0.32).
#### Step 2: Track management
With the track mangement we can manage multiple objects, each object we can initialize and delete, set a track state and a track score.Each object is going to be pass by EKF.
1. Implement [manage_tracks](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/trackmanagement.py#L111)
  1. Decrease score if doesn't sense with [sensor in fov](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/trackmanagement.py#L124).
  2. Delete tracks if the score is too low with [3 differents options](https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/trackmanagement.py#L130)
2. Update state with (handle_updated_track)[https://github.com/wgcv/nd013-c2-fusion-starter/blob/02e1baf5af6b71c463f3fc9a1d8abbeb03456a44/student/trackmanagement.py#LL159C9-L159C29]
<img width="1598" alt="Screenshot 2023-05-25 at 8 58 28 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/c9addb3f-646e-43fb-ac0c-54e0bd2f3b59">

#### Step 3: Data Association
We use nearest neighbor association for associate measurements to track, so now we are going to have mulit-track.
1. Calculate Mahalanobis
2. Check if a measurement lies inside a track's gate with chi-square

<img width="1650" alt="Screenshot 2023-05-29 at 12 20 56 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/9ba152cc-88b1-4180-b75d-154917e23cf9">
<img width="1644" alt="Screenshot 2023-05-29 at 12 21 18 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/c958d04b-bdf0-4ef1-bd54-20618facc709">
<img width="1642" alt="Screenshot 2023-05-29 at 12 26 16 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/371b5047-7ea4-4df9-8745-58c6bfa803ae">
We can see that the fov on lidar larger than the camera fov, because we can confirm a vehicle before see on the camera.

#### Step 4: Sensor Fusion module
Use lidar and camera to measurment when are in the FOV
<img width="1652" alt="Screenshot 2023-05-29 at 1 41 15 PM" src="https://github.com/wgcv/nd013-c2-fusion-starter/assets/8989089/09565c9c-d61a-41a8-aa13-d921f9eb0a4f">

The task of Track Measurement Association proved to be the most challenging for me. It required a thorough understanding of transformations, and debugging it was particularly difficult. As a result, it consumed a significant amount of time and effort before I could finally achieve the desired outcome.

### 2. Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)? 


### 3. Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?


### 4. Can you think of ways to improve your tracking results in the future?

