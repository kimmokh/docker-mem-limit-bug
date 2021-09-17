# docker-mem-limit-bug
PoC about Docker compose mem_limit environment variable substitution bug.

## Prerequisites

Docker desktop 4.0.1
```
➜  docker-mem-limit-bug git:(develop) ✗ docker version
Client:
Cloud integration: 1.0.17
Version:           20.10.8
API version:       1.41
Go version:        go1.16.6
Git commit:        3967b7d
Built:             Fri Jul 30 19:55:20 2021
OS/Arch:           darwin/amd64
Context:           default
Experimental:      true

Server: Docker Engine - Community
Engine:
Version:          20.10.8
API version:      1.41 (minimum version 1.12)
Go version:       go1.16.6
Git commit:       75249d8
Built:            Fri Jul 30 19:52:10 2021
OS/Arch:          linux/amd64
Experimental:     false
containerd:
Version:          1.4.9
GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
runc:
Version:          1.0.1
GitCommit:        v1.0.1-0-g4144b63
docker-init:
Version:          0.19.0
GitCommit:        de40ad0
```
## How to run

Start Docker Compose:
```shell
docker compose up
```

Outcome:
```shell
➜  docker-compose-mem-limit-bug docker compose up
panic: invalid type for size types.UnitBytes

goroutine 1 [running]:
github.com/compose-spec/compose-go/loader.glob..func15(0x1dc7040, 0xc00021e1a0, 0xc0004f32a0, 0xc00038a790, 0xc00021e101, 0x0)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:1016 +0x1a6
github.com/compose-spec/compose-go/loader.createTransformHook.func1(0x213cdc8, 0x1dc7040, 0x213cdc8, 0x1dc7040, 0x1dc7040, 0xc00021e1a0, 0xc0004f33d8, 0x169b505, 0x1dddde0, 0xc0003067a0)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:369 +0x7f
github.com/mitchellh/mapstructure.DecodeHookExec(0x1de8800, 0xc000591860, 0x1dc7040, 0xc00021e1a0, 0x86, 0x1dc7040, 0xc0000e6cd0, 0x186, 0x0, 0x1db1e40, ...)
	github.com/mitchellh/mapstructure@v1.4.1/decode_hooks.go:47 +0x28f
github.com/mitchellh/mapstructure.ComposeDecodeHookFunc.func1(0x1dc7040, 0xc00021e1a0, 0x86, 0x1dc7040, 0xc0000e6cd0, 0x186, 0x2f70bb8, 0x10, 0x10, 0x2d00108)
	github.com/mitchellh/mapstructure@v1.4.1/decode_hooks.go:68 +0xd4
github.com/mitchellh/mapstructure.DecodeHookExec(0x1db1e40, 0xc0003067a0, 0x1dc7040, 0xc00021e1a0, 0x86, 0x1dc7040, 0xc0000e6cd0, 0x186, 0x1015247, 0xc0004f3c08, ...)
	github.com/mitchellh/mapstructure@v1.4.1/decode_hooks.go:51 +0xe2
github.com/mitchellh/mapstructure.(*Decoder).decode(0xc00076c5e8, 0x1dbfe3b, 0x9, 0x1dc7040, 0xc00021e1a0, 0x1dc7040, 0xc0000e6cd0, 0x186, 0x94, 0x0)
	github.com/mitchellh/mapstructure@v1.4.1/mapstructure.go:431 +0x8ba
github.com/mitchellh/mapstructure.(*Decoder).decodeStructFromMap(0xc00076c5e8, 0x0, 0x0, 0x1dbd300, 0xc000144510, 0x15, 0x1f6bc80, 0xc0000e6a00, 0x199, 0x0, ...)
	github.com/mitchellh/mapstructure@v1.4.1/mapstructure.go:1377 +0x226f
github.com/mitchellh/mapstructure.(*Decoder).decodeStruct(0xc00076c5e8, 0x0, 0x0, 0x1dbd300, 0xc000144510, 0x1f6bc80, 0xc0000e6a00, 0x199, 0x1dbd300, 0xc000144510)
	github.com/mitchellh/mapstructure@v1.4.1/mapstructure.go:1203 +0x63a
github.com/mitchellh/mapstructure.(*Decoder).decode(0xc00076c5e8, 0x0, 0x0, 0x1dbd300, 0xc000144510, 0x1f6bc80, 0xc0000e6a00, 0x199, 0xc0000e6a00, 0x199)
	github.com/mitchellh/mapstructure@v1.4.1/mapstructure.go:454 +0x776
github.com/mitchellh/mapstructure.(*Decoder).Decode(0xc00076c5e8, 0x1dbd300, 0xc000144510, 0x0, 0xc0003067a0)
	github.com/mitchellh/mapstructure@v1.4.1/mapstructure.go:389 +0xf0
github.com/compose-spec/compose-go/loader.Transform(0x1dbd300, 0xc000144510, 0x1defc40, 0xc0000e6a00, 0x0, 0x0, 0x0, 0xc0005b03c0, 0x18)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:321 +0x1bc
github.com/compose-spec/compose-go/loader.LoadService(0xc00021e0d8, 0x5, 0xc000144510, 0xc0001640f0, 0x34, 0xc0004f4990, 0x0, 0xc0002ea400, 0xc000680000)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:524 +0x77
github.com/compose-spec/compose-go/loader.loadServiceWithExtends(0xc0001640f0, 0x47, 0xc00021e0d8, 0x5, 0xc0001444e0, 0xc0001640f0, 0x34, 0xc0004f4990, 0xc000144240, 0xc0004f43a0, ...)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:451 +0x133
github.com/compose-spec/compose-go/loader.LoadServices(0xc0001640f0, 0x47, 0xc0001444e0, 0xc0001640f0, 0x34, 0xc0004f4990, 0xc000144240, 0x1dbd300, 0x0, 0xc0001444e0, ...)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:435 +0x2d3
github.com/compose-spec/compose-go/loader.loadSections(0xc0001640f0, 0x47, 0xc0001444b0, 0x0, 0x0, 0xc0001640f0, 0x34, 0xc000144210, 0x1, 0x1, ...)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:256 +0x1c5
github.com/compose-spec/compose-go/loader.Load(0x0, 0x0, 0xc0001640f0, 0x34, 0xc000144210, 0x1, 0x1, 0xc000144030, 0xc00000e060, 0x1, ...)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/loader/loader.go:167 +0x345
github.com/compose-spec/compose-go/cli.ProjectFromOptions(0xc000146000, 0x0, 0x0, 0x0)
	github.com/compose-spec/compose-go@v0.0.0-20210901090333-feb401cda7f7/cli/options.go:331 +0x4bf
github.com/docker/compose/v2/cmd/compose.(*projectOptions).toProject(0xc0002ddc00, 0x28e4f88, 0x0, 0x0, 0x0, 0x0, 0x0, 0xc000494540, 0x0, 0x0)
	github.com/docker/compose/v2/cmd/compose/compose.go:163 +0x97
github.com/docker/compose/v2/cmd/compose.(*projectOptions).WithServices.func1(0x2126320, 0xc0000ba200, 0x28e4f88, 0x0, 0x0, 0xc000768018, 0x12)
	github.com/docker/compose/v2/cmd/compose/compose.go:108 +0x8b
github.com/docker/compose/v2/cmd/compose.Adapt.func1(0xc00027f400, 0x28e4f88, 0x0, 0x0, 0x0, 0x0)
	github.com/docker/compose/v2/cmd/compose/compose.go:62 +0x12f
github.com/spf13/cobra.(*Command).execute(0xc00027f400, 0xc0004b5750, 0x0, 0x0, 0xc00027f400, 0xc0004b5750)
	github.com/spf13/cobra@v1.2.1/command.go:856 +0x472
github.com/spf13/cobra.(*Command).ExecuteC(0xc0002e9680, 0xc0002e9680, 0xc0004b5740, 0x2)
	github.com/spf13/cobra@v1.2.1/command.go:974 +0x375
github.com/spf13/cobra.(*Command).Execute(...)
	github.com/spf13/cobra@v1.2.1/command.go:902
github.com/docker/cli/cli-plugins/plugin.RunPlugin(0xc0000ac0d0, 0xc00027f180, 0x1f6f82d, 0x5, 0x1f768a6, 0xb, 0x20e7bf0, 0xb, 0x0, 0x0, ...)
	github.com/docker/cli@v20.10.7+incompatible/cli-plugins/plugin/plugin.go:51 +0x146
github.com/docker/cli/cli-plugins/plugin.Run(0x1fe99d8, 0x1f6f82d, 0x5, 0x1f768a6, 0xb, 0x20e7bf0, 0xb, 0x0, 0x0, 0x0, ...)
	github.com/docker/cli@v20.10.7+incompatible/cli-plugins/plugin/plugin.go:64 +0x13f
main.main()
	github.com/docker/compose/v2/cmd/main.go:38 +0xd8
```
