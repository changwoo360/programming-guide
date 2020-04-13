### 限速思想介绍
在现实的生产环境中，数据传输并非存在固定的传输速率，而是表现为连续不断的突发请求。正是这种行为特征，导致服务器在面对大规模的并发请求时，很容易造成服务响应变慢，甚至系统宕机的情况。
因此，使用限流思想控制发送数据量大小、缓解服务器并发请求压力、在有限资源下保证服务正常稳定运行就变得及其重要。  

目前主流的限流思想有五种，分别是Token Bucket (令牌桶算法)、Leaky Bucket (漏桶算法)、Fixed Window (固定窗口算法)、
Sliding Log (滑动日志算法)和Sliding Window (滑动窗口算法)。本文将分别介绍这五种算法的运行原理，并使用python
程序实现它们，通过对比几种算法思想的优劣，分析各自的应用场景。









### Token Bucket (令牌桶算法)
思想
算法
python实现

```python
import time


class TokenBucket(object):
    """Simulate Token Bucket."""

    def __init__(self, rate, capacity):
        """
        Args:
            rate(int): produce token rate.
            capacity(int): The Token Bucket max capacity.
        """
        self._rate = rate
        self._capacity = capacity
        self._current_amount = 0
        self._last_request_time = int(time.time())

    def request(self, token_amount):
        """

        1. Get the number of tokens generated in time.
        2. Add new tokens to self._current_amount, compare tokens and self._capacity,
            tokens can't greater than self._capacity.
        3. If token_amount greater than tokens, discard this request.
        4. Update self._current_amount and self._last_request_time.

        Args:
            token_amount(int): Token amount in the request.

        Returns:
            Bool: True or False. If False, the request will be discard.

        """
        increment = (int(time.time()) - self._last_request_time) * self._rate

        _current_amount = min(increment + self._current_amount, self._capacity)

        if token_amount > _current_amount:
            return

        self._current_amount = _current_amount - token_amount
        self._last_request_time = int(time.time())

        return True
```

### Leaky Bucket (漏桶算法)

### Fixed Window (固定窗口算法)

### Sliding Log (滑动日志算法)

### Sliding Window (滑动窗口算法)



### 参考文章
- [令牌桶算法_百度百科](https://baike.baidu.com/item/%E4%BB%A4%E7%89%8C%E6%A1%B6%E7%AE%97%E6%B3%95/6597000?fr=aladdin)
- [采用令牌漏桶进行报文限流的方法](https://patents.google.com/patent/CN1536815A/zh)
- [常用限流方案的设计和实现](https://www.iteye.com/blog/manzhizhen-2311691)
- [高并发系统限流](https://www.iteye.com/blog/m635674608-2339587)
- [漏桶算法&令牌桶算法理解及常用的算法](https://www.jianshu.com/p/c02899c30bbd)
- [API开发中如何使用限速应对大规模访问](https://juejin.im/post/5be8da8a6fb9a049a62c188d)
- [Nginx流量拦截算法](https://blog.csdn.net/joeyon1985/article/details/78433243)

