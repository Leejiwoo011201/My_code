# My_code

## O5_code
#### timetable_algorithm 페이지의 코드

    import itertools  #import itertools: Python의 itertools 모듈을 가져옵니다. 이 모듈은 조합(combination)을 생성하는데 사용됩니다.



    # 한국어 요일 약어를 해당 요일의 인덱스로 변환하는 함수를 정의합니다.
    def day_to_index(day):
      return {'월': 0, '화': 1, '수': 2, '목': 3, '금': 4, '토': 5, '일': 6}[day]  

     # 그룹을 정렬하는 함수입니다. 입력으로 그룹들의 리스트를 받고, 그룹들을 정렬한 리스트를 반환합니다.
    def sort_groups(all_groups):
        if not all_groups:
            return []  # all_groups가 비어있을 경우 빈 리스트 반환
    
        sort_all_groups = []  # 정렬된 그룹을 저장할 리스트를 초기화합니다.
        for i in all_groups:
            tmp_group = []  # 임시 그룹을 초기화합니다.
            for j in i:
                tmp = []  # 임시 리스트를 초기화합니다.
                # 각 그룹의 요소에서 특정 위치의 정보를 추출하여 tmp에 추가합니다.
                tmp.append(j[0][0][7])   # 대학수학(1)의 위치
                tmp.append(j[0][0][15])  # 화7,수5,수6의 위치
                tmp.append(j[0][0][0])   # 1001의 위치
                tmp.append(j[0][0][14])  # 전체의 위치
                tmp.append(j[0][0][8])   # 자연계열의 위치
    
                class_list = []  # 강의 목록을 저장할 리스트를 초기화합니다.
                class_index = 0
                day = ' '
    
                for k in tmp[1].split(','):
                    myclass = tmp[4].split(',')
                    if day == ' ':
                        day = k[0]
    
                    if k[0] != day:
                        class_index += 1
                        day = k[0]
                    class_list.append(myclass[class_index])
    
                tmp[4] = class_list  # tmp의 마지막 요소에 강의 목록을 추가합니다.
                tmp_group.append(tmp)  # tmp_group에 tmp를 추가합니다.
            sort_all_groups.append(tmp_group)  # sort_all_groups에 tmp_group을 추가합니다.
        return sort_all_groups  # 정렬된 그룹 리스트를 반환합니다.
 
    #정렬된 그룹들을 기반으로 가능한 모든 시간표 조합을 생성하는 함수입니다. 입력으로 정렬된 그룹들의 리스트를 받고, 가능한 조합과 해당 시간표의 유효성 여부를 반환합니다.
    def generate_possible_combinations(sort_all_groups): 
        if not sort_all_groups:
            return []  # sort_all_groups가 비어있을 경우 빈 리스트 반환
    
        possible_combinations = []  # 결과를 저장할 리스트를 초기화합니다.
        
        # itertools.product를 사용하여 모든 그룹의 가능한 조합을 생성합니다. *sort_all_groups를 사용하여 그룹들의 리스트를 언패킹하여 조합을 생성합니다.
        for combination in itertools.product(*sort_all_groups):
            timetable = [[''] * 12 for _ in range(7)]  #  7x12 크기의 2D 리스트로, 초기값은 모두 빈 문자열로 설정된 시간표를 생성합니다.
            valid = True  # 유효성 여부를 나타내는 변수를 초기화합니다
    
            # 각 그룹의 과목과 시간표 확인
            for group in combination: #각 그룹에 대한 루프를 시작합니다.
                #course와 schedule을 추출하고, schedule이 비어 있지 않은지 확인합니다.
                course = group[0]
                schedule = group[1]
                if schedule == '':
                    valid = False
                    break
    
                schedule_list = schedule.split(',')  # schedule_list: schedule을 쉼표로 분리하여 개별 시간으로 나눕니다
    
                # 이미 시간표에 해당 시간이 존재하는 경우 중복이므로 valid를 False로 설정합니다.
                for time in schedule_list:
                    day = time[0]  # 시간표의 첫 번째 문자는 요일을 나타냄
                    if len(time) < 2:
                        valid = False
                        break
                    period = int(time[1:]) - 1  # 시간표의 나머지 문자는 교시를 나타냄
                    if timetable[day_to_index(day)][period] != '':
                        valid = False
                        break
    
                    # 해당 과목 추가
                    timetable[day_to_index(day)][period] = course
                    
                #valid가 False인 경우 루프를 종료합니다.
                if not valid:
                    break
    
            # 빈 시간 확인
            if valid:
                empty_slots = [['True' for _ in range(12)] for _ in range(7)]
                for day, schedule in enumerate(timetable):
                    for period, course in enumerate(schedule):
                        if course != '':
                            empty_slots[day][period] = 'False'
    
                # 조합 정보와 함께 결과 리스트에 추가
                possible_combinations.append([empty_slots, combination])
    
        possible_combinations_result_list = []  # 프론트로 보내줄 결과
        #가능한 조합과 빈 시간 정보를 순회하면서 결과 리스트에 조합을 추가합니다.
        for empty_slots, combination in possible_combinations:
            possible_combinations_result_list.append(combination)
    
        return possible_combinations_result_list, possible_combinations #프론트로 보낼 가능한 조합의 리스트를 반환합니다.
        
end code
