# Encryption & Decryption

Encryption is converting a "plain text" into a "cipher text". Decryption is on the contrary getting the plain text from the cipher text.

There are 2 methodologies of it: Symmetric or Asymmetric.

Symmetric Encryption: The same key is used for both encrypting and decrypting the data. E.g. AES algorithm.

Asymmetric Encryption: The keys to encrypt and decrypt the data are different. In short, there is no pre-shared key between the receiver and the transmitter. In this metholdogy, a key pair is used:public key and private key. They are mathematically relational but they can not be derived from each other. Meaning in that, one can not be used to derive the other.
The key-pair consists of a private key and a public key. The public key(shared key) of the remote end is used to encrypt a data blob. The encrypted data blob is decrypted by the remote using its own private key. 

**What is Block Cipher**<br>
The block cipher is the thing that takes the block of plain text and generates the cipher text using the key.
There are different ways of generating the block cipher.<br>
Assuming that AES is considered as the block cipher, the block size must be multiples of 128 bits even though the key size can be 128 / 192 / 256 bits.<br>
Therefore, e.g. when it is needed to encrypt a 32 bytes length plain text, the block cipher AES will generate the plain text by taking 128 bits of the block into account at a time.
 
**What are ECB and CBC?**<br>
ECB (Electronic Control Block):<br>
The block ciphers can be generated in different ways. ECB is one of the methodologies and in ECB, each 128bits of the plain text is processed with the same key to generate the cipher text.

CBC (Cipher Block Chaining):<br>
The block data is again split into 128 bits sub-blocks but in the CBC, the cipher text is generated using both the AES key and the previous cipher.<br>
The process starts with the IV. The plain text is forst XORed with the IV and then encryption starts with the AES key.
The results of the first chunk is used as the IV of the encryption of the second block.<br>
Basically, the 2nd plain data chunk is firstly XORed with the previous cipher and then encrypted with the key.
Dynamic IV provides different cipher text after the each of encryption.


**AES CBC 256 by using OpenSSL Terminal**<br>
1. *Generate the IV and Key:*<br>
openssl enc -aes-256-cbc -pass pass:hunkar_aes_cbc -P -nosalt<br>
This generates and prints out the 128 bits length IV and the 256 bits length key which are derivated from the password as below:<br>
key=2120BDBE8557E7AC561DAB6ADDEF71F3AE83F6593C15BD78BC3E52849530FEB2<br>
iv =1D6D8215B53EA9BA0B8E81E721021473<br>

2. *Encrypt the message:*<br>
openssl enc -aes-256-cbc -iv 1D6D8215B53EA9BA0B8E81E721021473 -K 2120BDBE8557E7AC561DAB6ADDEF71F3AE83F6593C15BD78BC3E52849530FEB2 -in input.plain  -out output.cipher

2. *Decrypt the message:*<br>
openssl enc -aes-256-cbc -iv 1D6D8215B53EA9BA0B8E81E721021473 -K 2120BDBE8557E7AC561DAB6ADDEF71F3AE83F6593C15BD78BC3E52849530FEB2 -in output.cipher  -out output.dec -d
