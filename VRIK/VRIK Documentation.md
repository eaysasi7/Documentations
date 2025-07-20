## VRIK Inspector Breakdown

<img class="image-style-align-right image_resized" style="aspect-ratio:378/112;width:49.64%;" src="[api/attachments/Tv2yBZfOhyTM/image/image.png](https://github.com/eaysasi7/Documentations/blob/main/VRIK/images/vrikbasic.png)" width="378" height="112">

There are 3 main sections

1.  Basic Settings
2.  References
3.  Solver

### 1\. Basic Settings

This first section gives you the Script, which cannot be edited and this has the same behavior as any other scripts added as a component to a game object like these Input Action Manager and Progress Bar scripts.

<figure class="table"><table><tbody><tr><td><figure class="image image-style-align-center"><img style="aspect-ratio:378/121;" src="api/attachments/RMskN2EKoDhu/image/image.png" width="378" height="121"></figure></td><td><figure class="image image-style-align-center image_resized" style="width:87.42%;"><img style="aspect-ratio:381/115;" src="api/attachments/HyXJ81WKiNIj/image/image.png" width="381" height="115"></figure></td></tr></tbody></table></figure>

<figure class="image image-style-align-right image_resized" style="width:46.73%;"><img style="aspect-ratio:373/70;" src="api/attachments/0bygEQHXnAB8/image/image.png" width="373" height="70"></figure>

Next is the **Fix Transforms** checkbox. This checkbox is mostly concerned with rigs that have animations, but it's advisable to keep this box checked, regardless.

If checked, the IK Solver will store the “initial state” of the transforms before the Update() method is called for the _next_ frame. The “initial state” of the transforms is the position, rotation, and scale of the transform in the _current_ frame. Therefore, the necessary movements/changes to the transforms will solve based on the state of the transform from the previous frame, whether in the middle of an animation or not. Some animations do not have data for all transforms in your rig. Fix Transforms ensures that all transforms are subject to the IK solver regardless of their inclusion in an animation which could override their position. This keeps all transforms looking and acting natural.

### 2\. References

This section is where you assign the bones of your humanoid rig by matching their functions, as closely as possible, to the given labels within the 5 sections of Spine, Left Arm, Right Arm, Left Leg, and Right Leg. These bone references are used in the **Solver** later on.

<figure class="table"><table><tbody><tr><td><p style="text-align:center;">Your Rig</p></td><td><p style="text-align:center;">VRIK References</p></td></tr><tr><td><figure class="image image_resized" style="width:82.82%;"><img style="aspect-ratio:220/355;" src="api/attachments/AYye6pRUpp0r/image/image.png" width="220" height="355"></figure></td><td><figure class="image image_resized" style="width:62.67%;"><img style="aspect-ratio:370/645;" src="api/attachments/9UOGgkcuQ21N/image/image.png" width="370" height="645"></figure></td></tr></tbody></table></figure>

### 3\. Solver

This section has settings and target references that the IK Solver will use to perform the inverse kinematics. Like the **References** section, this has several sub-sections to control the behavior of the humanoid rig. Each sub-section is detailed below.

<img src="api/attachments/VpmmuRx7mm3M/image/image.png" width="366" height="223">

#### Solver Settings

These are more general settings that affect how the Solver will interact with the rig, overall.

**IK Position Weight**

This is a slider from 0 to 1. The value 1 indicates that the IK Solver has the greatest effect on the rig's bones, while 0 indicates that the IK solver will have no effect on the bones.

**LOD**

LOD stands for Level of Detail. This slider has 3 values: 0, 1, 2. LOD setting affects how much computation load you are willing to use. 

*   LOD 0: Fullest quality solving.
*   LOD 1: Middle quality solving. Reduced shoulder and spine solving, and stretching of planted feet is disabled. This provides about a 30% performance gain.
*   LOD 2: Lowest quality solving. Essentially all solving is disabled, except , if Locomotion is enabled, root position and rotation will be updated.

**Scale**

This is a float field that scales the calculations of the solver to the size of your character rig. A scale of 1 is supposed to be equivalent to the “average adult size”. If your model is large and is approximately 3 times larger than the average adult size, you would put 3 in this field to scale the IK up to the size of your model.

**Plant Feet**

This setting is a checkbox. If checked, it will adhere the toe bones of the rig to the “floor”. The floor is where y = 0. 

#### Spine

This section defines **targets** and settings related to those targets for bones along the Spine. The targets are game objects whose positions and rotations that the bones of your rig will try to line up with as much as possible, even to the point to where the mesh of the rig will stretch unnaturally to reach the targets. It's behavior is very similar to the behavior of parent-child game object relationships, where the target acts like the parent of the bone in your rig.

<figure class="image image-style-align-right image_resized" style="width:39.75%;"><img style="aspect-ratio:354/604;" src="api/attachments/3kDt6RZzEzII/image/image.png" width="354" height="604"></figure>

**Head**

Head Target: This is a game object in your scene that the head bone will try to align with as closely as possible in position and rotation.

It is easiest to assign the Main Camera under your XR Rig. The camera that is acting as your eyes in the scene. However, this can put your view inside of your avatar's head mesh and the head mesh will block your view of your scene. It is good practice to make an empty game object, name it something relevant like “Head Target”, make it a child of the main camera (this is because the Head Target game object will match the rotation and position of the main camera), and slightly offset it from the main camera, slightly behind So that the main camera won't intersect with the head mesh and block your view while in VR.

Position Weight: A float from 0 to 1, here, dictates how closely the Head Bone will track to the Head Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Rotation Weight: A float from 0 to 1, here, dictates how closely the Head Bone will track to the Head Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

Head Clamp Weight: A float from 0 to 1. This setting is an allowance of rotation in degrees relative to the Head Target. A value of 0.5 will allow 90 degrees of rotation relative to the Head Target. A value of 0 allows 180 degrees of rotation relative to the Head Target. And a value of 1 completely locks the head rotation to that of the Head Target.

Min Head Height: A float. This is a measure of the head bone height relative to the root of the avatar on the Y axis. This is meant to hold the head up at least by the amount specified in the field, as a float, to prevent the spine from collapsing unnaturally.

Use Animated Head Height Weight: A float from 0 to 1, that is an allowance for animations to act on the avatar while adhering to the Head Target. 0 being not affected by the animation and 1 being most affected by the animation. This means that the head will be able to move away from the Head Target in the Y axis while an animation is acting on the character.

If this setting is anything other than 0, two more options will appear: 

Use Animated Head Height Range: A float. This value sets a threshold for how close the <img class="image-style-align-right image_resized" style="aspect-ratio:564/192;width:48.53%;" src="api/attachments/q1lRL4D49Ajp/image/image.png" width="564" height="192">Head Target’s height needs to be to the head bone’s height. If the difference between them is smaller than the number in this field, the system will use the Head Bone’s height as the Head Target’s Y position during the animation.

Animated Head Height Blend: A float. This value sets a buffer zone beyond the 'Use Animated Head Height Range.' If the difference between the Head Target’s height and the Head Bone’s height exceeds the 'Use Animated Head Height Range' plus the value in this field, the head will lock back to the Head Target’s Y position.

**Pelvis**

Pelvis Target: This is a game object in your scene that the pelvis bone will try to align with as closely as possible in position and rotation. You do not need to have anything in this setting, but it can help with tracking and natural movement.

<figure class="image image-style-align-right image_resized" style="width:46.96%;"><img style="aspect-ratio:471/117;" src="api/attachments/nReVmC7uu73n/image/image.png" width="471" height="117"></figure>

Pelvis Position Weight: A float from 0 to 1, here, dictates how closely the Head Bone will track to the Pelvis Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Pelvis Rotation Weight: A float from 0 to 1, here, dictates how closely the Head Bone will track to the Pelvis Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

Maintain Pelvis Position: A float from 0 to 1, that is an allowance for animations to act on the avatar while adhering to the Pelvis Target. 0 being not affected by the animation and 1 being most affected by the animation. This means that the pelvis will be able to move away from the Pelvis Target while an animation is acting on the character.

**Chest**

Chest Goal: This is a game object in your scene that the Chest Bone will try to turn towards by rotating on the X, Y, and Z axes. You do not need to have anything in this setting, but it can help with tracking and natural movement.

<figure class="image image-style-align-right image_resized" style="width:46.87%;"><img style="aspect-ratio:472/121;" src="api/attachments/GOrsfohR8tXv/image/image.png" width="472" height="121"></figure>

Chest Goal Weight: A float from 0 to 1, here, dictates how closely the Chest Bone will rotate on X, Y, and Z axes to face the Chest Goal game object. 0 being not at all and 1 being the most affected. 

Chest Clamp Weight: A float from 0 to 1. This setting is an allowance of rotation in degrees relative to the Head Bone. A value of 0.5 will allow 90 degrees of rotation relative to the Head Bone. A value of 0 allows 180 degrees of rotation relative to the Head Bone. And a value of 1 completely locks the chest rotation relative to the Head Bone, making them act more as one unit.

Rotate Chest By Hands: A float from 0 to 1. This setting dictates how much the chest should rotate in a natural manner relative to where the hands are positioned. A setting of 0 is not affected at all, and 1 is most affected. For example, if your character's hands are above their head, the chest will rotate up slightly; and if the hands are down below the knees and behind the body, the chest will rotate down towards the pelvis slightly. This gives a more natural and less stiff looking movement to the character.

**Spine**

<figure class="image image-style-align-right image_resized" style="width:46.44%;"><img style="aspect-ratio:563/111;" src="api/attachments/RsWhlGld8D1F/image/image.png" width="563" height="111"></figure>

Body Pos Stiffness: A float from 0 to 1. This setting determines how much the body will follow the position of the Head. 0 being not affected, and 1 being most affected.

Body Rot Stiffness: A float from 0 to 1. This setting determines how much the body will follow the rotation of the Head. 0 being not affected, and 1 being most affected.

Neck Stiffness: A float from 0 to 1. This setting determines how much the Chest will match the rotation of the head along the X, Y, and Z axes. 0 being not affected, and 1 being most affected.

Move Body Back When Crouching: A float. This setting determines how far the body is moved backwards when the character is crouching, to prevent intersecting body parts, collision issues, and keep a natural movement look to the character. The higher this number is, the further back the body will move when crouched.

**Root Rotation**

<figure class="image image-style-align-right image_resized" style="width:47.36%;"><img style="aspect-ratio:562/67;" src="api/attachments/6xBmjHKiICC7/image/image.png" width="562" height="67"></figure>

Max Root Angle: A float from 0 to 180. This is the degree of rotation that the Head can rotate before the Root is automatically rotated to match the Head's angle.

Root Heading Offset: A float from -180 to 180. This is the degree of rotation that the Root will maintain relative to the head. This is used, sometimes, in fighting games where a character needs to have an angled fighting stance.

#### Left Arm

<figure class="image image-style-align-right image_resized" style="width:44.64%;"><img style="aspect-ratio:505/468;" src="api/attachments/LCG1FhjCTGFA/image/image.png" width="505" height="468"></figure>

This section defines **targets** and settings related to those targets for bones in the Arm. The targets are game objects whose positions and rotations that the bones of your rig will try to line up with as much as possible, even to the point to where the mesh of the rig will stretch unnaturally to reach the targets. It's behavior is very similar to the behavior of parent-child game object relationships, where the target acts like the parent of the bone in your rig.

**Hand**

Target: This is a game object in your scene that the left hand bone will try to align with as closely as possible in position and rotation. This field should not be left blank. The hand bones need to be able to track the movements of your hands in VR, and this gives a target for the solver to use to create natural looking arm movement.

Position Weight: A float from 0 to 1, here, dictates how closely the Left Hand Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Rotation Weight: A float from 0 to 1, here, dictates how closely the Left Hand Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

**Shoulder**

Shoulder Rotation Weight: A float from 0 to 1. 0 being that the shoulders will not have any freedom of movement and 1 being most reactive to the solver. When the arm is moved the shoulder will move accordingly in a natural manner.

Shoulder Rotation Mode: There are two modes available in a drop-down menu. This determines how the shoulder is constrained and how it is allowed to rotate.

<figure class="image image-style-align-right image_resized" style="width:45.55%;"><img style="aspect-ratio:476/132;" src="api/attachments/OEJlqBMspsJO/image/image.png" width="476" height="132"></figure>

**Yaw Pitch** - Rotates the shoulder using yaw (side-to-side) and pitch (up-and-down) angles, mimicking natural human shoulder movements. This mode is ideal for realistic arm animations but may struggle with extreme poses.

**From To** - Directly rotates the shoulder to align the arm with the target direction, allowing for precise tracking in any orientation. This mode is more flexible but may result in less natural shoulder rotations in some cases.

Shoulder Twist Weight: A float from 0 to 1. This setting controls how much the left shoulder will rotate backwards when the left arm is raised.

Shoulder Yaw Offset: A float. This adjusts the side-to-side (yaw) rotation of the shoulder, in degrees, when aligning the arm with the Hand Target. Positive values rotate the shoulder outward, while negative values rotate it inward. You can use this setting to fine-tune the arm’s horizontal alignment for natural positioning or to correct tracking misalignments.

Shoulder Pitch Offset: A float. This adjusts the up-and-down (pitch) rotation of the shoulder, in degrees, when aligning the arm with the Hand Target. Positive values raise the arm higher, while negative values lower it. You can use this setting to fine-tune the arm’s vertical alignment for natural posture or make adjustments as needed for your specific character rigs.

**Bending**

Bend Goal: This is a game object in your scene that the elbow will attempt to bend towards, depending on the Bend Goal Weight applied. This is used mostly if your character needs to achieve a specific silhouette such as while holding a weapon or if the character is cartoony and always has bent elbows.

Bend Goal Weight: A float from 0 to 1. 0 being not affected at all. 1 being most affected where the elbow is facing towards the Bend Goal game object as closely and directly as possible.

<figure class="image image-style-align-right image_resized" style="width:43.71%;"><img style="aspect-ratio:475/127;" src="api/attachments/Z9rNSNEaLYJu/image/image.png" width="475" height="127"></figure>

Swivel Offset: A float from -180 to 180. This is a measure of degrees by which the arm can rotated around it's own length to align with the hand bone or Hand Target to achieve a natural looking arm and hand rotation. 

Wrist To Palm Axis: A vector 3. This is a way to define the wrist bone’s local axis so that it points toward the palm. You can use this to help correct the hand position, if the hand is rotated at an unnatural angle.

Palm to Thumb Axis: A vector 3. This is a way to define the hand bone’s local axis that points from the palm to the thumb. You can use this to help correct the hand position, if the hand is rotated at an unnatural angle.

**Stretching**

Arm Length Mlp: A float from 0.1 to 2. (The “lp” in “Mlp” stands for Length Multiplier) This setting affects the arm's overall length by displacing the hand and forearm to better align with your own actual arm length to make tracking and arm movement more natural by scaling the arm to align with your own actual arm length. Setting this field to 1 will leave all measurements as they are. Setting less than 1 will make the arm shorter and greater than 1 will make the arm longer.

<figure class="image image-style-align-right image_resized" style="width:49.57%;"><img style="aspect-ratio:475/63;" src="api/attachments/GYV2HVs7NyoH/image/image.png" width="475" height="63"></figure>

Stretch Curve: A curve. This setting defines how much the arm’s bones (upper arm and forearm) stretch or compress based on the distance to the Hand Target. For example, if your VR Controller falls on the floor, the character's arm might stretch to the floor while the controller remains there. If creating a curve for this field, the X-axis represents the normalized distance (target distance divided by natural arm length), and the Y-axis represents the stretch multiplier (e.g., 1.0 for no change, 1.1 for 10% stretching).

#### Right Arm

<figure class="image image-style-align-right image_resized" style="width:45.64%;"><img style="aspect-ratio:506/465;" src="api/attachments/T30eI40yfQKh/image/image.png" width="506" height="465"></figure>

This section defines **targets** and settings related to those targets for bones in the Arm. The targets are game objects whose positions and rotations that the bones of your rig will try to line up with as much as possible, even to the point to where the mesh of the rig will stretch unnaturally to reach the targets. It's behavior is very similar to the behavior of parent-child game object relationships, where the target acts like the parent of the bone in your rig.

**Hand**

Target: This is a game object in your scene that the right hand bone will try to align with as closely as possible in position and rotation. This field should not be left blank. The hand bones need to be able to track the movements of your hands in VR, and this gives a target for the solver to use to create natural looking arm movement.

Position Weight: A float from 0 to 1, here, dictates how closely the Right Hand Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Rotation Weight: A float from 0 to 1, here, dictates how closely the Right Hand Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

**Shoulder**

Shoulder Rotation Weight: A float from 0 to 1. 0 being that the shoulders will not have any freedom of movement and 1 being most reactive to the solver. When the arm is moved the shoulder will move accordingly in a natural manner.

Shoulder Rotation Mode: There are two modes available in this drop-down menu. This determines how the shoulder is constrained and how it is allowed to rotate.

<figure class="image image-style-align-right image_resized" style="width:43.2%;"><img style="aspect-ratio:475/126;" src="api/attachments/OTWUnRS8nD9Q/image/image.png" width="475" height="126"></figure>

**Yaw Pitch** - Rotates the shoulder using yaw (side-to-side) and pitch (up-and-down) angles, mimicking natural human shoulder movements. This mode is ideal for realistic arm animations but may struggle with extreme poses.

**From To** - Directly rotates the shoulder to align the arm with the target direction, allowing for precise tracking in any orientation. This mode is more flexible but may result in less natural shoulder rotations in some cases.

Shoulder Twist Weight: A float from 0 to 1. This setting controls how much the right shoulder will rotate backwards when the right arm is raised.

Shoulder Yaw Offset: A float. This adjusts the side-to-side (yaw) rotation of the shoulder, in degrees, when aligning the arm with the Hand Target. Positive values rotate the shoulder outward, while negative values rotate it inward. You can use this setting to fine-tune the arm’s horizontal alignment for natural positioning or to correct tracking misalignments.

Shoulder Pitch Offset: A float. This adjusts the up-and-down (pitch) rotation of the shoulder, in degrees, when aligning the arm with the Hand Target. Positive values raise the arm higher, while negative values lower it. You can use this setting to fine-tune the arm’s vertical alignment for natural posture or make adjustments as needed for your specific character rigs.

**Bending**

Bend Goal: This is a game object in your scene that the elbow will attempt to bend towards, depending on the Bend Goal Weight applied. This is used mostly if your character needs to achieve a specific silhouette such as while holding a weapon or if the character is cartoony and always has bent elbows.

Bend Goal Weight: A float from 0 to 1. 0 being not affected at all. 1 being most affected where the elbow is facing towards the Bend Goal game object as closely and directly as possible.

<figure class="image image-style-align-right image_resized" style="width:44.47%;"><img style="aspect-ratio:474/125;" src="api/attachments/d49yy0RGBMWY/image/image.png" width="474" height="125"></figure>

Swivel Offset: A float from -180 to 180. This is a measure of degrees by which the arm can rotated around it's own length to align with the hand bone or Hand Target to achieve a natural looking arm and hand rotation. 

Wrist To Palm Axis: A vector 3. This is a way to define the wrist bone’s local axis so that it points toward the palm. You can use this to help correct the hand position, if the hand is rotated at an unnatural angle.

Palm to Thumb Axis: A vector 3. This is a way to define the hand bone’s local axis that points from the palm to the thumb. You can use this to help correct the hand position, if the hand is rotated at an unnatural angle.

**Stretching**

Arm Length Mlp: A float from 0.1 to 2. (The “lp” in “Mlp” stands for Length Multiplier) This setting affects the arm's overall length by displacing the hand and forearm to better align with your own actual arm length to make tracking and arm movement more natural by scaling the arm to align with your own actual arm length. Setting this field to 1 will leave all measurements as they are. Setting less than 1 will make the arm shorter and greater than 1 will make the arm longer.

<figure class="image image-style-align-right image_resized" style="width:43.7%;"><img style="aspect-ratio:474/64;" src="api/attachments/HhFONxrBseNH/image/image.png" width="474" height="64"></figure>

Stretch Curve: A curve. This setting defines how much the arm’s bones (upper arm and forearm) stretch or compress based on the distance to the Hand Target. For example, if your VR Controller falls on the floor, the character's arm might stretch to the floor while the controller remains there. If creating a curve for this field, the X-axis represents the normalized distance (target distance divided by natural arm length), and the Y-axis represents the stretch multiplier (e.g., 1.0 for no change, 1.1 for 10% stretching).

#### Left Leg

<figure class="image image-style-align-right image_resized" style="width:45.55%;"><img style="aspect-ratio:514/308;" src="api/attachments/Dx39kH6zGFwW/image/image.png" width="514" height="308"></figure>

This section defines **targets** and settings related to those targets for bones in the Leg. The targets are game objects whose positions and rotations that the bones of your rig will try to line up with as much as possible, even to the point to where the mesh of the rig will stretch unnaturally to reach the targets. It's behavior is very similar to the behavior of parent-child game object relationships, where the target acts like the parent of the bone in your rig.

**Foot/Toe**

Target: This is a game object in your scene that the left foot bone will try to align with as closely as possible in position and rotation.

Position Weight: A float from 0 to 1. This dictates how closely the Left Foot Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Rotation Weight: A float from 0 to 1. This dictates how closely the Left Foot Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

**Bending**

Bend Goal: This is a game object in your scene that the knee will attempt to bend towards, depending on the Bend Goal Weight applied. This is used mostly if your character needs to achieve a specific silhouette such as a wide stance with knees out or if the character is cartoony and always has bent knees.

Bend Goal Weight: A float from 0 to 1. 0 being not affected at all. 1 being most affected where the knee is facing towards the Bend Goal game object as closely and directly as possible.

<figure class="image image-style-align-right image_resized" style="width:46.11%;"><img style="aspect-ratio:486/106;" src="api/attachments/8MuBWiefflqB/image/image.png" width="486" height="106"></figure>

Swivel Offset: A float from -180 to 180. This is a measure of degrees by which the leg can rotated around it's own length to align with the foot bone or Foot Target to achieve a natural looking leg and foot rotation. 

Bend To Target Weight: A float from 0 to 1. Not to be confused with _Bend Goal Weight_ – which adjusts how much the knee faces or bends towards a game object that is specifically put in the scene for the knee to point towards – this setting controls how much the knee bends towards the Foot Target. 0 being not affected at all, and 1 being most affected. The IK solver blends this weight with the value of the _Bend Goal Weight_ setting, if the _Bend Goal Weight_ is anything other than 0.

**Stretching**

Leg Length Mlp: A float from 0.1 to 2. (The “lp” in “Mlp” stands for Length Multiplier) This setting affects the leg's overall length by displacing the foot and lower leg to better align with your own actual leg length to make tracking and leg movement more natural by scaling the leg to align with your own actual leg length. Setting this field to 1 will leave all measurements as they are. Setting less than 1 will make the leg shorter and greater than 1 will make the leg longer.

<figure class="image image-style-align-right image_resized" style="width:53.65%;"><img style="aspect-ratio:487/65;" src="api/attachments/CpHpqeU7G99l/image/image.png" width="487" height="65"></figure>

Stretch Curve: A curve. This setting defines how much the leg’s bones (upper leg and lower leg) stretch or compress based on the distance to the Foot Target. For example, if your foot tracker flies off across the room, the character's leg might stretch across the room while the tracker remains there. If creating a curve for this field, the X-axis represents the normalized distance (target distance divided by natural leg length), and the Y-axis represents the stretch multiplier (e.g., 1.0 for no change, 1.1 for 10% stretching).

#### Right Leg

<figure class="image image-style-align-right image_resized" style="width:50.09%;"><img style="aspect-ratio:513/307;" src="api/attachments/3flDmSl1hi34/image/image.png" width="513" height="307"></figure>

This section defines **targets** and settings related to those targets for bones in the Leg. The targets are game objects whose positions and rotations that the bones of your rig will try to line up with as much as possible, even to the point to where the mesh of the rig will stretch unnaturally to reach the targets. It's behavior is very similar to the behavior of parent-child game object relationships, where the target acts like the parent of the bone in your rig.

**Foot/Toe**

Target: This is a game object in your scene that the right foot bone will try to align with as closely as possible in position and rotation.

Position Weight: A float from 0 to 1. This dictates how closely the Right Foot Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the position.

Rotation Weight: A float from 0 to 1. This dictates how closely the Right Foot Bone will track to the Target game object. 0 being not at all and 1 being the closest. This makes for a parent-child type relationship with the rotation.

**Bending**

Bend Goal: This is a game object in your scene that the knee will attempt to bend towards, depending on the Bend Goal Weight applied. This is used mostly if your character needs to achieve a specific silhouette such as a wide stance with knees out or if the character is cartoony and always has bent knees.

Bend Goal Weight: A float from 0 to 1. 0 being not affected at all. 1 being most affected where the knee is facing towards the Bend Goal game object as closely and directly as possible.

<figure class="image image-style-align-right image_resized" style="width:49.82%;"><img style="aspect-ratio:486/106;" src="api/attachments/QDtxdESRURBJ/image/image.png" width="486" height="106"></figure>

Swivel Offset: A float from -180 to 180. This is a measure of degrees by which the leg can rotated around it's own length to align with the foot bone or Foot Target to achieve a natural looking leg and foot rotation. 

Bend To Target Weight: A float from 0 to 1. Not to be confused with _Bend Goal Weight_ – which adjusts how much the knee faces or bends towards a game object that is specifically put in the scene for the knee to point towards – this setting controls how much the knee bends towards the Foot Target. 0 being not affected at all, and 1 being most affected. The IK solver blends this weight with the value of the _Bend Goal Weight_ setting, if the _Bend Goal Weight_ is anything other than 0.

**Stretching**

Leg Length Mlp: A float from 0.1 to 2. (The “lp” in “Mlp” stands for Length Multiplier) This setting affects the leg's overall length by displacing the foot and lower leg to better align with your own actual leg length to make tracking and leg movement more natural by scaling the leg to align with your own actual leg length. Setting this field to 1 will leave all measurements as they are. Setting less than 1 will make the leg shorter and greater than 1 will make the leg longer.

<figure class="image image-style-align-right image_resized" style="width:52.75%;"><img style="aspect-ratio:487/65;" src="api/attachments/yADf4FG7Zrg2/image/image.png" width="487" height="65"></figure>

Stretch Curve: A curve. This setting defines how much the leg’s bones (upper leg and lower leg) stretch or compress based on the distance to the Foot Target. For example, if your foot tracker flies off across the room, the character's leg might stretch across the room while the tracker remains there. If creating a curve for this field, the X-axis represents the normalized distance (target distance divided by natural leg length), and the Y-axis represents the stretch multiplier (e.g., 1.0 for no change, 1.1 for 10% stretching).

#### Locomotion

This section defines several settings related to locomotion – walking, running, etc. – that will affect the legs. This section is meant to provide fairly natural looking shuffling or stepping if full body tracking is not being used.

<figure class="image image-style-align-right image_resized" style="width:49.03%;"><img style="aspect-ratio:503/383;" src="api/attachments/XlWuM97wI7u8/image/image.png" width="503" height="383"></figure>

Mode: There are two modes available in this drop-down menu. This determines how the solver will work when the character is moving.

Procedural - Uses the settings below to create walking/shuffling-like leg movements

Animated - Relies on animations for walking, running, etc. (Note that if Animated is selected, other settings for adjusting the animation will appear.

Weight: A float from 0 to 1. This controls how much the legs are affected by the procedural locomotion. 0 being not affected and 1 being most affected.

**Procedural Settings**

Foot Distance: A float. Sets the distance between the feet (left and right) when standing or stepping. Measured in world units (e.g., meters in Unity). A larger value makes the character stand with feet farther apart, while a smaller value brings them closer together. Adjust to match the character’s natural stance width for realistic walking.

Step Threshold: A float. Defines the minimum distance the character’s root (hip or pelvis) must move before the solver triggers a new step. Measured in world units. A smaller value makes the character take steps more frequently, while a larger value requires more movement before stepping. Set higher for smoother, less twitchy steps.

Angle Threshold: A float. Sets the minimum angle (in degrees) the character’s root must rotate before triggering a new step. A smaller value makes the character step when turning slightly, while a larger value requires a bigger turn. Adjust to control how sensitive stepping is to rotation, preventing unnecessary steps during small turns.

Com Angle Mlp: A float. (Com is short for Center of Mass Angle Multiplier) Scales how much the character’s center of mass (pelvis) tilts or shifts when stepping, based on the angle of movement. A higher value makes the pelvis tilt more during turns or steps, adding realism to weight shifts. Keep low for subtle movements, higher for dynamic motion.

Max Velocity: A float. Sets the maximum speed (in world units per second) at which the character’s root can move during locomotion. Limits how fast the solver allows the character to walk or run procedurally. Increase for faster movement, decrease to keep steps slow and controlled.

Velocity Factor: A float. Scales how much the character’s root velocity (speed and direction) influences step timing and length. A higher value makes steps longer and faster in response to movement, while a lower value makes steps shorter and less responsive. Adjust to match the character’s movement speed with VR input.

Max Leg Stretch: A float from 0.9 to 1. Sets the maximum amount the leg’s bones (thigh and calf) can stretch to reach a step target. A value of 1.0 means no stretching (natural leg length), while 0.9 allows slight compression. Use to prevent unnatural leg extension during long steps, keeping movements realistic.

Root Speed: A float. Controls how fast the character’s root (pelvis) moves to follow the head or VR controller’s motion (in world units per second). A higher value makes the pelvis move quickly to stay under the head, while a lower value creates a slower, more grounded response. Adjust for natural body tracking.

Step Speed: A float. Sets how fast the foot moves to complete a step (in world units per second). A higher value makes steps quicker, while a lower value makes them slower and more deliberate. Adjust to control the pace of walking or stepping for a natural feel.

Step Height: A curve. This defines how high the foot lifts off the ground during a step, based on the step’s progress (0 to 1). The X-axis is the step’s timeline (0 = start, 1 = end), and the Y-axis is the foot’s height in world units. Shape the curve to make steps lift high (e.g., for marching) or stay low (e.g., for shuffling).

Max Body Y Offset: A float. Sets the maximum vertical distance (in world units) the character’s root (pelvis) can move up or down to match the head’s position in VR. Limits how much the body shifts vertically during steps or crouching. Increase for more flexibility in uneven terrain, decrease for a stable stance.

Heel Height: A curve. This defines how high the heel lifts relative to the foot’s position during a step, based on the step’s progress (0 to 1). The X-axis is the step’s timeline, and the Y-axis is the heel’s height in world units. Use to add realism to foot tilting, like lifting the heel when stepping forward.

Relax Leg Twist Min Angle: A float from 0 to 180. This sets the minimum angle (in degrees) at which the leg’s twist (rotation around its length) is relaxed when not stepping. A higher value allows more twist to remain, while a lower value forces the leg to untwist for a neutral pose. Adjust to prevent unnatural leg rotation when idle.

Relax Leg Twist Speed: A float. Controls how quickly the leg untwists to a neutral rotation when not stepping (in degrees per second). A higher value makes the leg snap back to a straight position faster, while a lower value makes it untwist slowly. Use for smooth transitions to a relaxed leg pose.

Step Interpolation: Drop-down menu. This setting defines how the VRIK solver interpolates (smooths) the character’s foot movements during locomotion. There are several options to choose from.

<figure class="image image-style-align-right"><img style="aspect-ratio:182/603;" src="api/attachments/moiQvlBNmT4q/image/image.png" width="182" height="603"></figure>

**None** - No smoothing is applied. The foot moves instantly to the target position without gradual transitions. This can feel abrupt or robotic but ensures precise tracking with no delay.

**In Out Cubic** - Smooths the foot’s movement with a cubic (third-degree) curve that starts slowly, speeds up in the middle, and slows down again at the end. Feels balanced and natural, like a gentle, flowing step.

**In Out Quintic** - Similar to In Out Cubic but uses a quintic (fifth-degree) curve, making the start and end even slower and the middle faster. Feels very smooth and polished, ideal for fluid, deliberate steps.

**In Out Sine** - Uses a sine curve for a smooth, wave-like transition that starts and ends slowly with a steady middle. Feels natural and organic, mimicking a soft, flowing step motion.

**In Quintic** - Starts very slowly and accelerates rapidly toward the target using a quintic (fifth-degree) curve. Feels like a hesitant start followed by a quick snap to position, good for cautious steps.

**In Quartic** - Starts slowly and speeds up using a quartic (fourth-degree) curve, slightly less extreme than In Quintic. Feels like a gentle buildup to a faster step, suitable for smooth but quick movements.

**In Cubic** - Starts slowly and accelerates with a cubic (third-degree) curve. Feels natural with a moderate buildup, good for steps that start softly but reach the target steadily.

**In Quadratic** - Starts slowly and speeds up with a quadratic (second-degree) curve, the gentlest of the “In” curves. Feels like a smooth, gradual step with a soft acceleration.

**In Elastic** - Starts slowly, overshoots the target slightly, and bounces back with an elastic effect. Feels playful or springy, like a lively step with a slight wobble at the end.

**In Elastic Small** - Like In Elastic but with a smaller overshoot and bounce. Feels subtly playful, adding a slight spring to the step without being too exaggerated.

**In Elastic Big** - Like In Elastic but with a larger overshoot and more pronounced bounce. Feels very bouncy and exaggerated, good for stylized or cartoonish stepping motions.

**In Sine** - Starts slowly and accelerates using a sine curve, focusing on a smooth initial buildup. Feels soft and flowing, like a gentle, wave-like step that picks up speed.

**In Back** - Starts slowly, pulls back slightly, then accelerates toward the target. Feels like a hesitation or “wind-up” before the step, adding a dramatic or cautious effect.

**Out Quintic** - Starts quickly and slows down dramatically near the target with a quintic (fifth-degree) curve. Feels like a fast step that eases into place carefully, ideal for precise landings.

**Out Quartic** - Starts quickly and slows down with a quartic (fourth-degree) curve, less extreme than Out Quintic. Feels like a quick step that gently settles into position.

**Out Cubic** - Starts quickly and slows down with a cubic (third-degree) curve. Feels like a natural step that decelerates smoothly, good for balanced, realistic movements.

**Out In Cubic** - Starts quickly, slows in the middle, and speeds up again with a cubic curve. Feels like a dynamic step with a brief pause, adding a unique rhythm to the motion.

**Out In Quartic** - Similar to Out In Cubic but with a quartic (fourth-degree) curve, making the middle pause more pronounced. Feels like a step with a deliberate, controlled rhythm.

**Out Elastic** - Starts quickly, overshoots the target, and bounces back with an elastic effect. Feels lively and springy, like a step that bounces slightly upon landing.

**Out Elastic Small** - Like Out Elastic but with a smaller overshoot and bounce. Feels subtly springy, adding a slight playful bounce to the step’s end.

**Out Elastic Big** - Like Out Elastic but with a larger overshoot and more pronounced bounce. Feels very exaggerated and bouncy, ideal for cartoonish or dramatic stepping.

**Out Sine** - Starts quickly and slows down with a sine curve, focusing on a smooth deceleration. Feels soft and flowing, like a step that gently glides into place.

**Out Back** - Starts quickly, overshoots slightly, then pulls back to the target. Feels like a step that lunges forward and corrects itself, adding a dynamic or dramatic effect.

**Out Back Cubic** - Combines Out Back’s overshoot with a cubic curve for smoother deceleration. Feels like a bold step that overshoots slightly and settles naturally.

**Out Back Quartic** - Combines Out Back’s overshoot with a quartic curve for even slower deceleration. Feels like a bold step with a pronounced overshoot and a very gentle landing.

**Back In Cubic** - Pulls back slightly, then accelerates and slows down with a cubic curve. Feels like a hesitant step with a smooth, natural finish, good for cautious movements.

**Back In Quartic** - Pulls back slightly, then accelerates and slows down with a quartic curve. Feels like a hesitant step with a very smooth, deliberate finish.

Offset: A vector 3. This defines a fixed offset (in world units) applied to the character’s root position during locomotion. Adjusts the pelvis’s position relative to the head or VR trackers (e.g., shift forward, sideways, or up). Use to fine-tune body alignment, like centering the character under the VR headset.

**Animated Settings**<img class="image-style-align-right image_resized" style="aspect-ratio:505/395;width:45.53%;" src="api/attachments/ADb1U09RHGlo/image/image.png" width="505" height="395">

Weight: A float from 0 to 1. This controls how much the legs are affected by the animation playing during locomotion. 0 being not affected and 1 being most affected.

Move Threshold: A float. Sets the minimum distance (in world units) the character’s root (pelvis) must move to trigger locomotion animations. A smaller value starts animations with slight movements, while a larger value requires more movement. Adjust to prevent animations from playing during small shifts, ensuring smooth transitions.

_Animation_

Min Animation Speed: A float from 0.1 to 1. Sets the slowest speed at which locomotion animations play (as a multiplier of the animation’s default speed). A value of 1.0 matches the default speed, while 0.5 slows it down. Use to ensure animations don’t play too fast during slow VR movements, keeping steps natural.

Max Animation Speed: A float from 1 to 10. Sets the fastest speed at which locomotion animations play (as a multiplier). A value of 1.0 matches the default speed, while 2.0 doubles it. Use to cap how fast animations play during rapid VR movements, preventing unnatural or jittery steps.

Animation Smooth Time: A float from 0.05 to 0.2. Controls how quickly the animation speed adjusts to match the character’s movement speed (in seconds). A smaller value makes speed changes instant, while a larger value smooths them out. Adjust for a natural transition between animation speeds, avoiding abrupt changes.

_Root Position_

Stand Offset: A vector 2. Defines a fixed offset (in world units) applied to the character’s root (pelvis) position when standing still. Shifts the pelvis relative to the VR headset or trackers (e.g., forward or up). Use to align the body naturally when idle, like centering it under the head.

<figure class="image image-style-align-right image_resized" style="width:49.05%;"><img style="aspect-ratio:474/126;" src="api/attachments/GWgSzVy6nj2C/image/image.png" width="474" height="126"></figure>

Root Lerp Speed While Moving: A float from 0 to 50. Sets how fast the character’s root (pelvis) moves to follow the VR headset or trackers during locomotion (in world units per second). A higher value makes the pelvis track quickly, while a lower value makes it lag slightly. Adjust for smooth body movement while walking.

Root Lerp Speed While Stopping: A float from 0 to 50. Sets how fast the root (pelvis) returns to its target position when the character stops moving (in world units per second). A higher value snaps the pelvis back quickly, while a lower value makes it ease in slowly. Use for a natural stop without abrupt shifts.

Root Lerp Speed While Turning: A float from 0 to 50. Sets how fast the root (pelvis) adjusts its position when the character turns (in world units per second). A higher value makes the pelvis follow turns quickly, while a lower value adds a slight delay. Adjust to balance responsiveness and smoothness during turns.

Max Root Offset: A float. Limits how far the root (pelvis) can move away from its ideal position (in world units) during locomotion. A larger value allows more freedom for the pelvis to shift, while a smaller value keeps it closer to the VR headset. Use to prevent unnatural body drifting.

_Root Rotation_

Max Root Angle Moving: A float from 0 to 180. Sets the maximum angle (in degrees) the root (pelvis) can rotate to align with the VR headset or trackers while moving. A larger value allows more rotation freedom, <img class="image-style-align-right image_resized" style="aspect-ratio:473/66;width:49.87%;" src="api/attachments/Z5WN4dhtb3ag/image/image.png" width="473" height="66">while a smaller value keeps the pelvis more stable. Adjust to control body rotation during walking.

Max Root Angle Standing: A float from 0 to 180. Sets the maximum angle (in degrees) the root (pelvis) can rotate when the character is standing still. A larger value allows the pelvis to turn more to match the headset, while a smaller value keeps it steady. Use to ensure a natural stance when idle.
