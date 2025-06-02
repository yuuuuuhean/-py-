## 国际贸易战略家游戏说明文档

## 游戏概述

《国际贸易战略家》是一款基于经济学原理的回合制策略游戏，玩家扮演国家经济部长，通过调整关税政策和进行贸易谈判来管理国家经济。游戏模拟了国际贸易中的核心经济关系，让玩家体验国家经济政策的制定与执行过程。

# 经济学原理应用
游戏融入了以下经济学概念：
- **关税效应**：调整关税直接影响双边贸易关系和国家收入
- **贸易协定**：建立贸易协定可带来长期经济利益
- **宏观经济指标**：GDP、失业率、贸易平衡等指标相互影响
- **经济周期**：随机事件模拟真实经济波动
- **决策延迟效应**：政策效果在后续回合显现

## 游戏玩法

### 核心机制
1. **回合制决策**：玩家有5个回合的任期制定经济政策
2. **四维经济系统**：
   - 国际关系：与4个主要贸易伙伴的关系值
   - 关税政策：对各国商品的进口税率
   - 贸易协定：与各国签订的双边协议
   - 国内经济：GDP、失业率和贸易平衡

3. **决策类型**：
   - 关税调整：提高或降低特定国家商品的关税
   - 贸易谈判：与关系良好的国家签订贸易协定
   - 回合推进：执行当前政策并观察经济变化

### 胜利条件
游戏结束后根据以下指标计算综合评分：
1. GDP增长率
2. 失业率改善程度
3. 签订的贸易协定数量
4. 国际关系平均值

评分等级：
- ≥90分：卓越领导！全球典范！
- 70-89分：成效显著！继续努力！
- 50-69分：平稳发展！仍有提升空间！
- <50分：需加强经济管理能力！

## 运行指南

### 系统要求
- Python 3.6+
- Pygame 2.0+

### 安装与运行
1. 安装Python
2. 安装依赖库：
   ```
   pip install pygame
   ```
3. 运行游戏：
   ```
   python trade_strategist.py
   ```

### 操作说明
- **主菜单**：使用鼠标点击按钮导航
- **经济决策**：
  - 关税调整：点击"关税调整"进入界面，选择国家调整税率
  - 贸易谈判：点击"贸易谈判"进入界面，选择国家进行谈判
- **游戏进度**：点击"下一回合"推进游戏
- **退出游戏**：点击窗口关闭按钮或按ESC键

## 代码结构

### 模块划分
1. **主程序模块**：游戏入口和主循环
2. **经济系统模块**：处理所有经济数据和逻辑
3. **UI组件模块**：按钮、面板等界面元素
4. **绘制模块**：处理游戏界面的渲染

### 关键代码段说明

#### 1. 经济系统核心 (`EconomySystem`类)
```python
class EconomySystem:
    def __init__(self):
        self.countries = ["美国", "欧盟", "东盟", "非洲联盟"]
        self.turn = 0
        self.max_turns = 5
        # 初始化经济数据
        self.relationships = {c: 50 for c in self.countries}
        self.tariffs = {c: 10.0 for c in self.countries}
        self.agreements = {c: False for c in self.countries}
        self.gdp = 10000.0
        self.unemployment = 5.0
        self.trade_balance = 0.0
        # 经济历史数据
        self.gdp_history = [10000.0]
        self.unemployment_history = [5.0]
        self.last_actions = []
```

#### 2. 游戏主逻辑 (`GameMain`类)
```python
class GameMain:
    def __init__(self):
        self.economy = EconomySystem()  # 经济系统实例
        self.current_page = "start_menu"  # 当前界面状态
        self.buttons = []  # 界面按钮集合
        self.generate_start_menu()  # 初始化主菜单
        
    def run(self):
        """游戏主循环"""
        while True:
            # 处理事件
            for event in pygame.event.get():
                # 退出处理
                if event.type == QUIT:
                    self.quit_game()
                
                # 按钮事件处理
                for btn in self.buttons:
                    btn.handle_event(event)
            
            # 根据当前页面渲染对应界面
            if self.current_page == "start_menu":
                self.draw_start_menu()
            elif self.current_page == "main":
                self.draw_main()
            # ...其他页面处理
            
            # 更新显示
            pygame.display.flip()
            clock.tick(30)  # 30FPS
```

#### 3. 经济决策执行
```python
def update_tariff(self, country, amount):
    """调整关税并计算影响"""
    self.tariffs[country] = max(0, self.tariffs[country] + amount)
    # 关税增加损害关系，减少改善关系
    self.relationships[country] += (-2 if amount > 0 else 1)
    self.last_actions.append(f"{country}关税{'增加' if amount > 0 else '减少'} {abs(amount)}%")

def negotiate(self, country):
    """贸易谈判逻辑"""
    if self.relationships[country] >= 60 and not self.agreements[country]:
        # 满足条件时签订协议
        self.agreements[country] = True
        self.relationships[country] += 10
    elif self.agreements[country]:
        # 取消现有协议
        self.agreements[country] = False
        self.relationships[country] -= 15
```

### 外部库使用
1. **Pygame**：
   - 创建游戏窗口和界面
   - 处理用户输入事件
   - 渲染游戏图形元素
   
2. **Random**：
   - 生成经济波动随机数
   - 创建背景装饰元素

3. **Sys**：
   - 处理程序退出

## 总结

《国际贸易战略家》通过简洁的界面和深度的经济模拟，让玩家体验国家经济政策制定的挑战。游戏结合了真实世界的经济原理，通过回合制决策系统展现了政策选择的长期影响。代码采用模块化设计，将经济逻辑与界面渲染分离，便于维护和扩展。

玩家在游戏中需要平衡短期利益与长期发展，考虑关税政策对国际关系的影响，通过战略性的贸易谈判为国家争取最大利益，最终实现国家经济的可持续发展。
