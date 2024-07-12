# nmap-did-what

**nmap-did-what** 是一个Grafana Docker容器和一个Python脚本，用于解析Nmap XML输出到SQLite数据库。SQLite数据库在Grafana中用作数据源，以仪表板的形式查看Nmap扫描的详细信息。

完整的教程在这里 - [使用Grafana的Nmap仪表板](https://hackertarget.com/nmap-dashboard-with-grafana/)

![Grafana仪表板](https://hackertarget.com/images/nmap-grafana-dashboard.webp)

## 概览

该项目由两个主要部分组成：

1. 一个Python脚本，用于解析Nmap XML输出，并将数据存储在SQLite数据库中。
2. 一个预配置了仪表板的Grafana Docker容器，用于可视化Nmap扫描数据。

### 文件结构

- **nmap-to-sqlite.py**：一个解析Nmap XML输出并将数据存储在SQLite数据库中的Python脚本。
- **Dockerfile**：基于官方Grafana镜像创建Docker容器，包括SQLite数据源和仪表板所需的配置。
- **docker-compose.yml**：一个Docker Compose文件，设置Grafana容器，配置它使用SQLite数据库，并包括用于持久化存储和配置的卷。
- **dashboard.yml**：一个配置文件，指定Grafana的仪表板提供者设置。
- **datasource.yml**：配置Grafana使用包含Nmap扫描数据的SQLite数据库作为数据源。
- **/data/nmap_results.db**：容器中SQLite数据库的位置。

## 使用方法

要开始使用nmap-did-what，请确保您的系统上已安装了Docker和Docker Compose。

按照以下步骤部署环境：

1. **克隆仓库**

```
git clone https://github.com/hackertarget/nmap-did-what.git
```

2. **解析Nmap XML输出**

运行`nmap-to-sqlite.py`脚本，将您的Nmap XML输出解析并存储在SQLite数据库中：

```
cd nmap-did-what/data/
python nmap-to-sqlite.py nmap_output.xml
```

3. **启动Grafana容器**

使用Docker Compose启动Grafana容器：

```
cd nmap-did-what
docker-compose up -d
```

4. **访问Grafana**

容器启动并运行后，通过您的网络浏览器访问Grafana仪表板：

```
http://localhost:3000
```


使用默认的Grafana凭据(admin/admin)，除非在配置中更改。Nmap仪表板应该已加载了来自您的Nmap扫描的数据。

可以在数据库中查看多次扫描，并且可以使用Nmap仪表板的时间过滤器根据扫描的时间戳查看扫描信息。

## 定制

- 修改`nmap-to-sqlite.py`脚本，从Nmap XML输出中提取额外信息或更改SQLite数据库的结构。
- 定制仪表板很容易实现，只需根据您的要求调整Grafana仪表板。导出仪表板的JSON并替换默认仪表板或创建额外的仪表板。启动一个带有预构建仪表板的Grafana Docker容器是一个不错的功能。
- 自动化是可能的，因为您可以使用cron作业运行nmap，使用nmap-to-sqlite.py解析XML，更新的数据库将具有新获取的扫描信息。

## 致谢

感谢Nmap和Grafana项目提供强大的开源工具，用于网络扫描和数据可视化。
