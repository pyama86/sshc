# sshc [![Build Status](https://travis-ci.org/k1LoW/sshc.svg?branch=master)](https://travis-ci.org/k1LoW/sshc) [![codecov](https://codecov.io/gh/k1LoW/sshc/branch/master/graph/badge.svg)](https://codecov.io/gh/k1LoW/sshc)

SSH client using ~/.ssh/config

## Usage

Describe `~/.ssh/config` as follows

```
Host myhost
  HostName 203.0.113.1
  User k1low
  Port 10022
  IdentityFile ~/.ssh/myhost_rsa
```

`sshc.NewClient()` returns `*ssh.Client` using `~/.ssh/config`

``` go
package main

import (
	"bytes"
	"log"

	"github.com/k1LoW/sshc"
)

func main() {
	client, err := sshc.NewClient("myhost")
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	session, err := client.NewSession()
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	defer session.Close()
	var stdout = &bytes.Buffer{}
	session.Stdout = stdout
	err = session.Run("hostname")
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	log.Printf("result: %s", stdout.String())
}
```

## sshc.Option

``` go
client, err := sshc.NewClient("myhost", User("k1low"), Port(1022))
```

Available options

- User
- Port
- Passphrase
- ConfigPath
    - UnshiftConfigPath
    - AppendConfigPath
    - ClearConfigPath
- UseAgent ( Default is `true` )

## References

- https://github.com/kevinburke/ssh_config
