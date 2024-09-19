# https://evjang.com/2024/08/31/motors.html

1X released a teaser video of NEO Beta to the world yesterday.

## Watch the video

[![YouTube Video]](https://www.youtube.com/watch?v=bUrLuUxv9gE&pp=ygVIeXQxcy5jb20gLSBJbnRyb2R1Y2luZyBORU8gQmV0YSAgQSBIdW1hbm9pZCBSb2JvdCBmb3IgdGhlIEhvbWVfMTA4MHAubXA0)

You may notice that despite its agility, NEO moves quietly. If you turn up the volume on the video, you can hear the gentle whirring of motors as it leans down and picks up the backpack. Industrial robots can move fast, but need to be slowed to a crawl before they touch anything and also need to be kept in safety cages. Meanwhile, NEO can gently make contact with the person in the video. How is this possible?

This blog post is a tutorial on motor inertia and the non-intuitive dynamics of geared actuators. Despite programming robots for 6 years at Google, I didn’t fully understand why this was so important until I joined 1X. After going through the physics calculations myself, I came away with a much deeper conviction that lightweight, high torque motors are a key ingredient to building general-purpose learning robots.


# Collision Involving a Single Motor

Imagine a wheel weighing 3kg with radius 0.4 meters spinning at 5 radians per second, with lever arm protruding from it. The lever arm comes into contact with a block that is fixed in place. Let’s assume the collision is perfectly inelastic, which means that the wheel is forced to come to a halt after the collision instead of bouncing off the block. To simplify calculations, let’s assume the lever arm has no mass and merely exists to stop the rotation of the wheel.

# check the calculator here https://evjang.com/2024/08/31/motors.html

Before the collision, rotational kinetic energy is given by ½ I ω² where I is the moment of inertia and ω is the angular velocity (rad/sec). We are assuming the lever arm is massless and therefore the inertia of the system is equivalent to a fixed cylinder: ½ mr², or (½)(3)(0.4)² = 0.24 kg m². With I=0.24 kg m² and ω = 5 rad/s, this comes out to 3 joules. You can play with the input fields to the javascript calculator I wrote above to calculate the kinetic energies for various wheel speeds and masses.

If the block was drifting in space, the wheel and block would collide and then move together at some new angular velocity, due to conservation of angular momentum. In an inelastic collision, the total momentum of the wheel and block would be conserved while their total kinetic energy is not. The wheel and block would have less than 3 joules of kinetic energy resulting from its new velocity (implied by its new momentum).

Energy is a conserved quantity, so where did the rest of the kinetic energy go? The answer is that some of the kinetic energy is conserved as motion, but the remainder of kinetic energy is dissipated as heat, sound and internal material deformation. When you hear a robot making loud noises while moving, that’s due to the inefficiencies in the transmission turning mechanical work into wasted energy (sound).

In our example the block is prevented from moving. The new velocity is 0, the new kinetic energy is also 0, which implies that forcing the wheel to come to rest requires all the kinetic energy to be dissipated. Fortunately, 3 joules is not a lot of energy; it is the equivalent of a small-medium dog (6 kg) trotting into you (1 m/s) and coming to a stop.

# Collision Involving a Gearbox

Now let’s consider a slightly modified system: there are two wheels, each with mass 1.5 kg (summing up to a total mass of 3 kg) and radius 0.4m. One wheel spins at 5 radians/sec and collides with the fixed block. The second wheel is spinning 10 times faster, driving the first wheel through a gear mechanism. This is also known as a 10:1 gear reduction, because this mechanism reduces the final output speed of the lever arm.

The kinetic energy of the system is the sum of rotational kinetic energies of both wheels. Like before, the system comes to rest upon collision, so all the kinetic energy must be dissipated as heat, noise, and material deformation. Here is an updated calculator:

# check the calculator here https://evjang.com/2024/08/31/motors.html

Ouch! Even though the lever arm contacts the block at the same speed as before, the total kinetic energy (150 joules) is 50x larger than the original 1-wheel system! If the gear ratio were increased to 100, as is the case in some robotic gearboxes, then the total kinetic energy to be dissipated is now 15,000 joules. This is roughly equivalent to a baseball (0.149 kg) colliding with you at 1000 miles per hour. Anything that lever arm collides with at those speeds will be destroyed (along with the gears themselves).

The safety properties of geared systems may seem a little counter-intuitive at first, because the final output speed of the lever remains the same and the total mass of the wheels (3kg) is the same. If you enclosed the wheels in a box and recorded a video of both lever arms spinning around unimpeded, you could not see the difference just by watching the video. However, big differences arise once contact with the world occurs, especially unplanned contact.

The physics of spinning motors have a lot of safety implications for humanoid robots making contact with the world. Most humanoid companies choose to deploy their robots in factories rather than homes because they rely on stiff, highly-geared actuation systems. These systems are not safe around people and must be fenced off in a cage. If you want the end effector of a geared robot to move swiftly, it means that on the other side of the limb and gear inside the robot, there is a motor that is spinning many times faster than the end effector is moving. Because kinetic energy is proportional to angular velocity squared, the inertia of the fast spinning gear actually dominates the dynamics of the robot, rather than the robot link itself. Russ Tedrake has a great explanation of these counterintuitive robot dynamics in his Robotic Manipulation course at MIT.

Why do people still use gears if they are so high-energy and unsafe? The reason is that gearboxes provide needed mechanical leverage: most motors are not strong enough to apply lots of torque, so mechanical engineers take a high-speed motor and add a gear to it to trade of speed for torque. Such geared systems are known as “stiff” and not “backdriveable”, since it takes a lot more work to resist the fast motor spinning at the other side of the gearbox.

Fortunately, 1X has invested the last decade in building high torque, low-speed motors, because our goal is to maximize the physical safety of our actuation systems. Our motors and actuation systems have low gear ratios and are lightweight, allowing us to be the first humanoid robot company to ship robots into homes and safely operate them around people.

# Implications for Retargeting Human Videos

An often-mentioned idea in robotic learning is to train policies on first-person videos of humans performing tasks instead of only collecting robot data. The train of thought goes something like this:

Data is a bottleneck to scaling general-purpose robots, but robot hardware is expensive and paying human teleoperators to do tasks with clunky hardware is also expensive. The throughput on teleoperation is usually several times slower than the human just doing the task.
If we strap head-mounted cameras to humans just going about their lives and have them wear big rubber gloves that hide their flesh, we can collect a large dataset of people doing all kinds of chores and tasks very quickly. The average person does so many diverse locomotion and manipulation tasks throughout their day without even thinking about it. Although the raw motor outputs are not easily sensed, actions can be recovered by inferring the change in pose during the video. This form of data collection unblocks general purpose robots while we wait for hardware to catch up.
There are many first-person and third-person videos on the Internet, so we can further scale up data by training on any video clip with a person doing something in it.
Once we learn a really good video-to-future-pose predictor, we can build a robot that wears the same head-mounted camera and rubber gloves, so that it can autonomously perform all the tasks.
Before you go and scale this type of data collection up, consider that humans and animals are much safer than robots because we don’t have fast-spinning parts inside of our bodies. Muscles have very low kinetic energy compared to a 5000 RPM motor, and we carry much less effective mass when moving around. Although the robot might be able to approximately match the joint angles of a human, it may have too much effective mass (via spinning motors) to perform agile tasks like flipping open light switches without looking or running gracefully. You may find that even if you can train a great position-controlled policy, your robot cannot apply those actions at human-speed because the forces it would apply upon contact would be so different from those of a human.

# If you want to turn videos of humans moving quickly into policies, you may need one or more of the following:

A very compliant and agile robot like NEO
Have the robot track the kinematic trajectories of video at slower than 1x speed, without attempting to track the dynamics of “human hardware”. This only works on static manipulation tasks and has many issues for contact-rich tasks like folding clothes and prepping food in a kitchen.
Decouple kinematic planning and dynamic planning, where kinematic plans optimize for matching the target positions while dynamic planning optimizes to match contact forces upon collision.