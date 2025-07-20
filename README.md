using UnityEngine;

public class WaterWave : MonoBehaviour
{
    public float waveSpeed = 1f;   // سرعة الموج
    public float waveHeight = 0.2f; // ارتفاع الموج
    private Vector3 startPos;

    void Start()
    {
        startPos = transform.position;
    }

    void Update()
    {
        float newY = startPos.y + Mathf.Sin(Time.time * waveSpeed) * waveHeight;
        transform.position = new Vector3(transform.position.x, newY, transform.position.z);
    }
}# -
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float jumpForce = 5f;
    private Rigidbody rb;
    private bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        float moveX = Input.GetAxis("Horizontal") * moveSpeed;
        float moveZ = Input.GetAxis("Vertical") * moveSpeed;

        Vector3 move = transform.right * moveX + transform.forward * moveZ;
        Vector3 newPos = rb.position + move * Time.deltaTime;
        rb.MovePosition(newPos);

        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
            isGrounded = false;
        }
    }

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.tag == "Ground")
        {
            isGrounded = true;
        }
    }
}PlayerController.csusing UnityEngine;
using System.Collections;

public class AnimalController : MonoBehaviour
{
    public float moveSpeed = 3f;       // سرعة حركة الحيوان
    public float waitTime = 2f;        // وقت الانتظار بين الحركات
    public float moveRadius = 10f;     // نصف قطر الحركة العشوائية
    private Vector3 startPosition;

    private bool isMoving = false;
    private Vector3 targetPosition;

    void Start()
    {
        startPosition = transform.position;
        StartCoroutine(MoveRoutine());
    }

    IEnumerator MoveRoutine()
    {
        while (true)
        {
            // ينتظر قبل الحركة
            yield return new WaitForSeconds(waitTime);

            // يحدد نقطة هدف عشوائية ضمن الدائرة
            Vector2 randomPoint = Random.insideUnitCircle * moveRadius;
            targetPosition = startPosition + new Vector3(randomPoint.x, 0, randomPoint.y);

            isMoving = true;

            // يتحرك نحو الهدف
            while (Vector3.Distance(transform.position, targetPosition) > 0.1f)
            {
                Vector3 direction = (targetPosition - transform.position).normalized;
                transform.position += direction * moveSpeed * Time.deltaTime;

                // يلف الحيوان باتجاه الحركة
                if(direction != Vector3.zero)
                    transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(direction), 0.1f);

                yield return null;
            }

            isMoving = false;
        }
    }
}
using UnityEngine;

public class WaterWave : MonoBehaviour
{
    public float waveSpeed = 1f;   // سرعة الموج
    public float waveHeight = 0.2f; // ارتفاع الموج
    private Vector3 startPos;

    void Start()
    {
        startPos = transform.position;
    }

    void Update()
    {
        float newY = startPos.y + Mathf.Sin(Time.time * waveSpeed) * waveHeight;
        transform.position = new Vector3(transform.position.x, newY, transform.position.z);
    }
}
الطبيعية 
