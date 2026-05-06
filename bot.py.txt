import os
import random
import asyncio
from datetime import time
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

BOT_TOKEN = os.getenv("TELEGRAM_BOT_TOKEN")
CHAT_ID = os.getenv("TELEGRAM_CHAT_ID")

DAILY_MISSIONS = [
    {
        "title": "Engage with Cronos App Content",
        "xp": 3,
        "task": "Cronos 공식 포스트에 인사이트 있는 댓글 또는 Quote 작성",
        "template": """오늘 미션: Engage with Cronos App Content

추천 답변:
Cronos App is interesting because it lowers the barrier for people who are not professional traders yet still want access to crypto, stocks, and prediction markets in one place.

This could be a strong onboarding path for mainstream users.

태그:
@CronosApp @Cronos_Chain

제출 전 체크:
- 공개 포스트인지 확인
- 단순 감탄사 금지
- Cronos App에 대한 관점 포함"""
    },
    {
        "title": "Translate Announcement",
        "xp": 3,
        "task": "Cronos 공식 발표를 한국어로 번역하고 짧은 해석 추가",
        "template": """오늘 미션: Translate Announcement

포맷:
[한국어 번역]
Cronos의 최신 발표 내용을 한국어로 자연스럽게 번역하세요.

[짧은 해석]
이번 업데이트는 Cronos App과 Cronos Network가 일반 사용자에게 더 쉬운 온체인 금융 경험을 제공하려는 방향성과 연결됩니다.

태그:
@CronosApp @Cronos_Chain

제출 전 체크:
- 원본 공식 포스트 링크 포함
- 단순 번역이 아니라 짧은 해석 추가
- 전체 스레드면 전체 번역"""
    },
    {
        "title": "Meme / Trend Content",
        "xp": 3,
        "task": "Cronos App을 한국 밈/트렌드와 연결한 짧은 콘텐츠 작성",
        "template": """오늘 미션: Meme / Trend Content

예시 포스트:
나: 주식장은 닫혔네...
Cronos App: 24/7 markets are open.

Stocks, crypto, and prediction markets in one account.
This is the kind of trading experience that actually fits internet-native users.

@CronosApp @Cronos_Chain

제출 전 체크:
- Cronos App과 직접 연결
- 짧고 재밌게
- 이미지/GIF 있으면 승인 확률 상승"""
    },
    {
        "title": "Create Short-form Content",
        "xp": 5,
        "task": "Cronos App 또는 Cronos Network에 대한 짧은 오리지널 포스트 작성",
        "template": """오늘 미션: Create Short-form Content

예시 포스트:
Most trading apps are built for people who already know exactly what they want to trade.

Cronos App feels different.

It brings stocks, crypto, and prediction markets into one simple experience, making financial access easier for mainstream users.

@CronosApp @Cronos_Chain

제출 전 체크:
- 15~100 words
- CTA 또는 관점 포함
- 태그 포함"""
    }
]

WEEKLY_MISSIONS = [
    {
        "title": "Create a video about Cronos App",
        "xp": 45,
        "template": """이번 주 고XP 미션: Create a video about Cronos App

30초 영상 스크립트:
Most people never trade because trading apps feel too complicated.

Cronos App is trying to change that.

It brings stocks, crypto, and prediction markets into one account, available 24/7.

For mainstream users, this could make trading more accessible, simple, and internet-native.

CTA:
Follow @CronosApp and explore the future of on-chain markets.

체크리스트:
- 30초 이상
- 자막 필수
- Cronos App을 제목/캡션에 명확히 언급
- 앱 화면, 공식 이미지, 그래픽 사용
- AI 배경만 있는 영상 금지"""
    },
    {
        "title": "Regional Content - Korea",
        "xp": 15,
        "template": """이번 주 추천 미션: Regional Content

주제:
한국 유저에게 Cronos App이 왜 흥미로운가?

핵심 포인트:
- 한국은 모바일 금융/투자 앱 사용성이 중요함
- 초보자는 복잡한 트레이딩 앱에 진입장벽을 느낌
- Cronos App은 stocks, crypto, prediction markets를 하나의 계정에서 제공
- 24/7 시장 접근성은 한국 유저에게 강한 포인트

주의:
단순 영어 콘텐츠 번역 금지.
한국 시장/유저 행동/문화 맥락을 반드시 포함."""
    }
]

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "Cronos Zealy 미션 봇 시작!\n\n"
        "명령어:\n"
        "/mission - 오늘 할 미션\n"
        "/tweet - X 포스트 초안\n"
        "/video - 영상 미션 스크립트\n"
        "/weekly - 주간 고XP 미션\n"
        "/submit - 제출 체크리스트"
    )

async def mission(update: Update, context: ContextTypes.DEFAULT_TYPE):
    m = random.choice(DAILY_MISSIONS)
    await update.message.reply_text(
        f"오늘 추천 미션\n\n"
        f"{m['title']} / {m['xp']} XP\n\n"
        f"{m['task']}\n\n"
        f"{m['template']}"
    )

async def tweet(update: Update, context: ContextTypes.DEFAULT_TYPE):
    posts = [
        """Most trading apps are built for experienced traders.

Cronos App feels more accessible.

Stocks, crypto, and prediction markets in one account could make trading easier for mainstream users.

@CronosApp @Cronos_Chain""",
        """The next wave of crypto adoption may not start with complex DeFi dashboards.

It may start with simple, familiar products.

Cronos App brings 24/7 access to stocks, crypto, and prediction markets in one place.

@CronosApp @Cronos_Chain""",
        """Cronos App is interesting because it connects mainstream trading behavior with on-chain infrastructure.

Simple UX first.
Crypto-native rails underneath.

That is a strong direction for broader adoption.

@CronosApp @Cronos_Chain"""
    ]
    await update.message.reply_text(random.choice(posts))

async def video(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(WEEKLY_MISSIONS[0]["template"])

async def weekly(update: Update, context: ContextTypes.DEFAULT_TYPE):
    m = random.choice(WEEKLY_MISSIONS)
    await update.message.reply_text(f"{m['title']} / {m['xp']} XP\n\n{m['template']}")

async def submit(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        """Zealy 제출 전 체크리스트

1. 링크가 공개 상태인가?
2. @CronosApp 또는 @Cronos_Chain 태그했나?
3. 공식 포스트/링크를 포함했나?
4. 내 의견/해석이 들어갔나?
5. 이미지, 캡처, GIF, 영상 등 시각 자료가 있나?
6. 너무 짧거나 복붙처럼 보이지 않나?
7. 퀘스트 조건의 글자 수/영상 길이를 충족했나?
8. 같은 내용 반복 제출은 아닌가?

승인 잘 나는 제출 문구:
Submitted my original content for Cronos Ambassador Program S3.
The post includes original perspective, Cronos-related context, official account tag, and public link."""
    )

async def daily_push(context: ContextTypes.DEFAULT_TYPE):
    if CHAT_ID:
        m = random.choice(DAILY_MISSIONS)
        await context.bot.send_message(
            chat_id=CHAT_ID,
            text=f"오늘의 Cronos Zealy 미션\n\n{m['title']} / {m['xp']} XP\n\n{m['template']}"
        )

def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("mission", mission))
    app.add_handler(CommandHandler("tweet", tweet))
    app.add_handler(CommandHandler("video", video))
    app.add_handler(CommandHandler("weekly", weekly))
    app.add_handler(CommandHandler("submit", submit))

    app.job_queue.run_daily(daily_push, time=time(hour=10, minute=0))

    app.run_polling()

if __name__ == "__main__":
    main()