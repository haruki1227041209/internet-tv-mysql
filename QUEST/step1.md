## テーブル設計

### テーブル：channels
| カラム名 | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|----------|-------------|------|---------|--------|----------------|
| id       | bigint(20)  |      | PRIMARY |        | YES            |
| name     | varchar(255)|      |         |        |                |

### テーブル：programs
| カラム名     | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|--------------|-------------|------|---------|--------|----------------|
| id           | bigint(20)  |      | PRIMARY |        | YES            |
| title        | varchar(255)|      |         |        |                |
| description  | varchar(255)| YES  | TEXT    |        |                |

### テーブル：genres
| カラム名 | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|----------|-------------|------|---------|--------|----------------|
| id       | bigint(20)  |      | PRIMARY |        | YES            |
| name     | varchar(100)|      |         |        |                |

### テーブル：programs_genres
| カラム名   | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|------------|-------------|------|---------|--------|----------------|
| id         | bigint(20)  |      | PRIMARY |        | YES            |
| program_id | bigint(20)  |      | INDEX   |        |                |
| genre_id   | bigint(20)  |      | INDEX   |        |                |

ユニークキー制約: (program_id, genre_id) に対して設定
外部キー制約:
- program_id に対して programs テーブルの id カラムから設定
- genre_id に対して genres テーブルの id カラムから設定

### テーブル：seasons
| カラム名      | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|---------------|-------------|------|---------|--------|----------------|
| id            | bigint(20)  |      | PRIMARY |        | YES            |
| program_id    | bigint(20)  |      | INDEX   |        |                |
| season_number | int(255)    |      |         |        |                |

外部キー制約: program_id に対して programs テーブルの id カラムから設定

### テーブル：episodes
| カラム名      | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|---------------|-------------|------|---------|--------|----------------|
| id            | bigint(20)  |      | PRIMARY |        | YES            |
| season_id     | bigint(20)  | YES  | INDEX   |        |                |
| title         | varchar(255)|      |         |        |                |
| description   | text        | YES  |         |        |                |
| length        | int(20)     |      |         |        |                |
| release_date  | date        |      |         |        |                |
| views         | int(20)     |      |         |        |                |

外部キー制約: season_id に対して seasons テーブルの id カラムから設定

### テーブル：airtime
| カラム名   | データ型    | NULL | キー     | 初期値 | AUTO INCREMENT |
|------------|-------------|------|---------|--------|----------------|
| id         | bigint(20)  |      | PRIMARY |        | YES            |
| channel_id | bigint(20)  |      | INDEX   |        |                |
| start_time | datetime    |      |         |        |                |
| end_time   | datetime    |      |         |        |                |
| episode_id | bigint(20)  | YES  | INDEX   |        |                |

外部キー制約:
- channel_id に対して channels テーブルの id カラムから設定
- episode_id に対して episodes テーブルの id カラムから設定
