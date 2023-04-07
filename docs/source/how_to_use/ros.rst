3. ROS Package Usage
====================

The **arx5_ros** contains the following packages:

-  **arx5_msgs**: ROS messages for the robotic arm
-  **arx5_control**: control algorithm and hardware interface for the
   robotic arm
-  **arx5_moveit**: MoveIt! configuration for the robotic arm
-  **arx5_teleop**: example of controlling the robotic arm with a
   joystick
-  **arx5_description**: URDF file for the robotic arm

3.1 arx5_msgs
-------------

This package contains custom ROS messages for the robotic arm.

3.1.1 JointCommand.msg
~~~~~~~~~~~~~~~~~~~~~~

::

   Header header
   string mode

   float64[] kp
   float64[] kd
   float64[] position
   float64[] velocity
   float64[] effort

This message is used for sending control commands to the robotic arm’s
joints. Each message includes the control mode, control parameters for
each of the 6 joints, and control values for each of the 6 joints.

3.1.2 JointFeedback.msg
~~~~~~~~~~~~~~~~~~~~~~~

::

   Header header

   float64[] position
   float64[] velocity
   float64[] current
   float64[] temperature

This message is used for sending feedback about the state of the robotic
arm. Each message includes the position, velocity, current, and
temperature of each of the 6 joints.

3.1.3 TeleopCommand.msg
~~~~~~~~~~~~~~~~~~~~~~~

::

   Header header

   float64 x
   float64 y
   float64 z
   float64 roll
   float64 pitch
   float64 yaw
   float64 base_yaw
   float64 gripper
   bool reset

This message is used for sending commands to the robotic arm using a
gamepad. Each message includes the pose of the end effector, the angle
of the 0th joint, the command for the gripper, and a reset command.

3.2 arx5_control
----------------

This package contains the control algorithm and motor interface for the
robotic arm.

-  Control Algorithm

   .. code:: sh

      roslaunch arx5_control arx5_teleop.launch

   This subscribes to **arx5_msgs/TeleopCommand** to obtain the angles
   for the 6 joints, and publishes **arx5_msgs/JointCommand** to control
   the 6 joint motors.

-  Actuator Interface

   .. code:: sh

      roslaunch arx5_control arx5_hardware.launch

   This subscribes to **arx5_msgs/JointCommand** to control the 6 joint
   motors, and publishes **arx5_msgs/JointFeedback** to provide feedback
   on the state of the 6 joint motors.

-  Visualization

   .. code:: sh

      roslaunch arx5_control arx5_monitor.launch

   This subscribes to **arx5_msgs/JointFeedback** messages and
   visualizes the angles of the 6 joints using matplotlib.

3.3 arx5_moveit
---------------

.. code:: sh

   roslaunch arx5_moveit arx5_moveit.launch

This package configures MoveIt! for the robotic arm. The arm can be
planned and controlled using MoveIt! by dragging the arm in the RViz
interface. See the MoveIt! official documentation for more information:
https://moveit.ros.org/.

3.4 arx5_teleop
---------------

.. code:: sh

   roslaunch arx5_teleop arx5_teleop.launch

This subscribes to the **joy** message published from
http://wiki.ros.org/joy\ and publishes **arx5_msgs/TeleopCommand**
messages based on the joystick inputs. It needs to be used in
conjunction with **arx5_control** package.

3.5 arx5_description
--------------------

This package contains the URDF file for the robotic arm, which can be
used by other packages.
