# Go SSH Keygeneration

This is a simple script to generate SSH keys for a user on a remote server.

## Usage

```bash
go get github.com/PunGrumpy/go-ssh-key/ssh
```

```go
package main

import (
    "fmt"
    "log"
    "os"

    "github.com/PunGrumpy/go-ssh-key/ssh"
)

func main() {
    var (
        publicKey, privateKey []byte
        err                   error
    )
    if publicKey, privateKey, err = ssh.GenerateKey(); err != nil {
        log.Fatal(err)
    }
    if err = os.WriteFile("my_private_key", privateKey, 0600); err != nil {
        log.Fatal(err)
    }
    if err = os.WriteFile("my_public_key", publicKey, 0644); err != nil {
        log.Fatal(err)
    }
    fmt.Println("Done ✅")
}
```

```bash
go run main.go
# or go run .
```
