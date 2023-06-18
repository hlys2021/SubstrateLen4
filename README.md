2. 在Offchain Worker中，使用Offchain Indexing特性实现从链上向Offchain Storage中写入数据
代码位置：https://github.com/hlys2021/SubstrateLen4/tree/ocw-test/pallets/oci

2.1 Cargo test 测试用例执行结果:
![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/cab3bc10-c84d-4cc4-88a5-4dfae3f3e070)

2.2 向Storage中读写数据测试结果
首先启动节点：

./target/release/node-template --dev --enable-offchain-indexing true
选择offchainIndexingModule模块 先择setOnChainData,输入数据并提交交易

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/07d375dc-70c0-4560-a89a-2fe4e5bdd971)


交易成功后，在命令行可以看到输出的LOG信息

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/bf7cc72f-7611-4500-b028-83745a15e19a)


3. 使用 js sdk 从浏览器frontend获取到前面写入Offchain Storage的数据

代码位置：https://github.com/hlys2021/SubstrateLen4/blob/ocw-test/frontendTS/main.ts

首先启动节点，然后执行 ts-node main.ts 执行TypeScript代码，反复检查LocalStorage是否为空，如果不为空则读取内容后终止 

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/c931ff16-addb-41da-b4c9-b3b465cb0f16)



打开https://polkadot.js.org/apps/#/extrinsics 使用Offchain Indexing特性实现从链上向Offchain Storage中写入数据

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/fa633774-7b0e-4a8f-b5c5-79a81ea9995e)



提交成功后，查看TypeScript执行的输出

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/6e8130b0-cb48-4066-93ae-9cdd7d9e5e83)



4 设计一个场景实例 (比如获取一个外部的价格信息)，实现从OCW中向链上发起带签名负载的不签名交易，并在Runtime中正确处理
代码位置：https://github.com/hlys2021/SubstrateLen4/tree/ocw-test/pallets/ocw-test

首先在offchain_worker方法中请求接口得到价格之后，调用send_unsigned_transaction并对内容进行签名，然后调用on-chain的pallet::call方法unsigned_extrinsic_with_signed_payload unsigned_extrinsic_with_signed_payload方法会将得到的价格写入pallet::storage变量Something 

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/57ec9261-2221-45bf-870d-98ca345c1066)



返回成功信息后，在https://polkadot.js.org/apps/#/chainstate可以验证结果 

![image](https://github.com/hlys2021/SubstrateLen4/assets/84297799/15e8d59d-c556-4ca9-9179-4ef48a3054eb)
