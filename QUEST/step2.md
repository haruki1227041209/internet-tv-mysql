## 1.データベースを構築します

```SQL:データベース作成
CREATE DATABASE internet_tv;
USE internet_tv;
```

## 2.[STEP1](https://github.com/haruki1227041209/internet-tv-mysql/blob/main/QUEST/step1.md "STEP1")で設計したテーブルを構築します

```SQL:テーブル作成
-- テーブル：channels
CREATE TABLE channels (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- テーブル：programs
CREATE TABLE programs (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT
);

-- テーブル：genres
CREATE TABLE genres (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- テーブル：programs_genres
CREATE TABLE programs_genres (
    program_id BIGINT(20),
    genre_id BIGINT(20),
    PRIMARY KEY (program_id, genre_id),
    FOREIGN KEY (program_id) REFERENCES programs(id),
    FOREIGN KEY (genre_id) REFERENCES genres(id)
);

-- テーブル：seasons
CREATE TABLE seasons (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    program_id BIGINT(20),
    season_number INT(20) NOT NULL,
    FOREIGN KEY (program_id) REFERENCES programs(id)
);

-- テーブル：episodes
CREATE TABLE episodes (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    season_id BIGINT(20),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    length INT(20),
    release_date DATE NOT NULL,
    FOREIGN KEY (season_id) REFERENCES seasons(id)
);

-- テーブル：airtimes
CREATE TABLE airtimes (
    id BIGINT(20) AUTO_INCREMENT PRIMARY KEY,
    channel_id BIGINT(20),
    start_time DATETIME NOT NULL,
    end_time DATETIME NOT NULL,
    episode_id BIGINT(20),
    views INT(20),
    FOREIGN KEY (channel_id) REFERENCES channels(id),
    FOREIGN KEY (episode_id) REFERENCES episodes(id)
);
```

## 3.サンプルデータを入れます
```SQL:
-- チャンネル
INSERT INTO channels (name) VALUES
('チャンネルA'),
('チャンネルB'),
('チャンネルC'),
('チャンネルD'),
('チャンネルE'),
('チャンネルF'),
('チャンネルG'),
('チャンネルH');

-- プログラム
INSERT INTO programs (title, description) VALUES
('ドラマA', 'ドラマAの説明'),
('コメディB', 'コメディBの説明'),
('ドキュメンタリーC', 'ドキュメンタリーCの説明'),
('ドラマD', 'ドラマDの説明'),
('コメディE', 'コメディEの説明'),
('アニメF', 'アニメFの説明'),
('ニュースG', 'ニュースGの説明'),
('バラエティH', 'バラエティHの説明'),
('ドキュメンタリーI', 'ドキュメンタリーIの説明'),
('ドラマJ', 'ドラマJの説明');

-- ジャンル
INSERT INTO genres (name) VALUES
('ドラマ'),
('コメディ'),
('ドキュメンタリー'),
('アクション'),
('ファンタジー');

-- プログラムジャンル
INSERT INTO programs_genres (program_id, genre_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 1),
(5, 2),
(6, 4),
(7, 3),
(8, 2),
(9, 3),
(10, 1),
(10, 5);

-- シーズン
INSERT INTO seasons (program_id, season_number) VALUES
(1, 1),  -- ドラマA シーズン1
(1, 2),  -- ドラマA シーズン2
(2, 1),  -- コメディB シーズン1
(3, 0),  -- ドキュメンタリーC (シーズンなし)
(4, 1),  -- ドラマD シーズン1
(4, 2),  -- ドラマD シーズン2
(5, 1),  -- コメディE シーズン1
(6, 1),  -- アニメF シーズン1
(7, 0),  -- ニュースG (シーズンなし)
(8, 1),  -- バラエティH シーズン1
(9, 0);  -- ドキュメンタリーI (シーズンなし)
(10, 1), -- ドラマJ シーズン1
(10, 2), -- ドラマJ シーズン2




-- エピソード
INSERT INTO episodes (season_id, title, description, length, release_date) VALUES
(1, 'ドラマA シーズン1 エピソード1', 'エピソード1の説明', 45, '2023-01-01'),
(1, 'ドラマA シーズン1 エピソード2', 'エピソード2の説明', 50, '2023-01-08'),
(2, 'ドラマA シーズン2 エピソード1', 'エピソード1の説明', 48, '2023-04-01'),
(3, 'コメディB シーズン1 エピソード1', 'エピソード1の説明', 30, '2023-02-01'),
(3, 'コメディB シーズン1 エピソード2', 'エピソード2の説明', 32, '2023-02-08'),
(5, 'ドラマD シーズン1 エピソード1', 'エピソード1の説明', 45, '2023-03-01'),
(5, 'ドラマD シーズン1 エピソード2', 'エピソード2の説明', 46, '2023-03-08'),
(6, 'ドラマD シーズン2 エピソード1', 'エピソード1の説明', 47, '2024-01-01'),
(7, 'コメディE シーズン1 エピソード1', 'エピソード1の説明', 28, '2023-05-01'),
(7, 'コメディE シーズン1 エピソード2', 'エピソード2の説明', 30, '2023-05-08'),
(8, 'アニメF シーズン1 エピソード1', 'エピソード1の説明', 24, '2023-06-01'),
(9, 'ニュースG 特別エピソード', '特別ニュースの説明', 60, '2023-07-01'),
(10, 'バラエティH シーズン1 エピソード1', 'エピソード1の説明', 35, '2023-08-01'),
(10, 'バラエティH シーズン1 エピソード2', 'エピソード2の説明', 36, '2023-08-08'),
(11, 'ドキュメンタリーI 特別エピソード', '特別ドキュメンタリーの説明', 50, '2023-09-01'),
(12, 'ドラマJ シーズン1 エピソード1', 'エピソード1の説明', 44, '2023-10-01'),
(12, 'ドラマJ シーズン1 エピソード2', 'エピソード2の説明', 46, '2023-10-08'),
(13, 'ドラマJ シーズン2 エピソード1', 'エピソード1の説明', 48, '2024-02-01');

-- 放送時間
INSERT INTO airtimes (channel_id, start_time, end_time, episode_id, views) VALUES
(1, '2023-07-01 10:00:00', '2023-07-01 10:45:00', 1, 1000),
(1, '2023-07-02 11:00:00', '2023-07-02 11:50:00', 2, 1200),
(1, '2023-07-03 14:00:00', '2023-07-03 14:45:00', 7, 1100),
(2, '2023-07-05 12:00:00', '2023-07-05 12:30:00', 4, 900),
(2, '2023-07-07 12:00:00', '2023-07-07 12:32:00', 5, 950),
(3, '2023-07-10 15:00:00', '2023-07-10 16:00:00', 11, 1600),
(4, '2023-07-12 13:00:00', '2023-07-12 13:28:00', 9, 800),
(4, '2023-07-14 13:00:00', '2023-07-14 13:30:00', 10, 850),
(5, '2023-07-16 16:00:00', '2023-07-16 16:24:00', 12, 700),
(5, '2023-07-18 20:00:00', '2023-07-18 20:35:00', 13, 1100),
(6, '2023-07-20 20:00:00', '2023-07-20 20:36:00', 14, 1150),
(7, '2023-07-22 22:00:00', '2023-07-22 23:00:00', 15, 1200),
(8, '2023-07-25 21:00:00', '2023-07-25 21:44:00', 16, 1400),
(8, '2023-07-28 21:00:00', '2023-07-28 21:46:00', 17, 1450),
-- 再放送データ
(1, '2023-07-08 10:00:00', '2023-07-08 10:45:00', 1, 900),
(1, '2023-07-09 11:00:00', '2023-07-09 11:50:00', 2, 950),
(2, '2023-07-13 12:00:00', '2023-07-13 12:30:00', 4, 850),
(2, '2023-07-15 12:30:00', '2023-07-15 13:00:00', 5, 900),
(4, '2023-07-19 13:00:00', '2023-07-19 13:28:00', 9, 750),
(4, '2023-07-21 13:30:00', '2023-07-21 14:00:00', 10, 800),
(5, '2023-07-23 16:00:00', '2023-07-23 16:24:00', 12, 650),
(6, '2023-07-26 20:00:00', '2023-07-26 20:35:00', 13, 1050),
(6, '2023-07-27 20:35:00', '2023-07-27 21:00:00', 14, 1100),
(7, '2023-07-30 22:00:00', '2023-07-30 23:00:00', 15, 1150);
```