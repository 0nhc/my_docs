API Reference
=============

-  Rate

The Rate class uses the chrono library in C++ to implement timing, which
is used to achieve fixed-frequency control in a loop.

.. code:: cpp

   class Rate(double frequency);

Usage:

.. code:: cpp

   #include <arx5_base/rate.h>
   #include <iostream>

   int main()
   {
       // Create a Rate object with a production frequency of 10Hz
       Rate rate(10);
       // Start timing
       while(1)
       {
           // Print the current time
           auto now = std::chrono::high_resolution_clock::now();
           auto us = std::chrono::time_point_cast<std::chrono::microseconds>(now);
           auto ms = std::chrono::time_point_cast<std::chrono::milliseconds>(now);
           std::cout << "Current time (ms): " << us.time_since_epoch().count() << std::endl;
           rate.sleep();
       }

       return 0;
   }

-  HardwareInterface

The HardwareInterface class implements communication with motors via
socket-can in Linux system to control the motors and obtain feedback.

.. code:: cpp

   class HardwareInterface(std::string init_hardware_type = "real", std::string init_control_mode = "position");

Usage:

.. code:: cpp

   #include<arx5_base/hardware_interface.h>

   int main()
   {
       // Create a hardware interface object
       HardwareInterface hardware_interface();
       
       // Control the joints
       hardware_interface.setJointAngles({0.0, 0.0, 0.0, 0.0, 0.0, 0.0});
       // Get joint states
       std::vector<double> joint_states = hardware_interface.getJointStates();
       // Print joint states
       for (int i = 0; i < 6; i++)
       {
           std::cout << "joint_states[" << i << "] = " << joint_states[i] << std::endl;
       }

       // Wait for 1 second
       sleep(1);

       // Control the joints
       hardware_interface.setJointAngles({0.0, -0.2, 0.0, 0.0, 0.0, 0.0});
       // Wait for 1 second
       sleep(1);
       // Get joint states
       joint_states = hardware_interface.getJointStates();
       // Print joint states
       for (int i = 0; i < 6; i++)
       {
           std::cout << "joint_states[" << i << "] = " << joint_states[i] << std::endl;
       }
       
       // Keep the robot arm in the original position
       while(1)
       {

       }

       return 0;
   }

-  JointTrajectories

The JointTrajectories class is used to store the FIFO position sequence
of the six joint motors of the robotic arm.

.. code:: cpp

   class JointTrajectories();

Usage:

.. code:: cpp

   #include <arx5_base/joint_trajectories.h>
   #include <iostream>

   int main()
   {
       // create a trajectory sequence object
       JointTrajectories joint_trajectories;
       
       // add an angle for each of the six joints to the sequence
       std::vector<double> joint_positions;
       joint_positions.push_back(1.0);
       joint_positions.push_back(2.0);
       joint_positions.push_back(3.0);
       joint_positions.push_back(4.0);
       joint_positions.push_back(5.0);
       joint_positions.push_back(6.0);
       joint_trajectories.push(joint_positions);
       // view current trajectory sequence
       joint_trajectories.print();
       
       // add another angle for each of the six joints to the sequence
       joint_positions.clear();
       joint_positions.push_back(2.0);
       joint_positions.push_back(3.0);
       joint_positions.push_back(4.0);
       joint_positions.push_back(5.0);
       joint_positions.push_back(6.0);
       joint_positions.push_back(7.0);
       joint_trajectories.push(joint_positions);
       // view current trajectory sequence
       joint_trajectories.print();
       
       // remove the head angle of the trajectory sequence
       joint_trajectories.pop();
       // view current trajectory sequence
       joint_trajectories.print();
       joint_trajectories.pop();
       joint_trajectories.print();
       joint_trajectories.pop();
       joint_trajectories.print();

       // test if pop can be called when there is no data
       joint_trajectories.pop();
       joint_trajectories.print();
       
       // test updating the trajectory sequence directly
       std::vector<std::vector<double>> new_trajs;
       for(double i=0; i<6; i++)
       {
           std::vector<double> traj;
           traj.push_back(10.0+i);
           new_trajs.push_back(traj);
       }
       joint_trajectories.update(new_trajs);
       joint_trajectories.print();

       return 0;
   }

-  KinematicsDynamics

The KinematicsDynamics class implements kinematics and dynamics
calculations using KDL library.

.. code:: cpp

   class KinematicsDynamics();

Usage:

.. code:: cpp

   #include <arx5_base/kinematics_dynamics.h>
   #include <iostream>

   int main()
   {
       KinematicsDynamics kinematics_dynamics;

       std::vector<double> fk_result;
       std::vector<double> ik_result;
       std::vector<double> id_result;
       std::vector<double> joint_angles;
       std::vector<double> end_effector_pose;

       // Forward kinematics test
       joint_angles = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0};
       fk_result = kinematics_dynamics.solveFK(joint_angles);
       std::cout << "FK result: " << fk_result[0] << ", " << fk_result[1] << ", " << fk_result[2] << ", " << fk_result[3] << ", " << fk_result[4] << ", " << fk_result[5] << std::endl;

       joint_angles = {0.0, -0.3, 0.6, -0.3, 0.0, 0.0};
       fk_result = kinematics_dynamics.solveFK(joint_angles);
       std::cout << "FK result: " << fk_result[0] << ", " << fk_result[1] << ", " << fk_result[2] << ", " << fk_result[3] << ", " << fk_result[4] << ", " << fk_result[5] << std::endl;
       
       // Inverse kinematics test
       end_effector_pose = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0};
       ik_result = kinematics_dynamics.solveIK(end_effector_pose);
       std::cout << "IK result: " << ik_result[0] << ", " << ik_result[1] << ", " << ik_result[2] << ", " << ik_result[3] << ", " << ik_result[4] << ", " << ik_result[5] << std::endl;

       end_effector_pose = {0.0, 0.0, 0.3, 0.0, 0.0, 0.0};
       ik_result = kinematics_dynamics.solveIK(end_effector_pose);
       std::cout << "IK result: " << ik_result[0] << ", " << ik_result[1] << ", " << ik_result[2] << ", " << ik_result[3] << ", " << ik_result[4] << ", " << ik_result[5] << std::endl;

       // Thread test
       while(1)
       {
           
       }

       return 0;
   }
