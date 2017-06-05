# Nimiq 核心 API 文档
所有的核心类都定义在 `Nimiq.` 命名空间

## Nimiq

### 基本安装

```
Nimiq.init($ => {
    // $ 是 Nimiq.Core 实例
});
```

### 安装的错误回应
- `Nimiq.ERR_WAIT`: Nimiq核心实例已在相同来源的另一个窗口运行，当其他所有窗口都关闭时，将执行成功回调
- `Nimiq.ERR_UNSUPPORTED`: 不支持此浏览器
- `Nimiq.ERR_UNKNOWN`: 载入时发生未知错误

```
Nimiq.init($ => {
    // $ 是 Nimiq.Core 实例
}, code => {
    switch (code) {
        case Nimiq.ERR_WAIT:
            alert('Another Nimiq instance is already running');
            break;
        case Nimiq.ERR_UNSUPPORTED:
            alert('Browser not supported');
            break;
        default:
            alert('Nimiq initialization error');
            break;
    }
});
```

### 获取现有实例
```
Nimiq.get().then($ => {
    // $ 是 Nimiq.Core 实例
});
```

## Nimiq.Core

### 属性
- `网络`: [Nimiq.Network](#network)
- `共识`: [Nimiq.Consensus](#consensus)
- `帐户`: [Nimiq.Accounts](#accounts)
- `区块链`: [Nimiq.Blockchain](#blockchain)
- `内存池`: [Nimiq.Mempool](#mempool)
- `钱包`: [Nimiq.Wallet](#wallet)
- `挖矿机`: [Nimiq.Miner](#miner)

### 方法
没有公用方法

### 事件
没有事件


<a name="network"></a>
## Nimiq.Network
不会自动连网，需要呼叫 `$.network.connect()` 以连接网络

### 属性
- `peerCount`
- `peerCountWebSocket`
- `peerCountWebRtc`
- `bytesReceived`
- `bytesSent`

### 方法
- `connect()`
- `disconnect()`

### 事件
- `peers-changed`
- `peer-joined (peer)`
- `peer-left (peer)`

### 示例
连接网络：
```
$.network.connect()
```

监听连接：
```
$.network.on('peers-changed', () => console.log('Peers changed'));
$.network.on('peer-joined', peer => console.log(`Peer ${peer} joined`));
$.network.on('peer-left', peer => console.log(`Peer ${peer} left`));
```


<a name="consensus"></a>
## Nimiq.Consensus

### 属性
- `established`

### 方法
没有公用方法

### 事件
- `syncing (targetHeight)`
- `established`
- `lost`

### 示例
监听 `established` 事件:
```
$.consensus.on('established', () => console.log('consensus established!'))
```


<a name="accounts"></a>
## Nimiq.Accounts

### 属性
没有公用属性

### 方法
- `getBalance(address)`
- `commitBlock(block)`
- `revertBlock(block)`
- `async hash()`

### 事件
- `<<base64(address)>> (balance, address)` 当地址余额改变

### 示例
查询帐户余额：
```
$.accounts.getBalance(<<address>>).then(balance => {
    console.log(balance.value)
    console.log(balance.nonce)
})
```
监听帐户余额改变：
```
$.accounts.on('a09rjiARiVYh2zJS0/1pYKZg4/A=').then(balance => {
    console.log(balance)
})
```


<a name="blockchain"></a>
## Nimiq.Blockchain

### 属性
- `head`
- `headHash`
- `totalWork`
- `height`
- `path`
- `busy`

### 方法
- `pushBlock(block)`
- `getBlock(hash)`
- `getNextCompactTarget()`
- `async accountsHash()`

### 事件
- `head-changed`
- `ready`

### 示例
显示区块链同步进度
```
let targetHeight = 0;
$.consensus.on('syncing', _targetHeight => {
    targetHeight = _targetHeight;
})

$.blockchain.on('head-changed', () => {
    const height = $.blockchain.height;
    ui.setProgress(height / targetHeight);
})
```


<a name="mempool"></a>
## Nimiq.Mempool

### 属性
没有公用属性

### 方法
- `pushTransaction(transaction)`
- `getTransaction(hash)`
- `getTransactions(maxCount = 5000)`

### 事件
- `transaction-added`
- `transactions-ready`


<a name="wallet"></a>
## Nimiq.Wallet

### 属性
- `address`
- `publicKey`

### 方法
- `createTransaction(recipientAddr, value, fee, nonce)`

### 事件
没有事件

### 示例
创建一笔交易：
```
$.wallet.createTransaction(recipientAddr, value, fee, nonce).then(transaction => {
    console.log(transaction)
})
```


<a name="miner"></a>
## Nimiq.Miner
在共识已建立前不应该开始挖矿，当失去共识时停止挖矿。挖矿机不会强制执行此规则，但呼叫者应该确保此项行为。

```
// 当共识已建立或再次建立时，自动开始挖矿
$.consensus.on('established', () => $.miner.startWork());

// 当失去共识时停止挖矿，所有客户端都应该遵守！
$.consensus.on('lost', () => $.miner.stopWork());
```

### 属性
- `working`
- `address`
- `hashrate`

### 方法
- `startWork()`
- `stopWork()`

### 事件
- `start`
- `stop`
- `block-mined`
- `hashrate-changed`
