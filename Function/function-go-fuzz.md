# Fuzzing a function with go-fuzz

Tool

    https://github.com/dvyukov/go-fuzz


Target function

    https://github.com/hashicorp/vault/tree/main/sdk/helper/xor

XORBase64

    // XORBase64 takes two base64-encoded strings and XORs the decoded byte slices
    // together, returning the final byte slice. It is an error to pass in two
    // strings that do not have the same length to their base64-decoded byte slice.
    func XORBase64(a, b string) ([]byte, error) {
        aBytes, err := base64.StdEncoding.DecodeString(a)
        if err != nil {
            return nil, fmt.Errorf("error decoding first base64 value: %w", err)
        }
        if aBytes == nil || len(aBytes) == 0 {
            return nil, fmt.Errorf("decoded first base64 value is nil or empty")
        }

        bBytes, err := base64.StdEncoding.DecodeString(b)
        if err != nil {
            return nil, fmt.Errorf("error decoding second base64 value: %w", err)
        }
        if bBytes == nil || len(bBytes) == 0 {
            return nil, fmt.Errorf("decoded second base64 value is nil or empty")
        }

        return XORBytes(aBytes, bBytes)
    }



Install the vault package

    go get github.com/hashicorp/vault

    go install github.com/dvyukov/go-fuzz/go-fuzz@latest github.com/dvyukov/go-fuzz/go-fuzz-build@latest

    which go-fuzz  [/home/diegoall/go/bin/go-fuzz]


Write the fuzzer in the path named fuzz.go

    cd $GOPATH/pkg/mod/github ..Debo llegar aca:
    /home/diegoall/go/pkg/mod/github.com/hashicorp/vault/sdk@v0.13.0/helper/xor

Execute:

    go-fuzz-build

    /home/diegoall/go/pkg/mod/github.com/hashicorp/vault/sdk@v0.13.0/helper/xor/fuzz.go:4:19: undefined: strings
    -: no required module provides package github.com/dvyukov/go-fuzz/go-fuzz-dep; to add it:
            go get github.com/dvyukov/go-fuzz/go-fuzz-dep
    typechecking of . failed

    root@pho3nix:/home/diegoall/go/pkg/mod/github.com/hashicorp/vault/sdk@v0.13.0/helper/xor# go get github.com/dvyukov/go-fuzz/go-fuzz-dep
    go: added github.com/dvyukov/go-fuzz v0.0.0-20240203152606-b1ce7bc07150

Generates xor-fuzz.zip file

    xor-fuzz.zip


Execute 

    go-fuzz

    root@pho3nix:/home/diegoall/go/pkg/mod/github.com/hashicorp/vault/sdk@v0.13.0/helper/xor# go-fuzz
    2024/08/27 23:55:45 workers: 16, corpus: 58 (1s ago), crashers: 0, restarts: 1/0, execs: 0 (0/sec), cover: 0, uptime: 3s
    2024/08/27 23:55:48 workers: 16, corpus: 59 (2s ago), crashers: 0, restarts: 1/0, execs: 0 (0/sec), cover: 197, uptime: 6s
    2024/08/27 23:55:51 workers: 16, corpus: 59 (5s ago), crashers: 0, restarts: 1/8270, execs: 1190880 (132295/sec), cover: 197, uptime: 9s
    2024/08/27 23:55:54 workers: 16, corpus: 59 (8s ago), crashers: 0, restarts: 1/9119, execs: 2361957 (196801/sec), cover: 197, uptime: 12s
    2024/08/27 23:55:57 workers: 16, corpus: 59 (11s ago), crashers: 0, restarts: 1/9349, execs: 3449953 (229970/sec), cover: 197, uptime: 15s
    2024/08/27 23:56:00 workers: 16, corpus: 60 (2s ago), crashers: 0, restarts: 1/9555, execs: 4634519 (257448/sec), cover: 197, uptime: 18s