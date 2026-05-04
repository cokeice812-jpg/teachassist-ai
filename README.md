"""快速演示 —— 不依赖完整项目，单独跑通一个 Agent"""

import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()

client = OpenAI(
    api_key=os.getenv("MIMO_API_KEY"),
    base_url=os.getenv("MIMO_BASE_URL"),
)

# 用一段简单的课文测试作文批改能力
essay = """
赤壁赋（默写练习）
壬戌之秋，七月既望，苏子与客泛舟游于赤壁之下。清风徐来，水波不兴。
举酒属客，诵明月之诗，歌窈窕之章。
"""

response = client.chat.completions.create(
    model="mimo-1",
    messages=[
        {"role": "system", "content": "你是一位高中语文老师，请从立意、结构、文采、语法四个维度对学生的默写进行点评。"},
        {"role": "user", "content": essay},
    ],
    temperature=0.4,
)

print(response.choices[0].message.content)
