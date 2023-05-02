# Go SSH Keygeneration

This is a simple script to generate SSH keys for a user on a remote server.

## Usage

```bash
touch main.go
go get github.com/PunGrumpy/go-ssh-demo/ssh
code main.go
```

### Generate Key

```go
package main

import (
    "fmt"
    "log"
    "os"

    "github.com/PunGrumpy/go-ssh-demo/ssh"
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
    if err = os.WriteFile("server_public_key", publicKey, 0644); err != nil {
        log.Fatal(err)
    }
    fmt.Println("Done ✅")
}
```

### Server Run

```go
package main

import (
	"fmt"
	"io/ioutil"

    "github.com/PunGrumpy/go-ssh-demo/ssh"
)

func main() {
	var (
		err error
	)
	serverKeyBytes, err := ioutil.ReadFile("my_private_key.pem")
	if err != nil {
		fmt.Printf("unable to read server key: %v", err)
	}

	authorizedKeysBytes, err := ioutil.ReadFile("server_public_key.pub")
	if err != nil {
		fmt.Printf("unable to read authorized keys: %v", err)
	}

	if err = ssh.StartServer(serverKeyBytes, authorizedKeysBytes); err != nil {
		fmt.Printf("unable to start SSH server: %v", err)
	}
}
```
