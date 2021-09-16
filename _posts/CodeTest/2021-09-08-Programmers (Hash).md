---
layout: article
title: Programmers (Hash) 
author: J_宋
tags: Programmers DataStructure 한글
mathjax: true
key: code01
---



### Hash 맛보기

#### 1. 완주하지 못한 선수

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다. 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요. (참가한 선수중에 동명인이 있을 수 있음)

- Hash Table로 해결하는 방법

```java
import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        // Key : 선수 이름, Value : 동명인 수 
      	HashMap<String, Integer> hashMap = new HashMap<>();
      	// 참여한 선수 array를 가지고 hashMap의 Value를 채우기
        for(String startPlayer : participant){
            hashMap.put(startPlayer, setPlusOne(hashMap, startPlayer));
        }
      	// 완주한 선수 array를 가지고 채워졌던 hashMap의 Value를 지우기
        for(String finishPlayer : completion){
            hashMap.put(finishPlayer, setMinusOne(hashMap, finishPlayer));
        }
        // 최종적으로 hashMap의 Value가 0이 아닌 Key를 찾기
        for(String player : hashMap.keySet()){
            if(hashMap.get(player) != 0){
                return player;
            }
        }
        
        return null;
    }
    
    public int setPlusOne(HashMap<String, Integer> hashMap, String player){
        if(hashMap.get(player) == null){
            return 1;
        }else{
            return hashMap.get(player)+1;
        }
    }
    
    public int setMinusOne(HashMap<String, Integer> hashMap, String player){
        if(hashMap.get(player) == 0){
            return -1;
        }else{
            return hashMap.get(player)-1;
        }
    }
}
```

 <img src="/assets/images/codeTest/hash/Screen Shot 2021-09-08 at 4.31.26 PM.png" alt="Screen Shot 2021-09-08 at 4.31.26 PM" style="zoom: 33%;" />

- 배열 정렬로 해결하는 방법

```java
import java.util.Arrays;

class Solution {
    public String solution(String[] participant, String[] completion) {
        Arrays.sort(participant);
        Arrays.sort(completion);

        int i;
        for(i=0; i<completion.length; i++){
            if(!participant[i].equals(completion[i])){
                // participant = [A, Ba, Ba, C, ...]
      					// completion  = [A, Ba, C, ...]
              	// 즉, Ba는 완주하지 못한 선수임 
                return participant[i];
            }
        }
      	// participant = [A, Ba, C, D]
      	// completion  = [A, Ba, C]
        // 즉, D는 완주하지 못한 선수임  
        return participant[i];
    }
}
```

 <img src="/assets/images/codeTest/hash/Screen Shot 2021-09-08 at 4.32.13 PM.png" alt="Screen Shot 2021-09-08 at 4.32.13 PM" style="zoom: 33%;" />



#### 2. 전화번호 목록

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다. 전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요. (각 전화번호의 길이는 1 이상 20 이하입니다, 같은 전화번호가 중복해서 들어있지 않습니다)

- 배열 정렬로 해결하는 방법

```java
import java.util.*;
 
class Solution {
    public boolean solution(String[] phoneBook) {
      // 주의1 : String 형 배열을 정렬하는 것과 int형 배열을 정령하는 것이 다르다
      Arrays.sort(phoneBook);
        for (int i = 0; i < phoneBook.length - 1; i++){   
          	// 주의2 : 요 문제에서는 String.contains(...)를 사용해선 안됨
          	// 왜냐하면 contains 하면 접두사 말고 뒷부분도 체크하지 않기때문에.
            if (phoneBook[i + 1].startsWith(phoneBook[i])){
                return false;
            }
        }
        return true;
    }
}
```

 

:warning: 주의 :

```java
    public static void main(String[] args){
        String[] testSet1 = {"119", "97674223", "1195524421"};
        int[] testSet2 = {119, 97674223, 1195524421};
        Arrays.sort(testSet1);
        System.out.println(Arrays.toString(testSet1)); // [119, 1195524421, 97674223]
        Arrays.sort(testSet2);
        System.out.println(Arrays.toString(testSet2)); // [119, 97674223, 1195524421]
    }
```

- `Collections.sort(...)`는 Collection의 List를 정렬할 때 사용한다.

- `Array.sort(...)` 는 argument의 type에 따라 사용되는 정렬이 다르다

  - **Object Type인 경우에는 `TimSort.sort() `** 

  - **Primitive Type인 경우에는 `DualPivotQuickSort.sort()`** 

    - `DualPivotQuickSort.sort()` 는 또 배열의 크기에 따라 정렬 방법을 다르게 한다

      :small_red_triangle: 배열의 크기가 286 이상일 경우 *Merge Sort*를 수행

      :small_red_triangle: 배열의 크기가 286 보다 작은 경우 *Quick Sort*를 Merge Sort보다 우선 수행한다

      :small_red_triangle: 배열의 크기가 47보다 작은 경우 *Insertion Sort*를 Quick Sort보다 우선 수행한다

      :small_red_triangle: Byte 배열의 크기가 29보다 큰 경우 *Counting Sort*를 Insertion Sort보다 우선 수행한다

      :small_red_triangle: Short, Char 배열의 크기가 3200보다 큰 경우 *Counting Sort*를 Quick Sort보다 우선 수행한다

    - `DualPivotQuickSort.sort()` 는 하나의 Pivot으로 두개의 part로 분할하는 Quick sort와 달리 Pivot이 하나 더 존재한다. 이로써 더 많은 상황에 대해서 O(nlogn)을 보장한다

    ❓ *Counting Sort* 가 위에 나와있는 정렬 중 가장 빠른데 Byte, Short, Char 배열에서만 사용되는 이유 :

    | Primitive Type | Byte/bit | 나타낼 수 있는 숫자 범위                                |
    | -------------- | -------- | ------------------------------------------------------- |
    | Byte           | 1/8      | -128 to 127                                             |
    | Short          | 2/16     | -32,768 to 32,767                                       |
    | Char           | 2/16     | 0 to 65,535                                             |
    | Int            | 4/32     | -2,147,483,648 to 2,147,483,647                         |
    | Long           | 16/64    | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |

    💁🏻 Byte, Short, Char 의 경우 나타낼 수 있는 숫자 범위가 작기 때문에 그만큼 배열의 값으로 들어갈 범위도 작다. 따라서 Counting Sort를 하기 위해 추가되는 되는 Count array의 크기가 그만큼 작아져서 효율이 올라감  

    ❓*Insertion Sort*의 평균 복잡도는 `O(n^2)`으로 `O(nlogn)`의 다른 정렬 알고리즘보다 성능이 떨어지는데 크기가 작은 배열에 우선 사용되는 이유 :

    💁🏻  실제로 적당히 작은 n에 대해서 Insertion Sort이 Quick Sort보다 빠른 속도를 보여줍니다.

    

    

#### 3. 위장

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다. 스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요. (clothes의 각 행은 [의상의 Brand , 의상의 종류]로 이루어져 있다. 같은 이름을 가진 의상은 존재하지 않으며 스파이는 하루에 최소 한 개의 의상을 입는다)

- Hash Table로 해결하는 방법

```java
import java.util.HashMap;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        HashMap<String, Integer> hashMap = new HashMap<>();
        // HashMap을 활용하여 Clothes Type별 Brand개수만 파악
        for(int i=0; i<clothes.length; i++){
            hashMap.put(clothes[i][1],hashMap.getOrDefault(clothes[i][1], 0)+1);
        }
      	// 각 Clothes Type별 Brand개수에 +1해줘서 다 곱해준다
        for(String key : hashMap.keySet()){
            answer = answer * (hashMap.get(key) + 1);
        }
      	// 마지막으로 -1을 해준다
        return answer-1;
    }
}
```

:small_red_triangle: `hashMap.getOrDefault(key, default)` : 

찾는 `key` 가 `hashMap` 에 존재하면 해당 `key` 에 매핑되어 있는 값을 반환하고, 그렇지 않으면 `default` 가 return됨.



:small_red_triangle: 풀이 : 같은 이름을 가진 의상은 존재하지 않고 하루에 최소 한 개의 의상은 입는다고 할 때

***Example 1.*** 

| Clothes Type | Brand                      |
| ------------ | -------------------------- |
| Eyewear      | Oakley, RayBan ... (A가지) |
| Footwear     | Nike, Adidas ... (B가지)   |

서로 다른 옷의 조합의 수 : 

= Eyewear만 착용할 경우 조합 + Footwear만 착용할 경우 조합 + Eyewear와 Footwear를 착용할 경우 조합

= A + B + (A*B)

= (A+1)(B+1) -1

***Example 2.*** 

| Clothes Type  | Brand                      |
| ------------- | -------------------------- |
| Eyewear       | Oakley, RayBan ... (A가지) |
| Footwear      | Nike, Adidas ... (B가지)   |
| Ear Protector | 3M, Mack's ... (C가지)     |

서로 다른 옷의 조합의 수 : 

= Eyewear만 착용할 경우 조합 + Footwear만 착용할 경우 조합 + Ear Protector만 착용할 경우 조합 + Eyewear와 Footwear를 착용할 경우 조합 + Eyewear와 Ear Protector를 착용할 경우 조합 + Footwear와 Ear Protector를 착용할 경우 조합 + Eyewear, Footwear 그리고 Ear Protector를 착용할 경우 조합

= A + B + C + AB+ AC+ BC + (A\*B\*C)

= A(1+B+C+BC) + B + C + (B\*C)

= A(B+1)(C+1) + (B+1)(C+1) -1 

= (A+1)(B+1)(C+1) -1

💁🏻 ***Example 1.*** 과 ***Example 2.*** 를 통해 결국 우리는 Clothes Type별 Brand 개수만 파악한 다음 +1해줘서 다 곱하고 -1하면 되는 것이었다



#### 4. 베스트 앨범

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

```java
import java.util.*;

class Solution {
    class Music{
        private int musicID;
        private int playedNum;
        
        public Music(int musicID, int playedNum){
            this.musicID = musicID;
            this.playedNum = playedNum;
        }
        
        public int getMusicID(){
            return musicID;
        }
        
        public int getPlayedNum(){
            return playedNum;
        }
        public boolean isPlayedNumEqual(Music playedNum2){
            if(playedNum == playedNum2.playedNum){
                return true;
            }else{
                return false;
            }
        }
        public boolean isEqual(Music music2){
            if(musicID == music2.getMusicID() && playedNum == music2.getPlayedNum()){
                return true;
            }else{
                return false;
            }
        }
        public boolean isMorePlayedThen(Music playedNum2){
            if(playedNum2 == null){
                return true;
            }
            if(playedNum>playedNum2.getPlayedNum()){
                return true;
            }else{
                return false;
            }
        }
    }
    
    public int[] solution(String[] genres, int[] plays) {
        HashMap<String, Integer> counterMap = new HashMap<>();
        HashMap<String, Music[]> rankingMap = new HashMap<String, Music[]>();
        
        for(int i=0; i<genres.length; i++){
            // counterMap 데이터 구축 부분 
            counterMap.put(genres[i], counterMap.getOrDefault(genres[i], 0)+plays[i]); 
            
            // rankingMap 데이터 구축 부분
            Music[] currentRankArr = rankingMap.get(genres[i]);
            Music[] newRankArr = null;
            Music newMusic = new Music(i, plays[i]);
            
            // rankingMap의 Value값 수정 
            if(currentRankArr == null){
                newRankArr = new Music[2];
                newRankArr[0] = newMusic;
            }else{
                newRankArr = fixArray(currentRankArr, newMusic);
            }
            rankingMap.put(genres[i], newRankArr);
        }
        
        // counterMap의 value값(총 재생횟수)에 근거하여 정렬하기
        List<Map.Entry<String, Integer>> list = new ArrayList<>(counterMap.entrySet());
        list.sort(Map.Entry.comparingByValue());
        Collections.reverse(list);
        // 정렬된 counterMap의 key를 사용하여 순서대로 rankingMap의 Value 불러들이기
        ArrayList<Integer> answerList = new ArrayList<>();
        for(int i=0; i<list.size(); i++){
            Music[] finalTop2 = rankingMap.get(list.get(i).getKey());
            for(int j=0; j<finalTop2.length; j++){
                if(finalTop2[j] == null){
                    continue;
                }else{
                    answerList.add(finalTop2[j].getMusicID());
                }
            }
        }
        
        return answerList.stream().mapToInt(Integer::intValue).toArray();
    }
    
   // rankingMap의 Value값 수정할 때 사용된는 함수
    public Music[] fixArray(Music[] oldRankArr, Music newMusic){
        oldRankArr[1] = compare(oldRankArr[1], newMusic);
        Music firstPlace = compare(oldRankArr[0], oldRankArr[1]);
        if(!firstPlace.isEqual(oldRankArr[0])){
            oldRankArr[1] = oldRankArr[0];
            oldRankArr[0] = firstPlace;
        }
        return oldRankArr;
    }
    // fixArray()이 진행되기 위해 사용되는 함수
    public Music compare(Music victor, Music challenger){
        if(challenger.isMorePlayedThen(victor)){
            return challenger;
        }else{
            if(challenger.isPlayedNumEqual(victor)){
                if(challenger.getMusicID() < victor.getMusicID()){
                    return challenger;
                }
            }
            return victor;
        }
    }
}
```

 <img src="/assets/images/codeTest/hash/Screen Shot 2021-09-10 at 11.46.40 AM.png" alt="Screen Shot 2021-09-10 at 11.46.40 AM" style="zoom: 33%;" />

- `Music` class의 구성

  - attribute : `int musicID`, `int playedNum`

  - method :

    | Method                      | Function                                                     |
    | --------------------------- | ------------------------------------------------------------ |
    | `isPlayedNumEqual(Music m)` | 두 `Music` 객체의 `playedNum` 이 동일할 경우 true, 아님 false |
    | `isEqual(Music m)`          | 두 `Music` 객체가 동일할 경우 true, 아님 false               |
    | `isMorePlayedThen(Music m)` | 객체 m이 null이거나 해당 `Music` 객체가 객체 m 보다 `playedNum` 이 더 클 경우 true, 아님 false |

- `solution`함수의 과정

  1. Input 데이터를 순서대로 읽으면서 `counterMap`과 `rankingMap`을 업데이트 한다

  - `rankingMap` 업데이트때 고려해야되는 부분 :
    - 장르 내에서 많이 재생된 노래를 먼저 수록합니다
    - 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.
  - `counterMap`과 `rankingMap`의 업데이트 과정 :

  ```java
  Input : 
  	String[] genres = {"classic", "pop", "classic", "classic", "pop"};
  	int[] plays = {500, 600, 150, 800, 2500};
  
  >>>>>>>>>>>>>>>>>>>>>>>> 새로운 Data 추가 : [genres = classic; musicID = 0; playedNum = 500]
  (counterMap 현황)
               장르         총 진행시간
         ------------------------------------
          classic            500
  (rankingMap 현황)
            장르                      1순위                           2순위
         ---------------------------------------------------------------------------------
          classic    [musicID = 0; playedNum = 500]                                    
  
  >>>>>>>>>>>>>>>>>>>>>>>> 새로운 Data 추가 : [genres = pop; musicID = 1; playedNum = 600]
  (counterMap 현황)
               장르         총 진행시간
         ------------------------------------
              pop            600
          classic            500
  (rankingMap 현황)
            장르                      1순위                           2순위
         ---------------------------------------------------------------------------------
              pop    [musicID = 1; playedNum = 600]                                    
          classic    [musicID = 0; playedNum = 500]                                    
  
  >>>>>>>>>>>>>>>>>>>>>>>> 새로운 Data 추가 : [genres = classic; musicID = 2; playedNum = 150]
  (counterMap 현황)
               장르         총 진행시간
         ------------------------------------
              pop            600
          classic            650
  (rankingMap 현황)
            장르                      1순위                           2순위
         ---------------------------------------------------------------------------------
              pop    [musicID = 1; playedNum = 600]                                    
          classic    [musicID = 0; playedNum = 500]     [musicID = 2; playedNum = 150] 
  
  >>>>>>>>>>>>>>>>>>>>>>>> 새로운 Data 추가 : [genres = classic; musicID = 3; playedNum = 800]
  (counterMap 현황)
               장르         총 진행시간
         ------------------------------------
              pop            600
          classic           1450
  (rankingMap 현황)
            장르                      1순위                           2순위
         ---------------------------------------------------------------------------------
              pop    [musicID = 1; playedNum = 600]                                    
          classic    [musicID = 3; playedNum = 800]     [musicID = 0; playedNum = 500] 
  
  >>>>>>>>>>>>>>>>>>>>>>>> 새로운 Data 추가 : [genres = pop; musicID = 4; playedNum = 2500]
  (counterMap 현황)
               장르         총 진행시간
         ------------------------------------
              pop           3100
          classic           1450
  (rankingMap 현황)
            장르                      1순위                           2순위
         ---------------------------------------------------------------------------------
              pop   [musicID = 4; playedNum = 2500]     [musicID = 1; playedNum = 600] 
          classic    [musicID = 3; playedNum = 800]     [musicID = 0; playedNum = 500] 
  ```

  

  2. `counterMap`의 value값(총 재생횟수)에 근거하여 `counterMap` 정렬하기 (많이 재생된 장르를 먼저 수록하기 위해서)
  3. 정렬된 `counterMap`의 key를 순서대로 사용하여 `rankingMap`의 Value 를 1순위, 2순위 순서로 결과 배열에 append하기

- `fixArray`와 `compare` 함수의 과정

  - `fixArray` 의 과정 :

     <img src="/assets/images/codeTest/hash/Screen Shot 2021-09-10 at 1.01.53 PM.png" alt="Screen Shot 2021-09-10 at 1.01.53 PM" style="zoom:43%;" />

  - `compare`  의 알고리즘 :

     <img src="/assets/images/codeTest/hash/Diagram.png" alt="Diagram" style="zoom:80%;" />

  

