## rust 使用 tonic 集成 protobuf
- 1、protobuf 文件编写
    ```rust
    syntax = "proto3";

    package voting;

    service Voting {
    rpc Vote(VotingRequest) returns (VotingResponse);
    }

    message VotingRequest {
    string url = 1;
    enum Vote {
        UP = 0;
        DOWN = 1;
    }

    Vote vote = 2;
    }

    message VotingResponse { string confirmation = 1; }
    ```
- 2、build.rs文件编写
    
    ```rust
    use std::io::Result;
    
    fn main() -> Result<()> {
        tonic_build::configure()
            .build_server(true)
            .build_client(true)
    				// 指定编译生成的 voting.rs 的文件夹
            .out_dir("protos")
    				// 指定 proto 文件的位置 和 文件扩展名
            .compile(&["protos/voting.proto"], &["protos"])?;
        Ok(())
    }
    ```
- 3、cargo.toml  文件修改
    ```rust
    [dependencies]
    prost = "0.11.0"
    tokio = { version = "1.21.0", features = ["macros", "rt-multi-thread"] }
    tonic = "0.8.1"

    [build-dependencies]
    tonic-build = "0.8.0"

