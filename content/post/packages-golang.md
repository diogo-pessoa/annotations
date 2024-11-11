---
title: "Packages Golang" 
date: 2024-10-01T10:54:43+01:00 
description: "" 
draft: true 
featured: false
toc: false 
codeMaxLines: 10 
codeLineNumbers: false 
figurePositionShow: true 
categories:
  - coding
tags:
  - golang
  - packages

---

## Summary

### Naming things

- [Package Names GoBlog](https://go.dev/blog/package-names)


* `go env`



### GOPATH -> Go modules

$GOPATH is being deprecated [2], even though the course explain how it works. I should look into 
how Go module works 

From the FAQ:
> Is the GOPATH variable being removed? 
> No. The GOPATH variable (set in the environment or by 
> go env -w) is not being removed. It will still be used to determine the default binary install location, module cache location, and checksum database cache location, as mentioned at the top of this page.

see: Go Modules [1]
 



## References

1. [Master Golang Udemy](https://www.udemy.com/course/master-go-programming-complete-golang-bootcamp/learn/lecture/17592610#overview)
2. [Deprecating GOPATH](https://go.dev/wiki/GOPATH#deprecating-and-removing-gopath-development-mode)
3. [Go Modules](https://go.dev/blog/using-go-modules)
