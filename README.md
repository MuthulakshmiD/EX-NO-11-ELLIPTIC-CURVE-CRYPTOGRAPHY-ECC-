# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)
#### Muthulakshmi D-212223040122

## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def mod_inverse(a, m):
    # Extended Euclidean Algorithm
    m0, x0, x1 = m, 0, 1
    if m == 1:
        return 0
    while a > 1:
        q = a // m
        a, m = m, a % m
        x0, x1 = x1 - q * x0, x0
    return x1 + m0 if x1 < 0 else x1

def point_addition(P, Q, a, p):
    if P.x == Q.x and P.y == Q.y:
        # Point doubling
        num = (3 * P.x**2 + a)
        den = mod_inverse(2 * P.y, p)
    else:
        # Point addition
        num = (Q.y - P.y)
        den = mod_inverse(Q.x - P.x, p)

    lam = (num * den) % p
    x_r = (lam**2 - P.x - Q.x) % p
    y_r = (lam * (P.x - x_r) - P.y) % p
    return Point(x_r, y_r)

def scalar_multiplication(P, k, a, p):
    result = P
    k -= 1
    while k > 0:
        result = point_addition(result, P, a, p)
        k -= 1
    return result

# Input elliptic curve parameters
p = int(input("Enter the prime number (p): "))
a = int(input("Enter curve parameter a: "))
b = int(input("Enter curve parameter b: "))
x = int(input("Enter base point G.x: "))
y = int(input("Enter base point G.y: "))
G = Point(x, y)

# Private keys
privateA = int(input("Enter Alice's private key: "))
privateB = int(input("Enter Bob's private key: "))

# Public keys
publicA = scalar_multiplication(G, privateA, a, p)
publicB = scalar_multiplication(G, privateB, a, p)

print(f"Alice's public key: ({publicA.x}, {publicA.y})")
print(f"Bob's public key: ({publicB.x}, {publicB.y})")

# Shared secrets
sharedA = scalar_multiplication(publicB, privateA, a, p)
sharedB = scalar_multiplication(publicA, privateB, a, p)

print(f"Shared secret by Alice: ({sharedA.x}, {sharedA.y})")
print(f"Shared secret by Bob:   ({sharedB.x}, {sharedB.y})")

if sharedA.x == sharedB.x and sharedA.y == sharedB.y:
    print("Key exchange successful. Shared secrets match!")
else:
    print("Key exchange failed. Shared secrets do not match.")
```
## Output:

![image](https://github.com/user-attachments/assets/ebe48e58-1e75-4f78-87dc-97dab9a422be)

## Result:
The program is executed successfully

