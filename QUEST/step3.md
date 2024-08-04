### 1.エピソード視聴数トップ3のエピソードタイトルと視聴数を取得してください

```SQL:
SELECT episodes.title AS episode_title, airtimes.views
FROM episodes
JOIN airtimes ON episodes.id = airtimes.episode_id
ORDER BY airtimes.views DESC
LIMIT 3;
```

### 2.エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください

```SQL:
SELECT programs.title AS program_title,
       season_counts.season_count,
       episode_counts.episode_count,
       episodes.title AS episode_title,
       airtimes.views
FROM episodes
JOIN airtimes ON episodes.id = airtimes.episode_id
JOIN seasons ON episodes.season_id = seasons.id
JOIN programs ON seasons.program_id = programs.id
JOIN (
    SELECT program_id, COUNT(*) AS season_count
    FROM seasons
    GROUP BY program_id
) AS season_counts ON seasons.program_id = season_counts.program_id
JOIN (
    SELECT seasons.program_id, COUNT(episodes.id) AS episode_count
    FROM episodes
    JOIN seasons ON episodes.season_id = seasons.id
    GROUP BY seasons.program_id
) AS episode_counts ON seasons.program_id = episode_counts.program_id
ORDER BY airtimes.views DESC
LIMIT 3;
```

### 3.本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします

※本日は2023-07-13とします

```SQL:
SELECT 
    channels.name AS channel_name,
    airtimes.start_time AS broadcast_start_time,
    airtimes.end_time AS broadcast_end_time,
    season_counts.season_count,
    episode_counts.episode_count,
    episodes.title AS episode_title,
    episodes.description AS episode_description
FROM airtimes
JOIN channels ON airtimes.channel_id = channels.id
JOIN episodes ON airtimes.episode_id = episodes.id
JOIN seasons ON episodes.season_id = seasons.id
JOIN (
    SELECT seasons.program_id, COUNT(*) AS season_count
    FROM seasons
    GROUP BY seasons.program_id
) AS season_counts ON seasons.program_id = season_counts.program_id
JOIN (
    SELECT seasons.program_id, COUNT(episodes.id) AS episode_count
    FROM episodes
    JOIN seasons ON episodes.season_id = seasons.id
    GROUP BY seasons.program_id
) AS episode_counts ON seasons.program_id = episode_counts.program_id
WHERE DATE(airtimes.start_time) = '2023-07-13'
ORDER BY airtimes.start_time;
```

### 4.コメディのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得してください

※本日は2023-07-13とします

```SQL:
SELECT
    airtimes.start_time AS broadcast_start_time,
    airtimes.end_time AS broadcast_end_time,
    season_counts.season_count,
    episode_counts.episode_count,
    episodes.title AS episode_title,
    episodes.description AS episode_description
FROM airtimes
JOIN channels ON airtimes.channel_id = channels.id
JOIN episodes ON airtimes.episode_id = episodes.id
JOIN seasons ON episodes.season_id = seasons.id
JOIN (
    SELECT seasons.program_id, COUNT(*) AS season_count
    FROM seasons
    GROUP BY seasons.program_id
) AS season_counts ON seasons.program_id = season_counts.program_id
JOIN (
    SELECT seasons.program_id, COUNT(episodes.id) AS episode_count
    FROM episodes
    JOIN seasons ON episodes.season_id = seasons.id
    GROUP BY seasons.program_id
) AS episode_counts ON seasons.program_id = episode_counts.program_id
WHERE channels.name = 'コメディ'
  AND airtimes.start_time BETWEEN '2023-07-13' AND '2023-07-19’
ORDER BY airtimes.start_time;
```
