erDiagram
    users {
        INT id PK
        VARCHAR name
        VARCHAR email UK "ไม่ซ้ำกัน"
        VARCHAR password "ควรเก็บแบบ hashed"
        TIMESTAMP email_verified_at "สามารถเป็นค่าว่างได้"
        VARCHAR remember_token "สามารถเป็นค่าว่างได้"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    boards {
        INT id PK
        INT user_id FK "เจ้าของ"
        VARCHAR name
        TEXT description "สามารถเป็นค่าว่างได้"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    columns {
        INT id PK
        INT board_id FK
        VARCHAR name
        INT order "ลำดับการแสดงผล"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    tasks {
        INT id PK
        INT column_id FK
        VARCHAR title
        TEXT description "สามารถเป็นค่าว่างได้"
        INT order "ลำดับการแสดงผล"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    board_user {
        INT board_id PK, FK
        INT user_id PK, FK
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    task_user {
        INT task_id PK, FK
        INT user_id PK, FK
        %% TIMESTAMP created_at (อาจมีหรือไม่มีก็ได้)
        %% TIMESTAMP updated_at (อาจมีหรือไม่มีก็ได้)
    }

    users ||--o{ boards : "เป็นเจ้าของ"
    boards ||--o{ columns : "มี"
    columns ||--o{ tasks : "มี"

    users }o--o{ board_user : "เป็นสมาชิกของ"
    boards }o--o{ board_user : "มีสมาชิก"

    users }o--o{ task_user : "ได้รับมอบหมาย"
    tasks }o--o{ task_user : "มีผู้รับผิดชอบ"

    %% แสดงความสัมพันธ์ FK แบบชัดเจน (เผื่อให้ดูง่ายขึ้นใน Mermaid)
    boards }|..|| users : "เจ้าของ (user_id)"
    columns }|..|| boards : "อยู่ใน (board_id)"
    tasks }|..|| columns : "อยู่ใน (column_id)"
    board_user }|..|| boards : "อ้างอิง (board_id)"
    board_user }|..|| users : "อ้างอิง (user_id)"
    task_user }|..|| tasks : "อ้างอิง (task_id)"
    task_user }|..|| users : "อ้างอิง (user_id)"
