Overview

This page outlines the specifications and implementation details for handling the BSN (Burgerservicenummer) number securely within the Pensioenservices. The primary objective is to ensure that BSNs are never exposed in plaintext, thereby preventing unauthorized access and unnecessary storage in logs.

Requirements





BSN Confidentiality: The BSN MUST NOT be shared as a plaintext value.



Encryption Protocol: When sharing the BSN in resource requests or responses, it must be encrypted using RSA encryption.



Key Management: The receiver’s public key will be used for encrypting the BSN, while the receiver’s private key will be used for decryption.

Encryption specifications





Encryption Algorithm: RSA



Key Length: 2048 bits



Padding Scheme: OAEP (Optimal Asymmetric Encryption Padding)



Input Format:





The BSN number must be encoded as a UTF-8 string.



Example BSN input: "12323455"



Output Format:





The encrypted output will be base64 encoded to ensure safe transmission over text-based protocols.

Implementation Steps





Obtain the Public Key:





The public key used for encryption must be sourced from the server's certificate.



Encrypting the BSN:





Convert the BSN number to a UTF-8 encoded string.



Use the RSA algorithm with OAEP padding to encrypt the BSN using the server's public key.



The output of the encryption process will be a byte array.



Base64 Encoding:





Convert the encrypted byte array into a base64 encoded string to facilitate safe transmission.



Sharing the Encrypted BSN:





The encrypted BSN can now be safely shared in resource requests or responses without exposing the original plaintext value.

Example Code Snippet

Below is an example code snippet in Python illustrating how to encrypt a BSN using RSA with OAEP padding. This is only for demonstration purposes.



from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import base64

def encrypt_bsn(bsn, public_key):
    # Convert BSN to UTF-8 encoded bytes
    bsn_bytes = bsn.encode('utf-8')
    
    # Load the public key
    rsa_key = RSA.import_key(public_key)
    cipher = PKCS1_OAEP.new(rsa_key)

    # Encrypt the BSN
    encrypted_bsn = cipher.encrypt(bsn_bytes)

    # Return the base64 encoded encrypted BSN
    return base64.b64encode(encrypted_bsn).decode('utf-8')

# Example usage
public_key = "-----BEGIN PUBLIC KEY-----\n...your public key...\n-----END PUBLIC KEY-----"
bsn = "12323455"
encrypted_bsn = encrypt_bsn(bsn, public_key)
print("Encrypted BSN:", encrypted_bsn)


Test credentials

Test private key:

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAxjDdgwniyVIwjQDOo+0OcuL5Zvm0S3br5WDoPyKtu8E9RQTm
dc0ZQ6n9qCDjQGtEHQA3pFY/GXPRFpTNBxZI2cKAYApKjXGtpbSzp3u4WWkV0ZqZ
tGdUynDDDhAquqP2yS7PZUw3WHbYbd25xWOIzeyWb+vuzlC5hCEoTmuMYWQO02Em
+OVok2Xm5owl4YhzF5G65SJhhEu6hIazIw07ZiCyWxleHFNFBbFLlhXkKYoZeBUa
QAH3J/g4rkKil7DwaL4YBSCCoryqL5dlayG6N9OPRnXLlBsMLBtiylkI5BEpOP7h
EA29x/qDjfuKixTOZX7QfEPuYtgp2arhpLaUQwIDAQABAoIBAEaYpGbncBWXRb0M
Qw8oQ2PQDtfL7ZmN+FRCEyheIXWGTV1W9AKVNsEf/b9US66oJuCPscJDiIF3/Ewn
X+r82KFcw56yO8Erc5fZiL8Jdm6+3GtHvyWAQzdDOG+7eYT3H9Kk7nJeVj6YQtrL
0YtnMnm4i1yCWayrAChuMw9TJK6IG64RiYJwrhtNU1PMuLLGOyuCpoHrnvtT3Q/R
ehzrGM0CgYEA5YLsAAskmhZr/NGHIMW0ex6jKxQPIdy+FpN0aVz5KWdPCUCrfOVX
798UWKXdssP47btJweO1LpBGkEPyvl3R0pUcCLC1mXSp9HYdpQeZpEsEzh/skS+H
3OB9E1OLa43Xg5iQFFzzY7FeCFWZ62zaHgWlrWHjHp4/dJXdJ3SVnqcCgYEA3RCO
UiDRJmf6hGx6O+x/L4btLxU3rgoK2VBn0Gap/eugAEVrVFAEi268o9RqC5WsoUB3
WpWY7z+1u0WH2MQT07Qs7rTD0WkaE96wNywKQWr09bMrZNqNe/dYSTUlfI0vO3YO
nq+PDEn42+hdtk5dty6Hnqy3B133Og7Nu8jkDQUCgYBrcfw8FPtgq6iIZisFm6jf
jrtCmpRu/OF8vjFcdisrR/MHWOAvi0q8wEnNKnF8lCRAX1KrU4fpsZ9nQHguyMtG
84l5RAjwO16pVHaSYNl7wacRPH7KhV7sd6f2WUFG69N+BvlEnzNOc6Wa70Asp/wA
3BHw9oeWOO1qDhLHutuktwKBgFo1/14UdkUt+VVAz0DU6bIDZZbpGae0AWldHoA0
5PnxxYFW0s29OZ2Muv5AGGZR1fiXzhzxH0/Ct+6MGTukCFNEG3Ht4lr6gOHK5i4d
krHPa9c4HmUxqrsD3MtDHOEO3E9vhdfL3JwvM5bJ5DQZFrVCD6X45VfkSJcwo1QW
TrnhAoGBAN3oEDnAlOrt22Cgd9aIVnB1ZQsSCzz9q4dJP+DyX1iVWLYmeBGGSRzI
sKHIKU8EI7mqJEtLfI1WBYzdlHOA7X9VXSuT5D18OUtyxAXhSXkBDEckaboYNFXk
LrBm8elCb2KXFIl2lrB2fVBvhQ0VHNKhOIRXmgpzxpuxygLxHZuR
-----END RSA PRIVATE KEY-----

Test certificate:

-----BEGIN CERTIFICATE-----
MIIDaDCCAlCgAwIBAgIUaGztAt91jvRF+VSD+AJxRDnYcVMwDQYJKoZIhvcNAQEL BQAwYTELMAkGA1UEBhMCTkwxFjAUBgNVBAgMDU5vb3JkIEhvbGxhbmQxEjAQBgNV BAcMCUFtc3RlcmRhbTEQMA4GA1UECgwHSU5OT1BBWTEUMBIGA1UEAwwLSU5OT1BB WS5jb20wHhcNMjQxMTIxMTgxMTIxWhcNMjUxMTIxMTgxMTIxWjBhMQswCQYDVQQG EwJOTDEWMBQGA1UECAwNTm9vcmQgSG9sbGFuZDESMBAGA1UEBwwJQW1zdGVyZGFt MRAwDgYDVQQKDAdJTk5PUEFZMRQwEgYDVQQDDAtJTk5PUEFZLmNvbTCCASIwDQYJ KoZIhvcNAQEBBQADggEPADCCAQoCggEBAMYw3YMJ4slSMI0AzqPtDnLi+Wb5tEt2 6+Vg6D8irbvBPUUE5nXNGUOp/agg40BrRB0AN6RWPxlz0RaUzQcWSNnCgGAKSo1x raW0s6d7uFlpFdGambRnVMpwww4QKrqj9skuz2VMN1h22G3ducVjiM3slm/r7s5Q eAnyTls9qXqED8k6PGFgWyxYv2S3z7R4EPCUeMS/ygcmRy2xesM+qKYTxBLEuwFl +xyi2yjzSkzab7TH9kPDtvqa/SAFbVvaxOJSoIK7V+vkkVfO35VIQfkdQtP7J8cv 8zeVoov0Tey/Idr6snpKzmXUGIMP6g19iKiG9uFh6AalGIvnmqEnSB3p8LZqrag+ 6PO5iHyJgA70nU5Rk1gj0ODIwKC4aNn2g/CuxKtyVW8F8o6bDJSnhbRxRPbXpeqA LaZmfoeCpoMlx+ZY7gioW4H2iW8pKsKvfn4/C3gRzD9V4qUA0L97sA1WNFWlJ5gA
AIsvtgMF3BOPW1Pj-----END CERTIFICATE-----


