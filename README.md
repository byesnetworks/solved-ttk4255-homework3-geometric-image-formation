Download Link: https://assignmentchef.com/product/solved-ttk4255-homework3-geometric-image-formation
<br>
For general information about the assignments, including grading critera and how to get help, consult the assignments.pdf document on BB. To get your assignment approved, you only need to complete 60% (weighting is next to each task). Upload the requested answers and figures as a single PDF. You don’t need to submit your code. You may use any convenient tool to create your report.

<h1>Relevant reading</h1>

For this assignment, you need an understanding of rigid body motion (3D rotation and translation). Szeliski 2.1.1-2.1.4 gives a summary of various geometric primitives and transformations, but most of you should have received an introduction to rigid body motion in an earlier course (TTK4135 or TTK4190), and can just skim over these sections. If you are currently taking either course, you may instead want to read ahead in the book used in those courses.

An introduction to the pinhole camera model can be found in countless textbooks and articles, with the primary differences being their chosen mathematical notation and writing style. While you are

welcome to read Szeliski 2.1.5 for the course book’s introduction, this assignment includes a summary of the most important parts, written with the notation that we will use throughout the assignments. Unlike Szeliski, we will distinguish between coordinates in the 3D world and coordinates in the image by upper case (e.g. <em>X,Y,Z</em>) and lower case (e.g. <em>u,v</em>), respectively.

Figure 1: Diagram of an ideal pinhole camera seen from the side and in 3D.

<h1>The pinhole camera model</h1>

A common model of geometric image formation is the <em>perspective projection</em>. The simplest imaging device that can be described by this model is the <em>pinhole camera</em>: a box with a small opening (the pinhole) at one end, through which light enters and reaches an imaging surface at the opposite end. The <em>ideal pinhole camera </em>is a theoretical device in which the opening is made infinitely small. This has the consequence that every point on the imaging surface receives light from a unique direction, all of

which intersect at the pinhole. The resulting mapping from scene to image is a perspective projection.

Letting <em>f </em>denote the distance between the pinhole and the imaging surface in an ideal pinhole camera, the relationship between a point (<em>X,Y,Z</em>) in the scene and its projected location (<em>X</em><sup>0</sup><em>,Y </em><sup>0</sup><em>,Z</em><sup>0</sup>) on the imaging surface can be derived by a consideration of similar triangles (see Fig. 1):

(1)

(2)

and finally <em>Z</em><sup>0 </sup>= −<em>f</em>. These equations will differ based on how you place the camera’s axes. In this course we will follow the convention in which positive <em>Z </em>is in front of the camera, positive <em>X </em>is to the right and positive <em>Y </em>is downward, forming a right-handed coordinate system. Regardless of the chosen convention, these equations imply that the image will appear to be inverted (i.e. rotated 180 degrees), compared with what you would see through the pinhole. This inversion also occurs in a modern digital camera, but the camera firmware and your operating system work together to ensure that the rotation is undone when the image is presented on your screen.

A modern digital camera similarly consists of a box with an opening at one end, which allows light to enter and hit an imaging surface (usually a CCD or CMOS sensor). The difference is that the opening is not infinitely small. Instead, it is made large to collect more light, and a lens is placed in front of the opening to focus the light. What is important to note is that the geometric transformation of points in the scene to points on the sensor can be—or at least, is often—modeled by the same equations (perspective projection). This is somewhat surprising when it is considered that modern cameras have complex optics which bend light at multiple stages. The fact that the model continues to be applicable is in part because camera and lens engineers strive to produce a perspective projection, as the resulting images appear most natural and pleasing to a human observer.

The assignments and projects in this course are copyrighted by Simen Haugo (<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0860697d6f67267b61656d66486f65696164266b6765">[email protected]</a>) and may not be copied, redistributed, reused or repurposed without written permission.

In a high-quality camera,the agreement can be good enough to not require further modeling. Otherwise, smaller errors can often be modeled during camera calibration, and corrected to produce an equivalent perspective projection image. Because of this, the perspective projection model is applicable for the majority of commercially available cameras. However, it is worth mentioning that some cameras, such as fisheye lens cameras, do not have as a goal to produce natural-looking images, and may deviate substantially from a perspective projection. In such cases, a different camera model may be warranted.

When working with a digital camera, we also need to model the transformation from locations on the physical sensor surface (in metric units) to the pixel coordinates (rows and columns) used to access the digital image. With few exceptions, the pixel coordinate system is normally placed in the upper left corner of the noninverted image, with the horizontal axis pointing to the right in the scene and the vertical axis pointing down in the scene. Letting (<em>u,v</em>) denote the horizontal and vertical pixel coordinates, these are then related to (<em>X</em><sup>0</sup><em>,Y </em><sup>0</sup>) by a negation, scale and offset:

<table width="643">

 <tbody>

  <tr>

   <td width="625">                                                                     <em>u </em>= <em>c<sub>x </sub></em>− <em>s<sub>x</sub>X</em><sup>0            </sup>and      <em>v </em>= <em>c<sub>y </sub></em>− <em>s<sub>y</sub>Y </em><sup>0</sup><em>,</em>which can be simplified and written in terms of the original (<em>X,Y,Z</em>) coordinates as:</td>

   <td width="19">(3)</td>

  </tr>

 </tbody>

</table>

and    <em>.                                            </em>(4)

Note that the negation above is mathematically equivalent to placing the imaging surface in front of the camera (i.e. at <em>Z </em>= <em>f</em>). However, this makes no sense physically, and misleadingly suggests that points with <em>Z </em>∈ [0<em>,f</em>] are behind the imaging surface and therefore not visible, which is false.

The parameters <em>c<sub>x</sub>,c<sub>y</sub>,s<sub>x</sub>,s<sub>y</sub>,f </em>are given the following names and have the following geometric interpretation in the ideal pinhole camera model:

<ul>

 <li><em>c<sub>x</sub>,c<sub>y</sub></em>: <em>Principalpoint </em>– the intersection point between the optical axis and the imaging surface (in pixels). The optical axis is the line that is orthogonal to the imaging surface and passes through the camera origin. In our chosen convention, this is the <strong>Z</strong><em><sup>~ </sup></em>-axis.</li>

 <li><em>s<sub>x</sub>,s<sub>y</sub></em>: <em>Pixel density </em>– the number of discrete sensing elements horizontally and vertically per unit length of the imaging surface (often in pixels per micrometer).</li>

 <li><em>f</em>: <em>Focal length </em>– the distance between the pinhole and the imaging surface (often in millimeters).</li>

</ul>

The principal point should not be confused with the <em>optical center </em>or <em>center of projection</em>, which is the 3D origin of the camera coordinate system, where the incoming light rays intersect. This distinction is made in the majority of computer vision literature, but Szeliski uses the term optical center for both quantities. In the ideal pinhole camera, the center of projection coincides with the pinhole. The center of projection in a real camera is less obvious, and may be behind, within or in front of the lens system.

You will more commonly see <em>s<sub>x </sub></em>and <em>s<sub>y </sub></em>as their inverse quantities, which represent the physical distance between horizontally and vertically adjacent pixels, respectively. In a sensor datasheet, these are usually specified as the <em>pixel size </em>or <em>pitch </em>in micrometers (microns). In modern digital cameras, pixels are usually square (<em>s<sub>x </sub></em>= <em>s<sub>y</sub></em>), so you may only see a single number listed.

Figure 2: A camera moving over an approximately planar surface, as described in Task 1.1-1.2. Note that the camera is drawn as a triangle with the imaging surface <em>in front </em>of the camera, which, as mentioned before, makes no sense. However, its use as a visual symbol is somewhat standard in robotics and computer vision literature. In 3D it is often drawn as a pyramid.

<h1>Part 1       Choosing a sensor and lens</h1>

You may at some point have to choose a camera sensor and lens combination to satisfy the imaging requirements of a particular application. Many camera vendors offer “calculators” that can help with this process. These are often based on the pinhole camera model, so it is possible to do this analysis yourself, using the equations presented previously.

Some key parameters for the sensor are its shutter speed, resolution and pixel size, which are specified in its datasheet. For the lens, the primary parameter is its focal length. The pixel size corresponds to 1<em>/s<sub>x </sub></em>and 1<em>/s<sub>y </sub></em>in the pinhole camera model. The focal length of a lens can to a first approximation be considered as equivalent to the identically named parameter <em>f </em>in the pinhole camera model. (Szeliski 2.2.3 provides a brief motivation for when and why this approximation is reasonable.)

<strong>Task 1.1: </strong>(10%) Imagine you are designing an aerial vehicle to capture images of an agricultural crop as in Fig. 2. Suppose that you have a camera sensor with square pixels of size 10×10 microns and suppose that the camera is looking straight down from a height of 50 meters. Assume that the ground is flat and determine what focal length you need in order for one pixel in the image to cover a ground distance of one centimeter. Give your answer in millimeters.

<strong>Task 1.2: </strong>(10%) Having overlap between images is important for image stitching (e.g. creating one large image of the entire crop). Suppose that the camera captures 5 images per second at a resolution of 1024×1024 pixels, and suppose that the camera is moving 50 meters per second along one of the lateral camera axes (e.g. <strong>X</strong><em><sup>~ </sup></em>as in Fig. 2). Determine the area of overlap between two consecutive images, using the focal length you found above. Give your answer as a percentage of the total image area.

Hint: Compute the distance (in pixels) by which any pixel in the image gets displaced from one image to the next, and divide the remainder by the image size (1024).

u (pixels)                                                                                             u (pixels)

Figure 3: Projection of points in Task 2.2 (left) and transformed points in Task 3.2 (right).

<h1>Part 2   Implementing the pinhole camera model</h1>

Although the pinhole camera model involves a non-linear operation (division by <em>Z</em>), it is often written as the following linear relationship:

<table width="422">

 <tbody>

  <tr>

   <td width="101">  <em>u</em>˜              <em>s<sub>x</sub>f</em><em>v</em>˜ =  0   <em>w</em>˜         0</td>

   <td width="37">0 <em>s<sub>y</sub>f</em>0</td>

   <td width="265"><em>c<sub>x</sub></em><em>X</em><em>c<sub>y</sub></em><em>Y </em> 1        <em>Z</em></td>

   <td width="19">(5)</td>

  </tr>

 </tbody>

</table>

|   {z         } <strong>K</strong>

or simply

<strong>u˜ </strong>= <strong>KX                                                                      </strong>(6)

where <strong>K </strong>is called the <em>calibration</em>– or <em>intrinsic matrix</em>. The vector <strong>u˜ </strong>= (<em>u,</em>˜ <em>v,</em>˜ <em>w</em>˜) is the <em>homogeneous </em>form of the pixel coordinate vector <strong>u </strong>= (<em>u,v</em>). These are implicitly related by

<em>wu</em>˜ = <em>u</em>˜    and   <em>wv</em>˜ = <em>v.</em>˜                                                         (7)

Like Szeliski, we denote homogeneous coordinates using the tilde “∼” symbol. The motivation for writing the equations in this form is probably only truly appreciated after taking a course in projective geometry. Nevertheless, you will frequently encounter the linear formulation in practice.

<strong>Task 2.1: </strong>(5%) Dividing a homogeneous vector by its last component is called dehomogenization. Show that the dehomogenized coordinates <em>u/</em>˜ <em>w</em>˜ and <em>v/</em>˜ <em>w</em>˜ are equal to <em>u </em>and <em>v </em>in Eq. 4, when <em>Z </em>6= 0.

<strong>Task 2.2: </strong>(5%) The hand-out code has a stub function called project in project.m (Matlab) and common.py (Python). The function should take a calibration matrix <strong>K </strong>and a 3×<em>N </em>array of points (<em>X,Y,Z</em>), and return a 2×<em>N </em>array of pixel coordinates (<em>u,v</em>), computed with the pinhole model.

Implement the function and test it on the points and calibration matrix provided in task2points.txt and task2K.txt. Include a figure showing the pinhole projection of the 3D points. The task2 script provides helper code to load the data and generate the required figure. Your figure should look similar to the left figure in Fig. 3 (minus the grid and labels).

<h1>Part 3       Homogeneous coordinates and transformations</h1>

So far, (<em>X,Y,Z</em>) has referred to a point’s coordinates in the camera coordinate system. However, the point may be expressed in a different coordinate system, perhaps attached to the environment or to an object. If so, we need to include an additional transformation that relates the two coordinate systems. This transformation is called a rigid body motion or Euclidean transformation, and is described by a 3D rotation and 3D translation. Given a point <strong>X</strong><em><sup>o </sup></em>expressed in a coordinate system (or <em>frame</em>) “<em>o</em>”, its coordinate vector in the camera frame “<em>c</em>” is

<strong>X</strong><em><sup>c </sup></em>= <strong>RX</strong><em><sup>o </sup></em>+ <strong>t</strong><em>,                                                                   </em>(8)

where <strong>R </strong>is a rotation matrix and <strong>t </strong>is a translation vector. Note that we distinguish between coordinates in different frames with a superscript. Sometimes <strong>R </strong>and <strong>t </strong>are grouped into a single 4×4 matrix, such that the above can be written as

(9)

with a scalar 1 appended to both vectors. More concisely we may write

<strong>X˜</strong>                                                                (10)

where <strong>T</strong><em><sup>c</sup></em><em><sub>o </sub></em>is read as “the transformation from <em>o </em>to <em>c</em>”. Using 4×4 matrices lets us combine sequential transformations by matrix multiplication. For example, given the transformation from <em>a </em>to <em>b</em>, and from <em>b </em>to <em>c</em>, the composite transformation from <em>a </em>to <em>c </em>is <strong>T</strong>.

<strong>Task 3.1: </strong>(5%) The vectors in Eq. 10 are generally homogeneous with the last component not necessarily equal to 1. They must therefore be divided by the last component to obtain the actual 3D coordinates. However, show that this step can be skipped when computing the projection (<em>u,v</em>) of a homogeneous vector <strong>X</strong><strong><sup>˜ </sup></strong>= (<em>X,</em><sup>˜ </sup><em>Y ,</em><sup>˜ </sup><em>Z,</em><sup>˜ </sup><em>W</em><sup>˜ </sup>), i.e. show that the last component (<em>W</em><sup>˜ </sup>) can simply be dropped.

<strong>Task 3.2: </strong>(5%) The points in Task 2 were in camera coordinates and had been pre-translated 6 units along the camera <strong>Z</strong><em><sup>~ </sup></em>-axis to be in front of the camera. The points in task3points.txt are instead given in the object coordinate frame indicated in Fig. 3 (right). Here, the object-to-camera transformation

<strong>T</strong><em><sup>c</sup></em><em><sub>o </sub></em>is a product of one translation by 6 units and two elemental rotations by 15<sup>◦ </sup>and 45<sup>◦</sup>, respectively.

Find an expression for <strong>T</strong><em><sup>c</sup></em><em><sub>o</sub></em>. Check your answer by modifying task2 to load the new points and apply your transformation before projection. The generated figure should look identical to Fig. 3 (right). Note: You don’t need to expand the matrix product; you can give your answer in terms of the elemental rotation matrices <strong>R</strong><em><sub>x</sub></em>(·)<em>,</em><strong>R</strong><em><sub>y</sub></em>(·)<em>,</em><strong>R</strong><em><sub>z</sub></em>(·), and a translation matrix <strong>T</strong><em><sub>z</sub></em>(·).

Tip: You may want to modify the project function to take a 4×<em>N </em>array of homogeneous coordinates. You can find definitions of the elemental rotation matrices on Wikipedia. Finally, you may use the provided function draw_frame to visualize the coordinate frame associated with <strong>T</strong><em><sup>c</sup></em><em><sub>o</sub></em>, as in the figure.

Figure 4: Quanser 3-DOF helicopter and its three degrees of freedom. The arrows indicate the direction of increasing yaw, pitch and roll.

<h1>Part 4        Image formation model for the Quanser helicopter</h1>

The Quanser 3-DOF helicopter consists of an arm with a counterweight at the back and a rotor carriage with two rotors at the front. It has three rotational degrees of freedom (Fig. 4):

<ul>

 <li>Yaw (<em>ψ</em>): Rotation around an axis perpendicular to the mounting platform</li>

 <li>Pitch (<em>θ</em>): Rotation of the arm up or down</li>

 <li>Roll (<em>φ</em>): Rotation of the rotor carriage around the arm</li>

</ul>

In the midterm project, you’ll estimate these angles from markers attached to the helicopter. To do this, you need a mathematical model that relates points on the helicopter to pixel coordinates in the image. A reasonable solution is to model the helicopter as a piecewise rigid body and attach a coordinate frame to each part of interest. The coordinate frames you should use are shown in Fig. 5. Your main task here is to define the 4 × 4 transformation matrices between the different coordinate frames, using the measurements indicated in Fig. 6.

<strong>Task 4.1: </strong>(5%) A square platform with four screw holes is shown in Fig. 7. The distance between adjacent screws is 11.45 cm. Define four coordinate vectors corresponding to the screw locations. The coordinate vectors should be expressed in the coordinate frame shown in Fig. 7a, which is aligned with the sides of the platform and has its origin on the closest screw.

<strong>Task 4.2: </strong>(5%) Verify that your answers above are correct by writing a script to reproduce Fig. 7a:

<ol>

 <li>Load and draw the image quanser.jpg. Adjust the limits of the figure to zoom in on the platform.</li>

 <li>Use the transformation <strong>T</strong><sup>camera</sup><sub>platform </sub>in platform_to_camera.txt to transform your coordinate vectors into camera coordinates. Assume that the camera satisfies a pinhole model with the matrix <strong>K </strong>in heli_K.txt and project the transformed coordinate vectors into the image.</li>

</ol>

Include the figure in your writeup. The drawn points should coincide with the screws. You don’t have to reproduce Fig. 7 exactly, e.g. the labels and stippled lines can be omitted.

As in Task 3.2, for the following tasks you don’t need to multiply and write out the entries of the 4×4 matrix. You can express your answer as a product of the elemental rotation matrices <strong>R</strong><em><sub>x</sub></em>(·)<em>,</em><strong>R</strong><em><sub>y</sub></em>(·)<em>,</em><strong>R</strong><em><sub>z</sub></em>(·) and translation matrices <strong>T</strong><em><sub>x</sub></em>(·)<em>,</em><strong>T</strong><em><sub>y</sub></em>(·)<em>,</em><strong>T</strong><em><sub>z</sub></em>(·).

<strong>Task 4.3: </strong>(10%) Yaw motion is enabled by a shaft passing through the platform center. The “base” frame follows the shaft: Its z-axis is parallel to the platform z-axis and its origin is in the middle of the platform. The remaining axes are obtained by a counter-clockwise rotation by <em>ψ </em>around z. Find an expression for the transformation <strong>T</strong><sup>platform</sup><sub>base </sub>(<em>ψ</em>) from the base frame to the platform frame.

<strong>Task 4.4: </strong>(10%) Pitch motion is enabled by a hinge that rotates with the shaft. The “hinge” frame is obtained by translating 32<em>.</em>5 cm along the base frame z-axis (i.e. up the shaft), and rotating counterclockwise by <em>θ </em>around the base frame y-axis. Find an expression for <strong>T</strong><sup>base</sup><sub>hinge</sub>(<em>θ</em>).

<strong>Task 4.5: </strong>(10%) The arm is rigidly attached to the hinge. As indicated in Fig. 5, the “arm” frame is centered in the cross-section of the arm with its x-axis pointing down the arm toward the rotors. It is obtained by translating −5 cm along the hinge frame z-axis. Find an expression for <strong>T</strong><sup>hinge</sup><sub>arm </sub>.

<strong>Task 4.6: </strong>(10%) Finally, the rotor carriage is allowed to rotate around a shaft parallel to the arm, located slightly below the arm centerline. The “rotors” frame is obtained from the arm frame by translating 65 cm along the x-axis, −3 cm along the z-axis, and rotating counter-clockwise by <em>φ </em>around the x-axis. Find an expression for <strong>T</strong><sup>arm</sup><sub>rotors</sub>(<em>φ</em>).

<strong>Task 4.7: </strong>(10%) Verify your answers by writing a script to reproduce Fig. 8:

<ol>

 <li>Load and draw the image quanser.jpg.</li>

 <li>Instantiate the transformation matrices using <em>ψ </em>= 11<em>.</em>6<sup>◦</sup>, <em>θ </em>= 28<em>.</em>9<sup>◦ </sup>and <em>φ </em>= 0<sup>◦</sup>. Use the draw_frame function to draw the coordinate frames. (Set the scale argument to about 0.05.)</li>

 <li>Load the marker coordinate vectors in heli_points.txt. The first three are given in the arm frame, and the last four are given in the rotors frame. Draw the points by applying the correct sequence of transformations, followed by projection.</li>

</ol>

Each projected point is meant to lie exactly on one of the corners of its associated marker. However, if you have done everything correctly, the projected points will be off by up to 10 pixels. This is due to measurement and modeling inaccuracies. (In the midterm project you will learn how to calibrate the model itself by tracking the helicopter through a long motion sequence.)

100           200           300           400           500           600             100           200           300           400           500          600

(a)                                                                                                    (b)

Figure 7: (a) Platform coordinate frame and points located on the four screws. (b) Base coordinate frame, located in the platform center with the z-axis perpendicular to the platform.

Figure 8: Helicopter frames and reprojected fiducial marker points