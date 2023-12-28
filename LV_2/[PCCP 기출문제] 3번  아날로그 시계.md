# [PCCP 기출문제] 3번 / 아날로그 시계
---
## 📃 문제 설명
```
시침, 분침, 초침이 있는 아날로그시계가 있습니다. 시계의 시침은 12시간마다, 분침은 60분마다, 초침은 60초마다 시계를 한 바퀴 돕니다.
따라서 시침, 분침, 초침이 움직이는 속도는 일정하며 각각 다릅니다. 이 시계에는 초침이 시침/분침과 겹칠 때마다 알람이 울리는 기능이 있습니다.
당신은 특정 시간 동안 알람이 울린 횟수를 알고 싶습니다.
알람이 울리는 횟수를 센 시간을 나타내는 정수 h1, m1, s1, h2, m2, s2가 매개변수로 주어집니다. 이때, 알람이 울리는 횟수를 return 하도록 solution 함수를 완성해주세요.
[문제 조건]
	- 0 ≤ h1, h2 ≤ 23
	- 0 ≤ m1, m2 ≤ 59
	- 0 ≤ s1, s2 ≤ 59
	- h1시 m1분 s1초부터 h2시 m2분 s2초까지 알람이 울리는 횟수를 센다는 의미입니다.
	- h1시 m1분 s1초 < h2시 m2분 s2초
	- 시간이 23시 59분 59초를 초과해서 0시 0분 0초로 돌아가는 경우는 주어지지 않습니다.
```
---
## 🔔 초기 구상
```
[초기 구상 내용]
```
---
## 📖 풀이 코드
```
class Solution {
    private static double hStand = 1.0/120;
    private static double mStand = 1.0/10;
    private static double sStand = 6;
    
    public int solution(int h1, int m1, int s1, int h2, int m2, int s2) {
        int answer = 0;
        answer = func(h2,m2,s2) - func(h1,m1,s1);      
        if((h1==0||h1==12)&&m1==0&&s1==0) answer +=1;
        return answer;
    }
    
    public static int func(int h, int m, int s){
        int cnt = 0;
        
        double startSecondAngle = (sStand * s) % 360; 
        double startMinuteAngle = (mStand * ((m * 60) + s)) % 360; 
        double startHourAngle = (hStand * ((h * 3600) + (m * 60) + s)) % 360;
        
        if(startSecondAngle>=startMinuteAngle) cnt +=1;
        if(startSecondAngle>=startHourAngle) cnt +=1;

        cnt += (h*60+m)*2;
        cnt -= h;
        if(h>=12) cnt -= 1;
        
        return cnt;
    }
}
```
---
## 💡 구현 내용
```
[구현 내용]
```
---
## ✔ 문제 회고
```
[문제 풀이 회고]
```


