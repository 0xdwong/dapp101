---
sidebar_position: 3
---

# Foundry 测试快速入门



##  使用 sol 测试 sol

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import {Test, console} from "forge-std/Test.sol";
import {Counter} from "../src/Counter.sol";

contract CounterTest is Test {
    Counter public counter;

    function setUp() public {
        counter = new Counter();
        counter.setNumber(0);
    }

    function test_Increment() public { 
        counter.increment();
        assertEq(counter.number(), 1);
    }

    function testFuzz_SetNumber(uint256 x) public {
        counter.setNumber(x);
        assertEq(counter.number(), x);
    }
}
```

使用命令快速测试：

```sh
forge test
forge test -match-test test_Increment
```

继承自 `Test`合约的每个以 `test` 开头的 `public` 方法将被执行，并且在每个测试方法执行前都会调用 `setUp()` 初始化测试环境。

## 测试技巧


### 创建测试钱包地址

在测试时，我们需要模拟多个用户钱包身份来参与测试，比如在存款合约中需要测试不同用户存款，此时只需要在 Test 合约中使用 `makeAddr()` 来创建。

```solidity
address alice = address(0x04855890416eba63cACB213f860e5D70Ab3F6870);
address bob = makeAddr("bob");
address eva = makeAddr("任意字符");
```

### 改变 msg.sender

当我们希望使用 `alice` 的钱包和合约交互时，可以在调用合约方法前，通过 `vm.prank(alice)` 的方式设置，如：

```solidity
function test_Increment() public {
    address alice= address(0x04855890416eba63cACB213f860e5D70Ab3F6870);
    vm.prank(alice);
    counter.inpcrement();
    assertEq(counter.number(), 1);
}
```

此时，当执行 `counter.increment()` 时，在 `counter` 的 `increment` 中看到的是 `msg.sender` 将是 alice 而不再是 Test 合约。

如果希望后面所有执行的 call 都使用 `alice` 身份执行，可以使用 `vm.startPrank(alice)` 设置，如：

```solidity
function test_Increment() public {
    address alice= address(0x04855890416eba63cACB213f860e5D70Ab3F6870);
    vm.startPrank(alice);
    
    counter.increment();
    counter.increment();
    counter.increment();
    counter.increment();

    vm.stopPrank();
    assertEq(counter.number(), 1);
}
```

但在使用 `startPrank` 时，需要在使用 `vm.stopPrank()` 来取消 alice 执行身份。


### 给测试钱包存入 ETH

新创建的测试钱包中并没有 ETH，如何给这些测试钱包打入  ETH 呢？ 在测试合约中使用 `deal(alice, 1000 ether)` 来存入 1000 ether 给 alice。

```solidity
function test_Some() public {
    address alice = makeAddr("alice");
    deal(alice, 1000 ether);
}
```

### 断言合约执行错误

在测试合约中，经常需要测试执行合约是否需要符合某种错误，使用 [expect-revert](https://book.getfoundry.sh/cheatcodes/expect-revert) 即可。

```solidity
vm.expectRevert("Owner can't buy");
mkt.buyNFT(address(nft), tokenId, address(token));
```

比如，上面代码是希望在执行 buyNFT 时期望 buyNFT 报错，revert 的错误信息期望是 “Owner can't buy”。


```solidity
function testMultipleExpectReverts() public {
    vm.expectRevert("INVALID_AMOUNT");
    vault.send(user, 0);

    vm.expectRevert("INVALID_ADDRESS");
    vault.send(address(0), 200);
}

```

### 测试 error 类型的Revert

在 Solidity 0.8.4 版本中支持自定义 `error` 类型，如：

以前：

```solidity
if (fromBalance < value) {
    revert("insufficient balance");
}
```

```solidity
if (fromBalance < value) {
    revert ERC20InsufficientBalance(from, fromBalance, value);
}
```

如何在Test合约中对 error 类型测试呢？ 
首选你需要知道 error 类型是如何编码的，实际上在 revert 抛出错误信息时，error 将被编码成如下等价代码：

```solidity
bytes memory err=abi.encodeWithSelector(ERC20InsufficientBalance.selector,from, fromBalance, value);
revert(err);
```

因此，在 Test 合约中，同样可以使用该方式检查 error :

```solidity
error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);

function testNeedApprove() public {
    address bob = makeAddr("bob");
    uint256 amount = 1000; 
    vm.expectRevert(abi.encodeWithSelector(ERC20InsufficientBalance.selector, address(bob), 0, amount));
    vm.prank(bob);
    usdt.transfer(alice,amount);
}
```

或者

```solidity
function testNeedApprove() public {
    address bob = makeAddr("bob");
    uint256 amount = 1000; 
    vm.expectRevert(
        abi.encodeWithSignature("ERC20InsufficientBalance(address,uint256,uint256)", address(bob), 0, price)
    );
    vm.prank(bob);
    usdt.transfer(alice,amount);
}
```


### 断言合约返回值是否符合预期

和场景的测试框架一样，使用 forge 测试时，一样提供了丰富的断言API，如下是断言两个值是否相等。

```solidity
assertEq(nft.ownerOf(tokenId), alice, "expect nft ower is alice");
```

上方是在测试方法中断言 NFT 的持有人是否是 alice，如果不是则测试失败，打印的提示信息是 “expect nft ower is alice”.




### 断言合约事件

如何测试合约执行是否有出现符合预期的合约Event记录呢？ 使用 [expect-emit](https://book.getfoundry.sh/cheatcodes/expect-emit)


API 有：

```solidty
function expectEmit() external; 
```

```solidty
function expectEmit(
    bool checkTopic1,
    bool checkTopic2,
    bool checkTopic3,
    bool checkData
) external;
```

```solidty
function expectEmit(
    bool checkTopic1,
    bool checkTopic2,
    bool checkTopic3,
    bool checkData,
    address emitter
) external;

```

断言在下一次调用期间发出特定日志。

使用步骤:

1. 调用作弊代码 `expectEmit` ，指定是否应该检查第一个、第二个或第三个 Topic，以及日志 Data 数据! 注意, expectEmit() 表示全部检查，Topic0 始终被检查。 
2. emit 一个我们期望在下一次 call 期间看到的事件。
3. 调用合约方法。

**例子**

```solidty
function testERC20EmitsBatchTransfer() public {
    // We declare multiple expected transfer events
    for (uint256 i = 0; i < users.length; i++) { 
        // topic0 (always checked), topic1 (true), topic2 (true), NOT topic3 (false), and data (true).
        vm.expectEmit(true, true, false, true);
        emit Transfer(address(this), users[i], 10);
    }

    // 期望出现 `BatchTransfer(uint256 numberOfTransfers)` 事件.
    vm.expectEmit(false, false, false, true);
    emit BatchTransfer(users.length);

    // We perform the call.
    myToken.batchTransfer(users, 10);
}
```