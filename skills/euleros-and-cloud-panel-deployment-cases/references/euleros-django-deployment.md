# Huawei Cloud EulerOS 上的 Django 与 uWSGI 入门部署记录

## 系统与 Nginx

原帖要求先连接服务器并执行：

```sh
lsb_release -a
```

作者识别所用镜像为 Huawei Cloud EulerOS。安装 Nginx：

```sh
dnf install nginx
```

若安装提示排除或忽略 nginx，检查 `/etc/dnf/dnf.conf` 中类似 `exclude=nginx` 的排除项，删除其中 `nginx` 并保存，再重新安装。原帖之后执行：

```sh
systemctl start nginx
systemctl status nginx
```

## uWSGI 最小 WSGI 示例

安装依赖、安装 uWSGI 并查看版本：

```sh
dnf install python3-devel gcc
pip install uwsgi
uwsgi --version
```

原帖在 `pyWorker` 目录下创建 Python 文件，示例应用为：

```python
def application(env, start_response):
    start_response('200 OK', [('Content-Type', 'text/html; charset=utf-8')])
    message = "大家好，我是若城"
    return [message.encode('utf-8')]
```

以 HTTP 方式在 8080 端口启动：

```sh
uwsgi --http :8080 --wsgi-file test.py
```

原帖说明访问公网 IP 的 8080 端口，并需在安全组入站规则开放 8080。该命令是前台运行示例，正文没有提供守护进程配置。

## Django 项目

原帖步骤：

```sh
pip install Django
python -m django startproject django_project
```

在 `django_project/settings.py` 中设置：

```python
ALLOWED_HOSTS = ["*"]
```

然后以开发服务器监听所有接口的 8080 端口：

```sh
python manage.py runserver 0.0.0.0:8080
```

原帖同样要求安全组开放 8080。`ALLOWED_HOSTS = ["*"]` 与开发服务器命令均为原帖示例，未给出生产部署配置。

原帖列出的设置片段还包括：

```python
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'
USE_I18N = True
USE_TZ = True
STATIC_URL = 'static/'
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```

并展示 `AUTH_PASSWORD_VALIDATORS` 列表包含 `UserAttributeSimilarityValidator`、`MinimumLengthValidator`、`CommonPasswordValidator`、`NumericPasswordValidator`。原帖没有给出该文件所需的完整导入上下文；`STATIC_ROOT` 使用 `os.path.join`，需确保项目设置中有相应 `os` 和 `BASE_DIR` 定义。收集静态文件的命令：

```sh
sudo python manage.py collectstatic
```

原帖后续称将继续配置 Nginx，但给定正文在此处结束，没有提供反向代理配置。

## 资源与证据边界

原帖参考文章链接： https://ruocheng.blog.csdn.net/article/details/141748572 。Django 4.2 文档链接： https://docs.djangoproject.com/en/4.2/topics/i18n/ 、https://docs.djangoproject.com/en/4.2/howto/static-files/ 、https://docs.djangoproject.com/en/4.2/ref/settings/#default-auto-field 。正文中的浏览器访问链接被采集为 `http://<`，目标地址不完整，因此不补造。

来源： https://www.coolapk.com/feed/61720968 。文章含 9 张图片，未读取图片；截图中的安装结果、镜像细节及后续 Nginx 配置可能未包含在可读正文中。
