import csv

def welcome():
    print("\n==== GradeBook Analyzer ====")
    print("1. Manual input")
    print("2. Load from CSV")
    print("3. Exit")

def load_manual():
    marks = {}
    n = int(input("Enter number of students: "))
    for _ in range(n):
        name = input("Name: ")
        score = int(input("Marks: "))
        marks[name] = score
    return marks

def load_csv():
    path = input("Enter CSV file path: ")
    marks = {}
    with open(path, 'r') as f:
        reader = csv.reader(f)
        for row in reader:
            if len(row) >= 2:
                name = row[0]
                score = int(row[1])
                marks[name] = score
    return marks

def calculate_average(marks_dict):
    return sum(marks_dict.values()) / len(marks_dict) if marks_dict else 0

def calculate_median(marks_dict):
    values = sorted(marks_dict.values())
    n = len(values)
    if n == 0:
        return 0
    if n % 2 == 1:
        return values[n // 2]
    return (values[n//2 - 1] + values[n//2]) / 2

def find_max_score(marks_dict):
    return max(marks_dict.values()) if marks_dict else None

def find_min_score(marks_dict):
    return min(marks_dict.values()) if marks_dict else None

def assign_grades(marks_dict):
    grades = {}
    for name, score in marks_dict.items():
        if score >= 90:
            grade = 'A'
        elif score >= 80:
            grade = 'B'
        elif score >= 70:
            grade = 'C'
        elif score >= 60:
            grade = 'D'
        else:
            grade = 'F'
        grades[name] = grade
    return grades

def grade_distribution(grades):
    dist = {'A':0,'B':0,'C':0,'D':0,'F':0}
    for g in grades.values():
        dist[g] += 1
    return dist

def pass_fail(marks_dict):
    passed = [name for name, score in marks_dict.items() if score >= 40]
    failed = [name for name, score in marks_dict.items() if score < 40]
    return passed, failed

def print_table(marks_dict, grades):
    print("\nName\tMarks\tGrade")
    print("----------------------")
    for name in marks_dict:
        print(f"{name}\t{marks_dict[name]}\t{grades[name]}")

def main():
    while True:
        welcome()
        choice = input("Enter choice: ")

        if choice == '1':
            marks = load_manual()
        elif choice == '2':
            marks = load_csv()
        elif choice == '3':
            print("Exiting...")
            break
        else:
            print("Invalid choice.")
            continue

        avg = calculate_average(marks)
        med = calculate_median(marks)
        mx = find_max_score(marks)
        mn = find_min_score(marks)

        print(f"\nAverage: {avg}")
        print(f"Median: {med}")
        print(f"Max: {mx}")
        print(f"Min: {mn}")

        grades = assign_grades(marks)
        dist = grade_distribution(grades)

        print("\nGrade Distribution:")
        for g, c in dist.items():
            print(f"{g}: {c}")

        passed, failed = pass_fail(marks)
        print(f"\nPassed: {passed}")
        print(f"Failed: {failed}")

        print_table(marks, grades)

        loop = input("\nRun again? (y/n): ")
        if loop.lower() != 'y':
            break

if __name__ == "__main__":
    main()
