import random

# 학생 정보 생성 함수
def generate_students(num_students=30):
    students = []
    for _ in range(num_students):
        name = ''.join(random.choices('ABCDEFGHIJKLMNOPQRSTUVWXYZ', k=2))
        age = random.randint(18, 22)
        score = random.randint(0, 100)
        students.append({"이름": name, "나이": age, "성적": score})
    return students

# 선택 정렬
def selection_sort(data, key):
    data = data[:]
    n = len(data)
    for i in range(n):
        min_idx = i
        for j in range(i + 1, n):
            if data[j][key] < data[min_idx][key]:
                min_idx = j
        data[i], data[min_idx] = data[min_idx], data[i]
    return data

# 삽입 정렬
def insertion_sort(data, key):
    data = data[:]
    for i in range(1, len(data)):
        current = data[i]
        j = i - 1
        while j >= 0 and data[j][key] > current[key]:
            data[j + 1] = data[j]
            j -= 1
        data[j + 1] = current
    return data

# 퀵 정렬
def quick_sort(data, key):
    if len(data) <= 1:
        return data
    pivot = data[len(data) // 2]
    left = [x for x in data if x[key] < pivot[key]]
    middle = [x for x in data if x[key] == pivot[key]]
    right = [x for x in data if x[key] > pivot[key]]
    return quick_sort(left, key) + middle + quick_sort(right, key)

# 기수 정렬 (성적 기준으로만)
def radix_sort(data, key):
    max_val = max(data, key=lambda x: x[key])[key]
    exp = 1
    output = data[:]
    while max_val // exp > 0:
        count = [0] * 10
        for item in output:
            index = (item[key] // exp) % 10
            count[index] += 1
        for i in range(1, 10):
            count[i] += count[i - 1]
        temp = [0] * len(output)
        for item in reversed(output):
            index = (item[key] // exp) % 10
            temp[count[index] - 1] = item
            count[index] -= 1
        output = temp
        exp *= 10
    return output

# 메뉴 출력 및 사용자 입력 처리
def main():
    students = generate_students()
    print("생성된 학생 정보:")
    for student in students:
        print(student)

    while True:
        print("\n메뉴:")
        print("1. 이름을 기준으로 정렬")
        print("2. 나이를 기준으로 정렬")
        print("3. 성적을 기준으로 정렬")
        print("4. 프로그램 종료")
        choice = input("선택: ")

        if choice == '4':
            print("프로그램을 종료합니다.")
            break

        print("\n사용할 정렬 알고리즘:")
        print("1. 선택 정렬")
        print("2. 삽입 정렬")
        print("3. 퀵 정렬")
        if choice == '3':
            print("4. 기수 정렬 (성적 기준에서만 사용 가능)")
        algorithm = input("선택: ")

        key_map = {'1': "이름", '2': "나이", '3': "성적"}
        sort_key = key_map.get(choice, None)
        if not sort_key:
            print("잘못된 선택입니다. 다시 시도하세요.")
            continue

        if algorithm == '1':
            sorted_students = selection_sort(students, sort_key)
        elif algorithm == '2':
            sorted_students = insertion_sort(students, sort_key)
        elif algorithm == '3':
            sorted_students = quick_sort(students, sort_key)
        elif algorithm == '4' and choice == '3':
            sorted_students = radix_sort(students, sort_key)
        else:
            print("잘못된 선택입니다. 다시 시도하세요.")
            continue

        print(f"\n{sort_key} 기준 정렬 결과:")
        for student in sorted_students:
            print(student)

if __name__ == "__main__":
    main()
