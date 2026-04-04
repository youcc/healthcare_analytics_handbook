# Patient Journey Mapping

## What You'll Learn

- What a patient journey is and why it is more useful than analyzing isolated events
- The four stages of a patient journey and what data to capture at each stage
- How to design an event-based data model for journeys
- How to identify where patients drop out of care (breakpoints)
- Key metrics for measuring journey quality and outcomes

Patient journey mapping is one of the most impactful analytical frameworks in healthcare — and one that translates directly into reduced readmissions, better care coordination, and improved patient outcomes. It is also what separates analysts who understand healthcare from those who are only processing rows in a table.

## What Is a Patient Journey?

Consider Maria, a fictional but realistic patient:

**Maria's Journey:**
- January 5: Feels chest pain, calls her doctor
- January 8: Gets appointment, doctor orders tests
- January 12: Blood tests and EKG at lab
- January 18: Results show high cholesterol, gets prescription
- February 2: Picks up medication at pharmacy
- February 15: Feels dizzy, calls nurse hotline
- February 20: Medication adjusted, gets new prescription
- March 5: Follow-up appointment, feeling better
- April 5: Routine check-up, cholesterol improving

Each step connects to the next. The chest pain led to tests, tests led to diagnosis, diagnosis led to treatment, and treatment needed adjustment before stabilizing.

**Patient journey mapping** is about capturing this entire sequence in your data model so you can:
- See how patients move through the healthcare system
- Identify where patients get stuck or lost
- Understand which paths lead to better outcomes
- Find opportunities to improve care
- Predict what a patient might need next

## Why Journey Mapping Changes Everything

Here are two ways of looking at the same patient:

### The Old Way: Isolated Events
```
- Emergency room visit: $2,500
- Lab test: $400
- Doctor visit: $200
- Prescription: $150
- Hospital admission: $15,000
```

This tells you what happened and what it cost. But it doesn't tell you **why** or **how these connect**.

### The Journey Way: Connected Story
```
1. Patient developed infection (emergency room)
2. Infection not fully treated (incomplete medication)
3. Infection worsened (return visit)
4. Developed complications (lab tests)
5. Required hospitalization (could have been prevented!)
```

Now you can see: if we had made sure the patient completed their antibiotics after step 1, steps 3-5 might never have happened! That would save money AND prevent suffering.

**This is the power of journey mapping** – you see causes and effects, not just events.

## The Four Stages of Every Patient Journey

### Stage 1: Awareness (Something's Wrong)

This is when the patient first realizes they need healthcare.

**Examples:**
- "I've been coughing for three weeks"
- "This pain won't go away"
- "It's time for my annual checkup"
- "I need to refill my medication"

**What to track:**
- How did they discover the problem?
- How long did they wait before seeking care?
- What symptoms or concerns did they have?
- Where did they go for information first?

**Why it matters:** Understanding how patients enter the system helps you improve access and education.

### Stage 2: Engagement (Entering the System)

This is when the patient actually makes contact with healthcare providers.

**Examples:**
- Calling to schedule an appointment
- Walking into urgent care
- Going to the emergency room
- Using a telemedicine app
- Calling a nurse hotline

**What to track:**
- First point of contact
- Wait time to get an appointment
- Which entry point they used (ER, clinic, phone)
- Any barriers they faced (no insurance, transportation issues)

**Why it matters:** Many patients get lost right here. If it's too hard to schedule or they wait too long, they might give up and get sicker.

### Stage 3: Treatment (Getting Care)

This is the main part of the journey where diagnosis and treatment happen.

**Examples:**
- Doctor appointments
- Lab tests and imaging
- Surgeries or procedures
- Physical therapy sessions
- Medications prescribed and taken
- Hospital stays

**What to track:**
- All encounters and interventions
- Test results and diagnoses
- Medication adherence (are they taking their meds?)
- Treatment plan changes
- Multiple providers involved
- Care coordination between providers

**Why it matters:** This is where most of the healing happens. You need to see if treatments are working and if care is coordinated.

### Stage 4: Outcomes (What Happened?)

This is the result of the journey – how did the patient do?

**Examples:**
- Symptoms resolved
- Condition improved
- Health maintained
- Needed ongoing management
- Experienced complications
- Got readmitted to hospital
- Unfortunately, some patients pass away

**What to track:**
- Health improvements or declines
- Patient satisfaction
- Quality of life measures
- Whether they completed treatment
- Time to recovery
- Complications or setbacks
- Long-term health status

**Why it matters:** This tells you if your healthcare system is actually helping people get better!

## Building Your Patient Journey Data Model

The standard approach for modeling patient journeys is an **event-based model** — every discrete thing that happens to a patient is one row in an event fact table.

### The Core: Patient Journey Event Fact Table

```
Patient_Journey_Event_Fact:
- Event_ID (unique ID for this event)
- Patient_ID (which patient)
- Event_Timestamp (exact date and time)
- Event_Sequence_Number (1st event, 2nd event, 3rd...)
- Event_Type (Visit, Test, Prescription, Hospitalization, etc.)
- Event_Category (Awareness, Engagement, Treatment, Outcome)
- Location_ID (where it happened)
- Provider_ID (who was involved)
- Diagnosis_ID (what was the health issue)
- Procedure_ID (what was done)
- Cost_Amount (what it cost)
- Days_Since_Previous_Event (time between events)
- Journey_Phase (Initial, Active Treatment, Follow-up, Completed)
```

### The Journey Dimension

Each journey groups all related events under a single identifier:

```
Patient_Journey_Dimension:
- Journey_ID (unique ID for the journey)
- Patient_ID (which patient)
- Journey_Start_Date (when it began)
- Journey_End_Date (when it completed, if it has)
- Primary_Diagnosis (main health issue)
- Journey_Type (Acute Illness, Chronic Management, Preventive Care, Surgical)
- Journey_Status (Active, Completed, Abandoned, Ongoing)
- Total_Events (how many things happened)
- Total_Duration_Days (how long from start to finish)
- Total_Cost (all costs combined)
- Outcome_Status (Resolved, Improved, Stable, Declined)
- Completion_Status (Completed Successfully, Incomplete, Lost to Follow-up)
```

### Connecting Events into Journeys

Each event links to a `journey_ID`, so you can reconstruct the full sequence for any patient:

**Example: Sarah's Diabetes Journey (Journey_ID = 12345)**
```
Event 1: Initial symptoms → called doctor
Event 2: Scheduled appointment
Event 3: Office visit → tests ordered
Event 4: Lab work → diagnosed with diabetes
Event 5: Education session about diabetes
Event 6: Prescription for metformin
Event 7: Picked up medication at pharmacy
Event 8: Follow-up visit → checking blood sugar
Event 9: Referred to nutritionist
Event 10: Nutrition counseling session
Event 11: Three-month checkup → doing well!
```

Each event connects to the next, forming a chain you can analyze end-to-end.

## Types of Patient Journeys

Not all journeys have the same structure. The type shapes how you model and analyze them:

### Journey Type 1: Acute Care Journey (Short and Intense)

**Example:** Broken arm, flu, appendicitis

**Characteristics:**
- Clear beginning and end
- Usually completes in days to weeks
- Intense care during the episode
- Clear success or failure outcome

**How to model it:**
- Track from first symptom to complete recovery
- Measure time to treatment
- Track if complications occurred
- Measure whether patient followed treatment plan

### Journey Type 2: Chronic Disease Journey (Long and Ongoing)

**Example:** Diabetes, heart disease, asthma

**Characteristics:**
- No clear end date
- Continues for years or lifetime
- Cycles of checkups and adjustments
- Success is about management, not cure

**How to model it:**
- Track from diagnosis date forward
- Measure control over time (is diabetes managed?)
- Look for patterns of flare-ups
- Track medication adherence over months/years
- Identify successful management strategies

### Journey Type 3: Preventive Care Journey (Regular Maintenance)

**Example:** Annual physicals, vaccinations, screenings

**Characteristics:**
- Regular, scheduled events
- Goal is to prevent future problems
- May continue entire lifetime
- Success is what DOESN'T happen

**How to model it:**
- Track adherence to schedule (did they come when supposed to?)
- Monitor gaps in care
- Look for early detection of problems
- Measure prevention success (fewer illnesses)

### Journey Type 4: Surgical Journey (Procedure-Centered)

**Example:** Hip replacement, heart surgery, cesarean section

**Characteristics:**
- Pre-op preparation phase
- The surgery event itself
- Post-op recovery and rehabilitation
- Clear milestones and benchmarks

**How to model it:**
- Track all pre-surgery events (tests, clearances, education)
- Capture surgery details
- Monitor recovery milestones
- Track complications or readmissions
- Measure return to function

## Measuring Journey Quality: Key Metrics

### Metric 1: Time to Treatment

**What it is:** How long from first symptom/concern until treatment begins

**Why it matters:** Faster treatment often means better outcomes

**Example:** For heart attacks, every minute counts. For cancer, early treatment dramatically improves survival.

**How to calculate:**
```
Days between first awareness event and first treatment event
```

### Metric 2: Journey Completion Rate

**What it is:** Percentage of patients who complete their full treatment plan

**Why it matters:** Incomplete treatment often leads to worse outcomes and wasted money

**Example:** If patients don't finish their antibiotics, infections come back worse.

**How to calculate:**
```
(Patients who reached final outcome event ÷ All patients who started journey) × 100
```

### Metric 3: Time Between Events (Gaps in Care)

**What it is:** How much time passes between healthcare encounters

**Why it matters:** Long gaps might mean patient is lost or avoiding care

**Example:** A diabetes patient should have checkups every 3 months. If they go 9 months, something's wrong.

**How to calculate:**
```
Days between consecutive events for the same patient
```

### Metric 4: Journey Cost Efficiency

**What it is:** Total cost of the journey compared to outcome achieved

**Why it matters:** Some paths cost less and work better

**Example:** Preventing hospital readmission through good follow-up care costs less than treating complications.

**How to calculate:**
```
Total cost of all events in journey ÷ Outcome success score
```

### Metric 5: Patient Path Variation

**What it is:** How similar or different are journeys for the same condition?

**Why it matters:** High variation might mean inconsistent care quality

**Example:** If 10 patients with pneumonia all follow different treatment paths, maybe there's no standard protocol.

**How to calculate:**
```
Compare event sequences for similar patients and measure differences
```

## Journey Branching and Decision Points

Patient journeys branch based on clinical decisions and patient behavior. Modeling those branches lets you compare outcomes across different paths:

### Example: Heart Disease Journey Branches

```
Starting Point: Patient diagnosed with high cholesterol

Branch A: Patient takes medication regularly
→ Cholesterol improves
→ Regular monitoring
→ Success!

Branch B: Patient doesn't take medication
→ Cholesterol stays high
→ Develops chest pain
→ Needs emergency care
→ More expensive, worse outcome

Branch C: Patient takes medication but has side effects
→ Medication changed
→ Finds medication that works
→ Success (but took longer)
```

A `Journey_Path` table tracks which branch each patient followed:

```
Patient_Journey_Path:
- Path_ID
- Journey_ID
- Patient_ID
- Decision_Point (Medication adherence, Test result, Symptom change)
- Path_Taken (Branch A, B, or C)
- Path_Outcome (Success, Complication, Ongoing)
- Path_Cost
- Path_Duration
```

This lets you answer questions like:
- "Which path leads to best outcomes?"
- "Why do some patients end up in Branch B?"
- "How can we help more patients get to Branch A?"

## Identifying Where Patients Get Lost

One of the most important things you'll do is find where patients drop out of their journey. These are called **breakpoints** or **leakage points**.

### Common Breakpoints:

**Breakpoint 1: Between Diagnosis and Treatment**
Patient finds out what's wrong but never starts treatment.

**Why it happens:**
- Couldn't afford medication
- Scared of side effects
- Didn't understand instructions
- Forgot to follow up

**Breakpoint 2: During Treatment**
Patient starts treatment but stops before completing it.

**Why it happens:**
- Felt better and stopped too soon
- Experienced side effects
- Lost insurance coverage
- Couldn't get time off work

**Breakpoint 3: After Initial Treatment**
Patient completes first treatment but doesn't come back for follow-up.

**Why it happens:**
- Feels fine and thinks they're done
- No one scheduled follow-up
- Transportation difficulties
- Changed insurance/providers

### How to Model Breakpoints

Add flags to your journey model:

```
Journey_Breakpoint_Analysis:
- Journey_ID
- Expected_Next_Event (what should happen next)
- Expected_Event_Date (when it should happen)
- Actual_Next_Event (what actually happened)
- Days_Overdue (if patient didn't return on time)
- Breakpoint_Flag (Yes/No)
- Breakpoint_Reason (if known)
- Intervention_Attempted (did anyone try to bring them back?)
- Journey_Restarted (did they come back later?)
```

## Real-World Example: Reducing Hospital Readmissions

Let me share a success story that shows why journey mapping matters:

**The Problem:**
A hospital noticed 20% of heart failure patients were readmitted within 30 days of discharge. That's expensive and bad for patients!

**The Journey Analysis:**
They mapped the discharge journey and found breakpoints:
- Day 0: Patient discharged with instructions
- Day 2: **BREAKPOINT** - Most patients should call with weight check, but 60% don't
- Day 7: **BREAKPOINT** - Should have follow-up appointment scheduled, but 40% don't
- Day 14: **BREAKPOINT** - Should see cardiologist, but 50% miss appointment
- Day 30: 20% back in emergency room

**The Solution:**
They redesigned the journey:
- Day 0: Discharge nurse schedules ALL appointments before patient leaves
- Day 2: Automated phone call reminds about weight check
- Day 5: Nurse calls if patient hasn't checked in
- Day 13: Appointment reminder plus transportation assistance offered
- Day 30: Readmission rate dropped to 8%!

Journey mapping made the breakpoints visible so the team could intervene at specific points rather than guessing.

## Practice Exercise

Design a patient journey model for **cancer screening and diagnosis**.

Think through:

**Stage 1: Awareness**
- How does patient learn they need screening?
- What triggers them to take action?

**Stage 2: Engagement**
- How do they schedule screening?
- What barriers might they face?

**Stage 3: Treatment (if cancer found)**
- What's the diagnostic process?
- How do they move from screening to diagnosis to treatment?
- Where might they get lost?

**Stage 4: Outcomes**
- What are the possible outcomes?
- How long does the journey take?
- What defines success?

**Now model it:**
1. Design your Event Fact table
2. List the key events in order
3. Identify potential breakpoints
4. Define success metrics
5. Think about how to help patients complete the journey

There is no single correct answer — the goal is to think through the data requirements before writing any code.

## Connecting Journeys Across Time

Patients often have multiple journeys, and those journeys are frequently related:

**Example: Connected Journeys**
```
Journey 1: Initial diagnosis and treatment of diabetes (2020)
→ Successful outcome
→ Patient enters chronic management phase

Journey 2: Develops heart disease (2022)
→ Often related to diabetes!
→ New journey begins, but builds on first journey

Journey 3: Needs kidney screening (2024)
→ Because diabetes AND heart disease increase risk
→ Another connected journey
```

**How to model this:**
```
Journey_Relationship:
- Primary_Journey_ID
- Related_Journey_ID
- Relationship_Type (Caused by, Related to, Complication of, Follow-up to)
- Time_Between_Journeys_Days
```

This structure lets you analyze how one health issue leads to others, and whether early intervention on Journey 1 reduced the severity or likelihood of Journey 2.

## The Ethics of Journey Mapping

Patient journey data is among the most sensitive data you will ever work with. Each record represents a real person's health history. A few principles worth internalizing:

**Respect privacy.** Never share identifiable patient information outside authorized contexts. Anonymize any case examples.

**Avoid attribution bias.** When a patient stops treatment or misses appointments, there is usually a systemic reason — cost, transportation, language barriers, fear. Your job is to identify those patterns, not assign blame.

**Account for social factors.** Patients without transportation, flexible work schedules, or English proficiency will have structurally different journeys. Models should surface those inequities for intervention, not treat them as noise.

**Use insights to fix systems, not blame individuals.** Journey analysis should point to process improvements — scheduling gaps, discharge protocols, follow-up workflows — not be weaponized against patients or individual clinicians.

## Why This Skill Matters

Patient journey mapping is in demand across nearly every healthcare organization:
- Hospitals need to reduce readmissions (which carries CMS financial penalties)
- Payers need to improve care coordination and close care gaps
- Health tech companies are building tools that depend on journey-aware data models
- Researchers use journey data to evaluate treatment protocols

More broadly, journey mapping trains you to see healthcare the way patients experience it — as a continuous story, not isolated billing events. That perspective is rare and valuable.

## Key Takeaways

- Patients have journeys, not just isolated encounters — context between events is where the insight lives
- Journeys follow four stages: **Awareness → Engagement → Treatment → Outcomes**
- Breakpoints are where patients fall out of care — identifying them is actionable
- Acute, chronic, preventive, and surgical journeys have different modeling requirements
- Core metrics: time to treatment, completion rate, gaps in care, cost efficiency, path variation
- Related journeys across time reveal how one condition cascades into others
- Ethical use of journey data means fixing systems, not blaming patients

## What's Next

Chapter 5 has covered the three core data modeling patterns in healthcare:
1. Dimensional modeling — structuring data for analytical queries
2. Time-series clinical data — tracking change and trends over time
3. Patient journey mapping — connecting events into meaningful care narratives

These three patterns appear together in most real healthcare analytics platforms. The next chapters move into SQL patterns, visualization, and advanced analytics that build directly on these foundations.