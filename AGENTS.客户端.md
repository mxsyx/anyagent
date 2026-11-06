## 工作流程

- 执行代码修改任务时，使用 ub-mode SKILL 中定义的工程流程。

## 素材

- 将所有资源文件临时存放在项目根目录的 tmp 目录中。
- 将图标资源导出为 SVG 格式，并封装为 @project/components/icons 中的组件。
- 将装饰性图片以 2 倍分辨率导出为 PNG 格式，使用系统已安装的 ffmpeg 转换为 WebP 格式，并使用 UUID 命名，格式为 uuid.webp. 随后，使用本地已安装的 ossutil 将其上传至 gearx Bucket 的 tmp 目录。accessKeyID 等凭据已在配置文件中完成配置。
- 代码中的所有资源引用均使用占位符，例如 <img src="uploaded-image-url" />.

## Tailwind CSS

- 优先复用 `apps/web/app/globals.css` 中已有的样式 Token，并将具有复用价值的新 Token 定义在该文件中。
- 避免使用可由 Tailwind CSS 标准工具类替代的任意像素值，例如使用 `left-27` 而非 `left-[108px]`。
- 避免添加与全局默认样式重复的工具类，例如默认字号为 `sm`、颜色为 `dark` 时，无需显式添加 `text-sm` 或 `text-dark`。

## 接口

- 当我明确指出接口尚未完成时，应使用 Next.js API Mock 对相关接口进行模拟，路径统一以 `/api/mock` 为前缀。

## 历史兼容

- 当前项目仍处于开发阶段，无需考虑历史数据和历史逻辑的兼容性。
