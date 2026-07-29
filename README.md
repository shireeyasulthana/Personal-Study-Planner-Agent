# Personal-Study-Planner-Agent
An AI-powered study planner that creates personalized study schedules based on subjects, exam dates, study hours, and priorities. It allows users to modify plans, add assignments, track daily progress, and receive revision suggestions for effective exam preparation.


# Install the latest Google GenAI SDK
!pip install -q -U google-genai

# Import required libraries
from google import genai
from IPython.display import display, Markdown

# Enter your Gemini API Key
API_KEY="YOUR_API_KEY"

# Create Gemini client
client = genai.Client(api_key=API_KEY)


# Generate Study Plan

def generate_plan(subjects,
                  exam_date,
                  study_hours,
                  priority):

    # Prompt for generating a personalized study plan
    prompt = f"""
You are an AI Personal Study Planner.

Create a COMPLETE 7-DAY study plan.

Subjects:
{subjects}

Exam Date:
{exam_date}

Daily Study Hours:
{study_hours}

Priority Subject:
{priority}

STRICT INSTRUCTIONS:

1. Generate EXACTLY 7 DAYS.
2. Show Day 1, Day 2, Day 3, Day 4, Day 5, Day 6, and Day 7.
3. Each day must include:
   - Study Time
   - Subject
   - Topics
   - Short Break
   - Revision
4. Give more time to {priority}.
5. Balance all subjects.
6. Do NOT stop after Day 3.
7. Do NOT shorten the response.
8. Return ONLY the study plan.

Example Format:

Day 1
09:00–10:30 : Python
10:30–10:45 : Break
10:45–12:15 : Java
12:15–01:00 : Revision

Day 2
...

Continue until Day 7.
"""

    # Generate response from Gemini
    response = client.models.generate_content(
        model="models/gemini-flash-lite-latest",
        contents=prompt
    )

    # Return generated study plan
    return response.text


# Modify Existing Study Schedule

def modify_schedule(plan, change):

    # Prompt to modify study schedule
    prompt = f"""
Modify the following study schedule.

Current Schedule:

{plan}

Requested Change:

{change}

Return updated timetable only.
"""

    response = client.models.generate_content(
        model="models/gemini-flash-lite-latest",
        contents=prompt
    )

    return response.text



# Add New Assignment or Exam

def add_assignment(plan, assignment):

    # Prompt to update study schedule
    prompt = f"""
Update this study schedule.

Current Schedule:

{plan}

New Assignment:

{assignment}

Rearrange timetable.

Return only updated schedule.
"""

    response = client.models.generate_content(
        model="models/gemini-flash-lite-latest",
        contents=prompt
    )

    return response.text



# Daily Progress Tracker

def progress_tracker(plan, progress):

    # Prompt to generate tomorrow's study suggestion
    prompt = f"""
Current Study Plan

{plan}

Today's Progress

{progress}

Suggest what should be studied tomorrow.

Return only suggestions.
"""

    response = client.models.generate_content(
        model="models/gemini-flash-lite-latest",
        contents=prompt
    )

    return response.text



# Generate Revision Plan

def revision(plan):

    # Prompt to create revision schedule
    prompt = f"""
Based on this study plan

{plan}

Create a revision schedule.

Return only revision plan.
"""

    response = client.models.generate_content(
        model="models/gemini-flash-lite-latest",
        contents=prompt
    )

    return response.text


# Store generated study plan
plan = ""


# Main Menu Loop

while True:

    # Display menu
    print("\n===================================")
    print(" PERSONAL STUDY PLANNER AGENT")
    print("===================================")
    print("1. Generate Study Plan")
    print("2. Modify Schedule")
    print("3. Add Assignment")
    print("4. Daily Progress")
    print("5. Revision Suggestion")
    print("6. Exit")

    # Read user choice
    choice = input("\nEnter Choice : ").strip()

    # ---------------------------
    # Generate Study Plan
    # ---------------------------
    if choice == "1":

        subjects = input("Subjects : ")
        exam_date = input("Exam Date : ")
        study_hours = input("Daily Study Hours : ")
        priority = input("Priority Subject : ")

        print("\nGenerating Study Plan...\n")

        # Generate study plan
        plan = generate_plan(
            subjects,
            exam_date,
            study_hours,
            priority
        )

        print("\n========== STUDY PLAN ==========\n")
        print(plan)

        print("\n✅ Study Plan Generated Successfully")
        print("You can now choose Options 2–5.")

    # ---------------------------
    # Modify Schedule
    # ---------------------------
    elif choice == "2":

        if plan == "":
            print("\n⚠ Please generate a study plan first.")
            continue

        change = input("Enter Changes : ")

        plan = modify_schedule(plan, change)

        print("\n========== UPDATED STUDY PLAN ==========\n")
        print(plan)

    # ---------------------------
    # Add Assignment
    # ---------------------------
    elif choice == "3":

        if plan == "":
            print("\n⚠ Please generate a study plan first.")
            continue

        assignment = input("New Assignment/Exam : ")

        plan = add_assignment(plan, assignment)

        print("\n========== UPDATED STUDY PLAN ==========\n")
        print(plan)

    # ---------------------------
    # Daily Progress
    # ---------------------------
    elif choice == "4":

        if plan == "":
            print("\n⚠ Please generate a study plan first.")
            continue

        progress = input("Today's Progress : ")

        result = progress_tracker(plan, progress)

        print("\n========== TOMORROW'S SUGGESTION ==========\n")
        print(result)

    # ---------------------------
    # Revision Suggestion
    # ---------------------------
    elif choice == "5":

        if plan == "":
            print("\n⚠ Please generate a study plan first.")
            continue

        result = revision(plan)

        print("\n========== REVISION PLAN ==========\n")
        print(result)

    # ---------------------------
    # Exit Program
    # ---------------------------
    elif choice == "6":

        print("\n🙏 Thank You For Using Personal Study Planner Agent!")
        break

    # ---------------------------
    # Invalid Menu Choice
    # ---------------------------
    else:

        print("\n❌ Invalid Choice. Please Enter a Valid Option.")



OUTPUT:-

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 1
Subjects : python
Exam Date : 12/08/2026
Daily Study Hours : 4
Priority Subject : python

Generating Study Plan...


========== STUDY PLAN ==========

Day 1
09:00–10:30 : Python - Variables, Data Types, and Basic Operators
10:30–10:45 : Break
10:45–12:15 : Python - Control Flow (If-Else Statements and Loops)
12:15–01:00 : Revision

Day 2
09:00–10:30 : Python - Functions, Scope, and Arguments
10:30–10:45 : Break
10:45–12:15 : Python - Data Structures (Lists, Tuples, Sets, and Dictionaries)
12:15–01:00 : Revision

Day 3
09:00–10:30 : Python - Object-Oriented Programming (Classes and Objects)
10:30–10:45 : Break
10:45–12:15 : Python - OOP Pillars (Inheritance, Polymorphism, and Encapsulation)
12:15–01:00 : Revision

Day 4
09:00–10:30 : Python - File Handling (Reading, Writing, and CSV Operations)
10:30–10:45 : Break
10:45–12:15 : Python - Exception Handling and Error Management
12:15–01:00 : Revision

Day 5
09:00–10:30 : Python - Advanced Concepts (Generators, Decorators, and Iterators)
10:30–10:45 : Break
10:45–12:15 : Python - Working with Modules and Standard Libraries
12:15–01:00 : Revision

Day 6
09:00–10:30 : Python - Problem Solving and Algorithm Practice
10:30–10:45 : Break
10:45–12:15 : Python - Mini Project Implementation and Code Optimization
12:15–01:00 : Revision

Day 7
09:00–10:30 : Python - Comprehensive Mock Exam and Review
10:30–10:45 : Break
10:45–12:15 : Python - Final Concepts Refinement and Weak Areas Practice
12:15–01:00 : Revision

✅ Study Plan Generated Successfully
You can now choose Options 2–5.

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 2
Enter Changes :  add one more subject - AI

========== UPDATED STUDY PLAN ==========

Day 1
09:00–10:30 : Python - Variables, Data Types, and Basic Operators
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to AI, History, and Core Concepts
12:15–01:00 : Revision

Day 2
09:00–10:30 : Python - Control Flow (If-Else Statements and Loops)
10:30–10:45 : Break
10:45–12:15 : AI - Python for AI (NumPy and Pandas Basics)
12:15–01:00 : Revision

Day 3
09:00–10:30 : Python - Functions, Scope, and Arguments
10:30–10:45 : Break
10:45–12:15 : AI - Data Preprocessing and Visualization (Matplotlib & Seaborn)
12:15–01:00 : Revision

Day 4
09:00–10:30 : Python - Data Structures (Lists, Tuples, Sets, and Dictionaries)
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Machine Learning (Supervised vs Unsupervised)
12:15–01:00 : Revision

Day 5
09:00–10:30 : Python - Object-Oriented Programming (Classes and Objects)
10:30–10:45 : Break
10:45–12:15 : AI - Basic Machine Learning Algorithms (Linear & Logistic Regression)
12:15–01:00 : Revision

Day 6
09:00–10:30 : Python - OOP Pillars, File Handling & Exceptions
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Neural Networks and Deep Learning Basics
12:15–01:00 : Revision

Day 7
09:00–10:30 : Python & AI - Mini Project Implementation and Code Optimization
10:30–10:45 : Break
10:45–12:15 : Python & AI - Comprehensive Mock Exam and Review
12:15–01:00 : Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 3
New Assignment/Exam : 6/08/2026

========== UPDATED STUDY PLAN ==========

Day 1 (August 6, 2026)
09:00–10:30 : Python - Variables, Data Types, and Basic Operators
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to AI, History, and Core Concepts
12:15–01:00 : Revision

Day 2 (August 7, 2026)
09:00–10:30 : Python - Control Flow (If-Else Statements and Loops)
10:30–10:45 : Break
10:45–12:15 : AI - Python for AI (NumPy and Pandas Basics)
12:15–01:00 : Revision

Day 3 (August 8, 2026)
09:00–10:30 : Python - Functions, Scope, and Arguments
10:30–10:45 : Break
10:45–12:15 : AI - Data Preprocessing and Visualization (Matplotlib & Seaborn)
12:15–01:00 : Revision

Day 4 (August 9, 2026)
09:00–10:30 : Python - Data Structures (Lists, Tuples, Sets, and Dictionaries)
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Machine Learning (Supervised vs Unsupervised)
12:15–01:00 : Revision

Day 5 (August 10, 2026)
09:00–10:30 : Python - Object-Oriented Programming (Classes and Objects)
10:30–10:45 : Break
10:45–12:15 : AI - Basic Machine Learning Algorithms (Linear & Logistic Regression)
12:15–01:00 : Revision

Day 6 (August 11, 2026)
09:00–10:30 : Python - OOP Pillars, File Handling & Exceptions
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Neural Networks and Deep Learning Basics
12:15–01:00 : Revision

Day 7 (August 12, 2026)
09:00–10:30 : Python & AI - Mini Project Implementation and Code Optimization
10:30–10:45 : Break
10:45–12:15 : Python & AI - Comprehensive Mock Exam and Review
12:15–01:00 : Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 3
New Assignment/Exam : python 8/08/2026

========== UPDATED STUDY PLAN ==========

Day 1 (August 6, 2026)
09:00–10:30 : Python - Variables, Data Types, and Basic Operators
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to AI, History, and Core Concepts
12:15–01:00 : Revision

Day 2 (August 7, 2026)
09:00–10:30 : Python - Control Flow (If-Else Statements and Loops)
10:30–10:45 : Break
10:45–12:15 : AI - Python for AI (NumPy and Pandas Basics)
12:15–01:00 : Revision

Day 3 (August 8, 2026)
09:00–10:30 : Python - Functions, Scope, and Arguments
10:30–10:45 : Break
10:45–12:15 : AI - Data Preprocessing and Visualization (Matplotlib & Seaborn)
12:15–01:00 : Revision

Day 4 (August 9, 2026)
09:00–10:30 : Python - Data Structures (Lists, Tuples, Sets, and Dictionaries)
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Machine Learning (Supervised vs Unsupervised)
12:15–01:00 : Revision

Day 5 (August 10, 2026)
09:00–10:30 : Python - Object-Oriented Programming (Classes and Objects)
10:30–10:45 : Break
10:45–12:15 : AI - Basic Machine Learning Algorithms (Linear & Logistic Regression)
12:15–01:00 : Revision

Day 6 (August 11, 2026)
09:00–10:30 : Python - OOP Pillars, File Handling & Exceptions
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Neural Networks and Deep Learning Basics
12:15–01:00 : Revision

Day 7 (August 12, 2026)
09:00–10:30 : Python & AI - Mini Project Implementation and Code Optimization
10:30–10:45 : Break
10:45–12:15 : Python & AI - Comprehensive Mock Exam and Review
12:15–01:00 : Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 3
New Assignment/Exam : java exam date - 10/08/2026

========== UPDATED STUDY PLAN ==========

Day 1 (August 6, 2026)
09:00–10:30 : Python - Variables, Data Types, and Basic Operators
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to AI, History, and Core Concepts
12:15–01:00 : Revision

Day 2 (August 7, 2026)
09:00–10:30 : Python - Control Flow (If-Else Statements and Loops)
10:30–10:45 : Break
10:45–12:15 : AI - Python for AI (NumPy and Pandas Basics)
12:15–01:00 : Revision

Day 3 (August 8, 2026)
09:00–10:30 : Python - Functions, Scope, and Arguments
10:30–10:45 : Break
10:45–12:15 : AI - Data Preprocessing and Visualization (Matplotlib & Seaborn)
12:15–01:00 : Revision

Day 4 (August 9, 2026)
09:00–10:30 : Java Exam Prep - Core Concepts & Syntax Review
10:30–10:45 : Break
10:45–12:15 : Java Exam Prep - Practice Problems and Mock Test
12:15–01:00 : Revision

Day 5 (August 10, 2026)
09:00–10:30 : Java Exam Day / Final Review
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Machine Learning (Supervised vs Unsupervised)
12:15–01:00 : Revision

Day 6 (August 11, 2026)
09:00–10:30 : Python - Data Structures (Lists, Tuples, Sets, and Dictionaries)
10:30–10:45 : Break
10:45–12:15 : AI - Basic Machine Learning Algorithms (Linear & Logistic Regression)
12:15–01:00 : Revision

Day 7 (August 12, 2026)
09:00–10:30 : Python - Object-Oriented Programming (Classes and Objects)
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Neural Networks and Deep Learning Basics
12:15–01:00 : Revision

Day 8 (August 13, 2026)
09:00–10:30 : Python - OOP Pillars, File Handling & Exceptions
10:30–10:45 : Break
10:45–12:15 : Python & AI - Mini Project Implementation and Code Optimization
12:15–01:00 : Revision

Day 9 (August 14, 2026)
09:00–10:30 : Python & AI - Comprehensive Mock Exam and Review
10:30–10:45 : Break
10:45–12:15 : Final Course Review and Q&A
12:15–01:00 : Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 4
Today's Progress : java

========== TOMORROW'S SUGGESTION ==========

Day 5 (August 10, 2026)
09:00–10:30 : Java Exam Day / Final Review
10:30–10:45 : Break
10:45–12:15 : AI - Introduction to Machine Learning (Supervised vs Unsupervised)
12:15–01:00 : Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 4
Today's Progress : i have learned about advance python 

========== TOMORROW'S SUGGESTION ==========

Based on your current study plan and the fact that you have covered advanced Python today, here is what you should study tomorrow:

**Day 4 (August 9, 2026)**
* **09:00–10:30:** Java Exam Prep - Core Concepts & Syntax Review
* **10:30–10:45:** Break
* **10:45–12:15:** Java Exam Prep - Practice Problems and Mock Test
* **12:15–01:00:** Revision

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 5

========== REVISION PLAN ==========

Day 1 (August 6, 2026)
12:15–01:00 : Review Python Basics (Variables, Data Types, Operators) and AI Core Concepts

Day 2 (August 7, 2026)
12:15–01:00 : Review Control Flow (Loops, If-Else) and NumPy/Pandas Basics

Day 3 (August 8, 2026)
12:15–01:00 : Review Python Functions/Scope and Data Preprocessing/Visualization (Matplotlib, Seaborn)

Day 4 (August 9, 2026)
12:15–01:00 : Review Java Core Concepts, Syntax, and Mock Test Mistakes

Day 5 (August 10, 2026)
12:15–01:00 : Review Java Exam Takeaways and Supervised vs. Unsupervised Machine Learning Concepts

Day 6 (August 11, 2026)
12:15–01:00 : Review Python Data Structures and Linear/Logistic Regression Algorithms

Day 7 (August 12, 2026)
12:15–01:00 : Review Python OOP (Classes/Objects) and Neural Network/Deep Learning Basics

Day 8 (August 13, 2026)
12:15–01:00 : Review OOP Pillars, File Handling, Exceptions, and Mini Project Code

Day 9 (August 14, 2026)
12:15–01:00 : Final Comprehensive Review of All Course Materials and Mock Exam Error Analysis

===================================
 PERSONAL STUDY PLANNER AGENT
===================================
1. Generate Study Plan
2. Modify Schedule
3. Add Assignment
4. Daily Progress
5. Revision Suggestion
6. Exit

Enter Choice : 6

🙏 Thank You For Using Personal Study Planner Agent!


        
