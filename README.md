# Oblivious-Data-Integrity-Verification (ODIV) using Partial Homomorphic Encryption (Addition)

> **Author:** Silene0259 (Joe T)
>
> **Initial Creation Date:** 05 February 2024
>
> **Upload:** 19 February 2024

**Description:** A method using homomorphic encryption for verifying the integrity of two files between two nodes without the node knowing what the file was/is, ie. the hash digest. This method, while *simple*, can be extremely **powerful** and easily **extendable**.

## Notice + Warning (Please Read)

I do not have the time to go in-depth about this as of right now. This is still in its infancy and I am currently doing multiple things at once. I was just hoping other people would catch onto the idea and expand on it. Its quite powerful and simple while being easily extensible. Feel free to add to this or correct me on anything.

This was done real quickly. If anyone else picks up on the idea could you please explain it better as I do not have the time. Its quite powerful but I seriously do not have the time right now.

There may be mistakes as this is written by a student but I believe it is a powerful construction that should be examined. This was written in my free time with no help in under a day.

## Introduction

### Background

In the field of computer science, advances like cloud computing, distributed networks, decentralized applications/networks, among other new advances have had a major impact on how systems communicate with each other. Advances in cryptography have allowed for major changes to be made to how systems operate and communicate.

---

This paper simply introduces a naive idea of using homomorphic encryption to homomorphically add two hashes together securely without revealing the initial hash from one side of the communication (which can also be extended to not revealing it for both parties.) While simple in nature, it introduces a **large amount of applications** to a variety of different fields and is just the beginning on the power of using homomorphic encryption especially combined with hash functions.

## Basics

### About The Construction And Usage

The **naive idea** behind this particular construction is that the following can happen:

> 1. I have **given data** (for example, an image or password) and its hash digest `digest = Hash(data)` that I would like to be able to see whether another node *knows of* or *has*.
> 2. I can then **prove** *without giving away* the data that the other node either knows of / has the data or does not have the data without revealing what the data is simply by homomorphically adding the hash.
>
> This works almost like a zero-knowledge proof in that the data is never learned unless they both possess the same data. It does not require much unlike zero-knowledge proof constructions and can simply be constructed using homomorphic encryption and hash functions.

This simple application of partially homomorphic encryption with simple addition allows for fast computations to be done on encrypted data without revealing that encrypted data.

### Requirements

The basic requirements for using **Oblivious Data Integrity Verification (ODIV)** is that the following *cryptographic applications* are available:

1. A chosen **Cryptographic Hash Function** (SHA1,SHA256,BLAKE2b/s,SHA3, etc.)
2. At least a **Partially Homomorphic Encryption** (PHE) cryptosystem that supports **simple addition**. For example, TFHE-rs offers the given requirements. The preferred way of making this work is using a type, like an unsigned 256-bit integer, but bit splicing can be done.

Another requirement is that there is a **given input data** that is then hashed. This can be *anything*, including an image, video, text, key, executable, or generally any file or input that can be digested by a hash function.

This input data *should* be known by both parties but in reality, only one of the parties needs to know it to verify that the other party knows what the original party is talking about.

### Example Construction

A client first has some **data** (`x`) they would like to verify with another node whether they have the same thing. This data could be anything ranging from a password, key, to an image or video. For this example, we will use an **image**.

In this scenario, the other node either knows the image or has no knowledge of it.

---

#### Naive Method By Addition of Same Digest (Is More Powerful Than This)

For this to work, the client needs to generate a secret key for homomorphic encryption. The client includes the digest of the hash as a unsigned-256 integer (`u256`) or by splicing it into multiple pieces to work with libraries that only support `u32` like TFHE-rs.

It then sends the encrypted data to the node. The node then adds on their data to the encrypted data by simply adding it up. It then sends it back to the client.

Finally, the client decrypts the data and checks whether the given data is the same by subtracting the hash from it. If it is the same, then they both have the same data.

This is only the beginning of how powerful this simple construction is as there are other possibilities like adding a login nonce to a password or having it so both parties do not learn anything from each other.

**Note:** Please expand on if you get what I am talking about.

### Pseudocode In Python (unfinished)

```python
import hashlib


# Basic Hashing
def hash_data(data):
    # Hash Input Data
    x = hashlib.sha256(data)

    # Return Hexadecimal Hash
    digest = x.hashdigest()

return digest
        

# Homomorphic-Encryption Pseudocode

# Generates client secret key for homomorphic encryption
def generate_client_secret_key():
    key = gen_sk()
    return key

def generate_server_keys():
	keys = gen_sk_server()
    return keys
        
def encrypt_on_client_side(client_sk,digest):
    encrypted_msg = client_sk.encrypt(digest)
    return encrypted_msg

def decrypt_on_client_side(client_sk,msg):
    cleartext = client_sk.decrypt(msg)
    return cleartext

def add_to_encrypted(encrypted_msg,server_keys,server_digest):
    output = server_keys.try_add(encrypted_msg + server_digest)
    return output

def main():
    #===========CLIENT==========#
    
    # Example Input Data
    x = "example data"
    
    # Client Hashes Data
    client_digest = hash_data(x)
    
    # Generate Secret Key
    client_sk = generate_client_secret_key()
    
    # Homomorphically Encrypted Message
    encrypted_msg = encrypt_on_client_side(client_sk,client_digest)
    
    #==========SERVER==========#
    
    # Example Input Data (same as client's)
    server_x = "example data"
    
    # Server Hashes Data
    server_digest = hash_data(server_x)
    
    # Note: Look into more at how exactly this is done in TFHE-rs
    server_keys = generate_server_keys()
    
    # Addition of Hashes Homomorphically
    added_to_encrypted_msg = add_to_encrypted(encrypted_msg,server_keys,server_digest)
    
    #==========CLIENT===========#
    
    # Convert To Cleartext Number (should just be hash + hash)
    cleartext = decrypt_on_client_side(client_sk,added_to_encrypted_msg)
    
    # Add together client digests (can also subtract)
    check_text = client_digest + client_digest
    
    if cleartext == check_text:
        print("The Input Data Is The Same.")
        return True
    else:
        print("The Input Data Is Different.")
		return False
    
    #Notice: This code is sloppy and is just here to showcase how simple the idea is. There are some other things that need to be changed.
```



## Reasoning Behind Construction

### What Is The Reason Behind Using Simple Addition?

It is much easier to implement partial homomorphic encryption as well as being much more efficient than **Fully-Homomorphic-Encryption (FHE)**.



# Resources

[Zama - Boolean SHA256 TFHE](https://www.zama.ai/post/boolean-sha256-tfhe-rs)



## Last Notice

Sorry for lack of better content. This was done in two hours and I just wanted to get the idea out there. It will be updated in the future. Hopefully others catch the idea of what I was trying to do.
