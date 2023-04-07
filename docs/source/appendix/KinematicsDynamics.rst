KinematicsDynamics
==================

The KinematicsDynamics class implements kinematics and dynamics
calculations using KDL library.

.. code:: cpp

   #ifndef KINEMATICS_DYNAMICS_H_
   #define KINEMATICS_DYNAMICS_H_

   #include <kdl/chain.hpp>
   #include <kdl/chainfksolver.hpp>
   #include <kdl/chainiksolver.hpp>
   #include <kdl/chainidsolver.hpp>
   #include <kdl/chainidsolver_recursive_newton_euler.hpp>
   #include <kdl/tree.hpp>
   #include <kdl/frames_io.hpp>

   #include <vector>
   #include <thread>

   #include "arx5_base/rate.h"

   class KinematicsDynamics
   {
   public:
       KinematicsDynamics();
       ~KinematicsDynamics()=default;

       // Forward kinematics: given six joint angles, return the end effector pose
       std::vector<double> solveFK(std::vector<double> joint_angles);

       // Inverse kinematics: given the end effector pose, return the six joint angles
       std::vector<double> solveIK(std::vector<double> end_effector_pose);

       // Inverse dynamics: given the end effector pose, return the six joint torques
       std::vector<double> solveID(std::vector<double> end_effector_pose);

       // Update joint states
       void setJointStates(std::vector<double> set_joint_states);

   private:
       // Abstract model of the robot arm
       KDL::Chain chain;

       // Generate KDL frame
       KDL::Frame generateFrame(std::vector<double> pose);

      // KDL forward kinematics solver
       std::shared_ptr<KDL::ChainFkSolverPos> fksolver_pos;
       std::shared_ptr<KDL::ChainIkSolverVel> iksolver_vel;
       std::shared_ptr<KDL::ChainIkSolverPos> iksolver_pos;
       // KDL inverse dynamics solver
       std::shared_ptr<KDL::ChainIdSolver_RNE> idsolver_tor;

       // Initial position of the robot arm
       KDL::Frame init_frame;

      // Joint states
       KDL::JntArray joint_angles;
       KDL::JntArray new_joint_angles;
       KDL::JntArray joint_velocities;
       KDL::JntArray new_joint_velocities;
       KDL::JntArray joint_acclerations;
       // End effector state
       KDL::Frame end_effector_pose;
   
       // Inverse solution
       KDL::JntArray ik_results;

       // Create a separate thread
       pthread_t thread_1;
       // Thread function: update joint states at a certain frequency
       void updateStates();
       // Thread running frequency
       double thread_frequency = 100.0;
   };

   #endif

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
