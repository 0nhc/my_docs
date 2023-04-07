HardwareInterface
=================

The HardwareInterface class implements communication with motors via
socket-can in Linux system to control the motors and obtain feedback.

.. code:: cpp

   #ifndef HARDWARE_INTERFACE_H
   #define HARDWARE_INTERFACE_H

   #include "../hardware/can.h"
   #include "../hardware/dbus.h"
   #include "../hardware/A8120.h"

   #include <vector>
   #include <string>
   #include <thread>

   class HardwareInterface
   {
       public:
           HardwareInterface(std::string init_hardware_type = "real", std::string init_control_mode = "position");
           ~HardwareInterface()=default;

           // Get joint states
           std::vector<double> getJointStates();
           // Control joint angles
           void setJointAngles(std::vector<double> set_joint_angles);
           // Control joint torques
           void setJointTorques(std::vector<double> set_joint_torques);
    
       private:
           // Hardware type (fake/real/sim)
           std::string hardware_type;
           // Control mode (position/torque)
           std::string control_mode;
           // Joint states
           std::vector<double> joint_states = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0};
           // Joint angle commands
           std::vector<double> joint_angles = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0};
           // Joint torque commands
           std::vector<double> joint_torques = {0.0, 0.0, 0.0, 0.0, 0.0, 0.0};
           // CAN interface
           can can_interface;
           // Actuator ID
           int motor_ids[6] = {1,2,4,5,6,7};
           // Independent thread
           pthread_t thread_1;
           // Thread function
           void updateHardware();
   };
    
   #endif

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
