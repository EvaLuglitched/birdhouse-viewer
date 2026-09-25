# 鸟屋 3D 查看器

用 GitHub Pages 托管，再用 iframe 嵌入 Squarespace。

## 文件夹里有什么

| 文件 | 作用 |
|---|---|
| `index.html` | 查看器页面 |
| `birdhouse.glb` | 压缩后的鸟屋模型，1.6 MB |
| `beeper.glb` | 压缩后的 beeper 模型，0.7 MB（切换到 Beeper 时才加载） |
| `vendor/three/` | three.js 本地副本（MIT 协议）。网站不依赖任何外部 CDN，可以长期稳定运行 |
| `.nojekyll` | 让 GitHub Pages 原样发布文件，不要删 |
| `birdhouse.glb.txt` | 只给 Claude 预览链接用，不用上传（已写进 `.gitignore`） |

## 一、用 VS Code 发布到 GitHub（只需做一次）

1. 安装 VS Code，并注册一个 GitHub 账号。
2. 在 VS Code 里选 **File → Open Folder**，打开这个 `birdhouse-viewer` 文件夹。
3. 点左侧的 **Source Control**（分叉图标），再点 **Publish to GitHub**。
   - 按提示登录 GitHub。
   - 选 **Publish to GitHub public repository**，仓库名比如 `birdhouse-viewer`。
4. 打开 GitHub 上的这个仓库，进入 **Settings → Pages**：
   - Source 选 **Deploy from a branch**。
   - Branch 选 **main**，文件夹选 **/ (root)**，然后点 Save。
5. 等一两分钟，页面顶部会显示网址：`https://你的用户名.github.io/birdhouse-viewer/`。
   用浏览器打开，能看到查看器就说明成功了。

## 二、嵌入 Squarespace

1. 打开要放查看器的页面，点 **Edit**，添加一个 **Code** 模块。
2. 粘贴下面这段代码，把网址换成第一部分得到的地址，并把 **Display Source** 关掉：

```html
<div style="position:relative;width:100%;height:80vh;min-height:520px;">
  <iframe src="https://你的用户名.github.io/birdhouse-viewer/"
          title="Smart bird feeder 3D viewer"
          loading="lazy"
          allow="fullscreen"
          style="position:absolute;inset:0;width:100%;height:100%;border:0;"></iframe>
</div>
```

3. 保存即可。以后修改都在 GitHub 这边进行，Squarespace 里这段代码不用再动。

## 三、以后怎么更新

在 VS Code 里改文件或替换 `birdhouse.glb`，然后在 **Source Control** 里填一句说明，依次点 **Commit** 和 **Sync**。一两分钟后网站会自动更新，Squarespace 页面也会同步显示新版本。

## 关于长期运行和流畅度

- **长期运行：** GitHub Pages 免费，没有到期时间，自带 HTTPS 和全球 CDN。免费额度大约是每月 100 GB 流量，模型每次加载约 1 MB，大约够 10 万次访问。
- **加载快：**
  - 模型是 meshopt 压缩的，传输约 1 MB。
  - iframe 是懒加载的，访客滚动到附近才开始加载，不拖慢你的 Squarespace 页面。
- **不卡顿：**
  - 只有在拖动、切换或调滑杆时才渲染，画面静止时几乎不占 CPU 和 GPU。
  - 渲染分辨率上限是 1.5 倍，高分屏上清晰，同时保持流畅。
- **不抢滚动：** 鼠标滚轮默认滚动页面，在查看器里点一下之后，滚轮才用来缩放模型。

## 切换 Birdhouse / Beeper

页面顶部有 **Birdhouse | Beeper** 切换按钮，每个模型有自己的一组视角按钮。Beeper 只在第一次切换时加载，之后来回切换是即时的。

如果想让嵌入的页面直接打开 Beeper，在网址后面加 `#beeper`：

```html
<iframe src="https://你的用户名.github.io/birdhouse-viewer/#beeper" ...></iframe>
```
