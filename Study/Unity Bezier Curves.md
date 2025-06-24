### 베지어 곡선이란

컴퓨터 그래픽 밎 관련 분야에서 사용되는 매개변수 곡선이다

점과 점 사이의  선형 보간을 통하여 그리는 곡선이라 볼 수 있다

A 와 B 의 선형 보간 값 D

B 와 C 의 선형 보간 값 E

D 와 E 의 선형 보간 값 F

이 있다고 가정한다면 선형 보간의 수식인

$$
(1 - t) * p1 + t * p2t = [0,1]
$$

에 따라서 t가 0에서 1로 갈때 점의 위치가 아래의 이미지 순서대로 이동한다

1.                                                                                          2. 

![image.png](image.png)

![image.png](image%201.png)

1.                                                                                          4. 

![image.png](image%202.png)

![image.png](image%203.png)

이때 점 F의 변화를 선으로 본다면

![image.png](image%204.png)

이런 곡선이 나오게 된다 이게 바로 선형 보간을 이용한 베지어 곡선인데

베지어 곡선은 시작과 끝을 제외한 중간점을 지나치지 않는다는 특징이 있다

위의 개념을 이용하여 구현하면

```cpp
using System.Collections.Generic;
using UnityEngine;

public class BezierCurves : MonoBehaviour
{
    public Transform _target; 
    public Transform _p1, _p2, _p3;
    public float _gizmoDetail = 10; 
    public float _speed = 0.2f; 

    private List<Vector3> _gizmoPoints = new List<Vector3>();
    private float t = 0f;
    
    private void OnDrawGizmos()
    {
        _gizmoPoints.Clear();

        if (_p1 != null && _p2 != null && _p3 != null && _gizmoDetail > 0)
        {
            for (int i = 0; i <= _gizmoDetail; i++) 
            {
                float t = (float)i / _gizmoDetail;
                Vector3 p4 = Vector3.Lerp(_p1.position, _p2.position, t);
                Vector3 p5 = Vector3.Lerp(_p2.position, _p3.position, t);
                _gizmoPoints.Add(Vector3.Lerp(p4, p5, t));
            }
        }

        for (int i = 0; i < _gizmoPoints.Count - 1; i++)
        {
            Gizmos.DrawLine(_gizmoPoints[i], _gizmoPoints[i + 1]);
        }
    }

    private void Update()
    {
        if (_p1 != null && _p2 != null && _p3 != null)
        {
            t += Time.deltaTime * _speed;
            if (t > 1f) t = 0f;

            // 베지어 곡선 계산
            Vector3 p4 = Vector3.Lerp(_p1.position, _p2.position, t);
            Vector3 p5 = Vector3.Lerp(_p2.position, _p3.position, t);
            _target.position = Vector3.Lerp(p4, p5, t);
        }
    }
}

```

[화면 녹화 중 2024-12-06 232209.mp4](%25ED%2599%2594%25EB%25A9%25B4_%25EB%2585%25B9%25ED%2599%2594_%25EC%25A4%2591_2024-12-06_232209.mp4)

세개의 포인트의 포지션을 기반으로 하여 베지어 곡선을 계산하고

그 계산된 곡선의 값에 따라서 traget 즉 영상에 보이는 구가 움직이게 된다.

![스크린샷 2024-12-06 233016.png](%25EC%258A%25A4%25ED%2581%25AC%25EB%25A6%25B0%25EC%2583%25B7_2024-12-06_233016.png)

이런식으로 베지어 곡선이 계산된다