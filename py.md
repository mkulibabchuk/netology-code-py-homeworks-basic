class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in self.courses_in_progress:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return "Ошибка"

    def __str__(self):
        return f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за домашние задания: {sum(sum(self.grades.values(), [])) / len(sum(self.grades.values(), []))}\nКурсы в процессе изучения: {", ".join(self.courses_in_progress)}\nЗавершенные курсы: {", ".join(self.finished_courses)}'

    def __eq__(self, other):
        if isinstance(other, Student):
            return sum(sum(self.grades.values(), [])) / len(
                sum(self.grades.values(), [])
            ) == sum(sum(other.grades.values(), [])) / len(
                sum(other.grades.values(), [])
            )
        else:
            return False

    def __lt__(self, other):
        if isinstance(other, Student):
            return sum(sum(self.grades.values(), [])) / len(
                sum(self.grades.values(), [])
            ) < sum(sum(other.grades.values(), [])) / len(
                sum(other.grades.values(), [])
            )
        else:
            return False


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def __str__(self):
        return f"Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {sum(sum(self.grades.values(), [])) / len(sum(self.grades.values(), []))}"

    def __eq__(self, other):
        if isinstance(other, Lecturer):
            return sum(sum(self.grades.values(), [])) / len(
                sum(self.grades.values(), [])
            ) == sum(sum(other.grades.values(), [])) / len(
                sum(other.grades.values(), [])
            )
        else:
            return False

    def __lt__(self, other):
        if isinstance(other, Lecturer):
            return sum(sum(self.grades.values(), [])) / len(
                sum(self.grades.values(), [])
            ) < sum(sum(other.grades.values(), [])) / len(
                sum(other.grades.values(), [])
            )
        else:
            return False


class Reviewer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)

    def rate_hw(self, student, course, grade):
        if (
            isinstance(student, Student)
            and course in self.courses_attached
            and course in student.courses_in_progress
        ):
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return "Ошибка"

    def __str__(self):
        return f"Имя: {self.name}\nФамилия: {self.surname}"


best_student = Student("Ruoy", "Eman", "your_gender")
best_student.courses_in_progress += ["Python"]
best_student.finished_courses += ["Введение в программирование"]

cool_lecturer = Lecturer("Some", "Buddy")
cool_lecturer.courses_attached += ["Python"]

cool_reviewer = Reviewer("Some", "Buddy")
cool_reviewer.courses_attached += ["Python"]

best_student.rate_lecturer(cool_lecturer, "Python", 10)
best_student.rate_lecturer(cool_lecturer, "Python", 10)
best_student.rate_lecturer(cool_lecturer, "Python", 10)

cool_reviewer.rate_hw(best_student, "Python", 10)
cool_reviewer.rate_hw(best_student, "Python", 10)
cool_reviewer.rate_hw(best_student, "Python", 10)

print(cool_reviewer)
print(cool_lecturer)
print(best_student)


def student_avg(student):
    total_grades = sum(sum(student.grades.values(), []))
    total_courses = len(student.grades)
    avg_grade = total_grades / total_courses
    return avg_grade


def lecturer_avg(lecturer):
    total_grades = sum(sum(lecturer.grades.values(), []))
    total_courses = len(lecturer.grades)
    avg_grade = total_grades / total_courses
    return avg_grade


student_avg_grade = student_avg(best_student)
lecturer_avg_grade = lecturer_avg(cool_lecturer)

print(f"Средний балл студента: {student_avg_grade}")
print(f"Средний балл лектора: {lecturer_avg_grade}")
