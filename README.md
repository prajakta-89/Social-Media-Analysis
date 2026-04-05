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

## Key Insight:
This query provides a holistic view of user engagement, capturing both content creation (posts) 
and participation (comments), which is more meaningful than measuring activity using a single metric.
Business Value:
Helps identify highly engaged users.
Useful for recognizing influencers, power users, or community leaders.
Can guide reward systems or targeted engagement strategies.
This solution strictly follows the given schema and accurately answers the analytical requirement.

## Relationships:
  • Each user can create multiple posts.
  • Each post can have multiple likes and comments.
  • Users can follow each other (self-join in followers table).
  • Posts can be tagged with multiple hashtags (many-to-many via post_hashtags).

-- ==============================================================
-- BONUS CHALLENGES
-- ==============================================================
-- 1. Engagement rate = (likes + comments) / posts
-- 2. Mutual followers
-- 3. Most used hashtags by top 5 influencers
-- 4. Country-wise engagement leaderboard
-- --------------------------------------------------------------
## 1.
```sql
SELECT
    u.user_id,
    u.username,
    (COUNT(DISTINCT l.like_id) + COUNT(DISTINCT c.comment_id)) * 1.0
    / NULLIF(COUNT(DISTINCT p.post_id), 0) AS engagement_rate
FROM users u
LEFT JOIN posts p
    ON u.user_id = p.user_id
LEFT JOIN likes l
    ON p.post_id = l.post_id
LEFT JOIN comments c
    ON p.post_id = c.post_id
GROUP BY u.user_id, u.username;
```
## 2.
```sql
SELECT
    f1.user_id AS user_1,
    f1.follower_user_id AS user_2
FROM followers f1
JOIN followers f2
    ON f1.user_id = f2.follower_user_id
   AND f1.follower_user_id = f2.user_id
WHERE f1.user_id < f1.follower_user_id;
```
## 3.
```sql
WITH top_influencers AS (
    SELECT
        u.user_id
    FROM users u
    JOIN followers f
        ON u.user_id = f.user_id
    GROUP BY u.user_id
    ORDER BY COUNT(f.follower_user_id) DESC
    LIMIT 5
)
SELECT
    h.hashtag_id,
    h.tag_name,
    COUNT(ph.post_id) AS usage_count
FROM top_influencers ti
JOIN posts p
    ON ti.user_id = p.user_id
JOIN post_hashtags ph
    ON p.post_id = ph.post_id
JOIN hashtags h
    ON ph.hashtag_id = h.hashtag_id
GROUP BY h.hashtag_id, h.tag_name
ORDER BY usage_count DESC;
```
## 4.
```sql
SELECT
    u.country,
    COUNT(DISTINCT l.like_id) + COUNT(DISTINCT c.comment_id) AS total_engagement
FROM users u
JOIN posts p
    ON u.user_id = p.user_id
LEFT JOIN likes l
    ON p.post_id = l.post_id
LEFT JOIN comments c
    ON p.post_id = c.post_id
GROUP BY u.country
ORDER BY total_engagement DESC;
```
