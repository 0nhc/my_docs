.. ARX5 Documentation documentation master file, created by
   sphinx-quickstart on Sat Mar 25 16:20:06 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Documentation
+++++++++++++

Welcome to the ARX5 robotic arm, an advanced and versatile robotic arm designed for a range of industrial and research applications. This document contains information about the installation, operation, and maintenance of the ARX5 robotic arm.

To utilize the ARX5 robotic arm's API, developers will need to have a basic understanding of the `Kinematics Dynamics Library (KDL) <https://www.orocos.org/kdl.html>`_ software, which provides the necessary kinematic and dynamic calculations to control the arm's movements.

.. raw:: html

    <div style="position: relative; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe width="720" height="540" src="https://www.youtube.com/watch?v=NW4ZE4y-8hU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
    
.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Introduction
   
   Installation <introduction/installation>
   Preparing for Startup <introduction/preparation>
   Specifications <introduction/specifications>
   
.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: How to Use
   
   Environment Configuration <how_to_use/environment>
   Using the SDK <how_to_use/sdk>
   ROS Packages <how_to_use/ros>
   Simulation <how_to_use/simulation>
   
.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Appendix

   API Reference <appendix/api>
