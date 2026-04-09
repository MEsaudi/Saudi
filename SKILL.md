---
name: me-thinktanks-weekly-update
description: 중동 싱크탱크 대시보드 주간 자동 업데이트 (매주 월요일 오전 9시)
---

You are updating an existing interactive HTML dashboard that tracks Middle East / Saudi Arabia think tanks, their recent reports, and X (Twitter) feeds. This task runs weekly to keep the dashboard fresh.

## Objective
Research new reports and activities from the 14 tracked institutions over the past 7 days and merge them into the dashboard, then save both copies of the HTML file.

## Files to update (IMPORTANT — both must be updated with identical content)
1. `/sessions/upbeat-practical-turing/mnt/outputs/middle-east-think-tanks-dashboard.html` (main dashboard)
2. `/sessions/upbeat-practical-turing/mnt/outputs/me-thinktanks-deploy/index.html` (deployable copy)

## Tracked institutions (14 total)
| # | Name (KR / EN) | Country | Website | X handle |
|---|---|---|---|---|
| 1 | KFCRIS (킹 파이살 연구·이슬람학 센터) | SA | kfcris.com/en | @kfcris_en |
| 2 | KAPSARC (킹 압둘라 석유연구센터) | SA | kapsarc.org | @KAPSARC |
| 3 | Rasanah (국제이란연구소) | SA | rasanah-iiis.org | @rasanahiiis |
| 4 | Prince Saud Al-Faisal Institute for Diplomatic Studies | SA | — | — |
| 5 | KACND (King Abdulaziz Center for National Dialogue) | SA | — | — |
| 6 | ECSSR (에미리츠 전략연구센터) | AE | ecssr.ae/en | @TheECSSR |
| 7 | AGDA (안와르 가르가시 외교 아카데미) | AE | agda.ac.ae | @AGDAUAE |
| 8 | ME Council (중동국제문제위원회) | QA | mecouncil.org | @ME_Council |
| 9 | AJCS (알자지라 연구센터) | QA | studies.aljazeera.net/en | @ajstudies |
| 10 | Derasat (바레인 전략·국제·에너지연구센터) | BH | derasat.org.bh | @DerasatBH |
| 11 | INSS (이스라엘 국가안보연구소) | IL | inss.org.il | @INSSIsrael |
| 12 | Brookings Doha Center | QA | brookings.edu/centers/center-for-middle-east-policy | — |
| 13 | GRC (Gulf Research Center) | SA | grc.net | @Gulf_Research |
| 14 | IMCTC (Islamic Military Counter Terrorism Coalition) | SA | — | — |

## Steps

### Step 1: Research new reports (last 7 days)
Use WebSearch to find NEW publications/reports from these institutions published in the past week. Priority targets (most active publishers):
- ME Council: Situation Assessment series at mecouncil.org
- INSS: Insights and publications at inss.org.il/publication/
- Rasanah: Monthly "Iran Case File" at rasanah-iiis.org/english/category/publications/monthly-reports/
- KAPSARC: Publications at kapsarc.org/our-offerings/publications/
- AJCS: Analyses at studies.aljazeera.net/en/reports

Search queries to try (use today's date context):
- `"Middle East Council" situation assessment [current month year]`
- `INSS Insight [current month year]`
- `Rasanah Iran Case File [current month year]`
- `KAPSARC publications [current month year]`
- `"Al Jazeera Centre for Studies" reports [current month year]`

### Step 2: Read the current dashboard file
Read `/sessions/upbeat-practical-turing/mnt/outputs/middle-east-think-tanks-dashboard.html`. The report data lives in a JavaScript array called `allReports` (search for `const allReports = [`). Each entry has this format:
```js
{ title: "...", org: "...", orgId: N, country: "SA|AE|QA|BH|IL", cat: ["geo"|"energy"|"culture"], date: "YYYY-MM-DD", dateLabel: "M월 D일", yearLabel: "YYYY", url: "...", tags: ["NEW"|"HOT"|] }
```

The X feed samples live in an array called `xAccounts` (search for `const xAccounts = [`). Each entry has `samplePosts: [...]`.

### Step 3: Update the data
- Add any newly found reports to the top of `allReports`. Mark reports from the last 7 days with `tags: ["NEW"]`. Mark notable/viral reports with `tags: ["NEW","HOT"]`.
- Remove the "NEW" tag from reports older than 7 days (keep "HOT" if applicable).
- Remove reports older than 6 months from today's date to keep the list current.
- Update `samplePosts` in `xAccounts` if you found notable recent activity from a specific account (use the report titles as proxy for posts if actual X content isn't accessible).
- Update the stat card: change `<div class="num">38+</div>` (or whatever current number) to reflect the new total count.
- Update the subtitle date in the header: `중동·사우디 싱크탱크 및 연구기관 인터랙티브 대시보드 | YYYY년 M월 기준` — replace with current month.

### Step 4: Save the file
Use the Edit tool to modify the main file:
`/sessions/upbeat-practical-turing/mnt/outputs/middle-east-think-tanks-dashboard.html`

Then copy it to the deploy folder using Bash:
```
cp /sessions/upbeat-practical-turing/mnt/outputs/middle-east-think-tanks-dashboard.html /sessions/upbeat-practical-turing/mnt/outputs/me-thinktanks-deploy/index.html
```

### Step 5: Report the update
Send a concise summary message (in Korean) listing:
- 이번 주 추가된 보고서 수 (N개)
- 가장 주목할 만한 1-3개 보고서 제목과 기관
- 업데이트된 파일 경로: `computer:///sessions/upbeat-practical-turing/mnt/outputs/me-thinktanks-deploy/index.html`
- 호스팅 서비스에 재배포가 필요하다는 안내 (Netlify/GitHub Pages 등)

## Constraints
- Use Korean for all UI text and summary messages.
- Do NOT remove the section structure, CSS, or existing institutions — only update the data arrays.
- If no new reports are found in a given week, still send a brief status message noting that no updates were made.
- Respect copyright: use short titles and link to original sources rather than copying report content.
- Keep the total report list under ~50 entries to maintain performance.
- The file uses a sorted array: `allReports.sort((a, b) => b.date.localeCompare(a.date))` runs at the bottom, so new entries can be inserted anywhere in the array.

## Success criteria
- Both files (`middle-east-think-tanks-dashboard.html` and `me-thinktanks-deploy/index.html`) are updated with identical content.
- At least one search attempt was made for each of the top 5 active publishers (ME Council, INSS, Rasanah, KAPSARC, AJCS).
- Summary message sent in Korean with report count and notable items.