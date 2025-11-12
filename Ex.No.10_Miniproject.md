# Ex.No: 10  Implementation of 2D/3D Bird game
### DATE:22/10/2025                                                                         
### REGISTER NUMBER : 212223240155
### AIM: 
To develop a Slingshot Bird  game in Unity 
### Algorithm:
```
1.Initialize slingshot components, line renderers, and bird variables.

2.Create and position a new bird at the slingshot’s idle position.

3.Detect mouse click and hold to start aiming.

4.Track mouse position in world space while dragging.

5.Clamp the drag distance within maximum range and boundaries.

6.Update slingshot bands and bird position visually based on drag.

7.On mouse release, calculate launch direction and force.

8.Apply the launch force to the bird’s Rigidbody to shoot it.

9.Call the bird’s Release() method to trigger post-launch behavior.

10.Reset slingshot visuals and spawn a new bird after a short delay.

11.Continuously update aiming, launching, and bird creation each frame.
```
### Program:

### slingshot.cs
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Slingshot : MonoBehaviour
{
    public LineRenderer[] lineRenderers;
    public Transform[] stripPositions;
    public Transform center;
    public Transform idlePosition;

    public Vector3 currentPosition;
    public float maxLength;
    public float bottomBoundary;
    bool isMouseDown;
    public GameObject birdPrefab;
    public float birdPositionOffset;
    Rigidbody2D bird;
    Collider2D birdCollider;
    public float force;

    void Start()
    {
        lineRenderers[0].positionCount = 2;
        lineRenderers[1].positionCount = 2;
        lineRenderers[0].SetPosition(0, stripPositions[0].position);
        lineRenderers[1].SetPosition(0, stripPositions[1].position);
        CreateBird();
    }

    void CreateBird()
    {
        bird = Instantiate(birdPrefab).GetComponent<Rigidbody2D>();
        birdCollider = bird.GetComponent<Collider2D>();
        birdCollider.enabled = false;
        bird.bodyType = RigidbodyType2D.Kinematic;
        ResetStrips();
    }

    void Update()
    {
        if (isMouseDown)
        {
            Vector3 mousePosition = Input.mousePosition;
            mousePosition.z = 10;
            currentPosition = Camera.main.ScreenToWorldPoint(mousePosition);
            currentPosition = center.position + Vector3.ClampMagnitude(currentPosition - center.position, maxLength);
            currentPosition = ClampBoundary(currentPosition);
            SetStrips(currentPosition);

            if (birdCollider)
                birdCollider.enabled = true;
        }
        else
        {
            ResetStrips();
        }
    }

    private void OnMouseDown() => isMouseDown = true;

    private void OnMouseUp()
    {
        isMouseDown = false;
        Shoot();
        currentPosition = idlePosition.position;
    }

    void Shoot()
    {
        bird.bodyType = RigidbodyType2D.Dynamic;
        Vector3 birdForce = (currentPosition - center.position) * force * -1;
        bird.linearVelocity = birdForce;

        bird.GetComponent<Bird>().Release();

        bird = null;
        birdCollider = null;
        Invoke("CreateBird", 2);
    }

    void ResetStrips()
    {
        currentPosition = idlePosition.position;
        SetStrips(currentPosition);
    }

    void SetStrips(Vector3 position)
    {
        lineRenderers[0].SetPosition(1, position);
        lineRenderers[1].SetPosition(1, position);

        if (bird)
        {
            Vector3 dir = position - center.position;
            bird.transform.position = position + dir.normalized * birdPositionOffset;
            bird.transform.right = -dir.normalized;
        }
    }

    Vector3 ClampBoundary(Vector3 vector)
    {
        vector.y = Mathf.Clamp(vector.y, bottomBoundary, 1000);
        return vector;
    }
}

```

### bird.cs
```
using UnityEngine;

public class Bird : MonoBehaviour
{
    private Rigidbody2D rb;

    void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    public void Release()
    {
        // This will be called from the Slingshot when you release the bird
        // You can add extra behavior here like enabling a trail, sounds, etc.
        Debug.Log("Bird released!");
    }
}
```
### Output:
![ai for games op1](https://github.com/user-attachments/assets/3f784a12-235c-425f-8ffb-b129f4eb72bb)


![ai for games op2](https://github.com/user-attachments/assets/2e128312-0932-4adf-993e-737a1316f996)


### Result:
Thus the game was developed using Unity and adopted AI technology.
