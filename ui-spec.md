# 玩偶城 — UI 设计规格文档

> 本文档为 AI 可读的完整 UI 设计规格，用于精确还原"玩偶城"网站。
> 技术栈：HTML + CSS + Vanilla JavaScript，单文件架构，移动端优先。

---

## 一、项目概述

| 属性 | 值 |
|------|-----|
| 项目名称 | 玩偶城 |
| 产品类型 | 玩偶电商平台（B2C） |
| 目标用户 | 女性用户 |
| 设计风格 | 微信小程序风格，移动端优先 |
| 主色调 | 暖粉色系（#FF6B81） |
| 页面数量 | 5 个核心页面 + 4 个弹窗 |
| 架构模式 | 双系统（客户平台 + 商家平台），单 HTML 文件 |
| 最大宽度 | 430px（居中显示） |

---

## 二、设计系统（Design Tokens）

### 2.1 色彩系统

```css
:root {
  /* 主色 - 暖粉色 */
  --pink-primary: #FF6B81;      /* 导航栏、按钮、标签、图标高亮 */
  --pink-light: #FF8E9E;        /* 渐变终止色、按钮悬停 */
  --pink-pale: #FFB6C1;         /* 次级背景、进度条 */
  --pink-mist: #FFE4E1;         /* 卡片底色、装饰区域 */
  --pink-bg: #FFF0F3;           /* 页面大背景 */
  --pink-ultra-light: #FFF8FA;  /* 输入框背景 */

  /* 点缀色 */
  --coral: #FF7F50;             /* 珊瑚色，限时标签 */
  --orange-warm: #FF9933;       /* 暖橙，倒计时 */

  /* 功能色 */
  --red-price: #FF4D4F;         /* 价格、紧急提示 */
  --green-success: #52C41A;     /* 成功、上架状态 */

  /* 中性色 */
  --white: #FFFFFF;
  --gray-bg: #F5F5F5;
  --text-primary: #333333;      /* 标题、正文 */
  --text-secondary: #666666;    /* 描述文字 */
  --text-weak: #999999;         /* 提示、占位符 */
  --border-light: #F0F0F0;      /* 分割线、边框 */
}
```

### 2.2 渐变色

| 用途 | CSS 值 |
|------|--------|
| 导航栏/顶部栏 | `linear-gradient(135deg, #FF6B81 0%, #FF8E9E 100%)` |
| 启动页背景 | `linear-gradient(160deg, #FFF0F3 0%, #FFE4E1 30%, #FFD1DC 60%, #FFB6C1 100%)` |
| 个人中心头部 | `linear-gradient(135deg, #FF6B81 0%, #FECFEF 100%)` |
| 购买按钮 | `linear-gradient(135deg, #FF6B81 0%, #FF8E9E 100%)` |
| 热卖标签 | `linear-gradient(135deg, #FF6B81, #FF7F50)` |
| 页面背景过渡 | `linear-gradient(180deg, #FFF0F3 0%, #FFFFFF 100%)` |

### 2.3 阴影

| 名称 | CSS 值 | 用途 |
|------|--------|------|
| shadow-card | `0 2px 12px rgba(255, 107, 129, 0.08)` | 卡片 |
| shadow-float | `0 4px 20px rgba(255, 107, 129, 0.15)` | 浮动按钮、弹窗 |
| shadow-pink | `0 8px 32px rgba(255, 107, 129, 0.3)` | Logo |

### 2.4 圆角

| 名称 | 值 | 用途 |
|------|-----|------|
| radius-sm | 8px | 小元素 |
| radius-md | 12px | 卡片、输入框 |
| radius-lg | 16px | 大卡片 |
| radius-xl | 20px | 弹窗、启动卡片 |
| radius-full | 999px | 按钮、标签、头像 |

### 2.5 字体

```css
font-family: -apple-system, BlinkMacSystemFont, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
```

字号规范：

| 用途 | 字号 | 字重 |
|------|------|------|
| 品牌名（启动页） | 28px | 800 |
| 页面标题 | 17px | 600 |
| 区块标题 | 16px | 700 |
| 商品名称 | 13px | 400 |
| 正文/描述 | 13px | 400 |
| 价格 | 18px | 700 |
| 原价 | 11px | 400（删除线） |
| 辅助文字 | 12px | 400 |
| 小标签 | 10-11px | 600 |
| 极小文字 | 10px | 400 |

### 2.6 动画

| 名称 | CSS | 用途 |
|------|-----|------|
| pageIn | `opacity 0→1, translateY 8px→0, 0.3s ease` | 页面切换 |
| fadeIn | `opacity 0→1, 0.2s ease` | 弹窗遮罩 |
| slideUp | `translateY 100%→0, 0.3s ease` | 底部弹窗 |
| modalPop | `opacity 0→1, scale 0.85→1, 0.3s cubic-bezier(0.34,1.56,0.64,1)` | 居中弹窗 |
| launchFadeIn | `opacity 0→1, translateY 20px→0, 0.8s ease` | 启动页品牌区 |
| logoFloat | `translateY 0→-8px→0, 3s ease-in-out infinite` | Logo 浮动 |
| cardSlideUp | `opacity 0→1, translateY 30px→0, 0.6s ease` | 启动页卡片 |
| floatDeco | `translateY 0→-20px, scale 1→1.05, 5-8s infinite` | 装饰圆 |
| shake | `translateX 0→-6px→6px→0, 0.4s ease` | 错误提示抖动 |

---

## 三、页面架构

### 3.1 整体结构

```
body
├── #launchPage          ← 启动选择页（fixed，z-index:1000）
│   ├── 装饰圆 ×4
│   ├── 品牌区（Logo + 标题 + 副标题）
│   └── 选择卡片 ×2（客户平台 / 商家平台）
│
├── #inviteModal         ← 邀请码弹窗（fixed，z-index:1100）
│   └── 弹窗内容（图标 + 标题 + 输入框 + 按钮）
│
└── #app                 ← 主应用容器（max-width:430px，居中）
    ├── #page-home       ← 客户首页
    │   ├── .top-nav
    │   ├── .swiper-container（轮播图）
    │   ├── .category-bar（分类导航）
    │   ├── .section-header + .product-grid（商品列表）
    │   └── （底部 Tab 栏在 #app 外层）
    │
    ├── #page-profile    ← 个人中心
    │   ├── .top-nav
    │   ├── .profile-header（登录/注册区）
    │   ├── .order-tabs（订单状态）
    │   ├── .menu-section（功能菜单）
    │   └── .order-list（订单列表）
    │
    ├── #page-merchant   ← 商家后台
    │   ├── .merchant-nav
    │   ├── .merchant-stats（统计卡片）
    │   ├── .merchant-tabs（管理标签）
    │   ├── #merchant-products（商品管理）
    │   ├── #merchant-users（用户管理）
    │   ├── #merchant-orders（订单管理）
    │   └── .fab（添加按钮）
    │
    ├── #tabBar          ← 客户底部 Tab 栏（fixed）
    │
    ├── #loginModal      ← 登录注册弹窗
    ├── #addressModal    ← 收货地址弹窗
    ├── #phoneModal      ← 修改手机号弹窗
    └── #editProductModal ← 编辑商品弹窗
```

### 3.2 页面切换逻辑

```javascript
// 状态管理
// launchPage: 初始显示，选择后隐藏（添加 class 'hidden'）
// page-home / page-profile: 通过 class 'active' 切换显示
// page-merchant: 通过 class 'active' 切换显示
// tabBar: 客户平台显示(flex)，商家平台隐藏(none)

// 流程：
// 1. 打开页面 → 显示 launchPage
// 2. 点击"客户平台" → 隐藏 launchPage → 显示 page-home + tabBar
// 3. 点击"商家平台" → 显示 inviteModal
// 4. 输入正确邀请码 → 隐藏 launchPage + inviteModal → 显示 page-merchant，隐藏 tabBar
// 5. 商家后台"切换客户" → 隐藏 page-merchant → 显示 tabBar + page-home

// 有效邀请码（演示）：WDC888, WD2026, MERCHANT
// 邀请码不区分大小写，最大长度 8 位
```

---

## 四、页面详细规格

### 4.1 启动选择页（#launchPage）

**定位**：`position: fixed; inset: 0; z-index: 1000;`
**背景**：`linear-gradient(160deg, #FFF0F3 0%, #FFE4E1 30%, #FFD1DC 60%, #FFB6C1 100%)`
**布局**：flex column，居中对齐

#### 品牌区（.launch-brand）
- Logo：88×88px，圆角 24px，渐变粉色背景，font-size 42px，浮动动画
- 标题："玩偶城"，28px，font-weight 800，letter-spacing 2px
- 副标题："发现你的专属玩偶伙伴"，13px，灰色

#### 装饰圆（.launch-deco）×4
- 4 个半透明圆形，absolute 定位，浮动动画
- 尺寸：200px / 140px / 80px / 60px
- 颜色：pink-primary / coral / pink-pale / pink-light
- opacity: 0.15

#### 选择卡片（.launch-card）×2

**卡片 1 - 客户平台**：
- 图标背景：`linear-gradient(135deg, #FFF0F3, #FFE4E1)`
- 图标：🛍️（26px）
- 标题："客户平台"（17px，font-weight 700）
- 描述："浏览商品、下单购买、管理订单"（12px，灰色）
- 右箭头："›"（18px）
- 点击 → `enterCustomerPlatform()`

**卡片 2 - 商家平台**：
- 图标背景：`linear-gradient(135deg, #FFF5E6, #FFE8CC)`
- 图标：🏪（26px）
- 标题："商家平台"（17px，font-weight 700）
- 描述："管理商品、处理订单、查看数据"（12px，灰色）
- 右箭头："🔒"（18px）
- 点击 → `showInviteModal()`

**卡片通用样式**：
- 背景：白色，圆角 20px，padding 24px 20px
- 阴影：`0 4px 24px rgba(255, 107, 129, 0.1)`
- 间距：gap 16px，max-width 320px
- 入场动画：cardSlideUp，第 1 张延迟 0.2s，第 2 张延迟 0.35s
- 点击缩放：`:active { transform: scale(0.97) }`

---

### 4.2 邀请码弹窗（#inviteModal）

**定位**：`position: fixed; inset: 0; z-index: 1100;`
**遮罩**：`rgba(0,0,0,0.4)`
**弹窗**：max-width 340px，白色，圆角 20px，padding 32px 24px 28px
**动画**：modalPop（弹性缩放入场）

#### 内容结构
1. 图标：64×64px 圆形，渐变背景 `#FFF5E6 → #FFE8CC`，🔑（28px）
2. 标题："商家入驻验证"（18px，font-weight 700）
3. 描述："请输入商家邀请码以进入管理后台"（13px，灰色）
4. 输入框：
   - 宽 100%，高 52px，圆角 12px
   - 背景：#FFF8FA，聚焦时变白色
   - 边框：2px solid #F0F0F0，聚焦时 #FF6B81
   - 字号 18px，font-weight 600，text-align center，letter-spacing 8px
   - placeholder："请输入邀请码"，letter-spacing 2px
   - maxlength: 8
5. 错误提示：12px，红色 #FF4D4F，默认隐藏，触发时显示 + shake 动画
6. 按钮行（flex，gap 10px）：
   - 取消：flex 1，高 46px，背景 #F5F5F5，圆角 full，灰色文字
   - 验证进入：flex 1，高 46px，渐变粉色背景，白色文字，阴影

**交互逻辑**：
- 点击遮罩关闭弹窗
- 回车键提交
- 空输入 → 提示"请输入邀请码"
- 错误码 → 提示"邀请码错误，请重新输入" + 清空输入 + 抖动
- 正确码 → 关闭弹窗 + 关闭启动页 + 进入商家后台

---

### 4.3 客户首页（#page-home）

#### 顶部导航（.top-nav）
- 高度：52px，sticky top
- 背景：`linear-gradient(135deg, #FF6B81, #FF8E9E)`
- 文字："🎀 玩偶城"，白色，17px，font-weight 600，letter-spacing 1px
- 阴影：`0 2px 8px rgba(255, 107, 129, 0.2)`

#### 轮播图（.swiper-container）
- 高度：30vh（min 180px，max 260px）
- 宽度：100%
- 幻灯片数量：5 张（商家后台管理，上限 5）
- 自动播放：3.5 秒间隔
- 支持触摸滑动和鼠标拖拽
- 过渡：`transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94)`
- 指示器：底部居中，6px 圆点，激活态 18px 宽

**5 张幻灯片背景渐变**：
1. `linear-gradient(135deg, #FFB6C1, #FF9A9E)` — "🌸 新品上架 / 春季限定·萌系玩偶系列"
2. `linear-gradient(135deg, #FECFEF, #FFD1DC)` — "🎁 满减活动 / 全场满99减20·限时特惠"
3. `linear-gradient(135deg, #FFD1DC, #FFC0CB)` — "⭐ 人气爆款 / 累计销量10万+·好评如潮"
4. `linear-gradient(135deg, #FFE4E1, #FFDAB9)` — "🧸 会员专享 / 注册即享新人8折优惠"
5. `linear-gradient(135deg, #FFC0CB, #FFB6C1)` — "🎀 品质保证 / 精选材质·安全放心"

每张幻灯片底部有渐变遮罩 `linear-gradient(transparent, rgba(0,0,0,0.3))`，白色文字标题 + 描述。

#### 分类导航（.category-bar）
- 背景：白色
- 5 个分类，等间距排列，padding 16px 12px
- 每个分类：48×48px 圆角 14px 图标（粉色背景 #FFE4E1）+ 11px 文字
- 分类列表：🧸 毛绒玩偶 / 🐰 动物系列 / 🌸 花卉系列 / 👑 公主系列 / 🎁 新品推荐

#### 区块标题（.section-header）
- 左侧 3px 粉色竖线（渐变 #FF6B81 → #FFB6C1）
- 标题："热门推荐"，16px，font-weight 700
- 右侧："查看全部 ›"，12px，灰色

#### 商品网格（.product-grid）
- 双列网格：`grid-template-columns: 1fr 1fr`
- 间距：gap 10px，padding 0 12px 16px

**商品卡片（.product-card）**：
- 背景：白色，圆角 12px，阴影 shadow-card
- 点击缩放：`:active { transform: scale(0.97) }`

结构：
```
.product-card
├── .product-img          ← 1:1 比例，粉色背景 #FFE4E1，居中 emoji
│   └── .tag（可选）      ← 左上角标签，渐变粉色背景，白色文字
├── .product-info
│   ├── .product-name     ← 13px，最多 2 行（-webkit-line-clamp: 2），min-height 36px
│   ├── .product-price-row
│   │   ├── .product-price     ← 18px，font-weight 700，红色 #FF4D4F
│   │   │   └── .symbol "¥"   ← 12px
│   │   ├── .product-price-old ← 11px，灰色，删除线
│   │   └── .product-sales    ← 10px，灰色
│   └── .product-buy-btn  ← 全宽，高约 30px，渐变粉色，白色文字，圆角 full
```

**6 个商品数据**：

| 商品 | Emoji | 标签 | 价格 | 原价 | 销量 |
|------|-------|------|------|------|------|
| 超柔软小熊毛绒玩偶 抱枕靠垫可爱公仔 | 🧸 | 热卖 | ¥39.9 | ¥79.9 | 已售2.1万 |
| 垂耳兔毛绒公仔 柔软可拥抱 生日礼物 | 🐰 | 新品 | ¥29.9 | ¥59.9 | 已售8,600 |
| 樱花兔季节限定款 高颜值桌面摆件 | 🌸 | 无 | ¥49.9 | ¥89.9 | 已售5,200 |
| 小狐狸布偶 可爱动物系列 儿童礼物 | 🦊 | 限时 | ¥35.9 | ¥69.9 | 已售3,800 |
| 国宝熊猫玩偶 加大号抱枕 办公室午休 | 🐼 | 无 | ¥59.9 | ¥119 | 已售1.5万 |
| 独角兽梦幻系列 彩虹尾巴毛绒公仔 | 🦄 | 热卖 | ¥45.9 | ¥89.9 | 已售6,700 |

---

### 4.4 个人中心（#page-profile）

#### 顶部导航
- 同首页，标题为"我的"

#### 个人信息头部（.profile-header）
- 背景：`linear-gradient(135deg, #FF6B81 0%, #FECFEF 100%)`
- padding：30px 20px 40px
- 底部圆角过渡：伪元素 `::after`，20px 圆角，粉色背景

**未登录状态**（#loginPrompt）：
- 大头像：72×72px，圆形，半透明白色背景，👤（32px）
- 文字："登录后享受更多优惠"，14px，白色
- 按钮："登录 / 注册"，padding 8px 32px，白色背景，粉色文字，圆角 full，阴影

**已登录状态**（#loggedInUser，默认隐藏）：
- 头像：64×64px，圆形，白色背景，🧸（28px），阴影
- 昵称："小仙女"，18px，font-weight 700，白色
- 等级："会员等级：普通会员"，12px，半透明白色

#### 订单状态标签（.order-tabs）
- 背景：白色，圆角 16px，padding 16px 8px
- margin-top: -20px（上移覆盖头部底部）
- 阴影：shadow-card
- 5 个标签等间距排列

| 标签 | 图标 | 角标 |
|------|------|------|
| 待付款 | 💳 | 2 |
| 待发货 | 📦 | 1 |
| 待收货 | 🚚 | 无 |
| 待评价 | ⭐ | 3 |
| 退款 | 🔄 | 无 |

每个标签：36×36px 圆角 10px 图标（粉色背景）+ 11px 文字
角标：16px 高，红色背景，白色文字，圆角 8px

#### 功能菜单（.menu-section）
- 背景：白色，圆角 16px，margin 12px，阴影 shadow-card

| 图标 | 文字 | 右侧值 | 点击 |
|------|------|--------|------|
| 📍 | 收货地址 | 3个地址 | → addressModal |
| 📱 | 手机号 | 138****8888 | → phoneModal |
| 🎫 | 优惠券 | 5张可用（红色） | 无 |
| ❤️ | 我的收藏 | 12件 | 无 |

每项：padding 14px 16px，32×32px 圆角 8px 图标（粉色背景），底部分割线

#### 订单列表（.order-list）
- padding：0 12px
- 标题："我的订单"（.section-header 样式）

**订单卡片（.order-card）**：
- 背景：白色，圆角 12px，padding 14px，margin-bottom 10px

结构：
```
.order-card
├── .order-card-header    ← flex，space-between
│   ├── .order-no         ← "订单号：WD20260518001"，12px 灰色
│   └── .order-status     ← "待发货"，13px，粉色，font-weight 600
├── .order-card-body      ← flex，gap 10px
│   ├── .order-thumb      ← 72×72px，圆角 8px，粉色背景，emoji
│   └── .order-detail
│       ├── h4            ← 商品名，13px
│       ├── .order-spec   ← 规格，11px 灰色
│       └── .order-price  ← 价格，15px，红色，font-weight 700
└── .order-card-footer    ← flex，justify-end，gap 8px
    └── .order-btn ×2    ← 圆角 full，12px
        └── .primary      ← 渐变粉色背景，白色文字
```

**3 个订单数据**：

| 订单号 | 商品 | 规格 | 价格 | 状态 | 按钮 |
|--------|------|------|------|------|------|
| WD20260518001 | 超柔软小熊毛绒玩偶 🧸 | 粉色·大号×1 | ¥39.9 | 待发货 | 查看详情 / 提醒发货 |
| WD20260515002 | 垂耳兔毛绒公仔 🐰 | 白色·中号×2 | ¥59.8 | 已完成 | 再次购买 / 去评价 |
| WD20260510003 | 小狐狸布偶 🦊 | 橙色·小号×1 | ¥35.9 | 待收货 | 查看物流 / 确认收货 |

---

### 4.5 商家后台（#page-merchant）

#### 顶部导航（.merchant-nav）
- 高度：52px，sticky top，白色背景
- 左侧："←" 返回按钮（20px），点击 → `switchToCustomer()`
- 中间："玩偶城 · 商家后台"，17px，font-weight 600
- 右侧："切换客户"，14px，粉色，font-weight 600

#### 统计卡片（.merchant-stats）
- flex 布局，gap 10px，padding 16px 12px
- 3 个等宽卡片，白色背景，圆角 12px，阴影 shadow-card

| 数值 | 标签 |
|------|------|
| 128 | 商品总数 |
| 2,456 | 累计订单 |
| ¥38.6万 | 总销售额 |

数值：22px，font-weight 700，粉色
标签：11px，灰色

#### 管理标签（.merchant-tabs）
- flex 布局，白色背景，底部边框
- sticky top: 52px
- 3 个标签等宽，padding 12px 0，14px

| 标签 | 对应内容 ID |
|------|------------|
| 商品管理（默认激活） | #merchant-products |
| 用户管理 | #merchant-users |
| 订单管理 | #merchant-orders |

激活态：粉色文字 + font-weight 600 + 底部 24px 粉色下划线

#### 商品管理（#merchant-products）
- 搜索栏：全宽，高 40px，圆角 full，白色背景，左侧🔍图标，placeholder "搜索商品名称..."

**商品列表项（.merchant-product-item）**：
- flex 布局，白色背景，圆角 12px，padding 12px，margin 0 12px 10px，gap 12px
- 点击缩放：`:active { transform: scale(0.98) }`

结构：
```
.merchant-product-item
├── .mp-thumb        ← 64×64px，圆角 8px，粉色背景，emoji
├── .mp-info
│   ├── h4           ← 商品名，14px，溢出省略
│   ├── .mp-meta     ← "库存: X · 已售: X"，11px 灰色
│   └── .mp-price    ← 价格，15px，红色，font-weight 700
└── .mp-actions      ← flex column，gap 6px
    ├── .mp-btn.edit  ← "编辑"，粉色背景
    └── .mp-btn.on/off ← "上架中"绿色 / "已售罄"灰色
```

**6 个商品数据**：

| 商品 | Emoji | 库存 | 已售 | 价格 | 状态 |
|------|-------|------|------|------|------|
| 超柔软小熊毛绒玩偶 | 🧸 | 356 | 21,000 | ¥39.9 | 上架中 |
| 垂耳兔毛绒公仔 | 🐰 | 128 | 8,600 | ¥29.9 | 上架中 |
| 樱花兔季节限定款 | 🌸 | 0 | 5,200 | ¥49.9 | 已售罄 |
| 小狐狸布偶 | 🦊 | 89 | 3,800 | ¥35.9 | 上架中 |
| 国宝熊猫玩偶 | 🐼 | 210 | 15,000 | ¥59.9 | 上架中 |
| 独角兽梦幻系列 | 🦄 | 45 | 6,700 | ¥45.9 | 上架中 |

**悬浮添加按钮（.fab）**：
- position: fixed，bottom: 80px，right
- 50×50px，圆形，渐变粉色背景，白色 "+"，font-size 24px
- 阴影：shadow-float
- 点击 → `showModal('editProductModal')`

#### 用户管理（#merchant-users，默认隐藏）
- 搜索栏同上，placeholder "搜索用户昵称/手机号..."

**用户列表项（.user-item）**：
- flex 布局，白色背景，圆角 12px，padding 14px 12px，margin 0 12px 10px，gap 12px

结构：
```
.user-item
├── .user-avatar     ← 44×44px，圆形，粉色背景，emoji
├── .user-info
│   ├── h4           ← 昵称，14px
│   ├── .user-phone  ← 手机号（脱敏），12px 灰色
│   └── .user-address ← "📍 地址"，11px 灰色，溢出省略
└── .user-order-count ← "X单"，粉色背景圆角 full，12px
```

**5 个用户数据**：

| 昵称 | 头像 | 手机号 | 地址 | 订单数 |
|------|------|--------|------|--------|
| 小仙女 | 👧 | 138****8888 | 上海市浦东新区陆家嘴环路1000号 | 8单 |
| 甜甜圈 | 👩 | 159****6666 | 北京市朝阳区三里屯太古里 | 5单 |
| 奶茶少女 | 💁‍♀️ | 177****3333 | 广州市天河区天河路385号 | 12单 |
| 草莓酱 | 👩‍🦰 | 186****9999 | 深圳市南山区科技园南区 | 3单 |
| 小太阳 | 🧑 | 135****2222 | 杭州市西湖区文三路478号 | 1单 |

#### 订单管理（#merchant-orders，默认隐藏）
- 搜索栏同上，placeholder "搜索订单号..."
- 订单卡片结构同客户平台，但包含买家信息

**3 个订单数据**：

| 订单号 | 商品 | 买家 | 状态 | 按钮 |
|--------|------|------|------|------|
| WD20260518001 | 超柔软小熊毛绒玩偶 🧸 | 小仙女·粉色大号×1 | 待发货（橙色） | 查看详情 / 去发货 |
| WD20260517004 | 独角兽梦幻系列 🦄 | 奶茶少女·彩虹款×2 | 已完成（绿色） | 查看详情 |
| WD20260516005 | 垂耳兔毛绒公仔 🐰 | 甜甜圈·白色中号×1 | 待处理（红色） | 查看详情 / 处理退款 |

---

### 4.6 底部 Tab 栏（#tabBar）

- position: fixed，bottom: 0，max-width 430px，居中
- 高度：60px，白色背景，顶部 1px 边框
- z-index: 200
- 仅在客户平台显示，商家平台隐藏

| Tab | 图标（SVG） | 文字 |
|-----|------------|------|
| 首页（默认激活） | 房子 icon | 首页 |
| 我的 | 人形 icon | 我的 |

- 图标：24×24px，stroke 风格，stroke-width 1.8
- 文字：10px，未激活灰色 #999，激活粉色 #FF6B81 + font-weight 600
- 激活图标放大：`transform: scale(1.1)`
- role="button"，tabindex="0"（无障碍）

---

## 五、弹窗规格

### 5.1 登录注册弹窗（#loginModal）

- 底部弹出样式（modal-content）
- 标题："登录 / 注册"
- 表单字段：
  - 手机号：tel 类型，placeholder "请输入手机号"，maxlength 11
  - 验证码：text 类型，placeholder "请输入验证码"，maxlength 6
- 提交按钮："登录 / 注册"，渐变粉色
- 底部文字："首次登录将自动注册 · 查看用户协议"（"查看用户协议"为粉色链接）
- 点击提交 → `doLogin()`：隐藏弹窗，切换到已登录状态

### 5.2 收货地址弹窗（#addressModal）

- 标题："收货地址管理"
- 表单字段（预填数据）：
  - 收货人：text，value "小仙女"
  - 手机号：tel，value "13888888888"
  - 所在地区：text，value "上海市 浦东新区"
  - 详细地址：text，value "陆家嘴环路1000号"
- 提交按钮："保存地址"

### 5.3 修改手机号弹窗（#phoneModal）

- 标题："修改手机号"
- 表单字段：
  - 当前手机号：tel，value "138****8888"，disabled
  - 新手机号：tel，placeholder，maxlength 11
  - 验证码：text，placeholder，maxlength 6
- 提交按钮："确认修改"

### 5.4 编辑商品弹窗（#editProductModal）

- 底部弹出，max-height 85vh，可滚动
- 标题："编辑商品"
- 分为两个区块：

**区块 1 - 商品信息**（.edit-section-title："商品信息"）：
- 商品名称：text，value "超柔软小熊毛绒玩偶"
- 商品描述：textarea，80px 高，预填描述文字
- 商品价格：number，value "39.9"
- 库存数量：number，value "356"
- 商品分类：select（毛绒玩偶/动物系列/花卉系列/公主系列）
- 商品状态：开关切换（.switch-toggle），默认开启

**区块 2 - 商品图片**（.edit-section-title："商品图片 (最多5张)"）：
- 图片上传区：4 个已填充（emoji 🧸）+ 1 个上传按钮
- 上传槽：80×80px，虚线边框 #FFB6C1，粉色背景，"+" 图标 + "上传图片" 文字
- 已填充槽：80×80px，粉色背景 #FFE4E1，emoji 32px

- 提交按钮："保存商品"

---

## 六、交互规格

### 6.1 触摸/点击反馈

所有可点击元素必须有以下反馈：
- `:active { transform: scale(0.97) }` — 卡片类
- `:active { opacity: 0.85 }` — 按钮类
- `transition: all 0.2s` — 过渡动画

### 6.2 页面切换

- 客户页面切换：隐藏当前 `.page`（移除 `active`），显示目标 `.page`（添加 `active`）
- 切换动画：`pageIn`（0.3s fade + translateY）
- 切换后自动滚动到顶部：`window.scrollTo(0, 0)`

### 6.3 轮播图手势

- touchstart → 记录起始 X 坐标，停止自动播放
- touchmove → 跟踪
- touchend → 计算差值，>50px 则切换，重启自动播放
- 同时支持鼠标拖拽（mousedown/mousemove/mouseup）
- mouseleave → 重启自动播放

### 6.4 弹窗

- 点击遮罩层关闭弹窗
- 底部弹窗：slideUp 动画
- 居中弹窗：modalPop 动画
- 邀请码弹窗支持 Enter 键提交

---

## 七、响应式规则

```css
.app-container {
  max-width: 430px;
  margin: 0 auto;
  min-height: 100vh;
}

@media (min-width: 431px) {
  .app-container {
    box-shadow: 0 0 40px rgba(0,0,0,0.08); /* 桌面端显示阴影边框 */
  }
}
```

- 隐藏滚动条：`::-webkit-scrollbar { display: none; }`
- 禁止缩放：`maximum-scale=1.0, user-scalable=no`
- 安全区域适配：`padding-bottom: env(safe-area-inset-bottom, 0)`

---

## 八、全局 CSS Reset

```css
* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
html, body { font-family: var(--font-main); background: var(--pink-bg); color: var(--text-primary); overflow-x: hidden; -webkit-font-smoothing: antialiased; }
```

---

## 九、文件结构

单 HTML 文件，内联 CSS + JS，无外部依赖。

```
wanoucheng.html
├── <style>     ← 所有 CSS（约 2500 行）
├── <body>
│   ├── #launchPage + #inviteModal    ← 启动相关
│   └── #app                          ← 主应用
│       ├── #page-home                ← 客户首页
│       ├── #page-profile             ← 个人中心
│       ├── #page-merchant            ← 商家后台
│       ├── #tabBar                   ← 底部导航
│       └── 4 个 .modal-overlay       ← 弹窗
└── <script>    ← 所有 JS（约 300 行）
```

---

## 十、验收标准

1. ✅ 打开页面首先显示启动选择页（全屏粉色渐变 + 品牌 + 双入口卡片）
2. ✅ 点击"客户平台"直接进入商城首页，底部 Tab 栏只有"首页"和"我的"
3. ✅ 点击"商家平台"弹出邀请码弹窗，输入错误码抖动提示，输入正确码进入商家后台
4. ✅ 客户首页：轮播图 30vh 自动轮播 + 分类导航 + 双列商品网格
5. ✅ 个人中心：上部 40% 登录区 + 订单状态 + 功能菜单 + 订单列表
6. ✅ 商家后台：统计卡片 + 三栏管理（商品/用户/订单）+ 悬浮添加按钮
7. ✅ 所有弹窗可正常打开/关闭，表单可交互
8. ✅ 整体暖粉色主色调，面向女性用户，移动端优先
9. ✅ 最大宽度 430px 居中显示
10. ✅ 无外部依赖，单文件可运行
