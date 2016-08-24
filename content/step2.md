# 全局CSS样式

设置全局 CSS 样式；基本的 HTML 元素均可以通过 class 设置样式并得到增强效果；还有先进的栅格系统。

- 总体
	- 文档类型
	- 移动设备优先
	- 排版和链接
	- 初始化css
	- 布局容器
- 栅格系统
- 排版方式
- 代码
- 表格
- 表单
- 按钮
- 图片
- 辅助类
- 响应式工具类

```
// Core variables and mixins
@import "custom";
@import "variables";
@import "mixins";

// Reset and dependencies
@import "normalize";
@import "print";

// Core CSS
@import "reboot";
@import "type";
@import "images";
@import "code";
@import "grid";
@import "tables";
@import "forms";
@import "buttons";
```
