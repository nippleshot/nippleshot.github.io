

### 완전탐색 맛보기

#### 1. 모의고사

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.



- 각 수포자의 정답을 count하는 방식 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] answers) {
        ArrayList<Integer> answerList = new ArrayList<>();
      	// 각 수포자들의 찍는 패턴
        int[] student1 = {1,2,3,4,5};
        int[] student2 = {2,1,2,3,2,4,2,5};
        int[] student3 = {3,3,1,1,2,2,4,4,5,5};
        // 각 수포자들의 정답 count
        int[] studentCount = {0,0,0};
        for(int i=0; i<answers.length; i++){
            if(answers[i]==student1[i%student1.length]){
                studentCount[0] = studentCount[0] + 1;
            }
            if(answers[i]==student2[i%student2.length]){
                studentCount[1] = studentCount[1] + 1;
            }
            if(answers[i]==student3[i%student3.length]){
                studentCount[2] = studentCount[2] + 1;
            }
        }
       	// 수포자들의 전체 정답 count에서 최대값 구하기
        int highest_score = Arrays.stream(studentCount).max().getAsInt();
      	// 수포자들의 정답 count 배열에서 최대값 나온 index를 최종 결과 배열에 담기
      	// (높은 점수가 여럿일 경우 오름차순 정렬까지 가능)
        for(int i=0; i<studentCount.length; i++){
            if(studentCount[i] == highest_score){
                answerList.add(i+1);
            }
        }

        return answerList.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

 <img src="/Users/song-giljae/nippleshot.github.io/assets/images/codeTest/exhaustiveSearch/Screen Shot 2021-09-10 at 6.19.24 PM.png" alt="Screen Shot 2021-09-10 at 6.19.24 PM" style="zoom:33%;" />



#### 2. 소수 찾기

