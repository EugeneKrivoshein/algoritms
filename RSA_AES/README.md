# Шифрование RSA, AES

## Задача: Реализация шифрования с использованием RSA и AES на языке Go

### описание 
Описание задачи
Шифрование — это процесс преобразования обычных данных (открытый текст) в зашифрованный текст (шифротекст), который может быть прочитан только после дешифрования. RSA и AES являются двумя популярными алгоритмами шифрования: RSA — это асимметричный алгоритм, использующий пару ключей (публичный и приватный), в то время как AES — это симметричный алгоритм, использующий один и тот же ключ для шифрования и дешифрования.

### Задача для исполнения
Реализуйте функции для шифрования и дешифрования данных с использованием алгоритмов RSA и AES.

## Реализация RSA в Go 

```go
package main

import (
    "crypto/rand"
    "crypto/rsa"
    "crypto/sha256"
    "fmt"
)

func generateRSAKeys() (*rsa.PrivateKey, error) {
    privateKey, err := rsa.GenerateKey(rand.Reader, 2048)
    if err != nil {
        return nil, err
    }
    return privateKey, nil
}

func rsaEncrypt(publicKey *rsa.PublicKey, plaintext []byte) ([]byte, error) {
    label := []byte("")
    hash := sha256.New()
    ciphertext, err := rsa.EncryptOAEP(hash, rand.Reader, publicKey, plaintext, label)
    if err != nil {
        return nil, err
    }
    return ciphertext, nil
}

func rsaDecrypt(privateKey *rsa.PrivateKey, ciphertext []byte) ([]byte, error) {
    label := []byte("")
    hash := sha256.New()
    plaintext, err := rsa.DecryptOAEP(hash, rand.Reader, privateKey, ciphertext, label)
    if err != nil {
        return nil, err
    }
    return plaintext, nil
}

func main() {
    privateKey, _ := generateRSAKeys()
    publicKey := &privateKey.PublicKey

    message := "Hello, RSA!"
    fmt.Println("Original message:", message)

    encrypted, _ := rsaEncrypt(publicKey, []byte(message))
    fmt.Println("Encrypted message:", encrypted)

    decrypted, _ := rsaDecrypt(privateKey, encrypted)
    fmt.Println("Decrypted message:", string(decrypted))
}
```

## Реализация AES в Go 

```go

package main

import (
    "crypto/aes"
    "crypto/cipher"
    "crypto/rand"
    "fmt"
    "io"
)

func aesEncrypt(key, plaintext []byte) ([]byte, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return nil, err
    }

    ciphertext := make([]byte, aes.BlockSize+len(plaintext))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        return nil, err
    }

    stream := cipher.NewCFBEncrypter(block, iv)
    stream.XORKeyStream(ciphertext[aes.BlockSize:], plaintext)
    return ciphertext, nil
}

func aesDecrypt(key, ciphertext []byte) ([]byte, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return nil, err
    }

    if len(ciphertext) < aes.BlockSize {
        return nil, fmt.Errorf("ciphertext too short")
    }
    iv := ciphertext[:aes.BlockSize]
    ciphertext = ciphertext[aes.BlockSize:]

    stream := cipher.NewCFBDecrypter(block, iv)
    stream.XORKeyStream(ciphertext, ciphertext)
    return ciphertext, nil
}

func main() {
    key := []byte("AES256Key-32Characters1234567890") // 32 bytes for AES-256
    message := "Hello, AES!"

    fmt.Println("Original message:", message)
    encrypted, _ := aesEncrypt(key, []byte(message))
    fmt.Println("Encrypted message:", encrypted)

    decrypted, _ := aesDecrypt(key, encrypted)
    fmt.Println("Decrypted message:", string(decrypted))
}
```

### Задачи для исполнения

-Понимание и реализация RSA и AES шифрования в Go.
-Протестировать функции шифрования и дешифрования на различных данных для проверки их правильности.
-Обдумать меры безопасности при работе с ключами и шифротекстом, такие как безопасное хранение ключей и защита шифротекста от модификации.

### Практическое применение

Шифрование данных необходимо для защиты конфиденциальной информации, особенно при передаче данных через незащищенные каналы, такие как интернет. RSA и AES являются краеугольными камнями многих современных систем безопасности, включая защиту финансовых транзакций, персональных данных и государственной информации.