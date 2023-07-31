# Why Encryption

* Encryption in flight (SSL)
  * data is encrypted before sending and decrypted after receiving
  * SSL certificates help with encryption (HTTPS)
  * encryption in flight ensures no MITM
* Server side encryption at rest
  * data is encrypted after being received by the server
  * data in decrypted before being sent
  * it is stored in an encrypted form thanks to a key (usually a data key)
  * the encryption/decryption keys must be managed somewhere and the server *must* have access to it
* client side encryption
  * data is encrypted by the client and never decrypted by the server
  * data will be decrypted by a receiving client
  * server *should not* be able to decrypt data
  * could leverage Envelope Encryption
