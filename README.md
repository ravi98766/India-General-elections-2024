#### End-to-End Election 2024 Result Analysis | Data Modeling → Insights → Storytelling & Power BI Dashboard Project ####

**Data Model – Foundation of the Analysis**

Objective: Build a scalable, clean, and filter-efficient data model to support cross-level election analysis.

**Key Tables Used** 
•	Statewise Results – State-level winners, margins, and alliances

•	Constituencywise Results – Winning candidate, party, total votes, margin

•	Constituencywise Details – Candidate-level vote breakup (EVM, Postal, % share)

•	Partywise Results – Party ↔ Alliance mapping

•	States (Dimension Table) – State normalization

**Data Modeling Highlights**
•	Star-schema–like structure with one-to-many relationships

•	Proper use of Party ID, State ID, and Constituency ID

•	Optimized for cross-filtering between alliances, parties, states, and constituencies

•	Enables drill-down from national → state → constituency → candidate

This robust model ensures high performance and accurate aggregation across all dashboards
<img width="1087" height="575" alt="0 data model" src="https://github.com/user-attachments/assets/9f756231-ea34-4a9d-a568-dfa866c7c556" />

**Landing Page – Navigation & Story Entry Point**

**Objective: Provide a clear, intuitive entry point for business users and stakeholders**

**Key Features**
•	Central navigation hub with 5 dashboards

•	Clean UI with strong election branding

•	Clearly communicates:

o	Voting period

o	Counting date

o	Scope of analysis

**Dashboards Linked**
•	Overview Analysis

•	State Demographic Analysis

•	State Analysis

•	Constituency Analysis

•	Details Grid

Designed for executive users to immediately understand “where to go” and “what they’ll get.”
<img width="1064" height="547" alt="1 landing page" src="https://github.com/user-attachments/assets/15216888-268a-4b2f-a8c3-1136b02341fa" />

**Overview Analysis – National Election Snapshot**

**Objective: Present a macro-level summary of the 2024 election results.**

**Key Insights**

•	Total Seats: 543

•	Alliance Performance

o	NDA: 292 seats (54%)

o	I.N.D.I.A.: 234 seats (43%)

o	Others: 17 seats (3%)

**Advanced Visual Elements**

•	Alliance-wise seat distribution

•	Party-wise seat contribution within each alliance

•	Clear visual storytelling of majority formation

This dashboard answers the most critical question: “Who formed the government and why?”
<img width="1518" height="847" alt="2 Overview Analysis" src="https://github.com/user-attachments/assets/4805f415-18d9-4244-a6ae-58bb08598ce3" />

**State Demographic Analysis – Geographic & Spatial Insights**

**Objective: Analyze how alliances performed across India geographically.**

**Key Visuals**

•	Filled Map:

o	NDA-majority states

o	I.N.D.I.A-majority states

•	Constituency-level scatter map

o	Each dot represents a constituency

o	Color-coded by alliance

o	Size reflects vote margin

**Insights Delivered**

•	Regional strongholds of each alliance

•	State-wise dominance patterns

•	Identification of politically competitive regions

Transforms raw results into geographic election intelligence.
<img width="1069" height="540" alt="3 State demographic analysis" src="https://github.com/user-attachments/assets/05c69ffe-e43b-4ec5-9e85-8f64ff55d43c" />

**State Analysis – Deep Dive into Individual States**

**Objective: Enable state-level performance comparison across alliances and parties.**

Example: Andhra Pradesh
•	Total Seats: 25

•	NDA: 21 seats

•	Others: 4 seats

**Visual Breakdown**

•	Party-wise seat table

•	Alliance-wise donut charts

•	Dynamic state slicer

•	Map-based geographic context

 This dashboard explains “why a state voted the way it did.”
 <img width="1063" height="543" alt="4 State Analysis" src="https://github.com/user-attachments/assets/6045e642-80da-42a0-96f6-358b0ddc561e" />

**Constituency Analysis – Micro-Level Election Outcomes**

**Objective: Provide constituency-level election intelligence.**

Example: Ahmednagar (Maharashtra)

•	Total Votes: 13,25,477

•	Winner: Nilesh Dnyandev Lanke (NCP)

•	Runner-up: BJP candidate

•	Vote Share Analysis

o	Winner: 47.14%

o	Runner-up: 44.95%

•	Candidates Participated: 26

**Key Metrics**

•	Total votes vs EVM vs Postal votes

•	Winning margin clarity

•	Rank-based candidate cards

This is where election results turn into tactical political insights.
<img width="1045" height="544" alt="5 Constituency Analysis" src="https://github.com/user-attachments/assets/566b9f8f-2953-40c2-b8c6-01a464fbdf7a" />

**Details Grid – Complete Election Record**

**Objective: Provide a tabular, audit-ready view of all constituencies.**

**Features**

•	Constituency-wise:

o	Winning candidate

o	Runner-up

o	Party & Alliance

o	EVM votes

o	Postal votes

o	Total votes

•	Sortable & filterable grid

•	Ideal for researchers, journalists, and analysts

Acts as the single source of truth for all election data.
<img width="1070" height="504" alt="6 Grid view" src="https://github.com/user-attachments/assets/61c6d258-56a7-4f3a-8227-025ed2d9f2c5" />


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

```
% of INDIA Seats = DIVIDE([INDIA Seats], [Total Seats], 0)
```
```
% of NDA Seats = DIVIDE([NDA Seats], [Total Seats], 0)
```
```
% of Other  Seats = DIVIDE([OTHER Seats], [Total Seats], 0)
```
```
3rd RunnerUp candidate = 
VAR MaxVotes =
    MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
VAR ThirdMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < SecondMaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN
CALCULATE(
    MAX(constituencywise_details[Candidate]),
    constituencywise_details[Total Votes] = ThirdMaxVotes
)
```
```
•	3rd RunnerUp party = 
VAR MaxVotes =
    MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
VAR ThirdMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < SecondMaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN
CALCULATE(
    MAX(constituencywise_details[Party]),
    constituencywise_details[Total Votes] = ThirdMaxVotes
)
```
```
•	3rd RunnerUp votes = 
"Total Votes: "& 
VAR MaxVotes =
    MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
VAR ThirdMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < SecondMaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN ThirdMaxVotes
```
```
•	3rd RunnerUp votes % = 
"Votes Share: "& 
VAR MaxVotes =
    MAX(constituencywise_details[% of Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[% of Votes] < MaxVotes
        ),
        constituencywise_details[% of Votes]
    )
VAR ThirdMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[% of Votes] < SecondMaxVotes
        ),
        constituencywise_details[% of Votes]
    )
RETURN ThirdMaxVotes & "%"
•	INDIA Seats = 
CALCULATE(
    [Total Seats],
    'partywise_results'[Party Alliance]="I.N.D.I.A."
)
```
```
•	INDIA Seats Count = CALCULATE(count(constituencywise_results[Constituency_Name]),partywise_results[Party Alliance]="I.N.D.I.A.")
```
```
•	NDA Seats = 
CALCULATE(
    [Total Seats],
    'partywise_results'[Party Alliance]="NDA"
)
```
```
•	NDA Seats Count = CALCULATE(count(constituencywise_results[Constituency_Name]),partywise_results[Party Alliance]="NDA")
```
```
•	OTHER Seats = 
CALCULATE(
    [Total Seats],
    'partywise_results'[Party Alliance]="OTHER"
)
```
```
•	Other Seats Count = CALCULATE(count(constituencywise_results[Constituency_Name]),partywise_results[Party Alliance]="OTHER")
```
```
•	RunnerUp candidate = 
VAR MaxVotes =
    MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN
CALCULATE(
    MAX(constituencywise_details[Candidate]),
    constituencywise_details[Total Votes] = SecondMaxVotes
)
```
```
•	RunnerUp party = 
VAR MaxVotes =
    MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN
CALCULATE(
    MAX(constituencywise_details[Party]),
    constituencywise_details[Total Votes] = SecondMaxVotes
)
```
```
•	RunnerUp votes = 
"Total Votes: " & 
VAR MaxVotes =
   MAX(constituencywise_details[Total Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[Total Votes] < MaxVotes
        ),
        constituencywise_details[Total Votes]
    )
RETURN SecondMaxVotes
```
```
•	RunnerUp votes % = 
"Votes Share: " & 
VAR MaxVotes =
    MAX(constituencywise_details[% of Votes])
VAR SecondMaxVotes =
    MAXX(
        FILTER(
            constituencywise_details,
            constituencywise_details[% of Votes] < MaxVotes
        ),
        constituencywise_details[% of Votes]
    )
RETURN SecondMaxVotes & "%"
```
```
•	State Secondary KPI = CALCULATE(SELECTEDVALUE(states[State],"NO STATE"), FILTER(constituencywise_results,constituencywise_results[Constituency_ID]=MAX(constituencywise_details[Constituency ID])))
```
```
•	Total Seats = 
SUM(partywise_results[Won])
```
```
•	Total Votes KPI = "Total votes: " & MAX(constituencywise_details[Total Votes])
```
```
•	Votes Share KPI = "Votes Share: " & MAX(constituencywise_details[% of Votes]) &  " % "
```
```
•	Winning Alliance = 
VAR NDA_Seats =
   CALCULATE (
        COUNTROWS ( constituencywise_results ),
        partywise_results[Party Alliance] = "NDA"
    )
VAR INDIA_Seats =
    CALCULATE (
        COUNTROWS ( constituencywise_results ),
        partywise_results[Party Alliance] = "I.N.D.I.A."
    )
RETURN
IF (NDA_Seats > INDIA_Seats, "NDA", "INDIA")
```
```
•	Party Alliance(results) = 
LOOKUPVALUE (
    partywise_results[Party Alliance],
    partywise_results[Party_ID], constituencywise_results[Party_ID]
)
```
```
•	Party Name(results) = 
LOOKUPVALUE (
    partywise_results[Party],
    partywise_results[Party_ID], constituencywise_results[Party_ID]
)
```
```
•	Party Short Name(results) = 
LOOKUPVALUE (
    partywise_results[Party Short],
    partywise_results[Party_ID], constituencywise_results[Party_ID]
)
```
```
•	selected state = SELECTEDVALUE(statewise_results[State], BLANK()) 
```
```
•	Party Alliance = 
IF(
    partywise_results[Party] = "Bharatiya Janata Party - BJP" ||
    partywise_results[Party] = "Telugu Desam - TDP" ||
    partywise_results[Party] = "Janata Dal  (United) - JD(U)" ||
    partywise_results[Party] = "Shiv Sena - SHS" ||
    partywise_results[Party] = "AJSU Party - AJSUP" ||   partywise_results[Party] = "Apna Dal (Soneylal) - ADAL" ||
    partywise_results[Party] = "Asom Gana Parishad - AGP" ||
    partywise_results[Party] = "Hindustani Awam Morcha (Secular) - HAMS" ||
    partywise_results[Party] = "Janasena Party - JnP" ||
    partywise_results[Party] = "Janata Dal  (Secular) - JD(S)" ||
    partywise_results[Party] = "Lok Janshakti Party(Ram Vilas) - LJPRV" ||
    partywise_results[Party] = "Nationalist Congress Party - NCP" ||
    partywise_results[Party]= "Rashtriya Lok Dal - RLD" ||
    partywise_results[Party] = "Sikkim Krantikari Morcha - SKM",
    "NDA",
    IF(
        partywise_results[Party] = "Indian National Congress - INC" ||
        partywise_results[Party] = "Aam Aadmi Party - AAAP" ||
        partywise_results[Party] = "All India Trinamool Congress - AITC" ||
        partywise_results[Party] = "Bharat Adivasi Party - BHRTADVSIP" ||
        partywise_results[Party]= "Communist Party of India  (Marxist) - CPI(M)" ||
        partywise_results[Party] = "Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)" ||
        partywise_results[Party] = "Communist Party of India - CPI" ||
        partywise_results[Party] = "Dravida Munnetra Kazhagam - DMK" ||
        partywise_results[Party] = "Indian Union Muslim League - IUML" ||
        partywise_results[Party] = "Jammu & Kashmir National Conference - JKN" ||
        partywise_results[Party] = "Jharkhand Mukti Morcha - JMM" ||
        partywise_results[Party] = "Kerala Congress - KEC" ||
        partywise_results[Party] = "Marumalarchi Dravida Munnetra Kazhagam - MDMK" ||
        partywise_results[Party] = "Nationalist Congress Party Sharadchandra Pawar - NCPSP" ||
        partywise_results[Party] = "Rashtriya Janata Dal - RJD" ||
        partywise_results[Party] = "Rashtriya Loktantrik Party - RLTP" ||
        partywise_results[Party] = "Revolutionary Socialist Party - RSP" ||
        partywise_results[Party] = "Samajwadi Party - SP" ||
        partywise_results[Party] = "Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT" ||
        partywise_results[Party] = "Viduthalai Chiruthaigal Katchi - VCK",
        "I.N.D.I.A.",
        "OTHER"
    )
)
```
```
•	Winning Alliance(legend) = 
VAR NDA_Seats =
    CALCULATE (
        COUNTROWS ( constituencywise_results ),
        partywise_results[Party Alliance] = "NDA"
    )
VAR INDIA_Seats =
    CALCULATE (
        COUNTROWS ( constituencywise_results ),
        partywise_results[Party Alliance] = "I.N.D.I.A."
    )
RETURN
IF (NDA_Seats > INDIA_Seats, "NDA", "INDIA")
```















