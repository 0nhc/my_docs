Rate
====

The Rate class uses the chrono library in C++ to implement timing, which
is used to achieve fixed-frequency control in a loop.

.. code:: cpp 

   #ifndef RATE_H_
   #define RATE_H_

   #include <chrono>
   #include <thread>

   class Rate {
   public:
       Rate(double frequency);

       void sleep();

   private:
       std::chrono::time_point<std::chrono::high_resolution_clock> last_time_;
       std::chrono::duration<double> expected_cycle_time_;
   };

   #endif // RATE_H_

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
