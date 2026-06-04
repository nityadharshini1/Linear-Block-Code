# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
# Program
import numpy as np

pb = []          # Parity matrix
Ik = []          # Identity matrix
p = []
m = []
h = []
h_dis = []
r_code = []
err = []

col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

for i in range(row):

    p = list(map(int,
    input(f"Enter the row values : {i+1} (Separated by space) : ").split()))

    pb.append(p)

p_mat = np.array(pb, dtype=int)

Ik = np.eye(row, dtype=int)

g_mat = np.hstack((p_mat, Ik))


n, k = g_mat.T.shape


m = np.array([
    [1 if (i >> (k-j-1)) & 1 else 0 for j in range(k)]
    for i in range(2**k)
])


c = np.mod(np.dot(m, g_mat), 2)

for i, row1 in enumerate(c):

    h_dis1 = np.sum(row1)

    h_dis.append(h_dis1)

h_mat = np.array(h_dis).reshape(1, -1)

d_min = np.min(np.sum(c[1:], axis=1))


h = p_mat[:, :col]

hp = np.hstack((np.eye(n-k, dtype=int), h.T))

ht = hp.T

print("\n")
print("The Generator Matrix is : ")

for r in g_mat:
    print(" ".join(map(str, r)))


print("\n")
print("Message Bits\tCodeword\tHamming Weight")

code_word = np.hstack((m, c, h_mat.T))

for r in range(code_word.shape[0]):

    format_row = (
        " ".join(map(str, code_word[r, :k]))
        + "\t\t"
        + " ".join(map(str, code_word[r, k:n+k]))
        + "\t\t"
        + str(code_word[r, -1])
    )

    print(format_row)


print("\n")
print(f"Minimum Hamming Distance : {d_min}")

s = d_min - 1

print("\nError Detection Capability")
print(f"dmin >= s + 1")
print(f"{d_min} >= s + 1")
print(f"s <= {s}")


t = (d_min - 1) // 2

print("\nError Correction Capability")
print(f"dmin >= 2t + 1")
print(f"{d_min} >= 2({t}) + 1")
print(f"t = {t}")

print("\n")
print("Parity Check Matrix")

for r in hp:
    print(" ".join(map(str, r)))


print("\n")
print("Parity Check Matrix Transpose")

for r in ht:
    print(" ".join(map(str, r)))


rc = list(map(int,
input("\nEnter the error codeword : ").split()))

r_code.append(rc)

r_c = np.array(r_code)

e = np.mod(np.dot(r_c, ht), 2)

print("\n")
print("Syndrome of given received codeword is : "
      + " ".join(map(str, e[0])))

print("\n")
print("Syndrome Matrix")

for i in range(n):

    combined_row = np.concatenate(
        (ht[i, :], np.eye(n, dtype=int)[i, :])
    )

    formatted_row = (
        " ".join(map(str, combined_row[:col]))
        + "\t"
        + " ".join(map(str, combined_row[col:]))
    )

    print(formatted_row)

for i in range(n):

    if np.array_equal(e[0], ht[i, :]):

        err = np.eye(n, dtype=int)[i, :]

print("\nThe error position is : "
      + " ".join(map(str, err.astype(int))))

correct = np.mod(err + rc, 2)

print("\nThe correct codeword is : "
      + " ".join(map(str, correct.astype(int))))
      
      <img width="472" height="807" alt="595166832-2754d9f2-51f0-4447-a6ec-1c9027906036" src="https://github.com/user-attachments/assets/d7f37189-39c6-4676-9f52-0b1b442f69ea" />

# Output Waveform


<img width="472" height="807" alt="595166832-2754d9f2-51f0-4447-a6ec-1c9027906036" src="https://github.com/user-attachments/assets/6d797008-e78c-44ae-af0d-b1fd5191554c" />

# Results
```
Thus linear block code operation for the given input is successfully verified.
```

