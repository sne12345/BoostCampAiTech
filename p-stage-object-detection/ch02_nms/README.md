nms : 예측한 박스들 중 IOU가 일정이상인 것들에 대해서 중복 데이터를 제거하는 작업 

1. 동일한 클래스에 대해 높은-낮은 confidence 순서로 정렬한다. (line 13)
2. 가장 confidence가 높은 boundingbox와 IOU가 일정 이상인 boundingbox는 동일한 물체를 detect했다고 판단하여 지운다.(16~20) 보통 50%(0.5)이상인 경우 지우는 경우를 종종 보았다.


출처: https://dyndy.tistory.com/275 [DY N DY]
