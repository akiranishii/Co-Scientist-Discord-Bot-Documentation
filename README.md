## 1. Getting Started

- **Invite the Bot**: Add TheraLab to your Discord server with the “Manage Guild” permission for slash commands.  
- **Discover Commands**:  
  - **`/help`** — General overview of all commands.  
  - **`/help command:"command_name"`** — Detailed help for a specific command.

---

## 2. Quickstart Workflow

**`/quickstart`** is the fastest way to create a **lab session** (container), auto-generate agents, and start a meeting:

```
/quickstart topic:"Your topic" 
             [agent_count:3] 
             [include_critic:true] 
             [public:false] 
             [live_mode:true] 
             [rounds:3] 
             [speakers_per_round:null]
```

**Quick Points**  
- **topic** (required): Discussion subject.  
- **agent_count**: Number of Scientist agents to create (default `3`).  
- **include_critic**: Whether to include a Critic agent (default `true`).  
- **public**: Make session publicly viewable (default `false`).  
- **live_mode**: Stream real-time responses (default `true`).  
- **rounds**: Number of conversation rounds (default `3`).  
- **speakers_per_round**: Agents per round (default = all agents).

Once run, it:
1. Ends any current active session (container).  
2. Starts a new session.  
3. Auto-creates a Principal Investigator, Scientists, optional Critic, and a Tool Agent.  
4. Launches a multi-round brainstorming meeting.

**Example**:
```
/quickstart topic:"Optimizing CRISPR for cystic fibrosis" 
            agent_count:2 
            include_critic:true 
            rounds:2
```

---

## 3. Manual Workflow

### 3.1 Lab Sessions (Containers)

- A **Lab Session** is like a “container” that stores **agents**, **meetings**, and **transcripts** together.
- You can reopen a session later to reuse the same agents and transcripts.

#### Start a Lab Session

```
/lab start title:"Session Title" 
           [description:"Purpose or context"] 
           [is_public:false]
```
- Creates a new lab container; ends any existing active session.  
- `is_public` (default `false`) makes it visible to others.

#### Reopen a Session

```
/lab reopen session_id:"1234" [confirm:false]
```
- Resumes a previously closed session (and all its agents/transcripts).

#### List Your Sessions

```
/lab list [limit:10] [include_closed:true]
```
- Shows your active and (optionally) closed sessions.

#### End a Session

```
/lab end [session_id:"ID"]
```
- Ends your current session (if `session_id` isn’t provided).  
- You can still reopen ended sessions later.

---

### 3.2 Manage Agents

Within an **active** lab session, you can create, update, or delete agents. **All parameters** for agent creation are shown below:

1. **Create an Agent**  
   ```
   /lab agent_create agent_name:"NAME" 
                     expertise:"AREA OF EXPERTISE" 
                     goal:"PRIMARY RESEARCH GOAL" 
                     role:"ROLE" 
                     model:"LLM MODEL"
   ```
   | **Parameter** | **Type**    | **Description**                                                            | **Required?** |
   |---------------|-------------|----------------------------------------------------------------------------|--------------:|
   | agent_name    | *String*    | The display name of your agent                                             | Yes           |
   | expertise     | *String*    | Agent’s domain knowledge (e.g., “Molecular Biology”)                        | Yes           |
   | goal          | *String*    | Main goal or purpose (e.g., “Investigate CRISPR off-target effects”)        | Yes           |
   | role          | *String*    | One of: “Principal Investigator,” “Scientist,” “Critic,” or “Tool Agent”    | Yes           |
   | model         | *String*    | The LLM to use: “openai,” “anthropic,” or “mistral”                         | Yes           |

   **Example** *(filling every parameter)*:
   ```
   /lab agent_create agent_name:"Dr. Fiona Grant" 
                     expertise:"Interdisciplinary CRISPR applications" 
                     goal:"Coordinate research directions and integrate findings" 
                     role:"Principal Investigator" 
                     model:"openai"
   ```

2. **Update an Agent**  
   ```
   /lab agent_update agent_name:"Dr. Fiona Grant"
                     [expertise:"Expanded CRISPR knowledge"]
                     [goal:"Lead multi-system integration"]
                     [role:"Scientist"]
                     [model:"anthropic"]
   ```
   - You only include the fields you want to change; unmentioned fields remain the same.

3. **Delete an Agent**  
   ```
   /lab agent_delete agent_name:"Dr. Fiona Grant"
   ```
   - Removes the agent from the current session.

4. **List All Agents**  
   ```
   /lab agent_list
   ```
   - Shows each agent’s name, expertise, goal, role, and model.

---

### 3.3 Team Meetings

```
/lab team_meeting agenda:"Topic or question" 
                   [rounds:3] 
                   [parallel_meetings:1] 
                   [agent_list:"Agent1,Agent2"] 
                   [auto_generate:false] 
                   [auto_scientist_count:3] 
                   [auto_include_critic:true] 
                   [temperature_variation:true] 
                   [live_mode:true] 
                   [speakers_per_round:null]
```

- **agenda**: The meeting’s main focus (required).  
- **rounds**: Number of times each agent speaks (default `3`).  
- **parallel_meetings**: Creates multiple parallel discussions (default `1`).  
- **agent_list**: Comma-separated agent names to include; if omitted, all session agents participate.  
- **live_mode**: Streams messages in real-time (default `true`).  
- **speakers_per_round**: Number of agents speaking each round (default = all).

#### Auto-Generate Parameters

- **auto_generate** (default `false`):  
  - If `true`, the bot automatically creates new agents (PI, Scientists, Critic) for the meeting.  
- **auto_scientist_count** (default `3`):  
  - Number of Scientist agents to auto-generate if `auto_generate:true`.  
- **auto_include_critic** (default `true`):  
  - Includes a Critic agent in auto-generation.  
- **temperature_variation** (default `true` when parallel_meetings>1):  
  - Variation in generation parameters across parallel runs for more diversity.

**Ending a Meeting**  
```
/lab end_team_meeting [meeting_id:"id"]
                      [end_all_parallel:true]
                      [force_combined_summary:false]
                      [is_public:false]
```
- Ends a specific or the most recent meeting.  
- If **parallel_meetings** > 1, set `force_combined_summary:true` to produce a comparative analysis across parallel runs.

---

### 3.4 View Meeting Transcripts

1. **List Meeting Transcripts**  
   ```
   /lab transcript_list
   ```
   - Displays all meetings (including parallel runs) in the active lab session.

2. **View a Transcript**  
   ```
   /lab transcript_view meeting_id:"12345"
   ```
   - Shows messages from that specific meeting, grouped by rounds.

---

## 4. Admin Commands

**`/admin_sync`**  
```
/admin_sync [global_commands:true] password:"YOUR_ADMIN_PASSWORD"
```
- Re-syncs slash commands. Requires an admin password.

---

## 5. Usage Flow Examples

Below is a **complete** example with **all agent parameters** used explicitly.

### 5.1 Manual Lab + Full Agent Creation

1. **Start a new lab session**:
   ```
   /lab start title:"CRISPR Gene Editing Session" 
              description:"Deep dive into off-target effects" 
              is_public:false
   ```
2. **Create a Principal Investigator** (all parameters):
   ```
   /lab agent_create agent_name:"Dr. Fiona Grant" 
                     expertise:"Interdisciplinary CRISPR applications" 
                     goal:"Coordinate research directions and integrate findings" 
                     role:"Principal Investigator" 
                     model:"openai"
   ```
3. **Create 2 Scientists** (all parameters):
   ```
   /lab agent_create agent_name:"Dr. Marcus Tan" 
                     expertise:"Molecular biology of gene editing" 
                     goal:"Investigate off-target CRISPR effects and propose mitigations"
                     role:"Scientist"
                     model:"anthropic"
   ```
   ```
   /lab agent_create agent_name:"Dr. Sandra Lopez"
                     expertise:"Bioinformatics & data analysis"
                     goal:"Analyze large genomic datasets for CRISPR mutational profiles"
                     role:"Scientist"
                     model:"openai"
   ```
4. **Create a Critic** (all parameters):
   ```
   /lab agent_create agent_name:"Dr. Halima Mensah"
                     expertise:"Ethical review and risk assessment"
                     goal:"Challenge assumptions and highlight ethical/regulatory pitfalls"
                     role:"Critic"
                     model:"mistral"
   ```
5. **Begin a team meeting**:
   ```
   /lab team_meeting agenda:"Optimizing CRISPR to reduce off-target mutations" 
                     rounds:3
   ```
6. **End the meeting**:
   ```
   /lab end_team_meeting
   ```
7. **Review transcripts**:
   ```
   /lab transcript_list
   /lab transcript_view meeting_id:"XYZ123"
   ```
8. **End the session** (still reopenable in future):
   ```
   /lab end
   ```

### 5.2 Single-Command Quickstart
```
/quickstart topic:"Universal Basic Income Feasibility" 
            agent_count:3 
            include_critic:true 
            rounds:2
```
- Automatically starts a new session, creates a PI + 3 Scientists + Critic + Tool Agent, and begins a 2-round discussion.

---

## 6. Prompt Suggestions

1. **Biology (CRISPR-Based Gene Therapy)**  
   ```
   Research Question:
   How can we optimize CRISPR to precisely target mutations in CFTR or DMD, minimize off-target edits, ensure robust delivery (viral vs. non-viral), and address germline vs. somatic ethical challenges?
   ```
   - **Try** `/quickstart topic:"CRISPR gene editing" rounds:2`
   - *(or use manual `/lab team_meeting` + auto-generate to create a fresh set of agents.)*

2. **Economics/Philosophy (Universal Basic Income)**  
   ```
   Research Question:
   How does a UBI affect labor force participation, inflation risk, and social welfare? What do moral/political philosophy (justice, autonomy, social contracts) contribute to its desirability?
   ```
   - **Try** `/lab team_meeting agenda:"UBI across socio-economic contexts" auto_generate:true parallel_meetings:2`

---

## 7. Troubleshooting & Tips

- **Commands Missing?** — Use `/admin_sync password:"ADMIN_PASSWORD"` to re-sync.  
- **Long Transcripts?** — The bot may truncate lengthy discussions; use multiple transcript views.  
- **Forgot Syntax?** — Run `/help` or `/help command:"some_command"` anytime.  

---

### Enjoy Your Research

A **Lab Session** is your container for everything—agents, meetings, transcripts. **Quickstart** automatically sets it all up, while the manual commands give you fine-grained control. Once created, your agents are stored and reusable whenever you reopen that lab. Dive into multi-agent brainstorming with parallel runs, critics, and real-time streams!
