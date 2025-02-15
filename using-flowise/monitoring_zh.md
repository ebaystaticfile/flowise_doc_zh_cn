# 监控

Flowise 原生支持 Prometheus 与 Grafana 和 OpenTelemetry 集成。但是，目前仅追踪高层级指标，例如 API 请求数、流程/预测数量等。指标计数器的完整列表，请参考 [此处](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/src/Interface.Metrics.ts#L13)。如需节点级别的详细可观测性，我们推荐使用 [分析](analytic_zh.md)。

## Prometheus

[Prometheus](https://prometheus.io/) 是一个开源的监控和告警解决方案。

在设置 Prometheus 之前，请在 Flowise 中配置以下环境变量：

代码块 0属性
```
ENABLE_METRICS=true
METRICS_PROVIDER=prometheus
METRICS_INCLUDE_NODE_METRICS=true
```
代码块 1

Prometheus 安装完成后，使用配置文件运行它。Flowise 提供了一个默认配置文件，可以在这里找到 [此处](https://github.com/FlowiseAI/Flowise/blob/main/metrics/prometheus/prometheus.config.yml)。

请确保 Flowise 实例也在运行。您可以打开浏览器并访问 9090 端口。在仪表盘上，您应该能够看到指标端点 - `/api/v1/metrics` 现在已启用。

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

默认情况下，Prometheus 可以从 `/api/v1/metrics` 拉取指标。

<figure><img src="../.gitbook/assets/image (177).png" alt="" width="563"><figcaption></figcaption></figure>

## Grafana

Prometheus 收集丰富的指标并提供强大的查询语言；Grafana 将指标转换为有意义的可视化图表。

Grafana 的安装方式多种多样。请参考 [指南](https://grafana.com/docs/grafana/latest/setup-grafana/installation/)。

Grafana 默认情况下将暴露 9091 端口：

<figure><img src="../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

在左侧边栏中，点击“添加新连接”，然后选择 Prometheus：

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

由于我们的 Prometheus 运行在 9090 端口：

<figure><img src="../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

滚动到底部并测试连接：

<figure><img src="../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

请记下工具栏中显示的数据源 ID，创建仪表盘时需要用到它：

<figure><img src="../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

连接成功添加后，我们可以开始添加仪表盘。在左侧边栏中，点击“仪表盘”，然后点击“创建仪表盘”。

Flowise 提供了 2 个模板仪表盘：

* [grafana.dashboard.app.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.app.json.txt)：API 指标，例如聊天流程/代理流程数量、预测计数、工具、助手、更新的向量等。
* [grafana.dashboard.server.json.txt](https://github.com/FlowiseAI/Flowise/blob/main/metrics/grafana/grafana.dashboard.server.json.txt)：Flowise Node.js 实例的指标，例如堆内存、CPU、RAM 使用率等。

如果您使用的是上面的模板，请将所有 `cds4j1ybfuhogb` 替换为您之前创建并保存的数据源 ID。

<figure><img src="../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

您也可以选择先导入，然后稍后编辑 JSON：

<figure><img src="../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

现在，尝试在 Flowise 上执行一些操作，您应该能够看到显示的指标：

<figure><img src="../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

## OpenTelemetry

[OpenTelemetry](https://opentelemetry.io/) 是一个用于创建和管理遥测数据的开源框架。要启用 OTel，请在 Flowise 中配置以下环境变量：

代码块 2属性
```
ENABLE_METRICS=true
METRICS_PROVIDER=open_telemetry
METRICS_INCLUDE_NODE_METRICS=true
METRICS_OPEN_TELEMETRY_METRIC_ENDPOINT=http://localhost:4318/v1/metrics
METRICS_OPEN_TELEMETRY_PROTOCOL=http # http | grpc | proto (默认为 http)
METRICS_OPEN_TELEMETRY_DEBUG=true
```

接下来，我们需要 OpenTelemetry Collector 来接收、处理和导出遥测数据。Flowise 提供了一个 [docker compose 文件](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/compose.yaml)，可用于启动 collector 容器。

```bash
cd Flowise
cd metrics && cd otel
docker compose up -d
```

Collector 将使用同一目录下的 [otel.config.yml](https://github.com/FlowiseAI/Flowise/blob/main/metrics/otel/otel.config.yml) 文件进行配置。目前仅支持 [Datadog](https://www.datadoghq.com/) 和 Prometheus，请参考 [Open Telemetry](https://opentelemetry.io/) 文档以配置其他 APM 工具，例如 Zipkin、Jaeger、New Relic、Splunk 等。

请确保替换 yml 文件中导出器的必要 API 密钥。
