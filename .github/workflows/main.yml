import os
import smtplib
from email.mime.text import MIMEText
from googleapiclient.discovery import build
from datetime import datetime, timedelta, timezone

# 配置信息（从 Secrets 获取）
YOUTUBE_API_KEY = os.environ.get('YOUTUBE_API_KEY')
CHANNEL_ID = os.environ.get('CHANNEL_ID')
EMAIL_ADDRESS = os.environ.get('EMAIL_ADDRESS')
EMAIL_PASSWORD = os.environ.get('EMAIL_PASSWORD')

def get_latest_video():
    youtube = build('youtube', 'v3', developerKey=YOUTUBE_API_KEY)
    request = youtube.search().list(
        channelId=CHANNEL_ID,
        part='snippet',
        order='date',
        maxResults=1,
        type='video'
    )
    response = request.execute()
    
    if not response['items']:
        return None
        
    item = response['items'][0]
    video_id = item['id']['videoId']
    video_title = item['snippet']['title']
    # 获取发布时间并转为 UTC
    publish_time_str = item['snippet']['publishedAt']
    publish_time = datetime.strptime(publish_time_str, '%Y-%m-%dT%H:%M:%SZ').replace(tzinfo=timezone.utc)
    
    return video_id, video_title, publish_time

def send_email(video_id, video_title):
    msg = MIMEText(f"新视频发布了！\n\n标题: {video_title}\n链接: https://www.youtube.com/watch?v={video_id}")
    msg['Subject'] = f"YouTube 更新: {video_title}"
    msg['From'] = EMAIL_ADDRESS
    msg['To'] = EMAIL_ADDRESS # 发给自己

    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        server.send_message(msg)

if __name__ == "__main__":
    result = get_latest_video()
    if result:
        v_id, v_title, v_time = result
        # 获取当前 UTC 时间
        now = datetime.now(timezone.utc)
        # 判断是否是过去 20 分钟内发布的（多留 5 分钟缓冲，防止 Actions 延迟）
        if now - v_time < timedelta(minutes=20):
            print(f"检测到新视频: {v_title}，发送邮件中...")
            send_email(v_id, v_title)
        else:
            print(f"最新视频 '{v_title}' 发布于 {v_time}，不是新片，跳过。")
