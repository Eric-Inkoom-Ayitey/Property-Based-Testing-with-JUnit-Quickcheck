# Microwave Controller - Property-Based Testing with JUnit Quickcheck

## Overview
This project implements property-based tests for a microwave controller system using JUnit Quickcheck. The goal is to create parameterized unit tests that can detect bugs in the microwave's timing and control logic.

## Assignment Description
The microwave controller manages cooking time, presets, and modes (Setup, Cooking, Suspended). This assignment focuses on writing property-based tests to verify:
1. **Cooking functionality** - Time countdown and mode transitions
2. **Preset buttons** - Correct time and power level configuration
3. **Unusual time handling** - Proper countdown from non-standard time formats (e.g., 1:99)

## Implementation

### Test Cases

#### 1. `checkCookingTimes`
Tests the basic cooking workflow:
- Accepts 4 digits for time input and elapsed cooking time
- Verifies correct time countdown
- Checks proper mode transitions (Setup → Cooking → Setup)
- Uses 100 trials with digits 0-9 and time 0-500 seconds

#### 2. `checkPreset`
Tests preset button functionality:
- Verifies valid presets set correct time and power level
- Handles invalid preset numbers appropriately
- Tests with preset range 0-7 (includes valid 1-5 and invalid values)
- Uses exception handling for correct vs buggy version compatibility

#### 3. `checkUnusualCookingTimes`
Tests edge cases with non-standard time formats:
- Verifies all digits remain in valid range (0-9)
- Ensures once "normal time" is reached, it stays normal
- Checks countdown accuracy at every second
- Normal time: tens-of-minutes ≤ 5, minutes ≤ 9, tens-of-seconds ≤ 5, seconds ≤ 9

## Key Features

### Helper Methods
- `secondsElapse(int time)` - Advances microwave time by specified seconds
- `secondsElapseCheckNormal(int time)` - Advances time while checking normal time invariant
- `calcTime(byte[] digits)` - Converts digit array to total seconds
- `timeIsNormal()` - Checks if current time is in normal format
- `timeIsOk()` - Verifies all digits are in valid range (0-9)

### Bugs Detected
The tests successfully detect the countdown bug in the Assignment version:
- **Bug**: Loop condition `i > 0` instead of `i >= 0` in DisplayController
- **Effect**: Tens-of-minutes digit never decrements, causing incorrect time calculations
- **Example**: Counting down from 10:00 incorrectly shows 1:9:59 instead of 9:59

## Technology Stack
- **Java** - Programming language
- **JUnit 4** - Testing framework
- **junit-quickcheck** - Property-based testing library
- **Gradle** - Build tool

## Running the Tests
```bash
# Run all tests
gradle test

# Run specific test class
gradle test --tests TestMicrowave

# Run with detailed output
gradle test --info
```

## Project Structure
```
src/
├── main/java/microwave/
│   ├── Microwave.java          # Main controller
│   ├── ModeController.java     # Mode management
│   ├── DisplayController.java  # Time display logic
│   └── Preset.java             # Preset configuration
└── test/java/microwave/
    └── TestMicrowave.java      # Property-based tests
```

## Key Learnings
1. Property-based testing is effective for finding edge cases
2. String-based `@InRange` annotations: `min="0"` not `minInt=0`
3. Exception handling needed for testing both correct and buggy versions
4. Helper methods improve test readability and reusability
5. Proper test ranges are crucial for detecting bugs efficiently

## Author
Created as part of software testing coursework focusing on property-based testing methodologies.

## License
Educational project - Free to use for learning purposes.
