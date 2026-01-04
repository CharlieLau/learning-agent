—— 从鲁莽行动到 ReAct 范式
📖 荒岛日记：第 4 天
饥饿感像火烧一样灼烧着胃壁。靠海边的贝壳已经无法满足我们的热量消耗了，我盯上了丛林深处的那头“铁皮野猪”。
我把磨尖的木矛递给 Friday，指着远处的野猪下达了指令： “Friday，去把那头猪杀了带回来。”
按照上一章的训练，Friday 准确理解了指令，没有产生幻觉。他大吼一声：“收到！执行狩猎任务！”然后……
他赤手空拳地冲了上去。 他完全忘记了手里的矛，也忘记了野猪有獠牙，更没有观察地形。结果可想而知，如果不是我眼疾手快把他拽回来，我就得在这个荒岛上孤独终老了。
Friday 坐在地上，困惑地看着我：“任务失败。根据计算，野猪的冲撞力大于我的骨骼强度。”
我气得想砸了笔记本电脑。Friday 的问题在于，他是“直肠子”。 从“接收指令”到“执行行动”，他中间少了一个关键步骤——思考（Reasoning）。
在复杂的现实任务中，人类不是直接行动的。我们会先观察环境，制定计划，然后再动手。
• 人类逻辑： 看见猪 -> 思考（它很凶，我需要陷阱） -> 行动（挖坑）。
• Friday 逻辑： 看见猪 -> 行动（冲啊！）。
🛠️ 技术生存手册：赋予 Friday 逻辑闭环
为了吃上猪肉，我必须在沙地上重构 Friday 的底层逻辑。我要引入 Datawhale 教程中提到的核心概念：ReAct 范式。
ReAct 是 Reasoning（推理）和 Acting（行动）的组合。它要求智能体在行动之前，必须先在大脑里生成一段“内心独白”。
我在沙地上画出了新的架构图：
❌ 旧模式 (Direct IO): User Input -> LLM -> Action (这就是刚才送死的原因)
✅ 新模式 (ReAct Loop): 这是一个循环，直到任务完成：
1. Observation（观察）： 现在的环境是什么样的？
2. Thought（思考）： 基于观察，我下一步该做什么？（这是关键！）
3. Action（行动）： 调用工具（比如拿矛、挖坑）。
4. Result（结果）： 行动后环境发生了什么变化？-> 回到第 1 步。
💻 开发者日志：植入“心法”
我打开 IDE，我知道单纯的 Prompt 已经不够用了，我需要写一段 Python 代码来强行插入这个思考循环。
# 简单的 ReAct Agent 伪代码实现

class ReActAgent:
    def __init__(self, llm, tools):
        self.llm = llm
        self.tools = tools # 比如 ['木矛', '陷阱', '火种']
        
    def solve(self, task):
        history = ""
        while True:
            # 1. 观察 (Observe)
            observation = self.look_around()
            
            # 2. 思考 (Think) - 强迫 LLM 生成推理过程
            # 这里的 Prompt 必须要求 Friday 输出 "Thought: ..."
            prompt = f"{history}\n任务: {task}\n请按照 Thought -> Action 的格式思考下一步。"
            response = self.llm.generate(prompt)
            
            # 3. 解析行动 (Act)
            if "Action:" in response:
                action = parse_action(response)
                result = action.execute()
                history += f"\nObservation: {result}"
            
            # 4. 判断是否完成
            if "Final Answer:" in response:
                return response
写完代码，我深吸一口气，按下了回车键。
“Friday，系统重启。加载模块：ReAct_V1.0。”
我再次指向那头野猪：“Friday，现在的任务是：捕猎野猪。”
这一次，Friday 没有直接冲出去。他站在原地，甚至闭上了眼睛，像是一个入定的老僧。他的嘴里开始喃喃自语（这正是 Chain of Thought 思维链的外化）：
Thought 1: 目标是野猪。野猪具有攻击性，直接接近太危险。 Observation: 我手里有木矛，前方 10 米处有一片茂密的灌木丛，适合隐蔽。 Thought 2: 我应该先潜伏到灌木丛后，利用地形优势发起突袭。 Action: Move_to("灌木丛")
Friday 真的动了！但他不是冲向野猪，而是猫着腰，悄无声息地钻进了灌木丛。
Observation: 已到达灌木丛。野猪正在低头吃草，背对着我。 Thought 3: 现在是最佳时机。我需要用木矛投掷它的腿部，限制其移动。 Action: Use_Tool("Throw_Spear", target="Boar_Leg")
“嗖——！” 木矛破空而出。
虽然准头稍微差了一点（这涉及到第 12 章的评估指标问题），但 Friday 终于学会了像一个猎人一样思考，而不是像一枚炮弹一样发射。
看着惊慌逃窜的野猪，我虽然还是没吃上肉，但我知道我们活下来了。 Friday 不再是一个只会聊天的 Chatbot，他开始有了解决问题的自主性（Agency）。
但随着任务越来越复杂，我发现 Friday 的工具箱里只有几根烂木头。 下一章，我要利用那个被海浪冲上来的现代集装箱——那是传说中的“低代码神器”，我要给 Friday 装备上真正的现代化武器。
