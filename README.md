# Gitea Bootstrap 5 登陆页面主题

一种全新的主题设计，可将 Gitea 服务器实例的公共首页替换为基于现代 Bootstrap 5 和 Font Awesome 的界面体验。已登录用户会看到更新后的快速操作仪表板行，而访客则会看到具有显著行动号召的营销风格的英雄布局。
!（定制的 Gitea 主页登录页面的截图，展示了基于 Bootstrap 的英雄式布局、快捷操作以及页脚的优化效果）（图片名称：asset/snap1.png）

## 包含内容

- `custom/templates/home.tmpl` – 用 Bootstrap 5 设计覆盖了默认的首页模板
- `custom/templates/custom/header.tmpl` – 引入了 Bootstrap、Font Awesome、排版以及内联主题样式
- `custom/templates/custom/body_inner_pre.tmpl` – 在导航栏上方添加了一个简洁的促销横幅
- `custom/templates/custom/footer.tmpl` – 提供了一个多列的页脚，其中包含精心挑选的资源链接

## 先决条件

- Gitea 1.18 及以上版本（已针对当前的“主”模板结构进行测试）
- 在您的 Alpine Linux 主机上访问“custom/”目录（使用官方包时，默认位置为“/var/lib/gitea/custom”）

## 安装

1. **停止 Gitea（可选，但更为安全）：**

    ```sh
    sudo rc-service gitea stop
    ```

2. **复制主题文件：**

    ```sh
    sudo rsync -av custom/ /var/lib/gitea/custom/
    ```

    如果您的“APP_DATA_PATH”路径不同，请调整目标路径。

3. **修正权限：**

    ```sh
    sudo chown -R git：git /var/lib/gitea/custom
    ```

    将“git:git”替换为运行 Gitea 服务的用户/组。

4. **启动或重新启动 Gitea：**

    ```sh
    sudo rc-service gitea start
    ```

5. **清除缓存（可选但建议执行）：** 在 `app.ini` 中添加或增加 `ui.asset_version` 的值，或者（如果您使用了 CDN）清除 `$GITEA_CUSTOM/public` 相关的缓存。

## 配置提示

无需进行任何配置更改，但如果您依赖于强力缓存，可以通过设置 `ui.use_service_worker = false` 来强制加载更新后的样式。
若要调整颜色或字体样式，请编辑 `custom/templates/custom/header.tmpl` 中的 `<style>` 标签；Bootstrap 的实用类可让您快速调整布局。
该模板通过 CDN 引入了 Bootstrap 和 Font Awesome。如果您的安装处于隔离状态，请将 CDN 的 URL 替换为自托管的副本，并将其放入 `custom/public` 目录中。

## 验证清单

复制完文件后，请访问您的实例根网址：

- 登出的访客应能看到带有行动按钮、特色亮点和安装清单的英雄登录页面。
- 登录的用户应能看到带有快速操作按钮的“欢迎回来”仪表板卡片。
- 顶部导航栏应以玻璃质感的背景和圆润的菜单胶囊呈现，其上方应有公告横幅。
- 脚部应显示新的三列资源部分，之后是标准的 Gitea 脚部。

## 故障排除

- **样式未更新** – 确保 `custom/templates/custom/header.tmpl` 复制正确，并在 `app.ini` 中将 `ui.asset_version` 提升版本以使服务工作区缓存失效。
- **CDN 被阻止或 SRI 不匹配** – 如果环境会去除查询参数或修改资产，应在 `custom/public/vendor/` 目录内本地托管 Bootstrap 和 Font Awesome，并更新 `custom/templates/custom/header.tmpl` 中的链接（当提供自托管副本时，删除 `integrity` 属性）。
- **导航栏下拉菜单隐藏在内容后面** – 确保使用的是最新的 `custom/templates/custom/header.tmpl`；它为 Gitea 的下拉菜单设置了更高的 `z-index`，从而使其位于仓库卡片之上。

## 更新

要恢复到原始外观，请删除这些覆盖设置：

```sh
sudo rm -rf /var/lib/gitea/custom/templates/home.tmpl \
            /var/lib/gitea/custom/templates/custom/header.tmpl \
            /var/lib/gitea/custom/templates/custom/body_inner_pre.tmpl \
            /var/lib/gitea/custom/templates/custom/footer.tmpl
```

然后重新启动 Gitea。

快乐的主题设定！🎨
