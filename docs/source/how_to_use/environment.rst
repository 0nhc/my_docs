1. Environment Configuration
============================

The system runs on the x86 platform of Ubuntu 20.04 or Ubuntu 22.04.

1.1 Dependency Installation
---------------------------

-  kdl

   .. code:: sh

      sudo apt install liborocos-kdl-dev

-  kdl_parser

   .. code:: sh

      sudo apt install libkdl-parser-dev

1.2 SDK Installation
--------------------

.. code:: sh

   git clone https://gitee.com/arx-discover/libarx5.git
   cd libarx5
   mkdir build && cd build
   cmake ..
   make
   sudo make install

1.3 ROS Package Installation (Optional)
---------------------------------------

1.3.1 ROS Installation
~~~~~~~~~~~~~~~~~~~~~~

Please refer to the official ROS installation guide.

`Noetic <https://wiki.ros.org/noetic/Installation/Ubuntu>`__

`Humble <https://docs.ros.org/en/humble/Installation.html>`__

1.3.2 Robotic Arm ROS Package Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

   mkdir catkin_ws
   cd catkin_ws
   mkdir src
   cd src
   git clone https://gitee.com/arx-discover/arx5_ros.git
   cd ..
   rosdep install --from-path src --ignore-src -r -y
   catkin_make

1.4 Hardware Permission Setting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Every time** the robotic arm is connected to the PC, execute the
following command to enable socket-can permission.

.. code:: sh

   # Execute this command every time you connect your PC with the USB2CAN module.
   sudo ip link set can0 up type can bitrate 1000000
