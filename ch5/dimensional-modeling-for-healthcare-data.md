# Dimensional Modeling for Healthcare Data

## What You'll Learn

- What dimensional modeling is and why it is the standard approach for healthcare analytics
- The two core building blocks: fact tables and dimension tables
- How to design a star schema for a healthcare use case
- Common mistakes and how to avoid them
- A hands-on exercise to build your first model

Dimensional modeling is one of the most practical and widely used patterns in healthcare data warehousing. Once you understand it, you will recognize it everywhere — in Epic reporting databases, in Tuva Project data models, in nearly every analytics layer you will encounter on the job.

Think of it as designing a smart filing system: you separate *what happened* from *the context around it*, which makes querying fast and analysis intuitive.

## What Is Dimensional Modeling?

Dimensional modeling is a database design approach optimized for analytical queries. Rather than normalizing data into many small tables (as you would in a transactional system), dimensional modeling organizes data around two types of tables — facts and dimensions — that make it fast and intuitive to answer business questions.

In healthcare, the questions analytics teams need to answer look like this:
- **What** treatments did patients receive?
- **When** did they visit?
- **Where** did care happen?
- **Who** provided care?
- **How much** did it cost?

Good data models let clinicians and operations teams find answers to these questions quickly. The downstream impact — better staffing decisions, earlier detection of care gaps, faster identification of billing errors — is significant.

## The Two Building Blocks: Facts and Dimensions

Every dimensional model has two types of tables:

### Facts: The "What Happened" Tables

**Facts** are the events or measurements that happened. Think of facts as the story of what actually occurred. In healthcare, facts are things like:

- A patient visit happened
- A medication was given
- A lab test was performed
- A bill was charged

Facts usually contain numbers you can add up or calculate. For example:
- The visit lasted 45 minutes
- The medication cost $127
- The patient's blood pressure was 120/80
- The hospital charge was $3,500

**Here's the key thing about facts:** They answer the question "What happened and how much?"

### Dimensions: The "Context" Tables

**Dimensions** are all the details that give context to what happened. They answer questions like who, what, where, when, and how. 

Think of dimensions as the supporting characters in your story. They describe:

- **Patient dimension**: Who is the patient? (age, gender, location)
- **Time dimension**: When did it happen? (date, month, year, day of week)
- **Provider dimension**: Who gave the care? (doctor name, specialty, hospital)
- **Diagnosis dimension**: What was wrong? (disease name, type, severity)

**Here's the key thing about dimensions:** They answer all the "W" questions – Who, What, Where, When, and sometimes Why.

## Your First Healthcare Data Model

Let's build a simple model for tracking patient visits to a clinic.

### Scenario: Sunny Valley Family Clinic

Sunny Valley Clinic wants to understand their patient visits better. They want to answer questions like:
- How many patients do we see each day?
- Which doctors are seeing the most patients?
- What are the most common reasons patients visit?
- How long are patients waiting?

Here's how we'd build this model:

### The Fact Table: Visit Facts

This is our main table that records each visit:

```
Visit_Fact Table:
- Visit_ID (unique number for each visit)
- Patient_ID (connects to Patient dimension)
- Date_ID (connects to Date dimension)
- Provider_ID (connects to Provider dimension)
- Diagnosis_ID (connects to Diagnosis dimension)
- Wait_Time_Minutes (how long they waited)
- Visit_Duration_Minutes (how long the visit lasted)
- Charge_Amount (what we billed)
```

### The Dimension Tables

Now we create tables for all the details:

**Patient Dimension:**
```
- Patient_ID
- Patient_Name
- Date_of_Birth
- Gender
- City
- State
- Zip_Code
```

**Date Dimension:**
```
- Date_ID
- Full_Date
- Day_of_Week (Monday, Tuesday, etc.)
- Month
- Year
- Is_Weekend (Yes or No)
- Is_Holiday (Yes or No)
```

**Provider Dimension:**
```
- Provider_ID
- Provider_Name
- Specialty (Family Medicine, Pediatrics, etc.)
- Years_of_Experience
```

**Diagnosis Dimension:**
```
- Diagnosis_ID
- Diagnosis_Name
- Category (Acute, Chronic, Preventive)
- Body_System (Respiratory, Cardiac, etc.)
```

## How It All Connects: The Star Schema

When you draw this out, it looks like a star — the fact table sits in the middle, and dimension tables radiate outward. That is why it is called a **star schema**.

Here's the beautiful part: this design lets you answer almost any question by mixing and matching dimensions:

- "How many male patients visited in January?" → Use Patient + Date dimensions
- "Which doctor sees the most diabetes patients?" → Use Provider + Diagnosis dimensions
- "What's our average wait time on Mondays?" → Use Date dimension + Wait_Time fact
- "How much revenue came from pediatric visits last year?" → Use Provider + Date dimensions + Charge_Amount fact

## Why This Matters for Healthcare

In healthcare, every number represents a real patient. When dimensional models are built well:
- Emergency rooms can predict busy times and staff appropriately
- Clinics can identify patients who need follow-up care
- Hospitals can spot medication errors before they harm patients
- Healthcare systems can reduce costs while improving outcomes

The model you design shapes what questions can be answered — and by extension, what decisions get made.

## Common Beginner Mistakes (And How to Avoid Them)

**Mistake #1: Putting too much in the fact table**
Don't put descriptive text in facts. "Patient John Smith" doesn't belong in a fact table. Just put "Patient_ID = 12345" and let the Patient dimension hold the name.

**Mistake #2: Making dimensions too small**
It's okay to have lots of details in dimensions! "Patient_Favorite_Color" might seem silly, but if it helps understand patient preferences, include it.

**Mistake #3: Forgetting the date dimension**
Every fact should have a date! Time is almost always important in healthcare analytics.

**Mistake #4: Using confusing names**
Be clear and consistent. If you use "Provider" in one table, don't switch to "Doctor" or "Physician" in another.

## Practice Exercise

Design a dimensional model for tracking COVID-19 vaccinations at a clinic.

- What facts would you track?
- What dimensions would you need? (patients, dates, vaccine types, providers...)
- What analytical questions would your model support?

Sketch the schema before moving to the next section. Comparing your design to a colleague's or to the Tuva Project's vaccination data model is a useful calibration exercise.

## What's Next

The next sections build on this foundation:
- More complex healthcare scenarios (hospital admissions, lab results)
- Handling data that changes over time (slowly changing dimensions)
- Time-series clinical data patterns
- Patient journey modeling across multiple encounters

## Key Takeaways

- **Dimensional modeling** organizes data into facts (what happened) and dimensions (context)
- **Facts** contain measurements and numeric values you can aggregate
- **Dimensions** contain descriptive attributes that answer who, what, where, when
- **Star schemas** place the fact table at the center, with dimensions joined around it
- Good data models are the foundation that makes every downstream analysis faster and more reliable