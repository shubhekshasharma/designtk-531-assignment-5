# Unwind - System Architecture

## System Overview

Unwind is a smart bedside device that creates an immersive wind-down ritual to help users transition to sleep. The system uses real-time dock detection to control ambient lighting and soundscapes. When the user's phone is docked, the calming experience plays; when they pick up their phone, it pauses. This creates a closed-loop feedback system where not scrolling is rewarded with a compelling sensory experience.

## How It Works

1. **Session Start:** User docks their phone on the device and taps to begin. Ambient lights activate, calming sounds play, and a sleep countdown displays showing remaining sleep hours before their alarm.

2. **Real-time Response:** A proximity/pressure sensor detects when the user picks up their phone. The moment the phone leaves the dock, the experience pauses, lights dim, sounds stop. When the phone is returned, the experience resumes.

3. **Session End:** User taps the device again when ready to sleep. Sounds transition to white noise, lights fade off, and the alarm is armed for morning.

4. **ML Personalization:** Over time, the system learns which sound profiles, light settings, and ritual configurations help the user fall asleep faster.

## System Components

**Device 1 - Phone/Phone Simulator:**
- Sends alarm time and user preferences
- Receives session analytics for ML learning
- Simulated via terminal for midterm demo

**Device 2 - Unwind Bedside Device:**
- Dock sensor (IR proximity or pressure sensor) detects phone presence
- Controls LED ambient lighting
- Manages speaker for soundscapes and white noise
- Displays real-time sleep countdown
- Simulated via terminal for midterm demo

**Communication:**
- MQTT broker handles real-time messaging between devices
- Dock sensor state changes publish to MQTT topics
- Backend subscribes to events and logs session data

## Data Flow

### Session Start Flow
1. User docks phone → sensor detects presence
2. User taps device → publishes to `device/{id}/events/unwind_start` topic
3. Device activates lights, sounds, countdown display
4. Backend logs session start, fetches ML recommendations
5. Real-time countdown updates every minute

### Phone Pickup Flow
1. Dock sensor detects phone removed
2. Device publishes to `device/{id}/events/phone_pickup` topic
3. Experience pauses (lights dim, sounds stop)
4. Backend logs pickup event with timestamp

### Phone Return Flow
1. Dock sensor detects phone docked again
2. Device publishes to `device/{id}/events/phone_docked` topic
3. Experience resumes (lights on, sounds play)
4. Backend logs dock event

### Session End Flow
1. User taps device to stop
2. Publishes to `device/{id}/events/unwind_stop` topic
3. Sounds transition to white noise, lights fade off
4. Backend calculates session duration, phone pickup count
5. ML service updates user patterns for future personalization

## Diagram Guide

**Diagram 1 - Unwind Session Start:** Shows the flow when user begins a wind-down session, including MQTT message creation, backend processing, and ML recommendation retrieval.

**Diagram 2 - Phone App:** Illustrates how the phone app communicates with the backend via REST API to fetch user data and personalized recommendations.

**Diagram 3 - Unwind Session Stop:** Details the end-of-session flow, including validation checks and database logging.

## Implementation

**Midterm (Software Demo):**
- Two terminal windows communicating via local MQTT broker
- Terminal 1 simulates dock/undock events and user taps
- Terminal 2 displays experience state and countdown
- Demonstrates closed-loop feedback system

**Final (Hardware):**
- Raspberry Pi/Arduino with IR proximity sensor
- LED strip for ambient lighting
- Speaker for soundscapes and white noise
- Small display for countdown
- Physical dock area with sensor
