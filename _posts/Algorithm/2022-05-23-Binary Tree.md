---
title: "Binary Tree(작성중)"
toc: true
#header:
  #image: /assets/images/spark/spark_logo.svg
  #caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - Algorithm
tags:
  - Algorithm
toc: true
toc_sticky: true

date: 2022-05-23
last_modified_at: 2022-05-23
---

<br><br>

# 이진 트리의 최대 깊이

<br>

[3, 9, 20, null, null, 15, 7]가 주어졌을 때 깊이는 3이다.

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/169836068-b15ab61f-37f4-44e9-a6ee-997d51d5bf67.png'>
</div>

<br>

깊이 측정은 BFS로도 가능하다.

```python
#pseudocode
def maxDepth(self, root: TreeNode) -> int:
  ...
  queue = collections.deque([root])
  depth = 0

  while queue:
    ...

  return depth

```

<br>

큐를 선언하고 반복 구조도 구성하여, BFS 반복을 이용해 풀이할 구조를 잡았다. 파이썬에서 큐는 일반적인 리스트로도 모든 연산이 가능하지만, 데트 자료형을 사용하면 이중 연결 리스트로 구성되어 있기 때문에 큐와 스택 연산을 모두 자유롭게 활용 가능할 뿐만 아니라 양방향 모두 O(1)에 추출할 수 있어 좋은 성능을 보인다는 점을 이미 여러 번 언급한 바 있다.

실제로도 양방향 추출이 빈번할 경우, 리트코드의 실행 속도 또한 데크가 리스트에 비해 훨씬 빠르다.

```python
#pseudocode
while queue:
  depth += 1
  for _ in range(len(queue)):
    cur_root = queue.popleft()
    ...
    if cur_root.has_child():
      queue.append(cur_root.child)
```

<br>

큐 변수에는 현재 깊이 depth에 해당하는 모든 노드가 들어 있고, queue.popleft() 로 하나씩 끄집어 내면서 cur_root.has_child()로 자식노드가 있는지 여부를 판별한 후 자식 노드를 다시 큐에 삽입한다. 

동일한 큐에 삽입하다 보니 자식 노드와 부모 노드는 섞일 것이다. 그러나 이 for 반복문에서는 자식 노드가 추출되는 일은 없을 것이다. 왜냐하면 처은 for 문 진입 시 부모 노드가 모두 추출된 이후에는 for 문을 빠져 나가게 되고, 다시 한바퀴 돌아 while queue 구문에서 이번에는 다음 번 깊이의 노드 반복이 진행될 것이다.

queue.append(cur_root.child)으로 깊이 별 큐에 삽입되는 자식 노드를 도식화해보자.

<br>

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/169838597-5eec06cf-0c46-4ce0-ba6d-79922a76423a.png'>
</div>

<br>

깊이별로 노드가 추가되는 BFS 구조를 나타낼 수 있다. 예제의 입력 값이 [3, 9, 20, null, null, 15, 7]일 때, depth가 1이면 queue에는 [3], 2이면 [9, 20], 3이면 [15, 7]이 삽입되어 처리된다.

깊이 depth는 while 구문의 반복 횟수이므로 BFS에서 반복 횟수는 곧 높이가 된다. 이제 반복 횟수를 리턴하면 최종 결과를 구할 수 있다.

<br>

```python
def maxDepth(self, root : TreeNode) -> int:
  if root is None:
    return 0
  queue = collections.deque([root])
  depth = 0

  while queue:
    depth += 1
    # 큐 연산 추출 노드의 자식 노드 삽입
    for _ in range(len(queue)):
      cur_root = queue.popleft()
      if cur_root.left:
        queue.append(cur_root.left)
      if cur_root.right:
        queue.append(cur_root.right)
    # BFS 반복 횟수 == 깊이
    return depth
```

<br><br>

## 이진 트리의 직경

