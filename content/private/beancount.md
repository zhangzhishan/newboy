# beancount
分摊插件: [amortize_over](https://gist.github.com/wzyboy/7dbf207c9a9d42ac2549232deb9a95d9)

```
option "insert_pythonpath" "True"
plugin "plugins.amortize_over"
```
你可以在 bean 文件所在目录建立一个 plugins 目录，然后把 amortize_over.py 放进去

说到插件，另一个我正在使用的非自带插件是 zerosum，用于自动处理两个账户之间的非实时转账问题 https://github.com/redstreet/beancount_plugins_redstreet/blob/master/zerosum/zerosum.py

plugin "plugins.zerosum" "{
  'zerosum_accounts': {
    'Equity:Transfers': ('Equity:Transfers:Matched', 7),
  },
  'flag_unmatched': True
}"
我就不用管信用卡还款去重问题了——导入借记卡和信用卡账单的时候，另一个账户全部填 Equity:Transfers 就可以。任何进出这个账户的两笔数字相同、符号相反、日期相差 7 天以内的交易，就会自动匹配上,没有匹配上的会标记为 !
这样的 zero sum accounts 可以建立多个，每个使用不同的配置。比如你可以建个 :Checking:Payable 账户，把支票写进去。等到支票 clear 的时候再把钱从里面挪出来。所有未 clear 的支票都会标红

朋友之间的借贷我也是记在 Equity 里的。比如我前段时间找朋友玩，因为要买门票什么的，我就全部先让朋友帮我付了，最后我走的时候再让朋友给我出个总账，我写个支票一次性还清。这时候朋友相当于一个信用卡，理论上应该记在 Liabilities:Payable:Alice 里。但是可能同一个朋友，下次来找我玩了呢？那我又得记 Assets:Receivables:Alice 了。于是我就把这两个账户合并成 Equity:ARAP:Alice 了

![accounts](_v_images/20190717094945054_28944.png)

sample query string:
```
1970-01-01 query "收支明细" "
SELECT
  year, month, account, sum(number) as Total, currency, payee, narration
FROM
    year >= 2011
WHERE
    account ~ 'Expenses' OR
    account ~ 'Income'
GROUP BY year, month, account, currency, payee, narration
ORDER BY year, month, currency, Account DESC
FLATTEN
"
```

https://github.com/mckelvin/beancount-boilerplate-cn/blob/dev/ledger/commodity.beancount#L12-L15
先像这样设置 price 价格源，然后用 bean-price 获取价格记录。然后fava上选converted to XXX 就可以显示汇率了
自动获取价格需要 https://github.com/mckelvin/beancount-boilerplate-cn/blob/dev/ledger/commodity.beancount#L31-L34 像这样定义 commodity