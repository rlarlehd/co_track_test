# 04. YOLOv8 + CoTracker

추적할 점을 사람이 직접 찍지 않고 **YOLO로 자동 생성**하는 실험입니다.

```
첫 프레임 → 640×640 리사이즈 → YOLOv8n 사람 탐지(conf ≥ 0.8)
        → bbox + padding crop → YOLOv8n-seg 마스크
        → 마스크 안쪽에 10px 간격 점 → 원본 해상도로 좌표 환산
        → 사람별 쿼리 그룹 → CoTracker 추적 → 그룹 중심에 ID 표시
```

| 파일 | 설명 |
|---|---|
| `yolo_test.ipynb` | 정지 이미지(`data/test.png`)에서 위 점 생성 단계만 시각화 |
| `yolo2cotracker.ipynb` | `data/sample22.avi`의 첫 프레임에서 점 그룹을 만들고 영상 전체를 추적 → `result/rec_whole.mp4`, 그룹 중심 라벨 영상 → `result/groupped.mp4` |

## 참고
- `yolo2cotracker.ipynb`의 라벨 단계는 `cv2.putText(frame, label, ...)`에 정수 `group_id`를 넘겨 OpenCV 오류가 납니다. `str(group_id)`로 넘겨야 합니다.
- 점 좌표 확인 그래프의 축 범위가 1920×1080으로 고정되어 있습니다.
