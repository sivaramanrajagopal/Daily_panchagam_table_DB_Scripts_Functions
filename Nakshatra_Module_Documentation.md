
Nakshatra Characteristics Module – Full Documentation
======================================================

🧩 Overview
-----------
This module allows dynamic retrieval and display of Nakshatra traits, deity, ruler (planet), favorable activities, and weaknesses in both English and Tamil using Supabase as the backend.

Technologies used:
- Supabase (PostgreSQL + RPC + JSONB)
- React + Tailwind frontend
- Next.js dynamic rendering
- Tamil-English bilingual support

------------------------------------------------------

🗃️ Supabase Table Structure
---------------------------
Table: nakshatra_characteristics

| Column                  | Type     | Description |
|-------------------------|----------|-------------|
| english_name            | TEXT     | Nakshatra in English |
| tamil_name              | TEXT     | Nakshatra in Tamil |
| deity                   | TEXT     | Associated deity |
| symbol                  | TEXT     | Nakshatra symbol |
| element                 | TEXT     | Earth, Water, Air, Fire |
| rasi                    | TEXT     | Associated Rasi |
| ruler                   | TEXT     | Planetary ruler (English) |
| ruler_tamil             | TEXT     | Planetary ruler (Tamil) |
| qualities               | TEXT[]   | Strength traits |
| favorable_activities    | TEXT[]   | Good actions to perform |
| unfavorable_qualities   | TEXT[]   | Cautions or weaknesses |
| description             | TEXT     | Optional long-form paragraph |

------------------------------------------------------

🔮 RPC Functions
----------------

1. get_chandrashtama_days
-------------------------
Returns upcoming dates where a user's Nakshatra falls under Chandrashtama.

```sql
CREATE OR REPLACE FUNCTION get_chandrashtama_days(
  user_nakshatra TEXT,
  start_date DATE DEFAULT CURRENT_DATE,
  end_date DATE DEFAULT CURRENT_DATE + INTERVAL '30 days'
)
RETURNS TABLE(date DATE, main_nakshatra TEXT, formatted_nakshatra TEXT)
LANGUAGE sql
AS $$
  SELECT
    date,
    main_nakshatra,
    main_nakshatra AS formatted_nakshatra
  FROM daily_panchangam
  WHERE
    user_nakshatra = ANY (
      SELECT jsonb_array_elements_text(chandrashtama_for)
    )
    AND date BETWEEN start_date AND end_date
  ORDER BY date;
$$;
```

2. calculate_personal_score
---------------------------
Calculates personal daily score using:
- Tarabalam logic
- Cosmic score from daily_panchangam
- Adjustment for Chandrashtama

Refer to canvas version in Supabase UI for full PL/pgSQL logic.

------------------------------------------------------

✅ Validation Checklist
-----------------------
- 27 Nakshatras fully populated with ruler/ruler_tamil
- All major columns available and queryable
- Tamil + English UI tested in browser
- React renders fallback for missing fields
- Supabase RPC returns expected rows

------------------------------------------------------

📁 Files for GitHub Check-in
----------------------------
- sql/nakshatra_characteristics_schema.sql
- sql/rpc_get_chandrashtama_days.sql
- sql/rpc_calculate_personal_score_reference.txt
- components/PersonalNakshatraDashboard.js
- pages/_document.js

------------------------------------------------------

📌 Why this Architecture?
-------------------------
| Feature                    | Reason                              |
|----------------------------|--------------------------------------|
| Supabase RPC               | Fast & efficient query logic         |
| Tamil + English columns    | Multilingual support without branching |
| JSONB + Arrays             | Scalable and clean                  |
| Centralized Nakshatra info| Editable without frontend changes    |

------------------------------------------------------

Prepared by: ChatGPT × Anup
Date: 2025-05-16
