
urx is a python library to control a robot from 'Universal robot'. It is published under the GPL license and comes with absolutely no guarantee, although it has been used in many application with several version of UR5 and UR10 robots.

It is meant as an easy to use module for pick and place operations, although it has been used for welding and other sensor based applications that do not require high update rate.

Both the 'secondary port' interface and the real-time/matlab interface of the UR controller are used. urx can optionally use the [python-math3d](https://launchpad.net/pymath3d) library to receive and send transformation matrices to the robot urx is known to work with all release robots from Universal Robot.

urx was primarily developed by [Olivier Roulet-Dubonnet](https://github.com/oroulet) for [Sintef Raufoss Manufacturing](http://www.sintef.no/manufacturing/).

#Example use:

```python
import urx

rob = urx.robot("192.168.0.100")
rob.set_tcp((0, 0, 0.1, 0, 0, 0))
rob.set_payload(2, (0, 0, 0.1))
rob.movej((1, 2, 3, 4, 5, 6), a, v) 
rob.movel((x, y, z, rx, ry, rz), a, v)
print "Current tool pose is: ",  rob.getl()
rob.movel((0.1, 0, 0, 0, 0, 0), a, v, relative=true)# move relative to current pose
rob.translate((0.1, 0, 0), a, v) #move tool and keep orientation
rob.stopj(a)

robot.movel(x, y, z, rx, ry, rz), wait=False)
while True :
    sleep(0.1) #sleep first since the robot may not have processed the command yet
    if robot.is_program_running():
        break

robot.movel(x, y, z, rx, ry, rz), wait=False)
while.robot.getForce() < 50:
    sleep(0.01)
    if not robot.is_program_running():
        break
robot.stopl()

try:
    robot.movel((0,0,0.1,0,0,0), relative=True)
except RobotError, ex:
    print "Robot could not execute move (emergency stop for example), do something", ex
```

#Development using Transform objects from math3d library:

```python
robot = Robot("192.168.1.1")
robot.set_tcp((0,0,0.23,0,0,0)
robot.csys.orient.rotate_zb(pi/4) #just an example
trans = robot.get_pose() # get current transformation matrix (tool to base)
trans.orient.rotate_yt(pi/2)
robot.set_pose(trans)
trans.pos += math3d.Vector(0,0,0.3)
robot.set_pose(trans)


#or only work with orientation part
o = robot.get_orientation()
o.rotate_yb(pi)
robot.set_orientation(o)
```

#Robotiq Gripper

urx can also control a Robotiq gripper attached to the UR robot.  The robotiq class was primarily developed by [Mark Silliman](https://github.com/markwsilliman).

##Example use:

```python
import sys
import urx
from urx.robotiq_two_finger_gripper import Robotiq_Two_Finger_Gripper

if __name__ == '__main__':
	rob = urx.Robot("192.168.0.100")
	robotiqgrip = Robotiq_Two_Finger_Gripper()

	if(len(sys.argv) != 2):
		print "false"
		sys.exit()

	if(sys.argv[1] == "close") :
		robotiqgrip.close_gripper()
	if(sys.argv[1] == "open") :
		robotiqgrip.open_gripper()

	rob.send_program(robotiqgrip.ret_program_to_run())

	rob.close()
	print "true"
	sys.exit()
```