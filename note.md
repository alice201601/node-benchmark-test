用于进行基准测试的工具叫做 AutoCannon， 用于执行 http 压力测试
# 1 安装 AutoCannon:
`npm i -g autocannon`
`npm i --save-dev autocannon`
然后运行命令：
`autocannon --help` 出现若干选项

# 2 测试并发请求
10000 个连接会 让node server 崩溃：
PS D:\yt\node-benchmark> autocannon -c 10000 -d 10 http://localhost:5000/
Running 10s test @ http://localhost:5000/
10000 connections

┌─────────┬─────────┬─────────┬─────────┬─────────┬────────────┬────────────┬──────────┐
│ Stat │ 2.5% │ 50% │ 97.5% │ 99% │ Avg │ Stdev │ Max │
├─────────┼─────────┼─────────┼─────────┼─────────┼────────────┼────────────┼──────────┤
│ Latency │ 5765 ms │ 8610 ms │ 9943 ms │ 9977 ms │ 8420.33 ms │ 1092.42 ms │ 10007 ms │
└─────────┴─────────┴─────────┴─────────┴─────────┴────────────┴────────────┴──────────┘
┌───────────┬─────┬──────┬─────────┬─────────┬─────────┬─────────┬────────┐  
│ Stat │ 1% │ 2.5% │ 50% │ 97.5% │ Avg │ Stdev │ Min │  
├───────────┼─────┼──────┼─────────┼─────────┼─────────┼─────────┼────────┤  
│ Req/Sec │ 0 │ 0 │ 451 │ 1745 │ 646.34 │ 632.95 │ 48 │  
├───────────┼─────┼──────┼─────────┼─────────┼─────────┼─────────┼────────┤  
│ Bytes/Sec │ 0 B │ 0 B │ 1.65 MB │ 6.38 MB │ 2.36 MB │ 2.31 MB │ 175 kB │  
└───────────┴─────┴──────┴─────────┴─────────┴─────────┴─────────┴────────┘

Req/Bytes counts sampled once per second.

20k requests in 22.4s, 21.2 MB read
4k errors (4k timeouts)

# 3 百万并发请求无法处理，但百万访问量已经达到 Google, Facebook 等的规模

node-benchmark> autocannon -c 1000000 -d 10 http://localhost:5000/

<--- Last few GCs --->

[19308:000001AE493DFAC0] 27025 ms: Scavenge 2014.7 (2078.7) -> 2010.9 (2080.2) MB, 13.5 / 0.0 ms (average mu = 0.713, current mu = 0.419) allocation failure
[19308:000001AE493DFAC0] 27062 ms: Scavenge 2016.0 (2080.2) -> 2012.9 (2082.2) MB, 18.0 / 0.0 ms (average mu = 0.713, current mu = 0.419) allocation failure
[19308:000001AE493DFAC0] 27194 ms: Scavenge 2019.0 (2082.2) -> 2015.5 (2099.9) MB, 110.3 / 0.0 ms (average mu = 0.713, current mu = 0.419) allocation failure

<--- JS stacktrace --->

FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory
1: 00007FF7D7357B7F v8::internal::CodeObjectRegistry::~CodeObjectRegistry+114079
2: 00007FF7D72E4546 DSA_meth_get_flags+65542
3: 00007FF7D72E53FD node::OnFatalError+301

# 4. 上述运行基准测试方法不实用，像业余人员的做法
而应该将命令放到文件中，以编程可控方式进行这种测试。
在工程里新建文件夹 `benchmarks`