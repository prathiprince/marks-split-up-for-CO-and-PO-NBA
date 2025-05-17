# marks-split-up-for-CO-and-PO-NBA
import random
import pandas as pd

# Total marks of each student
total_marks = [
    44,47,44,40,36,36,46,45,45,37,
    36,44,44,46,39,37,47,46,46,46,
    46,45,46,46,40,46,42,46,46,41,
    46,44,45,38,46,44,48,36,43,43
]

num_students = len(total_marks)

max_1_5 = 10  # total max marks for Q1-Q5 (5 * 2)
max_6 = 13
max_7 = 13
max_8 = 14

def distribute_marks(total, max_1_5=10, max_6=13, max_7=13, max_8=14):
    raw_1_5 = total * max_1_5 / 50
    raw_6 = total * max_6 / 50
    raw_7 = total * max_7 / 50
    raw_8 = total * max_8 / 50

    marks_1_5 = int(raw_1_5)
    marks_6 = int(raw_6)
    marks_7 = int(raw_7)
    marks_8 = int(raw_8)

    remainder = total - (marks_1_5 + marks_6 + marks_7 + marks_8)

    # Add remainder to marks_8 but cap at max_8
    marks_8 += remainder
    if marks_8 > max_8:
        overflow = marks_8 - max_8
        marks_8 = max_8
        
        marks_7 += overflow
        if marks_7 > max_7:
            overflow2 = marks_7 - max_7
            marks_7 = max_7
            
            marks_6 += overflow2
            if marks_6 > max_6:
                overflow3 = marks_6 - max_6
                marks_6 = max_6
                
                marks_1_5 += overflow3
                if marks_1_5 > max_1_5:
                    marks_1_5 = max_1_5  # cap finally

    base_per_q = marks_1_5 // 5
    extra = marks_1_5 % 5
    q_marks = [base_per_q] * 5
    for i in range(extra):
        q_marks[i] += 1

    q6_choice = random.choice(['a', 'b'])
    q7_choice = random.choice(['a', 'b'])
    q8_choice = random.choice(['a', 'b'])

    row = {
        'Q1': q_marks[0], 'Q2': q_marks[1], 'Q3': q_marks[2], 'Q4': q_marks[3], 'Q5': q_marks[4],
        'Q6.a': marks_6 if q6_choice == 'a' else 0,
        'Q6.b': marks_6 if q6_choice == 'b' else 0,
        'Q7.a': marks_7 if q7_choice == 'a' else 0,
        'Q7.b': marks_7 if q7_choice == 'b' else 0,
        'Q8.a': marks_8 if q8_choice == 'a' else 0,
        'Q8.b': marks_8 if q8_choice == 'b' else 0,
    }
    return row

# Collect all students' marks
all_students_marks = []
for total in total_marks:
    marks = distribute_marks(total)
    all_students_marks.append(marks)

# Create a DataFrame
df = pd.DataFrame(all_students_marks)

# Show the first few rows
print(df.head())

# Save to Excel file
df.to_excel('student_marks_distribution.xlsx', index=False)

print("Excel file 'student_marks_distribution.xlsx' created successfully!")
