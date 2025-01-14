# Sui-Go-SDK

<p align="center">
    <a href="https://github.com/block-vision/sui-go-sdk/blob/main/.github/workflows/ci.yml"><img src="https://github.com/block-vision/sui-go-sdk/actions/workflows/ci.yml/badge.svg"></a>
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-red.svg"></a>
    <a href="https://goreportcard.com/report/github.com/block-vision/sui-go-sdk"><img src="https://goreportcard.com/badge/github.com/securego/gosec"></a>
    <a href="https://pkg.go.dev/github.com/block-vision/sui-go-sdk"> <img src="https://pkg.go.dev/badge/github.com/block-vision/sui-go-sdk.svg"></a>
    <a href="https://discord.gg/Re6prK86Tr"><img src="https://img.shields.io/badge/chat-on%20discord-7289da.svg?sanitize=true"></a>
</p>

##
sui-go-sdk is go language sdk for Project 
sui-go-sdk is project [Sui](https://github.com/MystenLabs/sui) SDK for Go programming language.

### Notices
+ You don't need to load your `sui.keystore` file if you just want to send some unsigned transactions.
+ File `sui.keystore` in config folder is test-only. Replace and load your own sui.keystore if your need to sign transactions. 
+ PRs are open to everyone and let's build useful tools for Sui community!


### Features
+ Load your keystore file and sign your messages with specific address.
+ Provide methods `MoveCallAndExecuteTransaction`/`BatchAndExecuteTransaction`.
+ Customized request method `SuiCall`.
+ Unsigned methods can be executed without loading your keystore file.

* [Quick Start](#Quick-Start)
* [Examples](#Examples)

## Quick Start

### Install 
```shell
go get github.com/block-vision/sui-go-sdk
```

### Go Version
| Golang Version |
|----------------|
| \>= 1.17.3     | 

## Examples

### Get start
```go
package main

import (
	"context"
	"fmt"

	"github.com/block-vision/sui-go-sdk/models"
	"github.com/block-vision/sui-go-sdk/sui"
)

func main() {
	// configure your endpoint here
	cli := sui.NewSuiClient("https://gateway.devnet.sui.io:443")
	resp, err := cli.GetRecentTransactions(context.Background(), models.GetRecentTransactionRequest{
		Count: 5,
	})
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println(resp)

	//If you want to request for original json response, you can use SuiCall().
	rsp, err := cli.SuiCall(context.Background(), "sui_getRecentTransactions", 5)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println(rsp)

	keystoreCli, err := sui.SetAccountKeyStore("../sui.keystore.fortest")
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println(keystoreCli.Keys())
	fmt.Println(keystoreCli.GetKey("your-address"))
}
```

### Send unsigned transactions

```go
func main() {
	cli := sui.NewSuiClient("https://gateway.devnet.sui.io:443")

	keystoreCli, err := sui.SetAccountKeyStore("../sui.keystore.fortest")
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println(keystoreCli.Keys())
	fmt.Println(keystoreCli.GetKey("your-address"))

	resp, err := cli.MoveCall(context.Background(), models.MoveCallRequest{
		Signer:          "0x4d6f1a54e805038f44ecd3112927af147e9b9ecb",
		PackageObjectId: "0x0000000000000000000000000000000000000002",
		Module:          "devnet_nft",
		Function:        "mint",
		TypeArguments:   []interface{}{},
		Arguments:       []interface{}{"blockvision", "blockvision", "testurl"},
		Gas:             "0x14802aeff2f444c888303f8fbba6d4e8451c38f8",
		GasBudget:       1000,
	})
	
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	fmt.Println(resp)
}
```


### Send signed transactions

```go
func main() {
    cli := sui.NewSuiClient("https://gateway.devnet.sui.io:443")

    keystoreCli, err := sui.SetAccountKeyStore("../sui.keystore.fortest")
    if err != nil {
        fmt.Println(err.Error())
        return
	}

    fmt.Println(keystoreCli.Keys())
    fmt.Println(keystoreCli.GetKey("your-address"))

    resp, err := cli.MoveCallAndExecuteTransaction(context.Background(), models.MoveCallAndExecuteTransactionRequest{
        MoveCallRequest: models.MoveCallRequest{
            Signer:          "0x4d6f1a54e805038f44ecd3112927af147e9b9ecb",
            PackageObjectId: "0x0000000000000000000000000000000000000002",
            Module:          "devnet_nft",
            Function:        "mint",
            TypeArguments:   []interface{}{},
            Arguments:       []interface{}{"blockvision", "blockvision", "testurl"},
            Gas:             "0x14802aeff2f444c888303f8fbba6d4e8451c38f8",
            GasBudget:       1000,
        },
    })
    if err != nil {
        fmt.Println(err.Error())
        return
    }
    fmt.Println(resp)
}
```

## Contribution  
Contributions are welcomed and greatly appreciated.   
Please follow the PR/issue template.  
Thank you to all the people who participate in building better infrastructure! 

## Resources
+ [SDK Examples](https://github.com/block-vision/sui-go-sdk/tree/main/examples)
+ [Sui](https://github.com/MystenLabs/sui)


## License 
[Apache 2.0 license](LICENSE)




