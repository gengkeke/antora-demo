### 安装 Antora

##### 全局安装Antora CLI和站点生成器软件包

```shell
npm i -g @antora/cli@2.2 @antora/site-generator-default@2.2

npm install -g @antora/cli @antora/site-generator-default

antora -v
```

### 运行Antora生成站点
>要使用Antora生成网站，您需要一个剧本文件，该文件指向至少一个内容源存储库和一个UI捆绑包。
>Antora演示存储库被设置为Antora文档项目，因此我们现在可以将其用作您的内容源。Antora还提供了一个默认UI，供您直接使用。

##### antora-playbook.yml

```yaml
site:
  title: Antora Demo Site
  url: https://my-antora-demo-site.org
  start_page: component-b::index.adoc
content:
  sources:
  - url: https://gitlab.com/antora/demo/demo-component-a.git
    branches: master
  - url: https://gitlab.com/antora/demo/demo-component-b.git
    branches: [v2.0, v1.0]
    start_path: docs
ui:
  bundle:
    url: https://gitlab.com/antora/antora-ui-default/-/jobs/artifacts/master/raw/build/ui-bundle.zip?job=bundle-stable
    snapshot: true

```
正在使用Antora的默认UI作为网站的UI。Antora将负责将所有这些输入组合在一起以产生一个文档站点.

可以从URI或本地文件系统路径加载UI捆绑包。如果要使用自己的UI捆绑包，请按照README中有关[默认UI](https://gitlab.com/antora/antora-ui-default/blob/master/README.adoc)的说明进行操作。

>当UI值为url实际URL时，Antora会在首次运行时下载并缓存ZIP存档。
>在后续运行中，Antora（如果有）从缓存加载捆绑软件。这使Antora不必在每次生成站点时都必须下载捆绑软件。

>远程UI包
```yaml
ui:
  bundle:
    url: https://repo.example.org/path/to/ui-bundle-1.0.0.zip
```
>本地相对路径UI包
```yaml
ui:
  bundle:
    url: ./../docs-ui/build/ui-bundle.zip
```
>本地绝对路径UI包
```yaml
ui:
  bundle:
    url: /home/user/projects/docs-ui/build/ui-bundle.zip
```
>从start_path选择UI
```yaml
ui:
  bundle:
    url: /home/user/projects/docs-ui/build/ui-bundle-with-themes.zip
    start_path: dark
```
>自定义网站和UI输出目录.
>当将站点发布到默认目标目录（build/site），并且未指定UI的output_dir时，会将UI文件写入build/site/_。

>UI文件将发布到public/_/dark
```yaml
ui:
  bundle:
    url: /home/user/projects/docs-ui/build/ui-bundle-with-themes.zip
    start_path: dark
  output_dir: _/dark #UI输出目录
output:
  dir: ./public #站点输出目录
```

### 运行 Antora

```
 antora antora-playbook.yml
```
目标文件夹默认为build/site.

Antora将克隆内容存储库。每个存储库的克隆进度都显示在终端中。
```
[clone] https://gitlab.com/antora/demo/demo-component-a.git [################]
[clone] https://gitlab.com/antora/demo/demo-component-b.git [################]
```

### 缓存
Antora首次运行时，它将把通过网络获取的资源保存在本地缓存中。Antora缓存两种类型的资源：

* 克隆的git仓库
* 下载的UI包

##### 缓存的默认位置：
```
Linux：$ XDG_CACHE_HOME / antora（如果未设置，则为$ HOME / .cache / antora$XDG_CACHE_HOME）
macOS：$ HOME / Library / Caches / antora
Windows：$ APPDATA / antora / Caches
```
##### 指定缓存目录:
```yaml
runtime:
  cache_dir: ./.cache/antora
```
或者在--cache-DIR 命令行指定


##### 获取更新:
```yaml
runtime:
  fetch: true
```
> 如果要完全清除缓存，请在系统上找到Antora缓存目录并将其删除。

### 本地站点预览

> 由于Antora会生成静态站点，因此您无需将站点发布到服务器即可对其进行预览

##### 运行本地服务器
```shell
根目录：
npm i -g serve@6.5.8
指定文件夹：
serve build/site
```

```
   ┌────────────────────────────────────────────────┐
   │                                                │
   │   Serving!                                     │
   │                                                │
   │   - Local:            http://localhost:5000    │
   │   - On Your Network:  http://10.0.70.58:5000   │
   │                                                │
   │   Copied local address to clipboard!           │
   │                                                │
   └────────────────────────────────────────────────┘

```
