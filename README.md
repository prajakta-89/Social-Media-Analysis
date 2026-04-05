# Social-Media-Analysis
Social Media Analysis (SQL Project). This project analyzes social media data to extract insights about user engagement, content performance, and trends using SQL. we can analize the active , inactive users and hashtags like that...

## Tables and Key Columns:
  1. users(user_id, username, join_date, country)
  2. posts(post_id, user_id, content, created_at)
  3. comments(comment_id, post_id, user_id, comment_text, created_at)
  4. likes(like_id, post_id, user_id, created_at)
  5. followers(follower_id, user_id, follower_user_id, follow_date)
  6. hashtags(hashtag_id, tag_name, category)
  7. post_hashtags(id, post_id, hashtag_id)
