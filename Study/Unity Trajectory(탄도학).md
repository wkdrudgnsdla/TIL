위치를 시간에 대해 미분하면 속도

속도를 시간에 대해 미분하면 가속도

가속도를 시간에 대해 미분하면 속도

속도를 시간에 대해 미분하면 위치가 된다

```cpp
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Trajectory : MonoBehaviour
{
    public GameObject bullet;
    public Transform target;
    public Transform shootPoint;
    private bool isDrawingPath = false;
    private Vector3 velocity;

    private void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Debug.Log("LauncherProjectile");
            LauncherProjectile();
        }

        if (isDrawingPath)
        {
            DrawPath(velocity);
        }
    }

    void LauncherProjectile()
    {
        Vector3 distance = target.position - shootPoint.position;
        float distance_x = distance.x;
        float distance_y = distance.y;

        float velocityValue = 30f;  // 예: 10 m/s

        // 발사 각도 계산
        float launchAngle = CalculateLaunchAngle(distance_x, distance_y, velocityValue);
        Debug.Log($"Calculated Launch Angle: {launchAngle} degrees");

        // 발사체 발사
        velocity = GetVelocityVector(launchAngle, velocityValue);

        if (float.IsNaN(velocity.x) || float.IsNaN(velocity.y))
        {
            Debug.LogError("Calculated velocity vector is invalid!");
            return;
        }

        Rigidbody obj = Instantiate(bullet, shootPoint.position, Quaternion.identity).GetComponent<Rigidbody>();
        obj.linearVelocity = velocity;

        isDrawingPath = true;  // 경로 그리기 시작
    }

    float CalculateLaunchAngle(float distance_x, float distance_y, float velocity)
    {
        float gravity = Mathf.Abs(Physics.gravity.y);

        float part1 = Mathf.Pow(velocity, 2);
        float part2 = Mathf.Pow(velocity, 4) - gravity * (gravity * Mathf.Pow(distance_x, 2) + 2 * distance_y * Mathf.Pow(velocity, 2));

        if (part2 < 0)
        {
            Debug.LogError("No valid solution for the launch angle (negative discriminant).");
            return 45f;  // 기본 각도나 예외 처리
        }

        float sqrtPart2 = Mathf.Sqrt(part2);
        float angle1 = Mathf.Atan2(part1 - sqrtPart2, gravity * distance_x);
        float angle2 = Mathf.Atan2(part1 + sqrtPart2, gravity * distance_x);

        float angleInDegrees1 = angle1 * Mathf.Rad2Deg;
        float angleInDegrees2 = angle2 * Mathf.Rad2Deg;

        Debug.Log($"Calculated angles: {angleInDegrees1}°, {angleInDegrees2}°");

        return Mathf.Min(angleInDegrees1, angleInDegrees2);  // 적절한 각도를 선택
    }

    Vector3 GetVelocityVector(float angle, float velocity)
    {
        float angleRad = angle * Mathf.Deg2Rad;
        float velocityX = velocity * Mathf.Cos(angleRad);
        float velocityY = velocity * Mathf.Sin(angleRad);

        Debug.Log($"Calculated velocity vector: ({velocityX}, {velocityY})");

        return new Vector3(velocityX, velocityY, 0);
    }

    void DrawPath(Vector3 velocity)
    {
        Vector3 previousDrawPoint = transform.position;
        int resolution = 30;
        for (int i = 1; i <= resolution; i++)
        {
            float simulationTime = i / (float)resolution * 1f;

            Vector3 displacement = velocity * simulationTime + Vector3.up * Physics.gravity.y * simulationTime * simulationTime / 2f;
            Vector3 drawPoint = transform.position + displacement;
            Debug.DrawLine(previousDrawPoint, drawPoint, Color.green);
            previousDrawPoint = drawPoint;
        }
    }
}
```

위의 코드는 물체의 중력 영향을 바탕으로 탄도학을 시뮬레이션 하는 코드이다.

1.목표 지점과 발사 지점간의 거리를 구한 이후 그 거리를 x축과 y축으로 분리한다

탄도학 공식에 따라서x와 y축으로 나눈 것에 velocity를 이용하여 발사 각도를 구한다.

발사 각도는 두가지 값을 계산 후 작은 값을 선택해 반환한다.

각도는 라디안을 도 단위로 변환하여 반환한다.

- 탄도학 공식
    
    수평운동)
    
    $$
    x(t)=v0​cos(θ)⋅t
    $$
    
    수평위치를 구하는 공식, 이때 v0cos(θ)는 수평속도를 뜻한다.
    
    수직운동)
    
    $$
    y(t)=v0​sin(θ)⋅t−21​gt2
    $$
    
    수직위치를 구하는 공식. 여기서 v0sin(θ)는 초기 수직속도를 뜻한다
    
    21​gt2는 중력의 영향을 받는 속도 변화입니다.
    
    탄도학에선 주로 발사체가 목표에 도달하는 시간을 계산하고 이를 바탕으로 경로를 예측한다.
    
    수직운동 방정식에서 $y(t) = 0$일때 발사체가 땅에 떨어지는 시간  $tf$를 구할 수 있다
    
    이 방정식을 풀면 두가지의 해가 나오지만 의미있는 해는 
    
    $$
    tf​=g2v0​sin(θ)​
    $$
    
    이다.
    
    발사각도)
    
    발사 각도를 계산하려면 목표까지의 거리, 목표의 높이, 발사 속도가 필요하다.
    
    $$
    θ=21​arcsin(v02​g⋅d​)
    $$
    
    여기서 $d$는 목표 지점까지의 거리, $v0$은 초기 속도를 뜻한다 
    
    위의 내용은 중력만을 포함한 탄도학이지만 공기저항과 질량등이 들어간다면 훨씬복잡해진다.
    

2.발사 각도와 속도를 바탕으로 속도 벡터를 구한다.

속도벡터는 x축과 y축으로 구분되는데 각도에 따라 수평속도와 수직속도를 계산, 반환한다.

최종적으로 (velocityX, velocityY, 0)의 형태로 속도 벡터를 반환하고

총알의 RigidBody의 linearVelocity에 위에서 구한 속도벡터를 할당시킨다.