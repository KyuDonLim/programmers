# [PCCP 기출문제] 2번  석유 시추
---
## 📃 문제 설명
```
 세로길이가 n 가로길이가 m인 격자 모양의 땅 속에서 석유가 발견되었습니다. 석유는 여러 덩어리로 나누어 묻혀있습니다. 
 당신이 시추관을 수직으로 단 하나만 뚫을 수 있을 때, 가장 많은 석유를 뽑을 수 있는 시추관의 위치를 찾으려고 합니다. 
 시추관은 열 하나를 관통하는 형태여야 하며, 열과 열 사이에 시추관을 뚫을 수 없습니다.
 석유가 묻힌 땅과 석유 덩어리를 나타내는 2차원 정수 배열 land가 매개변수로 주어집니다. 
 이때 시추관 하나를 설치해 뽑을 수 있는 가장 많은 석유량을 return 하도록 solution 함수를 완성해 주세요.
 [문제 조건]
  - 1 ≤ land의 길이 = 땅의 세로길이 = n ≤ 500
    = 1 ≤ land[i]의 길이 = 땅의 가로길이 = m ≤ 500
    = land[i][j]는 i+1행 j+1열 땅의 정보를 나타냅니다.
	= land[i][j]는 0 또는 1입니다.
	= land[i][j]가 0이면 빈 땅을, 1이면 석유가 있는 땅을 의미합니다.
  - 1 ≤ land의 길이 = 땅의 세로길이 = n ≤ 100
    = 1 ≤ land[i]의 길이 = 땅의 가로길이 = m ≤ 100
```
---
## 🔔 초기 구상
```
 1. 이중 for문을 통해 해당 자리에서 석유가 있는 경우 dfs를 사용하여 석유량을 배열에 저장한다.
 2. dfs를 통해 석유량을 찾을 때에 석유를 찾았다면 해당 땅에 표시해놓아 같은 지역을 조사하지 않는다.
 3. land의 모든 것을 조사한 후 제일 많은 석유를 뽑을 수 있는 값을 찾는다.
```
---
## 📖 풀이 코드
```
 import java.util.*;
 class Solution {
     private static Map<Integer,Integer> oilLand;
     private static int[] dx = {1,0,0,-1};
     private static int[] dy = {0,1,-1,0};
     public int solution(int[][] land) {
         int answer = 0;
         int oilIdx = 2;
         oilLand = new HashMap<Integer,Integer>();
         int[] result = new int[land[0].length];
         for(int i = 0; i<land[0].length;i++){
             Set<Integer> isY = new HashSet<Integer>();
             for(int j = 0; j<land.length;j++){
                 if(land[j][i] > 1 && !isY.contains(land[j][i])){
                     result[i] += oilLand.get(land[j][i]);
                     isY.add(land[j][i]);
                 }else if(land[j][i] == 1){
                     isY.add(oilIdx);
                     result[i] += countOilByBfs(land,j,i,oilIdx++);
                 }
             }
         }
         
         for(int oil : result){
             answer = Math.max(answer, oil);
         }
         
         return answer;
     }
     
     private static int countOilByBfs(int[][] land, int startX, int startY, int landNumber){
         int cnt = 0;
         Queue<Pair> queue = new ArrayDeque<>();
         queue.offer(new Pair(startX, startY));     
         land[startX][startY] = landNumber;
         
         while(!queue.isEmpty()){
             cnt++;
             Pair now = queue.poll();
             int nowX = now.x;
             int nowY = now.y;
             for(int k = 0; k< dx.length; k++){
                 int nx = nowX + dx[k];
                 int ny = nowY + dy[k];
                 if(nx < 0 ||ny <0 
                     || nx >= land.length || ny >= land[0].length 
                     || land[nx][ny] != 1) continue;
                 land[nx][ny] = landNumber;
                 queue.offer(new Pair(nx, ny));
             }
         }
         oilLand.put(landNumber, cnt);
         return cnt;
     }
     
     private static class Pair{
         int x;
         int y;
         public Pair(int x, int y){
             this.x = x;
             this.y = y;
         }
     }
 }
```
---
## ❌ 발생한 문제점
```
 1. 정확성 측면에서는 맞았지만 효율성 측면에서 메모리가 부족하여 에러가 발생하였다.
```
---
## 💡 구현 내용
```
 1. 메모리 부족을 해결하기 위하여 DFS를 BFS로 바꾸어서 구현하였다.
   - countOilByBfs()을 사용하여 해당 지역 석유량을 조사
   - Pair라는 Class를 생성하여 x, y 값을 가지는 객체를 생성
   - landNumber를 지정하여 조사한 땅의 값을 변경 -> 원본을 수정해도 괜찮으니 사용
   - 석유량을 조사한 후 oilLand라는 Map에 landNumber를 Key를 지정하여 저장
 2. 조사한 땅은 값이 1보다 크기 때문에 1보다 크고 이미 값을 더했던 내용을 저장하는 Set인 isY에 없는 경우 oilLand에서 해당 Key값을 사용하여
    값을 더한다.
 3. 값이 1인 땅은 조사가 필요한 땅이므로 countOilByBfs()를 사용하여 값을 더한다.
 4. 모든 땅을 조사한 후 어느 지점이 가장 높은 석유를 얻을 수 있는지를 확인
```
---
## ✔ 문제 회고
```
 처음 구현하였을 때 삼중 for문 생겨 시간적으로 문제가 발생할 것이라고 생각을 하였지만, 오히려 메모리적인 측면에서 문제가 발생할 줄을 생각하지 못하였다.
 비슷한 문제를 할 때에는 bfs, dfs 중 dfs가 재귀함수를 통해 구현하는 것이 쉬워 자주 사용하였고 문제가 없었지만, 효율성을 따지는 문제를 풀 때에는 
 bfs를 사용해야겠다고 느낀 문제이다. 또한, Collections을 사용하여 더욱 효율적으로 구현할 수 있게 한 것이 좋았다.
```
