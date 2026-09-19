# IK Lab

IK Lab visualizes inverse kinematics, the problem of solving for the joint angles of a kinematic chain that place its end effector at a target position. This is a core problem in robotics, animation, and biomechanics, and it has no single canonical solution method, which is exactly what this lab is built to demonstrate.

Rather than implementing one solver, the lab runs four independently implemented methods simultaneously against the same live target, so their different strategies and convergence behavior can be compared directly rather than described abstractly:

- **FABRIK** (Forward And Backward Reaching Inverse Kinematics): a geometric method that treats the chain as a set of rigid links, pulling the end effector to the target and propagating position corrections backward and forward along the chain while preserving link lengths.
- **CCD** (Cyclic Coordinate Descent): an iterative method that rotates one joint at a time, from end effector toward base, so that the remaining chain segment points directly at the target, repeating until convergence.
- **Jacobian Transpose**: a gradient-based method that computes the manipulator Jacobian, the matrix relating small joint angle changes to small end effector displacement, and takes a scaled step in that direction.
- **Jacobian DLS** (Damped Least Squares): a numerically stabilized variant that solves a small linear system (via a 2x2 matrix inversion in this 2D implementation) instead of following the raw gradient, avoiding the instability that pure Jacobian methods exhibit near singular configurations. This is the approach most commonly used in production robot controllers.

The lab also models joint limits, since real revolute joints have finite range of motion. Elbow-style joints are constrained by default to prevent hyperextension. FABRIK does not natively respect joint limits, so its limit handling is implemented as a post-hoc angle clamp, which is called out explicitly in the UI rather than hidden. Unreachable targets are handled correctly: the chain fully extends toward the target rather than failing or oscillating.

Features:
- Four independently implemented IK solvers running concurrently on the same target
- Configurable chain length (2 to 6 joints) and per-link lengths
- Realistic joint limit constraints with documented FABRIK limitations
- Graceful degradation for unreachable targets
- Live tracking-error chart comparing convergence rate across all four methods

Tech: single self-contained HTML file, vanilla JavaScript. Forward kinematics, Jacobian computation, and the 2x2 matrix inversion for DLS are implemented from scratch with no external math or robotics libraries.

Usage: open `InverseKinematicsLab.html` in a browser or at my website, sid2010abc.github.io, drag the target, and select an active solver to highlight. Ghost overlays and the error chart can be toggled from the sidebar.
