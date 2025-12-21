# **PID Control for VEX V5 Robots**

## **What Is PID?**

PID (Proportional–Integral–Derivative) is a control algorithm that helps your VEX V5 robot move more accurately and smoothly. You can think of it like cruise control in a car—it constantly adjusts motor power to reach and maintain a target.

---

## **Understanding Error**

**Error** is the difference between where you want to be (the target) and where you currently are.

`error = target − current`

### **Example**

If your robot needs to drive to **1000 encoder ticks** and is currently at **300 ticks**:

`error = 1000 − 300 = 700 ticks`

As the robot moves closer to the target, the error decreases:

`700 → 400 → 100 → 0`

---

## **The Three PID Controllers**

### **Proportional (P)**

**What it does:**  
 Motor power is proportional to the current error.

**Formula:**

`output = kP × error`

**Behavior:**

* Large error → high power

* Small error → low power

* Zero error → no power

**Issue:**  
 As the robot approaches the target, power decreases. This can cause the robot to stop short or oscillate.

---

### **Derivative (D)**

**What it does:**  
 Responds to how fast the error is changing.

**Formula:**

`output = kD × (error − previousError)`

**Behavior:**  
 Acts like a brake when approaching the target too quickly. Helps prevent overshooting and oscillation.

---

### **Integral (I)**

**What it does:**  
 Accumulates error over time.

**Formula:**

`output = kI × (sum of all errors)`

**Behavior:**  
 Eliminates small, persistent errors caused by friction, drag, or uneven weight distribution.

---

## **Complete PID Formula**

`error = target − current`

`integral += error`

`derivative = error − previousError`

`output = (kP × error) + (kI × integral) + (kD × derivative)`

---

## **What Is an Encoder Tick?**

An **encoder tick** measures motor rotation.

* VEX V5 Smart Motors:

  * 1 rotation \= 360 ticks

  * 1 tick \= 1 degree of rotation

Using encoder ticks allows precise and repeatable movement.

### **Examples**

**Drive 24 inches with a 4-inch wheel**

 `(24 ÷ (4π)) × 360 ≈ 687 ticks`

*   
* **Turn 90°**  
   ≈ 500 ticks (depends on wheelbase)

* **Lift an arm to 45°**  
   ≈ 45 ticks (adjust for gear ratio)

---

## **Step-by-Step PID Tuning**

### **Step 1: Tune P Only**

Set:

 `kI = 0`

`kD = 0`

* 

Start with a small value:

 `kP ≈ 0.1`

*   
* Increase `kP` until the robot reaches the target quickly but starts oscillating

* Reduce `kP` by **30–50%**

**Goal:** Fast response without overshoot or wobble

---

### **Step 2: Add D**

* Keep `kP` from Step 1

Start with:

 `kD = kP × 10`

*   
* Increase `kD` to reduce oscillation

* Decrease `kD` if motion becomes sluggish

**Goal:** Smooth motion with minimal overshoot

---

### **Step 3: Add I (If Needed)**

Only add I if the robot consistently stops short.

Start very small:

 `kI = 0.001 or less`

*   
* Increase gradually until the target is reached

* Too much I causes overshoot and instability

---

## **Practical Tuning Tips**

### **Typical Starting Values (Driving)**

* `kP`: 0.1 – 0.5

* `kI`: 0.0001 – 0.01

* `kD`: 1 – 5

### **Best Practices**

* Make small changes (10–20%)

* Test the same movement repeatedly

* Take notes on behavior

### **Common Problems**

* **Oscillation:** `kP` too high or `kD` too low

* **Sluggish motion:** `kP` too low or `kD` too high

* **Overshoot then settle:** Increase `kD`

* **Stops short:** Increase `kP` or add `kI`

---

## **Full Linear & Rotation PID for VEX Robots**

### **Step 1: Tune Rotation PID First**

Set:

 `kI = 0`

`kD = 0`

*   
* Start with small `kP` (≈ 0.5)

* Command a turn (e.g., 90°)

* Increase `kP` until rotation is fast but stable

* Add `kD` to reduce overshoot

* Add `kI` only if the robot consistently under-rotates

**Goal:** Smooth, accurate rotation to the target heading

---

### **Step 2: Tune Linear PID**

Set:

 `kI = 0`

`kD = 0`

*   
* Start with `kP` ≈ 0.2–0.5

* Command a known driving distance

* Increase `kP` for faster response

* Add `kD` to prevent overshoot

* Add `kI` only if the robot consistently stops short

**Goal:** Accurate distance control

---

### **Step 3: Combine Linear \+ Rotation PID**

#### **Tank Drive Example**

* Linear PID output → forward/backward movement (`V_linear`)

* Rotation PID output → heading correction (`V_rotate`)

`Left Motor  = V_linear + V_rotate`

`Right Motor = V_linear − V_rotate`

* Linear PID controls distance

* Rotation PID controls angle

* Combined → smooth, accurate path following

For holonomic drives (X-drive, mecanum), the same concept applies: linear PID controls X/Y movement, rotation PID controls heading, and outputs are combined using drive kinematics.

---

### **Step 4: Field Testing & Fine-Tuning**

* Test on the real field with the robot fully loaded

* Make small adjustments to `kP` and `kD`

* Repeat until linear and rotation PID work together seamlessly

---

### **Step 5: Practical Advice**

* Always tune rotation PID before linear PID

* Use motion profiling for smoother acceleration and deceleration

* Log PID errors and motor outputs for easier debugging

---

## **Summary**

* PID improves robot accuracy by adjusting motor power based on error

* **Linear PID** controls distance

* **Rotation PID** controls heading

* Combined, they allow the robot to drive accurately while maintaining orientation

* Small, iterative tuning and real-field testing are essential

This guide now provides a complete, structured explanation of PID control for VEX V5 robots, including tuning strategies and combined linear/rotation control.

