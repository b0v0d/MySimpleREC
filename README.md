# MySimpleREC
It's my small recording tool with openCV

### 기능 요약
1. 코드 내 cv2.VideoCapture의 인자 따라 녹화할 영상 설정
  0: 노트북 내장 카메라 영상
  주소: 주소의 영상
2. Space Bar를 누르면 녹화 시작/종료
3. 녹화중이 아닐 때: 화면 상단에 초록색 원, FPS 표시
4. 녹화중일 때: 화면 상단에 빨간색 원만 표시
5. 'u' key: 녹화 영상의 FPS 증가
6. 'd' key: 녹화 FPS 감소
7. 'esc' key: 프로그램 종료

### 코드 분석
```python
import cv2
recording = False
cap = cv2.VideoCapture('http://210.99.70.120:1935/live/cctv001.stream/playlist.m3u8')
output_file = 'recorded_video.avi'
```
openCV를 활용/ 녹화하고자 하는 영상의 주소를 인자로 설정/ 녹화된 영상의 저장 이름을 설정

```pyhton
frame_width = 640
frame_height = 480
fps = 24.0
```
녹화할 원본 영상을 화면에 표시할 때 가로, 세로의 길이 설정/ 녹화된 영상의 FPS 설정

```python
if recording:
  cv2.circle(frame, (30, 30), 10, (0, 0, 255), -1)
else:
  cv2.circle(frame, (30, 30), 10, (0, 255, 0), -1)
  cv2.putText(frame, f"FPS:{fps}", (250, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 255), 2, cv2.LINE_AA)
```
녹화중이면 화면에 표시된 영상에 빨간 원을 표시, 녹화중이 아니면 초록 원을 표시

```python
key = cv2.waitKey(1)
if key == ord(' '):
  recording = not recording
  if recording:
    out = cv2.VideoWriter(output_file, cv2.VideoWriter_fourcc(*'XVID'), fps, (frame_width, frame_height))
  else:
    out.release()
                
if not recording:
  if key == ord('d') and fps > 1:
    fps = fps - 1
  if key == ord('u'):
    fps = fps + 1
```
space bar를 누르면 녹화 시작,종료/ 녹화중이 아닐 때 'u'키를 누르면 FPS 증가, 'd'키를 누르면 FPS 감

```python
if key == 27:
  break
```
esc를 누르면 프로그램 종료
