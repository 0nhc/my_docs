JointTrajectories
=================

The JointTrajectories class is used to store the FIFO position sequence
of the six joint motors of the robotic arm.

.. code:: cpp

   #ifndef JOINT_TRAJECTORIES_H
   #define JOINT_TRAJECTORIES_H

   #include <vector>

   class JointTrajectories
   {
   public:
       JointTrajectories();
       ~JointTrajectories()=default;
       // Add a trajectory point to the trajectory queue
       void push(std::vector<double> joint_positions);
       // Delete the first trajectory point in the trajectory queue
       void pop();
       // Update trajectories
       void update(std::vector<std::vector<double>> new_trajectories);
       // Return the first trajectory point in the trajectory queue
       std::vector<double> front();
       // Print all the trajectory points in the trajectory queue
       void print();
       // Return the number of trajectory points in the trajectory queue
       int size();

   private:
       // The trajectory queue
       std::vector<std::vector<double>> trajectories_;
   };
   
   #endif

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
