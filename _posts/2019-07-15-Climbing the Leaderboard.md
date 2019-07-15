---
title: "Climbing the Leaderboard"
excerpt: "(Java) Climbing the Leaderboard 풀이"
categories:
  - Algorithm
  - HackerRank
tags:
  - java
  - algorithm
  - hackerrank
toc: true
toc_sticky: true
---

## 문제

링크: [Climbing the Leaderboard (Medium Level)](https://www.hackerrank.com/challenges/climbing-the-leaderboard/problem)

Alice의 랭킹을 구하는 문제입니다.

현재 scores와 Alice의 score를 input 값으로 받아 Alice의 Ranking을 return 하는 문제입니다.
leaderboard는 Dense Ranking 을 사용하여, 점수가 같으면 같은 랭킹으로 판단됩니다.
주의할 점은 leaderboard의 score는 내림차순이며, alice의 score는 오름차순 입니다.

    [input]
    7 						// score num
    100 100 50 40 40 20 10 	// scores array
    4 						// alice score num
    5 25 50 120 			// alice scores array


    [output]
    6 4 2 1


|  Score  | 100  | 100  |  50  |  40  |  40  |  20  |  10  |
| :-----: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| Ranking |  1   |  1   |  2   |  3   |  3   |  4   |  5   |

|  Alice Score  |                  5                   |                  25                  |                  50                  |                 120                  |
| :-----------: | :----------------------------------: | :----------------------------------: | :----------------------------------: | :----------------------------------: |
| Alice Ranking | <span style="color:red">**6**</span> | <span style="color:red">**4**</span> | <span style="color:red">**2**</span> | <span style="color:red">**1**</span> |



## 풀이

처음 문제를 보고 medium level 치고 쉽네 라고 얕봤는데 'Terminated due to timeout' Error의 늪에 오래 빠져있다 나왔습니다(...)

1. 우선 중복되는 score를 제거하기 위해(같은 점수는 랭킹이 같기 때문) HashSet 자료구조를 사용하였으며, HashSet을 다시 List로 변경 후 내림차순 정렬 하였습니다.

```java
//중복되는 score 제거. scoreList index+1 = ranking
Set<Integer> scoreSet = new HashSet<>();
for (int i = 0; i < scores.length; i++) {
    scoreSet.add(scores[i]);
}

List<Integer> scoreList = new ArrayList<>(scoreSet);
//내림차순 정렬
Collections.sort(scoreList);
Collections.reverse(scoreList);
```

2. leaderboard의 score는 내림차순이고 alice의 score는 오름차순이기에 비교하기 쉽게 alice socre array의 for문을 거꾸로 돌았습니다.

   alice의 score가 leaderboard의 score보다 크거나 같을 경우 rank 숫자를 한개씩 증가시켰습니다. 만약 leaderboard를 다 돌아도 alice의 score가 leaderboard의 score보다 크거나 같은 경우가 없으면 마지막 랭크이므로 처음에 꼴찌로 가정하고 for문을 시작하였습니다.

   여기서 point는 startIndex 입니다. 그냥 for문을 돌게 되면 몇몇 TestCase에서 <u>'Terminated due to timeout'</u> 이 발생하게 됩니다.

   **alice의 score가 점점 낮아질수록 leaderboard 앞에 순위는 굳이 판단할 필요가 없어집니다.**
   **따라서 alice의 점수가 leaderboard보다 작을 경우 startIndex를 한칸씩 옮겨서 for문을 수행하였습니다.** 이렇게 되면 수행시간이 짧아집니다.


```java
int startIndex = 0;
for (int i = alice.length - 1; i >= 0; i--) {
    int aliceScore = alice[i];
    //꼴찌로 가정
    result[i] = scoreList.size() + 1;
    for (int j = startIndex; j < scoreList.size(); j++) {
        if (scoreList.get(j) <= aliceScore) {
            result[i] = j + 1;
            break;
        } else {
            startIndex++;
        }
    }
}
```

***
위 풀이로 모든 테스트케이스를 패스하였습니다!

전체코드는 [Github](https://github.com/ejiaah/Algorithm-Practice/blob/master/HackerRank/(Java)%20Climbing%20the%20Leaderboard_190714.java)에서 확인할 수 있습니다.

(개인적인 풀이방식이며 더 좋은 방법이나 아이디어는 언제든 댓글 달아주세요~)

