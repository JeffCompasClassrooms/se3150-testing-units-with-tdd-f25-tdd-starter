# Triangle Specification

We used Claude to help us write a spec for a triangle class. Here was the spec we settled on. I enhanced it a little... **Teacher's prerogative.** Use this as the spec for the first part of Assignment 2.

---

## Triangle Class Specification

### Overview
Create a `Triangle` class in Python that represents a triangle using either side lengths or angles. The class must maintain internal consistency and validity checking.

---

## Class Requirements

### Constructor
```python
Triangle(angle_a=None, angle_b=None, angle_c=None, side_a=None, side_b=None, side_c=None)
```

**Parameters:**
- `angle_a, angle_b, angle_c`: Angles in degrees (float)
- `side_a, side_b, side_c`: Side lengths (float, must be positive)

**Behavior:**
- Must provide either **all 3 angles OR all 3 sides** (not both, not partial)
- If angles provided: must sum to exactly 180.0 degrees
- If sides provided: must satisfy triangle inequality (sum of any two sides > third side)
- Sets internal `valid` flag to `True` if triangle is valid, `False` otherwise

---

### Attributes

**Public Attributes**
- `angle_a, angle_b, angle_c`: Current angles in degrees (float)
- `side_a, side_b, side_c`: Current side lengths (float)
- `valid`: Boolean flag indicating if triangle is currently valid

---

### Methods

#### `set_angles(angle_a, angle_b, angle_c)`
**Purpose:** Set all three angles of the triangle

**Parameters:**
- `angle_a, angle_b, angle_c`: New angles in degrees (float)

**Behavior:**
- Updates the triangle's angles
- Does **not** automatically validate — requires explicit call to `validate()`
- After setting angles, `valid` flag should be considered unreliable until `validate()` is called

---

#### `set_sides(side_a, side_b, side_c)`
**Purpose:** Set all three side lengths of the triangle

**Parameters:**
- `side_a, side_b, side_c`: New side lengths (float, should be positive)

**Behavior:**
- Updates the triangle's side lengths
- Does **not** automatically validate — requires explicit call to `validate()`
- After setting sides, `valid` flag should be considered unreliable until `validate()` is called

---

#### `validate()`
**Purpose:** Validate the current triangle configuration, calculate missing values, and update the valid flag

**Returns:** `bool` — `True` if triangle is valid, `False` otherwise

**Validation Rules:**
- **Angle Validation:** All three angles must sum to exactly 180.0 degrees
- **Side Validation:** All sides must be positive **and** satisfy triangle inequality:
  - `side_a + side_b > side_c`
  - `side_a + side_c > side_b`
  - `side_b + side_c > side_a`
- **Consistency:** If both angles and sides are set, they should be mathematically consistent

**Calculation Behavior:**
- If only sides are provided: Calculate angles using **Law of Cosines**:
  - `angle_a = arccos((b² + c² - a²) / (2bc))`
  - `angle_b = arccos((a² + c² - b²) / (2ac))`
  - `angle_c = arccos((a² + b² - c²) / (2ab))`
- If only angles are provided: Cannot uniquely determine sides (infinite solutions)
  - Sides should remain `None` or unset
  - Triangle is valid if angles sum to 180°, but geometric operations requiring sides may not work
- If both provided: Verify mathematical consistency between angles and sides

**Behavior:**
- Updates the internal `valid` flag
- Calculates missing angles from sides when possible
- Returns the validation result

---

#### `get_area()`
**Purpose:** Calculate the area of the triangle

**Returns:** `float` — The area of the triangle

**Behavior:**
- Should only work if triangle is valid (check `valid` flag)
- If invalid, should raise an exception or return `None`
- Use **Heron's formula**:
  - `area = √[s(s-a)(s-b)(s-c)]` where `s = (a+b+c)/2`

---

#### `get_perimeter()`
**Purpose:** Calculate the perimeter of the triangle

**Returns:** `float` — The perimeter of the triangle

**Behavior:**
- Should only work if triangle is valid (check `valid` flag)
- If invalid, should raise an exception or return `None`
- `perimeter = side_a + side_b + side_c`

---

#### `is_right_triangle()`
**Purpose:** Determine if the triangle is a right triangle

**Returns:** `bool` — `True` if triangle is a right triangle, `False` otherwise

**Behavior:**
- Should only work if triangle is valid
- Check if any angle is exactly 90 degrees **OR** if sides satisfy Pythagorean theorem
- For Pythagorean theorem: `c² = a² + b²` where `c` is the longest side

---

#### `is_isosceles()`
**Purpose:** Determine if the triangle is isosceles (two sides equal)

**Returns:** `bool` — `True` if triangle is isosceles, `False` otherwise

**Behavior:**
- Should only work if triangle is valid
- Check if any two sides are equal (within floating-point tolerance)
- An equilateral triangle is also isosceles

---

#### `is_equilateral()`
**Purpose:** Determine if the triangle is equilateral (all sides equal)

**Returns:** `bool` — `True` if triangle is equilateral, `False` otherwise

**Behavior:**
- Should only work if triangle is valid
- Check if all three sides are equal (within floating-point tolerance)
- All angles should be 60 degrees
