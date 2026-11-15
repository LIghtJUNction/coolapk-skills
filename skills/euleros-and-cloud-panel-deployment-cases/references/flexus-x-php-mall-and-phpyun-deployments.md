# Flexus X 上 PHP 商城（CRMEB / 分销商城）与 PHPYUN 部署配置记录

本记录整理自 4 篇基于宝塔面板 / Nginx / PHP / MySQL 的 Web 应用部署与二次开发社区记录，原帖图片与外部博客原文均未在本库核验，未实机复现。

## 1. CRMEB 多门店商城部署与 Swoole 代理 [61905338](https://www.coolapk.com/feed/61905338)

- 运行环境：Linux、PHP `7.1–7.4`、MySQL、Apache/Nginx。
- 在站点 Nginx 配置中添加反向代理规则（指向 Swoole 本地端口 `http://127.0.0.1:20199`），排除动态脚本后缀 `location ~* \.(php|jsp|cgi|asp|aspx)$`。
- 修改 `config/swoole.php` 第 10 行端口号后，在项目根目录启动 Swoole：

```bash
php think swoole
```

## 2. CRMEB 集成虎皮椒支付二次开发要点 [61905270](https://www.coolapk.com/feed/61905270)

- 参考虎皮椒官方资源：[WxApp-SDK.zip](https://www.xunhupay.com/wp-content/uploads/WxApp-SDK.zip)、[支付 API 文档](https://www.xunhupay.com/doc/api/pay.html)。
- 数据库配置表插入：向 `eb_system_config_tab` 插入支付分组（`eng_title = 'hpjpay'`），并向 `eb_system_config` 插入对应参数项。
- 涉及改造的后端代码路径：
  - `app\services\pay\OrderPayServices.php`
  - `app\services\pay\PayServices.php`
  - `app\common.php`
  - `app\api\controller\v1\PayController.php`
  - `crmeb\services\pay\storage\HpjPay.php`
  - `crmeb\services\pay\extend\hpjpay\HpjPay.php`
  - `crmeb\services\pay\extend\hpjpay\Client.php`

## 3. H5 分销商城（宝塔 + ThinkPHP 伪静态）[61918499](https://www.coolapk.com/feed/61918499)

- 调试环境要求：`PHP >= 7.2`、`MySQL >= 5.6`。
- 宝塔 LTS 安装命令：

```bash
url=https://download.bt.cn/install/install_lts.sh
if [ -f /usr/bin/curl ]; then curl -sSO $url; else wget -O install_lts.sh $url; fi
bash install_lts.sh ed8484bec
```

- 站点 Nginx 伪静态规则（ThinkPHP 风格）并通过 phpMyAdmin 导入 SQL 数据库文件：

```nginx
rewrite ^(.*)$ /index.php?s=$1 last; break;
```

## 4. PHPYUN 人才招聘系统环境与 Nginx 伪静态 [61916434](https://www.coolapk.com/feed/61916434)

- 环境要求：`PHP 7.4`、`MySQL >= 5.6`、Apache/Nginx，PHP 需安装扩展：`ioncube`、`fileinfo`、`memcache`、`redis`、`exif`、`sg11`。
- 原帖给出了主站点 `nginx.conf` 伪静态规则（节选核心路由模式）：

```nginx
rewrite ^/news.html$ /news.html last;
rewrite ^/(.*)/c_(.*)\.html$ /$1/index.php?yunurl=c_$2 last;
rewrite ^/company/list/(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*)\.html$ /company/index.php?city=$1&mun=$2&welfare=$3&hy=$4&pr=$5&rec=$6&keyword=$7&page=$8 last;
rewrite ^/company/(.*)\.html$ /company/index.php?yunurl=$1 last;
rewrite ^/ask/(.*)\.html$ /ask/index.php?yunurl=$1 last;
rewrite ^/resume/list/(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*).html?(.+)$ /resume/index.php?c=search&job=$1&city=$2&salary=$3&age=$4&all=$5&tp=$6&order=$7&page=$8&keyword=$args last;
rewrite ^/resume/([0-9]+)\.html$ /resume/index.php?c=show&id=$1 last;
rewrite ^/resume/(.*)\.html$ /resume/index.php?yunurl=$1 last;
rewrite ^/job/list/(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*)-(.*)\.html?(.+)$ /job/index.php?c=search&job=$1&city=$2&salary=$3&all=$4&tp=$5&cert=$6&order=$7&page=$8&keyword=$query_string last;
rewrite ^/job/(.*)\.html$ /job/index.php?c=comapply&id=$1 last;
rewrite ^/job/(.*)\.html$ /job/index.php?yunurl=$1 last;
rewrite ^/wap/(.*)\.html(.*)$ /wap/index.php?yunurl=$1 last;
```

（完整规则还包含 `announcement`、`once`、`tiny`、`map`、`evaluate`、`lietou`、`train`、`zph`、`zphnet`、`m_*`、`c_*`、`part`、`login`、`register`、`company-show` 等同类 `yunurl` 映射。）
