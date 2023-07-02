# 1. 为 proof of existence (poe) 模块的可调用函数 create_claim, revoke_claim, transfer_claim 添加 benchmark 用例，并且将 benchmark 运行的结果应用在可调用函数上；
代码地址：https://github.com/hlys2021/SubstrateLen4/tree/benchmarking/pallets/poe

运行效果图：
![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/a6c0b07f-20ed-4a91-aad5-c459adb55d58)



命令行：
```j
cargo build --release --features runtime-benchmarks

./target/release/node-template benchmark pallet \
--chain dev \
--execution wasm \
--wasm-execution compiled \
--pallet pallet_poe \
--extrinsic "*" \
--steps 20 \
--repeat 10 \
--json-file=raw.json \
--output ./pallets/poe/src/weights.rs \
--template .maintain/frame-weight-template.hbs
```
生成weights的文件： https://github.com/hlys2021/SubstrateLen4/tree/benchmarking/pallets/poe/src/weights.rs

# 2. 选择 node-template 或者其它节点程序，生成 Chain Spec 文件（两种格式都需要）

# 2.1 生成aura-chain-spec文件

```j

./target/release/node-template build-spec --chain Substrate_Aura > aura.json
```
生成aura.json文件位置：https://github.com/hlys2021/SubstrateLen4/blob/benchmarking/aura.json

./target/release/node-template build-spec --chain=aura.json --raw > aura-raw.json
```

# 2.2 生成babe-chain-spec文件
代码地址：https://github.com/hlys2021/SubstrateLen4/tree/consensus_babe

./target/release/node-template build-spec --chain staging > babe.json
./target/release/node-template build-spec --chain=babe.json --raw > babe-raw.json
![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/5bf965f3-1122-4d42-ba05-ba7f54b6e0c1)

consensus_aura和consensus_babe转换代码修改参考：https://github.com/kaichaosun/substrate-stencil/commit/e0a7aaf17e2e003ce80cf8062005be202c6cb017
