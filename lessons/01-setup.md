# Lesson 1 — Foundations of Robotics Control

Welcome to **VEX Robotics Control Systems**.

Before we build PID loops, tune autonomous routines, or design odometry systems, we need to understand the most important idea in robotics:

> Control is about reducing error.

Everything in this course will build on that concept.

---

# 🔹 Part 1 — What Is a Motor?

A motor converts electrical energy into rotational motion.

In VEX (using PROS), we control a motor by sending it a power value:

```cpp
pros::Motor motor(1);

motor.move(127);    // Full forward
motor.move(-127);   // Full reverse
motor.move(0);      // Stop
```

Power range:
- -127 to 127 (percentage control)
- Or use `move_voltage()` for millivolt precision control

Now ask yourself:

If you run a motor at 127 power for 1 second,  
will it always travel the same distance?

The answer is **no**.

Why?

- Battery voltage changes  
- Friction changes  
- Load changes  
- Wheel slip occurs  
- Mechanical alignment shifts  

This is called **open-loop control**.

---

# 🔹 Part 2 — Open Loop vs Closed Loop

## Open Loop

Open loop means:

You tell the motor what to do.  
You do NOT measure the result.

Example:

```cpp
motor.move(127);
pros::delay(1000);
motor.move(0);
```

The robot moves for 1 second.

There is no measurement.  
There is no correction.  
There is no guarantee.

That is why some autonomous runs look perfect once  
and completely wrong the next time.

---

## Closed Loop

Closed-loop control means:

1. Set a target  
2. Measure the current state  
3. Compare them  
4. Adjust output automatically  

Closed-loop systems require **feedback**.

---

# 🔹 Part 3 — What Is Feedback?

Feedback means the robot can measure itself.

Common VEX feedback sources:

- Motor encoders (built-in)  
- Rotation sensors  
- IMU (heading sensor)  
- Tracking wheels  

Example:

```cpp
double position = motor.get_position();
```

Now the robot knows how far it has moved.

That allows us to ask:

> How far away are we from our goal?

That difference is called **error**.

---

# 🔹 Part 4 — Understanding Error

Error is defined as:

```
error = target - current
```

Example:

Target: 1000 ticks  
Current: 750 ticks  

Error = 250 ticks

The robot is 250 ticks away from its goal.

If error is:

- Large → we need more correction  
- Small → we need less correction  
- Zero → we are done  

Control systems exist to drive error toward zero.

---

# 🔹 Part 5 — Why Autonomous Is Inconsistent

If your robot:

- Hits the target perfectly one run  
- Misses badly the next run  

It usually means one of these is happening:

- No feedback control  
- Poorly tuned control  
- No proper exit condition  
- Mechanical variation  
- Motor overheating  
- Battery voltage variation  

Time-based movement is unreliable.

Measurement-based movement is reliable.

Consistency in robotics comes from:

- Measuring  
- Correcting  
- Settling properly  

That’s what this course will teach.

---

# 🔹 Part 6 — A Simple Control Attempt

Imagine we write this:

```cpp
while(true) {
    double error = target - motor.get_position();

    if (abs(error) < 5) {
        break;
    }

    motor.move(100);
}
```

Is this smart control?

No.

Why?

- Power never changes  
- It does not slow down near the target  
- It will overshoot  
- It may oscillate  
- It may never settle cleanly  

This introduces the idea that:

Power should depend on error.

That idea leads directly to **Proportional control**.

---

# 🔹 Part 7 — The Control Loop Pattern

Every advanced system we build (P, PID, PIDF, Odometry) follows this structure:

1. Set a target  
2. Measure current state  
3. Compute error  
4. Adjust output  
5. Repeat until settled  

The math will become more advanced.

But the structure never changes.

---

# 🔹 Part 8 — Why This Matters

If you understand:

- What error is  
- Why feedback matters  
- Why open-loop fails  
- Why measurement creates consistency  

You are already thinking like a controls engineer.

This foundation separates:

Copy/paste coders  
from  
Engineers who understand what they are building.

---

# 🧠 Check Your Understanding

Before moving to Lesson 2, answer these:

1. What is the difference between open-loop and closed-loop control?  
2. Why does time-based movement cause inconsistency?  
3. What is error?  
4. Why is feedback necessary?  
5. What happens if you never adjust power based on error?  

If you can confidently answer these,  
you’re ready for the next step.

---
