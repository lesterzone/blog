---
layout: post
title: backup dot files
---

Goal:

Create a zip file with dotfiles and all things we want to backup.

We need:

- aws account, to store files in S3
- golang, because, why not?

```sh
    botname
```

The script:

```go
package main

import (
	"fmt"
	"os"
	"os/exec"
	"strings"
	"time"
)

func main() {
	now := time.Now().String()[:19] // take 2020-04-08 22:53:02 from full time
	value := strings.Replace(now, " ", "-", -1)
	value = strings.Replace(value, ":", "-", -1)

	params := []string{
		"-r", value + ".zip",
		"-P", os.Getenv("SOME SECRET HERE"),
		os.Getenv("HOME") + "/.bashrc",
		os.Getenv("HOME") + "/.vimrc",
		os.Getenv("HOME") + "/.gitconfig",
		os.Getenv("HOME") + "/.developconfig",
		os.Getenv("HOME") + "/.sideprojectsconfig",
		os.Getenv("HOME") + "/.gitattributes",
		os.Getenv("HOME") + "/Library/Application Support/Code/User/keybindings.json",
		os.Getenv("HOME") + "/Library/Application Support/Code/User/settings.json",
		os.Getenv("HOME") + "/.zshrc",
	}

	_, err := execute("zip", params)

	if err != nil {
		fmt.Printf("Error on copy %+v", err)
		return
	}

	uploadParams := []string{
		"s3",
		"cp",
		value + ".zip",
		"s3://"+os.Getenv("S3_BACKUP_BUCKET")+"/" + value + ".zip",
		// "--profile",
		// "AWS_PROFILE_HERE_IF_NEEDED",
	}

	fmt.Println("uploading " + value)

	_, err = execute("aws", uploadParams)

	if err != nil {
		fmt.Printf("Error on upload %+v", err)
		return
	}

	fmt.Println("cleaning up....")
	_, err = execute("rm", []string{value + ".zip"})

	if err != nil {
		fmt.Printf("Error on remove zip %+v", err)
		return
	}
}

func execute(command string, arguments []string) (string, error) {
	cmd := exec.Command(command, arguments...)
	out, err := cmd.CombinedOutput()
	if err != nil {
		fmt.Println(fmt.Sprint(err) + string(out))
		return "", err
	}

	output := string(out[:])
	return output, err
}
```