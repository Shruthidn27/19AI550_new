# Ex.No: 10  Implementation of 2D/3D game coin collector 
### DATE:22/10/2025                                                                         
### REGISTER NUMBER : 212223240155
### AIM: 
To develop a game a coin collector in Unity 
### Algorithm:
```
1.Initialize player components and variables.

2.Detect horizontal movement input from the player.

3.Move the player left or right based on input.

4.Detect jump input and apply upward force if grounded.

5.Check collisions with the ground to update grounded state.

6.Detect trigger collisions with coins and collect them.

7.Continuously update movement, jumping, and collisions each frame.
```
### Program:
```
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float jumpForce = 10f;
    private Rigidbody2D rb;
    private bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        // Move left/right
        float move = Input.GetAxis("Horizontal");
        rb.velocity = new Vector2(move * moveSpeed, rb.velocity.y);

        // Flip direction
        if (move > 0)
            transform.localScale = new Vector3(1, 1, 1);
        else if (move < 0)
            transform.localScale = new Vector3(-1, 1, 1);

        // Jump
        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }

    private void OnCollisionExit2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = false;
        }
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Coin"))
        {
            Destroy(other.gameObject);
            Debug.Log("Coin collected!");
        }
    }
}

```
### Output:
![ai for games op1](https://github.com/user-attachments/assets/3f784a12-235c-425f-8ffb-b129f4eb72bb)


![ai for games op2](https://github.com/user-attachments/assets/2e128312-0932-4adf-993e-737a1316f996)


### Result:
Thus the game was developed using Unity and adopted AI technology.
