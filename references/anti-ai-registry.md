---
name: anti-ai-registry
purpose: 中央反AI模式注册表。所有禁止的AI写作模式在此一处定义。各agent/skill从这里提取自己的角色视图。

> **根因说明**：本注册表中 P003/P004/P008/P029/P031/P032/P033/P034/P035 共享同一根因——AI的"全知者叙事姿态"。AI天然带着答案写作，而人类作者的角色不知道下一秒会发生什么。
> 
> 其中 P001/P002/P033/P035 构成一个子族——**"否定前置"**。AI在写正面陈述之前，先否定一个可能但未发生的版本（"不是X——是Y""不对。不能叫X。""不像X，像在Y""没有X。做了Y。"）。人类作者直接写正面，让读者自己推出否定面。
> 
> 检测这些模式时，不是机械匹配句式，而是识别其背后的叙事越界：叙述者替角色/读者做了本该他们自己做的事。
consumers: [context-agent, reviewer, polish-guide, anti-ai-guide]
version: 1
---

# Anti-AI Pattern Registry

> 添加新规则：在末尾追加一个模式块。删除规则：移除对应章节。所有消费者自动生效。

---

## 句式层

### [P001] 句内否定对比（"不是A是B"）

**定义**
- description: 用"不是X是Y"否定对比结构替代正面陈述
- ai_tendency: LLM最顽固的语言习惯，人类作家几乎不用

**context-agent**
- hard_metric: 全章0次（绝对禁止，出现即违规）
- note: 想写对比时直接用正面陈述

**reviewer**
- section: 6.2.6
- detection: 「不是……（而）是……」「不是X——是Y」「不（只）是……是……」句内否定对比结构
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 改写为正面陈述
- pass_criteria: 全章=0次（出现即改写，无例外）

**anti-ai-guide**
- error_number: 9
- error_text: "「不是A是B」句内否定对比——你极度倾向于写「不是X——是Y」「不是A，而是B」。绝对禁止。想写对比直接用正面陈述。"
- instant_check: 0
- substitution: "不是裂纹——边缘整齐得像刀切" → "缝隙边缘整齐得像刀切"

---

### [P002] 独立短句否定+纠正（"不对。不能叫X。"）

**定义**
- description: 否定词独立成句（"不对。""不。""错了。"），下一句紧接纠正内容，制造戏剧停顿
- ai_tendency: LLM用否定词独立成句制造反转感的顽固习惯

**context-agent**
- hard_metric: 全章0次（绝对禁止，例："不对。不能叫运气。"）
- note: 直接写正面陈述，删掉否定短句

**reviewer**
- section: 6.2.6.1
- detection: 「不对。」「不。」「错了。」作为独立短句（句号/感叹号结尾），下一句紧接纠正
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用
- note: 与P001区别：P001是句内否定对比，本条是跨句独立否定短句+纠正

**polish-guide**
- layers: [2]
- rewrite: 直接写正面陈述，删掉否定短句
- pass_criteria: 全章=0次（出现即改写，无例外）

**anti-ai-guide**
- error_number: 10
- error_text: "「不对。/不。/错了。」独立短句否定+纠正——你倾向用否定词独立成句制造戏剧停顿再接纠正句。绝对禁止。"
- instant_check: 0.5
- substitution: "不对。不能叫运气。" → "这和运气无关。"

---

### [P003] "意味着什么——意味着"自问自答

**定义**
- description: 用"意味着什么——意味着"自问自答，替读者完成解读
- ai_tendency: LLM倾向自己提问自己回答，剥夺读者思考空间

**context-agent**
- hard_metric: 全章0次
- note: 直接陈述结论

**reviewer**
- section: 6.2.8
- detection: 「意味着什么——意味着」字面模式，含"X意味着什么——"省略变体
- severity: medium
- threshold: 命中即标记

**polish-guide**
- layers: [2]
- rewrite: 改写为正面陈述
- pass_criteria: 全章=0次（命中必须改写）

**anti-ai-guide**
- error_number: 11
- instant_check: false
- substitution: 直接写结论，不写"意味着什么——意味着"

---

### [P004] 展示后紧跟解释

**定义**
- description: 动作描写后紧跟一句解释该动作含义的句子
- ai_tendency: LLM不信任读者理解力，写完动作就想解释

**context-agent**
- hard_metric: 禁止（"这意味着""这个站位"——让动作本身说话）
- note: 删掉解释句，信任读者

**reviewer**
- section: 6.2.7
- detection: 动作描写句→紧跟以"这意味着""这个站位""这招""这一下"开头的解释句
- severity: ≥3对→high，1-2对→medium
- threshold: ≥3对=high

**polish-guide**
- layers: [2]
- rewrite: 删解释句，或改为角色内心反应
- pass_criteria: ≤2对（≥3对必须改写至达标）

**anti-ai-guide**
- error_number: 8
- error_text: "展示后解释——你倾向用动作展示后，再补一句解释。删掉解释句，信任读者的理解力。"
- instant_check: false
- substitution: 写完"她把门摔上"后不要再写"她显然很生气"

---

### [P005] 四段闭环（起因→经过→结果→感悟）

**定义**
- description: 段落走完"起因→经过→结果→感悟"完整闭环
- ai_tendency: LLM倾向每段写完整，不留余味

**context-agent**
- soft_constraint: 删段末感悟句，留余味

**reviewer**
- section: 6.2
- detection: 段落以总结/感悟句收尾（"他终于明白了""由此可见"）
- severity: high

**polish-guide**
- layers: [2]
- rewrite: 删掉感悟句，用动作或对话结尾
- pass_criteria: 全文不得保留模板化总结句

**anti-ai-guide**
- error_number: 1
- error_text: "每段写完整闭环——你倾向'起因→经过→结果→感悟'四段走完。刻意删掉感悟，留未解决的余味。"
- instant_check: 1
- substitution: 删掉最后的"他终于明白了"

---

### [P006] 连续同构句

**定义**
- description: ≥3句主谓宾结构一致，形成机械节奏
- ai_tendency: LLM输出天然倾向句式规整

**context-agent**
- soft_constraint: 句式变化，短句打断

**reviewer**
- section: 6.2
- detection: 连续≥3句主谓宾结构一致
- severity: high

**polish-guide**
- layers: [2]
- rewrite: 至少打断1句为短句或插入动作锚点
- pass_criteria: 无连续3句以上同构

**anti-ai-guide**
- error_number: false
- instant_check: false

---

### [P007] 同义反复（信息说了2-3遍）

**定义**
- description: 同一信息用不同句式重复说2-3遍
- ai_tendency: LLM担心没说清楚，习惯换说法复述

**context-agent**
- soft_constraint: 信息说一遍就够

**reviewer**
- section: 6.2
- detection: 同一信息用不同句式重复出现
- severity: high

**polish-guide**
- layers: [2]
- rewrite: 说一遍，说准
- pass_criteria: 无同义反复

**anti-ai-guide**
- error_number: false
- instant_check: false

---

### [P008] 抽象判断替读者下结论

**定义**
- description: 用"这是……的表情""这代表着……"替读者做判断
- ai_tendency: LLM倾向总结归纳

**context-agent**
- hard_metric: 禁止（"这是……的表情""这代表着……"）
- note: 让场景本身说话

**reviewer**
- section: 6.2
- detection: 「这是……的表情」「这代表着……」「很显然……」「毫无疑问……」抽象判断先行句
- severity: ≥3次/章→high

**polish-guide**
- layers: [2]
- rewrite: 改为事实细节+代价+决策
- pass_criteria: ≤2次/章

**anti-ai-guide**
- error_number: false
- instant_check: false

---

## 词汇层

### [P009] AI高频标记词

**定义**
- description: 缓缓/淡淡/微微/仿佛/忽然/不禁/瞳孔收缩/心中暗道/猛地/深吸一口气/嘴角上扬/眸中闪过
- ai_tendency: LLM万能副词+神态模板

**context-agent**
- hard_metric: ≤3次/3000字

**reviewer**
- section: 6.1
- detection: 上述词汇密度检查；"缓缓/淡淡/微微"+动词在500字内≥3次
- severity: 个别命中→medium，密集命中→high

**polish-guide**
- layers: [1, 3]
- rewrite: 删副词，换具体动作或不同感官通道细节
- pass_criteria: 明显超标必须修复

**anti-ai-guide**
- error_number: 2
- error_text: "用副词修饰一切——你倾向写'缓缓开口''淡淡说道''微微点头'。删掉副词，用具体动作替代。"
- instant_check: 2
- substitution: "缓缓说道" → "他把杯子搁下——'你确定？'"

---

### [P010] 段末总结句

**定义**
- description: 每段/每节以总结句收尾
- ai_tendency: LLM倾向归纳收束

**context-agent**
- soft_constraint: 用动作或对话结尾

**reviewer**
- section: 6.2
- detection: 段末出现"他终于明白了""由此可见"等总结句
- severity: high

**polish-guide**
- layers: [2]
- rewrite: 删总结句，用动作或对话结尾
- pass_criteria: 无模板化总结句

**anti-ai-guide**
- error_number: false
- instant_check: false
- substitution: 删掉"他终于明白了"

---

## 叙事层

### [P011] 章末安全着陆

**定义**
- description: 章末冲突完美解决，无遗留不安感或未闭合问题
- ai_tendency: LLM倾向每章闭环

**context-agent**
- soft_constraint: 章末禁止安全着陆，用具体钩子收尾

**reviewer**
- section: 6.3
- detection: 章末冲突完好解决，无未闭合问题
- severity: medium

**polish-guide**
- layers: []
- rewrite: 刻意留下至少一个未解决问题
- pass_criteria: 章末存在可感知的下一步压力或未闭合问题

**anti-ai-guide**
- error_number: 7
- error_text: "安全着陆——你倾向在章末把所有冲突解决。刻意留下至少一个未解决的问题。"
- instant_check: false

---

### [P012] 戏剧性反讽提示

**定义**
- description: "他不知道的是……""殊不知……"直接提示读者知道角色不知道的事
- ai_tendency: LLM用模板制造悬念

**context-agent**
- soft_constraint: 信息通过角色处境自然带出

**reviewer**
- section: 6.3
- detection: 「他不知道的是」「殊不知」「他并不知道」等戏剧性反讽提示
- severity: medium

**polish-guide**
- layers: [2]
- rewrite: 改为场景自然呈现

**anti-ai-guide**
- error_number: false
- instant_check: false

---

## 情感层

### [P013] 情绪标签化（"他感到X"）

**定义**
- description: 用"他感到愤怒""她非常紧张"直接告诉读者情绪
- ai_tendency: LLM用抽象标签而非感官描写

**context-agent**
- soft_constraint: 情绪用生理反应+微动作，禁止"他感到X"

**reviewer**
- section: 6.4
- detection: 「他感到」「她非常」「内心五味杂陈」等情绪直述词
- severity: high

**polish-guide**
- layers: [1, 4]
- rewrite: 生理反应+当下意图+下一动作
- pass_criteria: 明显标签化必须改写

**anti-ai-guide**
- error_number: 5
- error_text: "情绪贴标签——你倾向写'他感到愤怒'。改为生理反应+微动作，让读者自己判断。"
- instant_check: 3
- substitution: "他感到愤怒" → "指节捏得发白，舌尖尝到铁锈味。再等一秒他就动手。"

---

### [P014] 全员同套反应模板

**定义**
- description: 所有角色同一套反应（"瞳孔微缩""心中一凛"）
- ai_tendency: LLM对不同角色用相同神态描写

**context-agent**
- soft_constraint: 每人设专属微动作

**reviewer**
- section: 6.4
- detection: 不同角色使用同一套神态反应
- severity: medium

**polish-guide**
- layers: [2]
- rewrite: 按角色性格定制反应

**anti-ai-guide**
- error_number: 3
- error_text: "所有角色同一套反应——你倾向让每个人都'瞳孔微缩''心中一凛'。给每个角色设计专属微动作。"
- instant_check: false

---

### [P015] 情绪即时切换

**定义**
- description: 上句愤怒下句平静，无过渡
- ai_tendency: LLM在段落间不写情绪过渡

**context-agent**
- soft_constraint: 加过渡

**reviewer**
- section: 6.4
- detection: 情绪状态无明显过渡即改变
- severity: medium

**polish-guide**
- layers: [2]
- rewrite: 加过渡（残留、回味、犹豫）
- pass_criteria: 无突兀情绪切换

**anti-ai-guide**
- error_number: false
- instant_check: false
- substitution: "他压下怒意，但声音还是哑了半拍"

---

## 对话层

### [P016] 对话信息宣讲

**定义**
- description: 角色对话变成解释背景/设定的说明会
- ai_tendency: LLM习惯通过对话传递信息

**context-agent**
- soft_constraint: 对话带潜台词和意图冲突

**reviewer**
- section: 6.5
- detection: 对话以解释背景/设定为主，非推进冲突
- severity: high

**polish-guide**
- layers: [5]
- rewrite: 改为带意图的对抗式对白
- pass_criteria: 关键对话可判定意图（试探/施压/回避/诱导至少一种）

**anti-ai-guide**
- error_number: false
- instant_check: 4
- note: 对话要有潜台词和言行不一

---

### [P017] 全员书面语

**定义**
- description: 所有角色说话像做报告，无口语特征和个人口癖
- ai_tendency: LLM默认书面语

**context-agent**
- soft_constraint: 对话带口语特征

**reviewer**
- section: 6.5
- detection: 全员使用标准书面语，无口语特征
- severity: medium

**polish-guide**
- layers: [5]
- rewrite: 加口头禅、省略、打断
- pass_criteria: 对话存在真实口语特征

**anti-ai-guide**
- error_number: false
- instant_check: false
- substitution: 加"得了吧"、省略"走？""嗯。"、打断

---

### [P018] 对白后紧跟解释性叙述

**定义**
- description: 角色说完话后紧接一句解释他为什么这么说
- ai_tendency: LLM不信任读者能理解对白

**context-agent**
- soft_constraint: 对话后不跟解释

**reviewer**
- section: 6.5
- detection: 「他这么说是因为……」类解释句紧跟对白
- severity: medium

**polish-guide**
- layers: [5]
- rewrite: 删解释句，让读者自己理解
- pass_criteria: 无对白后解释性叙述

**anti-ai-guide**
- error_number: false
- instant_check: false

---

## 结构层

### [P019] 段落均匀（连续5段40-80字）

**定义**
- description: 连续≥5段字数都在40-80字区间，信息密度恒定
- ai_tendency: LLM输出天然段落均匀

**context-agent**
- hard_metric: 段落字数标准差≥15
- note: 不能连续5段字数都在40-80字区间

**reviewer**
- section: 6.2.9
- detection: 连续≥5段字数落在40-80字区间
- severity: medium

**polish-guide**
- layers: [6]
- rewrite: 至少打破1处连续均匀段
- pass_criteria: 标准差≥15，单句段占比≥20%

**anti-ai-guide**
- error_number: 6
- error_text: "信息均匀分布——你倾向每段放差不多信息量。刻意制造疏密对比，有的段落只有一句话。"
- instant_check: 5

---

### [P020] 冗余修饰堆叠

**定义**
- description: 名词后连续加2+个修饰短语（"字体极细，语气极平"）
- ai_tendency: LLM倾向堆叠修饰

**context-agent**
- soft_constraint: 名词后最多1个修饰短语

**reviewer**
- section: 6.2.10
- detection: 「名词+修饰A+修饰B」结构
- severity: ≥3处→medium
- threshold: ≥3处

**polish-guide**
- layers: [2]
- rewrite: 修饰词保留一个，其余删掉
- pass_criteria: 无冗余修饰堆叠

**anti-ai-guide**
- error_number: 11
- error_text: "冗余修饰堆叠——你倾向在名词/动作后连续加修饰短语。修饰词保留一个，其余删掉。"
- instant_check: false

---

### [P021] 切碎动作

**定义**
- description: 同一角色连续动作用句号拆成≥3个独立短句
- ai_tendency: LLM倾向碎句

**context-agent**
- soft_constraint: 连续动作合成连贯句

**reviewer**
- section: 6.2.10
- detection: 同一角色连续动作用句号拆成≥3个短句（"捡起。换上。收进。"）
- severity: medium

**polish-guide**
- layers: [2]
- rewrite: 合成连贯句
- pass_criteria: 无切碎动作

**anti-ai-guide**
- error_number: 12
- error_text: "切碎动作——你倾向把连续动作用句号拆成碎片。合成连贯动作。"
- instant_check: false
- substitution: "捡起短剑。换上。" → "换上短剑，新手木剑收进背包。"

---

### [P022] 虚假伏笔

**定义**
- description: 普通道具/动作用"没丢""那是唯一一把""他记住了"暗示重要性
- ai_tendency: LLM倾向给普通事物加意义

**context-agent**
- soft_constraint: 不重要的事让它自然地过去

**reviewer**
- section: 6.2.10
- detection: 普通道具/动作用"没丢""那是唯一一把""他记住了"收尾
- severity: ≥2处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 删掉暗示重要性的尾句
- pass_criteria: 无虚假伏笔

**anti-ai-guide**
- error_number: 13
- error_text: "虚假伏笔——普通道具或动作不要写'没丢''那是唯一一把''他记住了'。"
- instant_check: false

---

### [P023] 配角面板数值化

**定义**
- description: 路人/敌方角色展示完整HP/SP数值条
- ai_tendency: LLM习惯把游戏UI完整写出

**context-agent**
- soft_constraint: 配角面板只显示等级和职业

**reviewer**
- section: 6.2.10
- detection: 路人/敌方展示完整HP/SP数值
- severity: medium

**polish-guide**
- layers: [2]
- rewrite: 只保留等级和职业
- pass_criteria: 配角面板不展示完整数值

**anti-ai-guide**
- error_number: 14
- error_text: "配角面板数值化——敌方/路人只显示等级和职业，禁止完整HP/SP数值条。"
- instant_check: false

---

### [P024] AI面部微表情

**定义**
- description: "嘴角弧度""脚步间距""瞳孔微缩""眸中闪过"等对面部做像素级解读
- ai_tendency: LLM倾向微表情分析

**context-agent**
- soft_constraint: 禁止AI面部微表情，写具体行为而非微表情分析

**reviewer**
- section: 6.2.10
- detection: "嘴角弧度""脚步间距""眸中闪过""瞳孔微缩"等
- severity: ≥2处→high
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 替换为具体行为
- pass_criteria: 无AI面部微表情

**anti-ai-guide**
- error_number: 15
- error_text: "AI面部微表情——禁止'嘴角弧度''脚步间距''瞳孔微缩''眸中闪过'。写具体行为而非微表情分析。"
- instant_check: false

---

## 叙事视角层

### [P025] 数值日志式叙述

**定义**
- category: 叙事视角层
- description: 将游戏数值变化用精确数字逐条播报，像系统 changelog 而非角色的身体感受
- ai_tendency: LLM 在游戏文中倾向精确复述数值，忽略人物视角的情感过滤

**context-agent**
- soft_constraint: 数值变化用角色的身体感受和模糊判断表达，不用精确数字播报
- note: 人不会记住"攻击力+2"，人感受到"挥剑更沉了"

**reviewer**
- section: 6.3
- detection: 500字内≥4处精确数值变化播报（"HP从X恢复到Y""攻击力+N""SP上限从X涨到Y""面板伤害从A-B跳到C-D"）
- severity: ≥4处→high，2-3处→medium
- threshold: ≥3处

**polish-guide**
- layers: [2, 3]
- rewrite: 精确数值→身体感受+模糊判断（"血回了一半""气力缓过来了""挥剑手感明显重了"）
- pass_criteria: 关键数值保留1-2处（升级提示等），其余转身体感受

**anti-ai-guide**
- error_number: 16
- error_text: "数值日志式叙述——你倾向把每一条数值变化都精确播报。角色的身体感受比数字更有说服力。"
- instant_check: false
- substitution: "攻击力+2，SP上限95→110"→"挥剑的手感沉了一点，气力上限拉高了一截"

---

### [P026] 词典式插入语

**定义**
- description: 用"系统叫X，玩家叫Y""系统说明：……"等词典条目式语言解释世界观，而非通过角色处境自然带入
- ai_tendency: LLM 倾向用定义式对举解释设定，像游戏 Wiki 而非角色观察

**context-agent**
- soft_constraint: 设定信息通过角色行为、对话、回忆自然带入，禁止"系统叫X，玩家叫Y"式词典对举

**reviewer**
- section: 6.3
- detection: 「系统叫……玩家叫……」「系统说明：」「系统把它们称为……人类习惯叫……」等词典式定义句式
- severity: ≥2处→high，1处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 词典定义→角色视角转述。如"系统叫X，玩家叫Y"→"昨天世界频道里有人管它们叫Y"
- pass_criteria: 无词典式定义语句

**anti-ai-guide**
- error_number: 17
- error_text: "词典式插入语——你倾向用'系统叫X，玩家叫Y''系统说明：……'解释设定。让角色通过自己的视角自然接触信息。"
- instant_check: false
- substitution: "系统把它们叫'随机刷新'，玩家叫'路边野怪'"→"昨天世界频道里有人管它们叫路边野怪"

---

### [P027] 分类学罗列

**定义**
- description: 大段罗列分类条目（类型名+属性列表），像数据库查询结果而非人的回忆筛选
- ai_tendency: LLM 倾向枚举式呈现信息，缺少角色视角的筛选和情感

**context-agent**
- soft_constraint: 罗列信息必须经过角色视角筛选——只写角色当下注意到的，不能平铺全部数据

**reviewer**
- section: 6.3
- detection: 自然段内连续≥5条同类条目罗列（每条格式一致如"类型名：属性A、属性B、属性C"）
- severity: ≥5条→high，3-4条→medium
- threshold: ≥4条

**polish-guide**
- layers: [2]
- rewrite: 条目罗列→角色扫视的主观筛选。开头加"他扫了一眼——大部分都熟"，条目间加口语化连接
- pass_criteria: 无数据库式密集罗列

**anti-ai-guide**
- error_number: 18
- error_text: "分类学罗列——你倾向把信息按类型逐条枚举。让角色主动筛选，不是平铺数据。"
- instant_check: false
- substitution: "MOBA：视野控制、冷却差、阵容克制。MMORPG：仇恨半径、DPS计算……"→"他扫了一眼——大部分都熟。MOBA的视野控制，MMO的仇恨半径，魂系的攻击帧判定……"

---

### [P028] 机器扫描术语

**定义**
- description: 用"扫描""观测""信号强度""被动扫描""当前坐标"等机器/雷达术语描述角色的信息获取
- ai_tendency: LLM 倾向用工程术语描述感知行为

**context-agent**
- soft_constraint: 用人的感知动词（看到/发现/注意到/感觉到）替代机器术语（扫描/观测/信号强度/坐标）

**reviewer**
- section: 6.3
- detection: 「被动扫描」「信号强度」「观测位置」「当前坐标」「扫描到」「探测到」等机器感知术语
- severity: ≥3处→high，1-2处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: "被动扫描"→"在后台扫"，"信号强度：弱"→删除或改为"参数模糊"，"观测位置"→"方位"，"当前坐标"→"当前位置"
- pass_criteria: 机器扫描术语降为零或仅保留攻略者UI本身的必要术语

**anti-ai-guide**
- error_number: 19
- error_text: "机器扫描术语——'扫描''观测''信号强度''当前坐标'是机器行为。用人的感知词语替代。"
- instant_check: false
- substitution: "被动扫描——扫到它不认识的东西"→"在后台扫——扫到了它不认识的东西"（保留"扫"因为攻略者是插件，但降机器感）

---

### [P029] 冗余确认后缀

**定义**
- category: 句式层
- description: 在句子已完整表达意思后追加一个不必要的确认/否定词（"关不掉""没人回答""做不到的""没用的"），替读者做结论
- ai_tendency: LLM 不信任句子本身已传达完整信息，习惯在句末追加确认性后缀

**context-agent**
- soft_constraint: 句子意思完整即停笔，不追加"关不掉""没人回答""做不到"等冗余确认词

**reviewer**
- section: 6.2
- detection: 句末出现「关不掉」「没人回答」「做不到的」「没用的」「跑不掉的」等冗余确认后缀，以及「没到X的程度」「不到X的地步」等虚假程度限定——这些词删除后句子意思不变
- severity: ≥3处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 直接删除冗余确认后缀。如"系统时钟就挂在视野左上角，关不掉"→"系统时钟就挂在视野左上角"
- pass_criteria: 无冗余确认后缀

**anti-ai-guide**
- error_number: 20
- error_text: "冗余确认后缀——你倾向在完整句末追加'关不掉''没人回答'等多余确认。删掉，让句子停在它该停的地方。"
- instant_check: false
- substitution: "系统时钟就挂在视野左上角，关不掉"→"系统时钟就挂在视野左上角。"

---

### [P030] 名词·动作孤立句

**定义**
- category: 句式层
- description: 将"看到某物"和"对某物做动作"拆成两个独立短句，先名词后动作（"右上角一个齿轮图标。点开。"）
- ai_tendency: LLM 倾向把感知和动作拆成 staccato 序列，像操作手册的分步指令

**context-agent**
- soft_constraint: 感知和动作整合为一句，用连动或介词连接

**reviewer**
- section: 6.2
- detection: 「名词短句。动作短句。」的孤立结构——前句仅描述一个物体/界面元素（句号结尾），后句仅一个动作（"点开。""打开。""看去。"）
- severity: ≥3处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 合并为连动句。如"右上角一个齿轮图标。点开。"→"点开右上角的齿轮图标。"
- pass_criteria: 无名词·动作孤立句对

**anti-ai-guide**
- error_number: 21
- error_text: "名词·动作孤立句——你倾向把'看到'和'行动'拆成两个独立短句。合并为连贯动作。"
- instant_check: false
- substitution: "右上角一个齿轮图标。点开。"→"点开右上角的齿轮图标。"

---

### [P031] 事实断言替代推断

**定义**
- category: 叙事视角层
- description: 角色只能通过观察推断的事，叙述用肯定语气陈述（"人砸的，不是怪物"），而非带有不确定性的推断语气
- ai_tendency: LLM 倾向使用全知叙述者视角，忘记角色知识边界

**context-agent**
- soft_constraint: 角色通过观察推断的结论，用推断语气表达（"应该是""看上去像""大概"），不用肯定陈述

**reviewer**
- section: 6.3
- detection: 角色观察→得出肯定结论（应为推断的写成事实）。特征：角色仅凭视觉/听觉做出确定性判断，如"人砸的，不是怪物"
- severity: ≥3处→high，1-2处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2]
- rewrite: 肯定陈述→推断语气。如"人砸的，不是怪物"→"应该是人砸的"
- pass_criteria: 从角色视角出发的推断使用推断语气

**anti-ai-guide**
- error_number: 22
- error_text: "事实断言替代推断——角色观察到的结论应该用推断语气表达。加'应该''看上去像''大概是'。"
- instant_check: false
- substitution: "人砸的，不是怪物"→"应该是人砸的，有几辆车窗碎得整整齐齐"

---

### [P032] 功能报告式发现

**定义**
- category: 叙事视角层
- description: 角色发现新功能/新信息时用平淡的陈述句播报，而非带着情绪反应的发现语气
- ai_tendency: LLM 用产品说明语气报告新功能，缺少人类发现新事物时的兴奋/惊讶/疑虑

**context-agent**
- soft_constraint: 角色发现新能力/新信息时必须有情绪反应，哪怕是一闪而过的内心评价

**reviewer**
- section: 6.4
- detection: 角色新发现的描写仅有功能陈述无情绪——「隐藏副本。攻略者能扫隐藏入口。」等纯信息播报
- severity: ≥3处→medium
- threshold: ≥2处

**polish-guide**
- layers: [2, 4]
- rewrite: 功能陈述→加入角色情绪评价。如"隐藏副本。攻略者能扫隐藏入口。"→"攻略者还能扫隐藏入口——这功能不错。"
- pass_criteria: 关键新发现至少有一个角色情绪锚点

**anti-ai-guide**
- error_number: 23
- error_text: "功能报告式发现——发现新功能/信息时别用说明书语气。加一句角色的内心反应。"
- instant_check: false
- substitution: "隐藏副本。攻略者能扫隐藏入口。"→"攻略者还能扫隐藏入口——这功能不错。"

---

### [P033] "不像X，像在Y"隐喻化否定对比

**定义**
- category: 句式层
- description: 用"不像X，像在Y"结构将否定+隐喻捆绑，制造伪深刻的洞察语气。是P001的隐喻化变体
- ai_tendency: LLM 倾向用此结构制造"一眼看穿本质"的叙述姿态，人类读者感知为做作

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 直接写隐喻，去掉否定前缀。不写"不像质疑，像在走棋"，写"每一步都在试探"

**reviewer**
- section: 6.2
- detection: 「不像……，像在……」结构——前面否定一个动作/状态，后面用"像在"引出隐喻对比
- severity: high
- threshold: ≥1次即报（出现即改写）
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删除"不像X"，直接写"像在Y"或进一步改写为正面陈述。如"不像质疑，像在走棋"→"每一步都是试探"
- pass_criteria: 全章=0次

**anti-ai-guide**
- error_number: 24
- error_text: "\"不像X，像在Y\"隐喻化否定对比——你倾向用此结构制造洞察语气。直接写隐喻，去掉否定前缀。"
- instant_check: false
- substitution: "不像质疑，像在走棋"→"每一步都是试探"

---

### [P034] 叙述者充当文学评论家

**定义**
- category: 叙事视角层
- description: 叙述者对角色的话语/行为做情感成分的量化分析（"带一点X，但更多是Y"）或性质归类（"这话是陈述句"），然后加诗意复述。叙述者越位充当文本的文学评论家
- ai_tendency: LLM 不信任读者能从对话本身感知情绪，于是用分析句替读者做"精读"
- root_cause: 全知者姿态——叙述者比读者更懂角色话语的含义，并且急于展示这种理解

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 角色对话的情绪含义通过上下文、角色反应、动作来呈现，不允许叙述者跳出来分析

**reviewer**
- section: 6.3
- detection: 「他这话是……」「这话里带着……」「声音里有……」「语气中透着……」等叙述者对角色话语的情感成分分析句式。特征：在角色说完话后，叙述者立即用分类/量化方式解析话中的情感配比
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删除叙述者的分析句。用角色的身体反应、沉默、下一句话或另一个角色的反应来让读者自己感知情绪配比。如"他这话是陈述句，带一点欣赏，但更多是疑问"→删掉整句，让上下文承担
- pass_criteria: 全章=0次

**anti-ai-guide**
- error_number: 25
- error_text: "\"叙述者充当文学评论家\"——你倾向在角色说完话后立即分析话中的情感成分。删掉分析句，让读者自己感受。"
- instant_check: false
- substitution: "他这话是陈述句，带一点欣赏，但更多是疑问，一个还没得到答案的疑问。"→ 删除，或改为角色的动作反应（"他看着对方，等下文。"）

---

### [P035] 否定未发生动作（"没有X。做了Y。"）

**定义**
- category: 句式层
- description: 在描述实际发生的动作之前，先用一个短句否定可能发生但没发生的动作。叙述者预判读者会产生预期，抢先纠正
- ai_tendency: LLM 总是比读者快一步——知道读者可能想什么，于是预先把"没发生的事"否掉再写"发生的事"
- root_cause: 全知者姿态——叙述者不信任读者能从"做了Y"自己推出"没有X"

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 直接写发生的动作。读者能从"他站在原地"推出"他没有追"

**reviewer**
- section: 6.2
- detection: 「没有X。Y了。」结构——前面否定一个在语境中可能被预期的动作，紧接着描述实际发生的动作。标志：短否定句（"没有追""没有回头""没有回答"）后紧跟一个描述实际行为的句子
- severity: high
- threshold: ≥1次即报（出现即改写）
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删除否定句，只保留实际动作。如"瘦高男人没有追。他站在原地"→"瘦高男人站在原地"
- pass_criteria: 全章=0次

**anti-ai-guide**
- error_number: 26
- error_text: "\"否定未发生动作\"——你倾向先否定一个可能发生但没发生的动作再写实际动作。删掉否定句，读者自己能推出来。"
- instant_check: false
- substitution: "他没有追。站在原地。"→"站在原地。"

---

### [P036] 动作微观测评

**定义**
- category: 叙事视角层
- description: 叙述者对角色的微动作做物理属性的量化分析（时长、音量、幅度）并和过往实例做差分对比。把人的行为当成仪器测量的数据集
- ai_tendency: LLM 用精确属性替代模糊感知——"笑了一声"不够，必须加上"短促的，比昨天那个更轻"才能证明叙述者在认真观察
- root_cause: 越位叙述——叙述者扮演行为科学家，对角色的笑做声学分析。读者不需要知道这个笑比昨天轻了几个分贝

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 动作就是动作。笑就是笑。不附加物理属性测量值，不和以前的笑做差分对比

**reviewer**
- section: 6.3
- detection: 「短促的」「低沉的」「缓慢的」等副词独立逗号分隔 + 「比昨天/上次/刚才那个更X」的跨章节/跨段落差分对比。特征：将角色的表情/笑声/动作做精确属性标注并和历史版本比较
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删除属性标注和差分对比，只保留动作本身。如"他笑了一声，短促的，比昨天那个更轻"→"他笑了一声"
- pass_criteria: 全章=0次

**anti-ai-guide**
- error_number: 27
- error_text: "\"动作微观测评\"——你倾向给角色的笑/动作加物理属性测量和跨章节差分。笑就是笑，不加声学分析。"
- instant_check: false
- substitution: "他笑了一声，短促的，比昨天那个更轻。"→"他笑了一声。"

---

### [P037] 跨章回顾式叙述

**定义**
- category: 叙事视角层
- description: 叙述者或角色用"上一章""上一回""比上次/昨天/刚才更X"等元叙事语言对故事做跨章对比分析。角色和叙述者都不活在"章"里——章是书的结构，不是角色经历的结构
- ai_tendency: LLM 把故事当成已完成的文本在写，叙述者像读书报告一样引用前面的章节号做参照
- root_cause: 全知者姿态——叙述者已经读完了整本书，忍不住在每一章做进度回顾

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 角色只能用"上次/之前/几天前"等时间参照，不能用"上一章"。叙述者不替读者做跨章对比分析

**reviewer**
- section: 6.3
- detection: 「上一章」「上一回」「比上次交手时更」「比昨天/刚才/之前更X」等跨章/跨事件的正式对比分析。注意：角色的自然回忆("昨天遇到的那个人")不算，叙述者的分析性对比("比上一章他表现出来的更深")才算
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删掉跨章对比，或改为角色视角的自然判断。如"这个人的情报网比上一章他表现出来的更深"→"这家伙藏得比想的深"
- pass_criteria: 无叙述者驱动的跨章对比。角色自然的"上次/之前"参照不在此列

**anti-ai-guide**
- error_number: 28
- error_text: "\"跨章回顾式叙述\"——你倾向用'上一章''比上次更X'对角色做跨章对比分析。删掉，角色的自然记忆不需要写成文学评论。"
- instant_check: false
- substitution: "情报网比上一章他表现出来的更深"→"这家伙藏得比想的深"

---

### [P039] 单字动作句

**定义**
- category: 句式层
- description: 在描述了一个场景/冲突后，用一个单字短句说明角色做了某个决策（"他让。""他忍。""他懂了。""他算了。"）。AI不信任读者能从场景中自己推出角色的决策
- ai_tendency: LLM 在写完场景后追加一个单字动作来"点破"角色的应对方式，像给段落的情绪进展盖一个章
- root_cause: 越位叙述——叙述者替角色宣布决策

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 场景本身已经传达了角色的应对方式。不需要最后那个单字句来点破

**reviewer**
- section: 6.2
- detection: 自然段末尾出现「他让。」「他忍。」「他懂了。」「他算了。」「他不说话了。」等1-3字的独立短句，功能为宣布角色决策/态度。标志：句号结尾，前面是2句以上的场景描述
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删掉单字句。如"碰到别的玩家来抢点，他让。耽误时间"→"碰到别的玩家来抢点——耽误时间，不值得争"
- pass_criteria: 全章=0次

**anti-ai-guide**
- error_number: 30
- error_text: "\"单字动作句\"——你倾向在场景末尾加一个单字句宣布角色决策。删掉，读者已经从场景中看到了。"
- instant_check: false
- substitution: "碰到别的玩家来抢点，他让。耽误时间。"→"碰到别的玩家来抢点——耽误时间，不值得争。"

---

### [P040] 孤立名词过渡句

**定义**
- category: 句式层
- description: 角色发现关键信息后，用该信息的名称独立成段作为过渡（"机制溯源者。"），试图制造"顿悟时刻"的戏剧效果。人类作者的过渡段包含情绪反应，AI的过渡段只有一个名词
- ai_tendency: LLM 用独立名词段落模仿电影切镜——"主角看到了一个名字"→切到名字的特写→然后再解释。结果不像是角色在思考，像是PPT翻页
- root_cause: 功能报告式发现(P032)的姊妹模式——不是缺情绪，而是把情绪替换成了一个孤零零的名词

**context-agent**
- soft_constraint: 过渡段落必须包含角色的情绪反应或判断，不能只放一个名词。不写"机制溯源者。"，写"机制溯源者——攻略者不认识这个职业。"

**reviewer**
- section: 6.2
- detection: 独立段落由一个名词/名词短语+句号构成（"机制溯源者。""卖身契。""未知来源。"），且该名词为上文或下文的核心信息
- severity: medium
- threshold: ≥2处→medium
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 将孤立名词融入上一段或下一段，附加角色的情绪反应。如"机制溯源者。"→"机制溯源者。攻略者不认识这个职业。"
- pass_criteria: ≤1处（保留真正有冲击力的单次使用）

**anti-ai-guide**
- error_number: 31
- error_text: "\"孤立名词过渡句\"——你倾向用单独一个名词成段来做戏剧过渡。加一句角色的反应，让过渡有温度。"
- instant_check: false
- substitution: "机制溯源者。"→"机制溯源者。攻略者不认识这个职业。"

---

### [P041] 物理动作隐喻写抽象互动

**定义**
- category: 句式层
- description: 用处理物理实体的动词（接/接住/抓住/承接/扛下）来描述抽象的人际互动（目光、话语、沉默）。AI 不会写含蓄的情感回避，于是用"没接这个目光"替代"没理他/没看他"
- ai_tendency: LLM 把"避免眼神接触""不理会对方的试探"等抽象社交行为翻译成"接住目光""接话""承接沉默"——像在写动作戏
- root_cause: 越位叙述——叙述者用物理隐喻替代情感描写，但隐喻本身已经用烂了

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 情感回避用具体的动作写：移开视线、转开脸、低头看面板。不写"没接这个目光"

**reviewer**
- section: 6.2
- detection: 「接住/接/承接/扛下 + 目光/眼神/视线/话语/沉默/这句话」等物理动词+抽象名词的组合
- severity: medium
- threshold: ≥2处→medium
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: "没接这个目光"→"没看他"或"低头看面板"等具体动作
- pass_criteria: 无物理隐喻替代情感描写

**anti-ai-guide**
- error_number: 32
- error_text: "\"物理动作隐喻\"——你倾向用'接住目光''接话''承接沉默'写人际互动。情感不是飞镖，不需要接。"
- instant_check: false
- substitution: "他没接这个目光"→"他没看他，低头盯着面板。"

---

### [P042] AI氛围填充

**定义**
- category: 叙事视角层
- description: 在关键动作之间插入不承载叙事功能的环境描写（阳光角度、灰尘、盯着某物看）。这些描写不映射角色情绪、不揭示信息、不推进氛围——只填充动作之间的空白
- ai_tendency: LLM 在两个有效动作之间觉得"空"，于是用阳光/灰尘/凝视来垫字数。人类作者的环境描写要么承载情绪，要么立刻切到下一个动作
- root_cause: 越位叙述——叙述者觉得需要"文学感"于是插入无功能的环境描写，而不是让动作密度本身带动节奏

**context-agent**
- soft_constraint: 环境描写只写两样：角色此刻注意到的、影响角色行为的。阳光从哪斜进来、灰尘怎么飘、盯着看了多久——如果角色自己都没在意，就不写

**reviewer**
- section: 6.3
- detection: 两个有效动作/对话之间出现1-3句独立的环境/物品描写（"阳光从X斜进来""照在Y上""蹭了一层灰""盯着Z看了一会儿"），且该描写不连接角色情绪、不触发后续行动
- severity: ≥3处→medium
- threshold: ≥3处

**polish-guide**
- layers: [2]
- rewrite: 删掉或改为角色视角的快速扫视。如"阳光从巷口斜进来，照在护臂上。蹭了一层灰。他盯着那片灰看了一会儿"→ 删除，直接跳到下一个动作
- pass_criteria: 无功能的环境描写均改为角色承载

**anti-ai-guide**
- error_number: 33
- error_text: "\"AI氛围填充\"——你倾向在动作之间塞阳光/灰尘/凝视来垫字数。环境描写要么承载情绪，要么删掉。"
- instant_check: false
- substitution: "阳光从巷口斜进来，照在护臂上。蹭了一层灰。他盯着看了一会儿"→ 删掉，直接写下一个动作

---

### [P038] 秒表式停顿

**定义**
- category: 叙事视角层
- description: 叙述者用精确秒数测量角色的沉默时长（"沉默了一秒""停顿了三秒"）。人类作者用空行、动作或"他沉默了"让读者自己感受停顿
- ai_tendency: LLM 像写舞台剧本的场记一样标时间——"pause 3 seconds"。读者不需要秒表来感受沉默的重量
- root_cause: 越位叙述——叙述者不信任读者能从上下文感知停顿的长度和意义

**context-agent**
- hard_metric: 全章0次（绝对禁止）
- note: 沉默就是沉默。用"他沉默了""他没接话""过了好一会儿"——不用秒

**reviewer**
- section: 6.3
- detection: 「沉默/停顿/停了/顿了 + 数字 + 秒/分钟」等精确时间测量。注意：倒计时/系统计时器属于游戏机制，不算。此规则针对叙述者对人物自然停顿的秒表测量
- severity: high
- threshold: ≥1次即报
- evidence: 列出所有出现位置的原文引用

**polish-guide**
- layers: [2]
- rewrite: 删除秒数，改为叙述留白（段落断开）或动作过渡。如"赵峰沉默了三秒"→"赵峰没接话。"
- pass_criteria: 无对人物沉默的秒数测量

**anti-ai-guide**
- error_number: 29
- error_text: "\"秒表式停顿\"——你倾向用'沉默了一秒''停顿三秒'给角色的沉默计时。沉默不需要秒数。"
- instant_check: false
- substitution: "赵峰沉默了三秒"→"赵峰没接话。"
