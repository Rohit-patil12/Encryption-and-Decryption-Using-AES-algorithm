AES (Advanced Encryption Standard) is a symmetric key encryption algorithm used widely across the globe to secure data. It operates on fixed-size blocks (128 bits) and supports key sizes of 128, 192, or 256 bits. AES performs several rounds of processing depending on the key length. For example:

10 rounds for a 128-bit key
12 rounds for a 192-bit key
14 rounds for a 256-bit key
The main steps in AES encryption and decryption are as follows:

AES Encryption Process:
Key Expansion: The AES key is expanded into multiple round keys. The number of round keys depends on the length of the key (128, 192, or 256 bits).

Initial Round:

AddRoundKey: The first round key is XORed with the input data.
Main Rounds (Repeated until the last round):

SubBytes: Each byte of the block is replaced with a byte from the S-box (a non-linear substitution table).
ShiftRows: The rows of the block are shifted by different offsets (row 1 shifts by 1, row 2 by 2, and so on).
MixColumns: This step mixes the data within each column (only in the rounds before the last).
AddRoundKey: A round key is XORed again.
Final Round (without MixColumns):

SubBytes
ShiftRows
AddRoundKey
Output: The output after the final round is the encrypted cipher text.

AES Decryption Process:
Decryption is the reverse of encryption. The same key is used for decryption because AES is symmetric encryption. The operations are performed in reverse order:

Initial Round:

AddRoundKey: XOR the cipher text with the final round key.
Main Rounds (Repeated until the first round):

InvShiftRows: Reverse the row shifts.
InvSubBytes: Apply the inverse of the S-box.
AddRoundKey: XOR with the round key.
InvMixColumns: The inverse of the MixColumns step is applied (except in the last round).
Final Round:

InvShiftRows
InvSubBytes
AddRoundKey
Output: The result is the original plaintext.

Example Using Python:
We can implement AES encryption and decryption in Python using the pycryptodome library. Here’s an example:

python
Copy
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Generate a random 128-bit key
key = get_random_bytes(16)  # 128-bit key

# Create an AES cipher object
cipher = AES.new(key, AES.MODE_CBC)

# Sample plaintext
plaintext = b'This is a secret message.'

# Pad the plaintext to be a multiple of 16 bytes
padded_data = pad(plaintext, AES.block_size)

# Encrypt the data
ciphertext = cipher.encrypt(padded_data)

# Decrypt the ciphertext
decipher = AES.new(key, AES.MODE_CBC, iv=cipher.iv)
decrypted_data = unpad(decipher.decrypt(ciphertext), AES.block_size)

# Output the results
print("Original Text:", plaintext)
print("Encrypted Text:", ciphertext)
print("Decrypted Text:", decrypted_data)
Key Points:
AES uses the same key for both encryption and decryption.
The CBC (Cipher Block Chaining) mode is used here to ensure that each block of plaintext depends on the previous block, making encryption more secure.
Padding is applied because AES operates on fixed-length blocks (128 bits/16 bytes). The padding ensures that the plaintext is a multiple of 16 bytes.
The key used for encryption must be securely managed and protected.
