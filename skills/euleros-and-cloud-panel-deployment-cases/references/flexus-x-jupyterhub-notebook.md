# 在 Flexus X 上配置 JupyterHub 与 Notebook

## 实践环境与应用说明

原帖介绍在 Flexus X 上使用宝塔安装 Jupyter Notebook/JupyterHub，供机器学习学习使用。Jupyter Notebook 被描述为基于浏览器、以单元格运行代码并支持文本、图表和多种语言的交互式环境。

原帖称宝塔安装时可选择端口、密码和存储目录；若服务器已安装 Docker，宝塔会优先使用 Docker，否则使用本地 shell 脚本。安装后需开启外网访问，并修改安装数据目录中的 `jupyterhub_config.py`。这些为原帖操作说明，未提供具体安装界面版本。

## 宝塔安装命令片段

原帖命令排版为：

```sh
url=查看链接 [ -f /usr/bin/curl ];then curl -sSO $url;else wget -O install_lts.sh $url;fi;bash install_lts.sh ed8484bec
```

其中 URL 被正文替换为“查看链接”，且 `url=` 后缺少可见的 URL 和条件语句起始部分。该片段存在排版/采集缺损，不能据此猜补为可执行安装命令。外链上下文给出的地址为 `https://download.bt.cn/install/install_lts.sh;if`，它包含疑似命令拼接残片，不能当作已确认的脚本直链。

## JupyterHub 配置

原帖展示的配置片段如下，保留注释状态：

```python
c = get_config()

# 管理员用户列表
c.Authenticator.allowed_users = {'jupyterhub'}
c.Authenticator.admin_users = {'jupyterhub'}

#c.Authenticator.admin_users = {'root'}  # 管理员用户
#c.DummyAuthenticator.password = "123"  # 初始密码设置

# 使用 PAMAuthenticator 作为身份验证类
c.JupyterHub.authenticator_class = 'jupyterhub.auth.PAMAuthenticator'

# 允许自动创建系统用户
c.LocalAuthenticator.create_system_users = True

# 设置每个用户的 book类型 和 工作目录（创建.ipynb文件自动保存的地方）
c.Spawner.notebook_dir = '~'
c.Spawner.default_url = '/lab'
c.Spawner.args = ['--allow-root']
```

配置允许用户集合与管理员集合均为 `jupyterhub`，使用 PAMAuthenticator，并设置允许创建系统用户。`root` 管理员和 DummyAuthenticator 密码两行都被注释。原帖还设置 notebook 目录为 `~`、默认 URL 为 `/lab`，并将 `--allow-root` 传给 Spawner。原帖要求重启应用后访问 `http://ip+8000`，其中 8000 是安装时配置的端口；实际部署应以所选端口为准。

## 安装依赖与 K-Means 示例

原帖在 Notebook 代码单元中执行：

```python
!pip install numpy matplotlib scikit-learn joblib
```

随后构造两类一维压力样本（正常均值约 50、堵塞均值约 30，标准差均为 10），用 K-Means 分成两个簇，绘图、保存模型并对新样本预测：

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
import joblib

def generate_data(n_samples=100):
    np.random.seed(0)
    pressure_normal = np.random.normal(loc=50, scale=10, size=(n_samples // 2, 1))
    pressure_blocked = np.random.normal(loc=30, scale=10, size=(n_samples // 2, 1))
    pressures = np.vstack((pressure_normal, pressure_blocked))
    return pressures

X = generate_data(n_samples=100)
kmeans = KMeans(n_clusters=2, random_state=0)
kmeans.fit(X)

centroids = kmeans.cluster_centers_
normal_cluster = 0 if centroids[0] > centroids[1] else 1
blocked_cluster = 1 if normal_cluster == 0 else 0

plt.scatter(X, np.zeros_like(X), c=kmeans.labels_, cmap='viridis', marker='o')
plt.scatter(kmeans.cluster_centers_, np.zeros_like(kmeans.cluster_centers_), c='red', marker='x', s=100, label='Centroids')
plt.xlabel('Pressure')
plt.title('K-Means Clustering of Pipe Pressure Data')
plt.legend()
plt.show()

joblib.dump(kmeans, 'kmeans_model.pkl')
print("模型已保存到 'kmeans_model.pkl'")

loaded_model = joblib.load('kmeans_model.pkl')
new_data = np.array([[45], [55], [25], [35]])
predictions = loaded_model.predict(new_data)
for i, pressure in enumerate(new_data):
    status = "正常" if predictions[i] == normal_cluster else "堵塞"
    print(f"压力: {pressure[0]} 预测结果: {status}")
```

这是教学用合成数据示例。类别含义通过两个聚类中心的大小映射；它不包含真实管道数据、校准过程或准确率评估。原帖记录将模型保存为当前工作目录下的 `kmeans_model.pkl`。

## 资源与证据边界

- 外链上下文中的宝塔地址：https://download.bt.cn/install/install_lts.sh;if （原文链接包含 `;if`，疑似命令残片，不能确认其为有效下载 URL。）

原帖没有提供可恢复的宝塔完整安装命令 URL。JupyterHub 配置和示例代码来自正文；没有独立运行验证。图片共 5 张未读取。

来源：[61930568](https://www.coolapk.com/feed/61930568)（图片与评论未读，未在本库实机复现）。

