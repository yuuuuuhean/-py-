## 《国际贸易战略家》说明文档
# 一、游戏背景与设计思想
1.1 经济学原理应用
关税调节机制：玩家可通过调整关税影响国际贸易关系（+5%关税降低关系值，-5%提升关系），模拟现实中的贸易保护主义与自由贸易效应1。
双边协议系统：当国家关系≥60时可签订贸易协议（提升关系+10），协议生效后带来GDP增益，体现国际合作对经济的促进作用。
动态经济模型：GDP受关税政策、协议状态及随机事件影响，失业率呈波动变化，符合宏观经济周期特性。

1.2 核心玩法
回合制策略：5回合任期制，每回合执行关税/谈判操作后推进经济指标。
多目标平衡：需同时关注GDP增长、失业率控制、国际关系维护三大核心指标。
决策反馈系统：每次操作记录在last_actions，影响后续经济走势。
# 二、游戏运行与操作指南
2.1 环境要求
pip install pygame==2.6.0  # 安装依赖库

2.2 操作说明
按键/按钮|功能描述
开始履职	|进入主游戏界面
关税调整	|打开关税调节菜单（±5%）
贸易谈判	|开启协议签订/解除界面
下一回合 |推进游戏进程并更新经济指标
ESC键	|返回主菜单
# 三、代码结构解析
3.1 模块划分
├── Button类        # 按钮UI组件（处理绘制与事件）
├── EconomySystem   # 经济系统核心逻辑
│   ├── update_tariff()   # 关税调整算法
│   └── calculate_score() # 综合评分模型
└── GameMain        # 游戏主控模块
    ├── set_page()       # 页面状态管理
    └── draw_*()系列     # 各界面渲染逻辑

3.2 关键代码段说明
# 经济指标计算（EconomySystem类）
def calculate_score(self):
    gdp_change = (self.gdp_history[-1] - self.gdp_history) / self.gdp_history
    unemployment_change = self.unemployment_history - self.unemployment_history[-1]
    return int(100 + gdp_change*500 + unemployment_change*10 + agreements*20)
# 评分公式融合经济增长、失业改善、协议数量三要素
# 页面状态机（GameMain类）
def set_page(self, page):
    self.current_page = page
    self.buttons = []  # 动态生成当前页面的按钮
# 实现界面切换的核心逻辑
3.3 使用的外部库
Pygame：图形渲染、事件处理、音效	
random：生成随机经济波动	
sys：系统退出控制	
# 四、进阶设计说明
可视化设计
动态背景：采用COLORS中的渐变背景色组，随回合数变化（bg_index = turn % 3）
数据图表：实时绘制GDP/失业率折线图，使用pygame.draw.lines()实现
装饰元素：随机生成的几何图形增强界面动感（deco_elements列表）
