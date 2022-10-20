# week-4-lab
### This is the installation of Turtlesim
- Step 1 we have to make our system always up-to-date
```
sudo apt update
```
![image](https://user-images.githubusercontent.com/95737530/195253041-824edf0c-1e0c-4c04-8eb9-d8f899296c1e.png)
- Step 2 Installing the turtlesim library
```
sudo apt install ros-rolling-turtlesim
```
![image](https://user-images.githubusercontent.com/95737530/195253482-f99daf75-2344-4296-97b0-1483f82bb005.png)
- Step 3 checking package is installed or not 
```
ros2 pkg executables turtlesim
```
![image](https://user-images.githubusercontent.com/95737530/195253945-021295d2-2850-4fa6-8026-44fdbd870162.png)
- commanding by terminal 
```
ros2 run turtlesim turtlesim_node
```
![image](https://user-images.githubusercontent.com/95737530/195254474-6eb4c9f6-ab09-44b6-9893-22875c1f9201.png)
- turtle appears in the center of window   
![image](https://user-images.githubusercontent.com/95737530/195254624-47fe5235-7227-4794-b8e2-ffa3f816764d.png)
- this is a command which used for moving turtle 
![image](https://user-images.githubusercontent.com/95737530/195255087-cef5a9d1-ce33-4c81-be8b-08a129f033b9.png)
- We can control turtle with arrow keys
```
ros2 run turtlesim turtle_teleop_key
```

[Screencast from 10-12-2022 02:12:36 PM.webm](https://user-images.githubusercontent.com/95737530/195261460-1fb2205d-f27c-47c1-839c-b945ef078106.webm)
### Installation of RQT
- Step 1 Always make it up to date 
```
sudo apt update
```
![image](https://user-images.githubusercontent.com/95737530/195263029-580599aa-08d6-4094-b109-2e5d4928e2e0.png)
- Step 2 Install the rqt library and its plugins
```
sudo apt install ~nros-rolling-rqt*
```
![image](https://user-images.githubusercontent.com/95737530/195263370-ab619f4d-5c22-430d-81d5-31a6f5b24b65.png)
- Step 3 Using rqt
```
rqt
```
![image](https://user-images.githubusercontent.com/95737530/195264887-71516f39-bb9b-4cb9-8f52-dedc30aaa59a.png)
- To start rqt_console in a new terminal with the following command
```
ros2 run rqt_console rqt_console
```

![image](https://user-images.githubusercontent.com/95737530/195265221-8ee66513-cb92-42e0-9958-e53433fb6fb4.png)
# ROS 2 Calcon
- Step 1 Installing calcon extensions
```
sudo apt install python3-colcon-common-extensions
```

![image](https://user-images.githubusercontent.com/95737530/195266583-6d871c25-3d74-4af4-bacc-ccbc8cec94ae.png)
- Step 2 Creating workspace
```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```
![image](https://user-images.githubusercontent.com/95737530/195267230-459851a6-23c9-4698-b5c9-499cbd266f4f.png)
- Add some sources
```
git clone https://github.com/ros2/examples src/examples -b humble
```
- build the workspace 
```
colcon build --symlink-install
```
![image](https://user-images.githubusercontent.com/95737530/196668869-b923eaf4-efb2-458b-8327-e7d4243b25a9.png)
- Source the environment
```
. install/setup.bash
```
- Setup
  -The command colcon_cd allows you to quickly change the current working d
irectory of your shell to the directory of a package.
```
echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=/opt/ros/rolling/" >> ~/.bashrc
```
## Creating workspace
- Step 1 Source ROS 2 environment
```
source /opt/ros/rolling/setup.bash
```
- Step 2 Create a new Directory
 ```
 mkdir -p ~/ros2_ws/src
 cd ~/ros2_ws/src
 ```
 - Step 3 Clone the Github repo
 ```
 git clone https://github.com/ros/ros_tutorials.git -b rolling-devel
 ```
 - Step Resolve dependencies
 ```
 rosdep install -i --from-path src --rosdistro rolling -y
 ```


- 1 Create a package
```
ros2 pkg create --build-type ament_python py_pubsub
```
- 2 Write the publisher node
```
wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/min
imal_publisher/examples_rclpy_minimal_publisher/publisher_member_functio
n.py
```
### This a python file 
```
import rclpy
from rclpy.node import Node

from std_msgs.msg import String


class MinimalPublisher(Node):

    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.i = 0

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello World: %d' % self.i
        self.publisher_.publish(msg)
        self.get_logger().info('Publishing: "%s"' % msg.data)
        self.i += 1


def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_publisher.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```
### 2.1 Testing the code 
The first lines of code after the comments import rclpy so its Node class can be used.
```
import rclpy
from rclpy.node import Node
```
The next statement imports the built-in string message 
type that the node uses to structure the data that it passes on the topic.
```
from std_msgs.msg import String
```
Next, the MinimalPublisher class is created, which inherits from (or is a subclass of) Node.
```
class MinimalPublisher(Node):
```
Next, a timer is created with a callback to execute every 0.5 seconds. 
self.i is a counter used in the callback.
```
def __init__(self):
    super().__init__('minimal_publisher')
    self.publisher_ = self.create_publisher(String, 'topic', 10)
    timer_period = 0.5  # seconds
    self.timer = self.create_timer(timer_period, self.timer_callback)
    self.i = 0
```
timer_callback creates a message with the counter value appended, 
and publishes it to the console with get_logger().info.
```
def timer_callback(self):
    msg = String()
    msg.data = 'Hello World: %d' % self.i
    self.publisher_.publish(msg)
    self.get_logger().info('Publishing: "%s"' % msg.data)
    self.i += 1
```
Lastly, the main function is defined.
```
def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_publisher.destroy_node()
    rclpy.shutdown(
```
First the rclpy library is initialized, then the node is created, and then 
it “spins” the node so its callbacks are called.

  ### 2.2 Add dependencies
  
  As mentioned in the previous tutorial, make sure to fill in the 
  <description>, <maintainer> and <license> tags:
```
  <description>Examples of minimal publisher/subscriber using rclpy</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```
  After the lines above, add the following dependencies corresponding 
  to node’s import statements:
```
  <exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```
  This declares the package needs rclpy and std_msgs when its code is executed.
  
Make sure to save the file.
  ### 2.3 Add an entry point
  
  Open the setup.py file. Again, match the maintainer, maintainer_email, 
  description and license fields to your package.xml:
```
  maintainer='YourName',
maintainer_email='you@email.com',
description='Examples of minimal publisher/subscriber using rclpy',
license='Apache License 2.0',
```
```
  entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
        ],
},
```
  Don’t forget to save.
  
  ### 2.4 Check setup.cfg

The contents of the setup.cfg file should be correctly populated automatically, like so:
```
[develop]
script-dir=$base/lib/py_pubsub
[install]
install-scripts=$base/lib/py_pubsub
```
This is simply telling setuptools to put your executables in lib, 
  because ros2 run will look for them there.
  
## 3 Write the subscriber node
  
 Return to ros2_ws/src/py_pubsub/py_pubsub to create the next node. 
  Enter the following code in terminal
```
  wget https://raw.githubusercontent.com/ros2/examples/foxy/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py

```
  ![image](https://user-images.githubusercontent.com/95737530/196692059-1fce0000-1fcb-44de-8f98-0219b5876d36.png)
Now the directory should have these files:
```
  __init__.py  publisher_member_function.py  subscriber_member_function.py
```
  ### 3.1 Examine the code
    
Open the subscriber_member_function.py with  text editor.
```
  import rclpy
  from rclpy.node import Node

from std_msgs.msg import String
class MinimalSubscriber(Node):

    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            String,
            'topic',
            self.listener_callback,
            10)
        self.subscription  # prevent unused variable warning

    def listener_callback(self, msg):
        self.get_logger().info('I heard: "%s"' % msg.data)


    def main(args=None):
    rclpy.init(args=args)

    minimal_subscriber = MinimalSubscriber()

    rclpy.spin(minimal_subscriber)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_subscriber.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```
The subscriber node’s code is nearly identical to the publisher’s. 
  The constructor creates a subscriber with the same arguments as the 
  publisher. Recall from the topics tutorial that the topic name and message 
  type used by the publisher and subscriber must match to allow them to communicate.
    
```
  self.subscription = self.create_subscription(
    String,
    'topic',
    self.listener_callback,
    10)
```
  he callback definition simply prints an info message to 
  the console, along with the data it received. 
  Recall that the publisher defines msg.data = 'Hello World: %d' % self.i
    
```
  def listener_callback(self, msg):
    self.get_logger().info('I heard: "%s"' % msg.data)
```
  The main definition is almost exactly the same, replacing 
  the creation and spinning of 
  the publisher with the subscriber.
    
```
    minimal_subscriber = MinimalSubscriber()

    rclpy.spin(minimal_subscriber)
```
Since this node has the same dependencies as the publisher, there’s 
  nothing new to add to package.xml. The setup.cfg file can also remain untouched.
  
### 3.2 Add an entry point

Reopen setup.py and add the entry point for the subscriber node below the 
  publisher’s entry point. The entry_points field should now look like this:
```
 entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
                'listener = py_pubsub.subscriber_member_function:main',
        ],
},
    
```
    
 Make sure to save the file, and then pub/sub system should be ready for use.
    
  ### 4 Build and run
    
It’s good practice to run rosdep in the root of your workspace (ros2_ws) 
    to check for missing dependencies before building
  
```
    rosdep install -i --from-path src --rosdistro foxy -y
```
 Still in the root of  workspace, ros2_ws, build  new package
```
    colcon build --packages-select py_pubsub
```
    
![image](https://user-images.githubusercontent.com/95737530/196710262-1d8e7cbb-c3d8-4f32-93a3-f05eac6e0890.png)

Open a new terminal, navigate to ros2_ws, and source the setup files
    
```
    . install/setup.bash
```
Now run the talker node:
```
    ros2 run py_pubsub talker
```
Open another terminal, source the setup files 
    from inside ros2_ws again, and then start the listener node:
```
    ros2 run py_pubsub listener
```
With combination Ctrl+C in each terminal to stop the nodes from looping.

## Creating an action
- Set up a workspace and create a package named action_tutorials_interfaces:
```
mkdir -p ros2_ws/src #you can reuse existing workspace with this naming convention
cd ros2_ws/src
ros2 pkg create action_tutorials_interfaces
```
![image](https://user-images.githubusercontent.com/95737530/196892969-b634b2cf-7094-4ff1-a727-0c9be4ef26e2.png)
## Tasks
### 1 Denfining an action

Say we want to define a new action “Fibonacci” for computing the Fibonacci sequence.

Create an action directory in our ROS 2 package action_tutorials_interfaces:

```
cd action_tutorials_interfaces
mkdir action
```
Within the action directory, create a file called Fibonacci.action with the following contents:
```
int32 order
---
int32[] sequence
---
int32[] partial_sequence
```
The goal request is the order of the Fibonacci sequence we want to compute,
the result is the final sequence, and the feedback is the partial_sequence computed so far.
### 2 Building an action
This is accomplished by adding the following lines to our CMakeLists.txt before the 
ament_package() line, in the action_tutorials_interfaces:
![image](https://user-images.githubusercontent.com/95737530/196898650-06159041-5628-4a89-8aea-b47d23d1a4be.png)
```
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "action/Fibonacci.action"
)
```
We should also add the required dependencies to our package.xml:

```
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<depend>action_msgs</depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```
Note, we need to depend on action_msgs since action definitions include additional metadata (e.g. goal IDs).

We should now be able to build the package containing the Fibonacci action definition:
```
# Change to the root of the workspace
cd ~/ros2_ws
# Build
colcon build
```
![image](https://user-images.githubusercontent.com/95737530/196901556-6f221171-6175-4047-98d3-2edd017e124a.png)
We’re done!

By convention, action types will be prefixed by their package name and the word action. 
So when we want to refer to our new action, it will have the full name action_tutorials_interfaces/action/Fibonacci.
```
# Source our workspace
# On Windows: call install/setup.bat
. install/setup.bash
# Check that our action definition exists
ros2 interface show action_tutorials_interfaces/action/Fibonacci
```
![image](https://user-images.githubusercontent.com/95737530/196929945-67289c47-4c2c-4f87-9363-e27c102a1ce5.png)

## Writing an action server and client (Python)

### Step  1 Writing an action server
Open a new file in  home directory, let’s call it fibonacci_action_server.py, and add the following code:
```
import rclpy
from rclpy.action import ActionServer
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionServer(Node):

    def __init__(self):
        super().__init__('fibonacci_action_server')
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)

    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')
        result = Fibonacci.Result()
        return result


def main(args=None):
    rclpy.init(args=args)

    fibonacci_action_server = FibonacciActionServer()

    rclpy.spin(fibonacci_action_server)


if __name__ == '__main__':
    main()
 ```
Line 8 defines a class FibonacciActionServer that is a subclass of Node. The class is initialized 
by calling the Node constructor, naming our node fibonacci_action_server
```
        super().__init__('fibonacci_action_server')
```
In the constructor we also instantiate a new action server:
```
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)
```
An action server requires four arguments:

    A ROS 2 node to add the action client to: self.

    The type of the action: Fibonacci (imported in line 5).

    The action name: 'fibonacci'.

    A callback function for executing accepted goals: self.execute_callback. This callback must return a result message for the action type.

An action server requires four arguments:
- A ROS 2 node to add the action client to: self.
- The type of the action: Fibonacci (imported in line 5).
- The action name: 'fibonacci'.
- A callback function for executing accepted goals: self.execute_callback. This callback must return a result message for the action type.
We also define an execute_callback method in our class:

```
    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')
        result = Fibonacci.Result()
        return result
```
Now if you restart the action server and send a
nother goal, you should see the goal finished with the status SUCCEEDED.

Now let’s make our goal execution actually compute and return the requested Fibonacci sequence:
```
def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')

        sequence = [0, 1]

        for i in range(1, goal_handle.request.order):
            sequence.append(sequence[i] + sequence[i-1])

        goal_handle.succeed()

        result = Fibonacci.Result()
        result.sequence = sequence
        return result
```
After computing the sequence, we assign it to the result message field before returning
## 1.2 Publishing feedback

We’ll replace the sequence variable, and use a feedback message to store the sequence instead.
After every update of the feedback message in the for-loop, we publish the feedback message and sleep for dramatic effect:
```
import time


import rclpy
from rclpy.action import ActionServer
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionServer(Node):

    def __init__(self):
        super().__init__('fibonacci_action_server')
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)

    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')


        feedback_msg = Fibonacci.Feedback()

        feedback_msg.partial_sequence = [0, 1]


        for i in range(1, goal_handle.request.order):

            feedback_msg.partial_sequence.append(

                feedback_msg.partial_sequence[i] + feedback_msg.partial_sequence[i-1])

            self.get_logger().info('Feedback: {0}'.format(feedback_msg.partial_sequence))

            goal_handle.publish_feedback(feedback_msg)

            time.sleep(1)


        goal_handle.succeed()

        result = Fibonacci.Result()

        result.sequence = feedback_msg.partial_sequence

        return result


def main(args=None):
    rclpy.init(args=args)

    fibonacci_action_server = FibonacciActionServer()

    rclpy.spin(fibonacci_action_server)


if __name__ == '__main__':
    main()
```
After restarting the action server, we can confirm that feedback is now published by 
using the command line tool with the --feedback option
```
ros2 action send_goal --feedback fibonacci action_tutorials_interfaces/action/Fibonacci "{order: 5}"
```
## 2 Writing an action client
We’ll also scope the action client to a single file. Open a new file, let’s call it fibonacci_action_client.py, and add the following boilerplate code:
```
import rclpy
from rclpy.action import ActionClient
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionClient(Node):

    def __init__(self):
        super().__init__('fibonacci_action_client')
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')

    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        return self._action_client.send_goal_async(goal_msg)


def main(args=None):
    rclpy.init(args=args)

    action_client = FibonacciActionClient()

    future = action_client.send_goal(10)

    rclpy.spin_until_future_complete(action_client, future)


if __name__ == '__main__':
    main()
```
We’ve defined a class FibonacciActionClient that is a subclass of Node. 
The class is initialized by calling the Node constructor, naming our node fibonacci_action_client:
```
        super().__init__('fibonacci_action_client')
```
Also in the class constructor, we create an action client using the custom action definition from the previous tutorial on Creating an action:
```
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')
```
We create an ActionClient by passing it three arguments:

- A ROS 2 node to add the action client to: self
- The type of the action: Fibonacci
- The action name: 'fibonacci'

Our action client will be able to communicate with action servers of the same action name and type.
We also define a method send_goal in the FibonacciActionClient class:
```
    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        return self._action_client.send_goal_async(goal_msg)
```
Finally, we call main() in the entry point of our Python program.

Let’s test our action client by first running the action server built earlier:
```
python3 fibonacci_action_server.py
```
In another terminal, run the action client:
```
python3 fibonacci_action_client.py
```
These messages printed by the action server as it successfully executes the goal
```
[INFO] [fibonacci_action_server]: Executing goal...
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3, 5])
# etc.
```
![image](https://user-images.githubusercontent.com/95737530/196942152-e0986b58-00b2-40ca-aed2-84c239db961b.png)





