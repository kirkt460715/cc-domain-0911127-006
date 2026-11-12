# 许可床位登记与护理供给核对

registry 管理机构版本及地区层级；bed_events 校验、暂存和重放床位事件；indicators 定义分子分母；audit 保存修订、对账、查询与归档证据。

机构有效期、执照上限、床位生效时间和记录可知时间分别处理。争议和隔离事件不直接混入可发布指标。演练中的机构与资产都是本地合成记录，不是行政登记事实。

## 运行

Python 3.11，使用标准库及内置 SQLite，无第三方依赖。无需安装第三方包。 在项目根目录执行，命令不访问公网。演示在临时目录内创建数据，结束后自行清理，不需要预置凭据或另启服务。

```sh
python3 bed_audit.py demo --fixture fixtures/registry.json
```

## 开发检查

测试覆盖当前产品的核心计算、边界输入、事务或版本恢复及异常路径。编译或语法检查可单独运行。

```sh
python3 -m unittest discover -s tests -v
python3 -m compileall -q registry bed_events indicators audit bed_audit.py
```

## 容器复现

先在有网络的准备阶段取得运行镜像。之后可在断网容器中复制项目到临时工作区运行，避免向源目录写入缓存。

```sh
docker build -t local/analysis-product .
docker run --rm --network none local/analysis-product
```

具体接口参数可通过主入口的帮助命令查看。数据库、归档和导出文件应存入调用者指定的工作目录，不提交到源码仓库。
