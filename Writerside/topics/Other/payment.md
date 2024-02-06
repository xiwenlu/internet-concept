# 支付

## 微信扫码支付模式一和模式二区别？

### 扫码与生成订单的顺序
1. 模式一：用户首先扫描二维码，然后再生成订单。这意味着在用户扫描二维码时，实际要支付的金额可能尚未确定。
2. 模式二：先生成订单，然后再由用户进行扫码。在这种情况下，用户扫描的二维码所代表的金额已经是确定的。

### 二维码的性质
1. 模式一中的二维码主要是商品的二维码，用户扫描后可以得到商品的详细信息，并决定是否购买。
2. 模式二中的二维码则是订单的二维码，具有时效性。这是因为订单的金额是已经确定的，而订单的状态（如支付、取消等）可能会随时间发生变化。