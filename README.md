## 1. Getting Started

- **Invite the Bot**: Add TheraLab to your Discord server with appropriate permissions (particularly “Manage Guild” for slash commands).  
- **Discover Commands**:
  - **`/help`**: General overview of all commands.
  - **`/help command:"command_name"`**: Detailed help for a specific command.

---

## 2. Quickstart Workflow

**`/quickstart`** is the fastest way to spin up everything you need—**creating a lab session, auto-generating agents, and kicking off a discussion** in one go.

```
/quickstart topic:"Your topic" 
             [agent_count:3] 
             [include_critic:true] 
             [public:false] 
             [live_mode:true] 
             [rounds:3] 
             [speakers_per_round:null]
```

- **topic** (required): Main subject/question to discuss.
- **agent_count**: Number of Scientist agents (default `3`).
- **include_critic**: Add a Critic agent (default `true`).
- **public**: Make session public (default `false`).
- **live_mode**: Stream responses in real-time (default `true`).
- **rounds**: Number of discussion rounds (default `3`).
- **speakers_per_round**: Agents speaking each round (default = all agents).

When you run **`/quickstart`**:
1. It **starts a new Lab Session** (a “container” that holds meetings, agents, transcripts).
2. It **creates a set of agents** (Principal Investigator, Scientists, optional Critic, and a Tool Agent).
3. It **immediately begins a multi-round brainstorming meeting** on the specified topic.  
4. **All newly created agents remain stored** within that session and can be accessed later—e.g., for future meetings in the same lab or when the lab is reopened.

**Example**:
```
/quickstart topic:"Optimizing CRISPR for cystic fibrosis" 
            agent_count:2 
            include_critic:true 
            rounds:2
```
This command:
- Ends any active session.
- Starts a new session (container).
- Auto-creates a PI, 2 Scientists, a Critic, and a Tool Agent.
- Begins a 2-round discussion on CRISPR.

---

## 3. Manual Workflow

### 3.1 Create or Reopen a Lab Session

A **Lab Session** is essentially a container that holds your agents, meetings, and transcripts for a particular line of research. You can have multiple labs (containers) for different projects, easily reopen them later, and pick up where you left off.

1. **Start a Session**  
   ```
   /lab start title:"Session Title" 
              [description:"Purpose or context"] 
              [is_public:false]
   ```
   - Creates a new Lab Session container and **ends any existing active session**.  
   - `is_public` (default `false`) makes the entire lab (agents, transcripts, etc.) visible to others.  
   - Once started, you can deploy agents to it, run meetings, then revisit everything in the future.

2. **Reopen a Session**  
   ```
   /lab reopen session_id:"1234" [confirm:false]
   ```
   - Reactivates a previously ended Lab Session, restoring all agents and transcripts so you can resume or start new meetings.

3. **List Sessions**  
   ```
   /lab list [limit:10] [include_closed:true]
   ```
   - Shows up to `limit` sessions.  
   - `include_closed` includes ended sessions in the listing.

4. **End a Session**  
   ```
   /lab end [session_id:"ID"]
   ```
   - Ends your active session if no ID is given.  
   - You can still reopen this session later.

> **Tip**: “Ending” a session does not delete it; you can **reopen** it anytime to continue research with the same agents.

---

### 3.2 Manage Agents

Agents are AI participants (Principal Investigators, Scientists, Critics, or Tools) that exist **inside** a particular lab container.

1. **Create an Agent**  
   ```
   /lab agent_create agent_name:"Dr. Smith" 
                     [expertise:"Quantum Physics"] 
                     [goal:"Entanglement research"] 
                     [role:"Scientist"] 
                     [model:"openai"]
   ```
2. **Update an Agent**  
   ```
   /lab agent_update agent_name:"Dr. Smith"
                     [expertise:"Advanced quantum physics"]
                     [goal:"Expand entanglement approaches"]
                     [role:"Principal Investigator"]
                     [model:"anthropic"]
   ```
3. **Delete an Agent**  
   ```
   /lab agent_delete agent_name:"Dr. Smith"
   ```
4. **List Agents**  
   ```
   /lab agent_list
   ```
   - Shows all agents (and their properties) in your active lab session.

---

### 3.3 Conduct a Team Meeting

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

**Key Parameters**  
- **agenda** (required): Discussion topic.  
- **rounds**: How many times each agent speaks (default `3`).  
- **parallel_meetings** (default `1`): Number of parallel runs of this same agenda—great for generating multiple perspectives.  
- **agent_list**: Comma-separated agents to include. If omitted, all agents in the session are used.  
- **live_mode**: Show streamed messages in real-time (default `true`).  
- **speakers_per_round**: Number of agents speaking each round (default = all).  

**Auto-Generate Parameters** (similar to **`/quickstart`**)
1. **auto_generate** (default `false`):  
   - If `true`, TheraLab will **create agents automatically** for you. This is handy if you haven’t created any agents yet or want a new set for this meeting.  
2. **auto_scientist_count** (default `3`):  
   - Creates that many Scientist agents if `auto_generate:true`.  
3. **auto_include_critic** (default `true`):  
   - Includes a Critic agent if you auto-generate.  
4. **temperature_variation** (default `true` when `parallel_meetings>1`):  
   - Varies generation parameters across parallel runs for more diverse outcomes.

**End a Meeting**  
```
/lab end_team_meeting [meeting_id:"id"] 
                      [end_all_parallel:true] 
                      [force_combined_summary:false] 
                      [is_public:false]
```
- **meeting_id**: Ends a specific meeting or the most recent one.  
- **end_all_parallel**: Ends all parallel runs under the same meeting.  
- **force_combined_summary**: If `true`, generates a combined analysis across parallel runs (great for comparing outcomes).

---

### 3.4 View Meeting Transcripts

1. **List Meeting Transcripts**  
   ```
   /lab transcript_list
   ```
   - Shows all meetings in your current lab.  
   - Parallel runs appear as separate entries but are grouped under the same session.

2. **View a Transcript**  
   ```
   /lab transcript_view meeting_id:"12345"
   ```
   - Displays the meeting's conversation by round.

---

## 4. Admin Commands

**`/admin_sync`**  
```
/admin_sync [global_commands:true] password:"YOUR_ADMIN_PASSWORD"
```
- Clears and re-syncs slash commands. Requires valid admin password.

---

## 5. Usage Flow Examples


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
    

### 5.2 Quickstart in One Shot
```
/quickstart topic:"Optimizing CRISPR for cystic fibrosis" 
            agent_count:2 
            include_critic:true 
            rounds:2
```
- Instantly creates a new session, auto-generates multiple agents, and starts a discussion.

---

## 6. Prompt Suggestions

Below are high-complexity prompts ideally suited for multi-agent brainstorming:

1. **Biology (CRISPR-Based Gene Therapy)**  
   ```
   Research Question:
   How can CRISPR-based interventions be optimized to precisely correct disease-causing mutations in genes like CFTR or DMD, while minimizing off-target edits, ensuring robust delivery mechanisms, and handling ethical concerns of germline vs. somatic applications?
   ```
   - **Try** `/quickstart topic:"CRISPR gene editing" rounds:2`  
   or  
   - **Try** `/lab team_meeting agenda:"Advanced CRISPR" auto_generate:true auto_scientist_count:3`

2. **Economics/Philosophy (Universal Basic Income)**  
   ```
   Research Question:
   To what extent does a Universal Basic Income bolster social welfare without triggering harmful inflation, labor force withdrawal, or unsustainable debt, and how do moral/political philosophies inform UBI’s broader viability?
   ```
   - Example Agents:  
     - Principal Investigator (Dr. Fiona Grant) – Interdisciplinary economist  
     - Scientist 1 (Dr. Marcus Tan) – Macroeconomist  
     - Scientist 2 (Dr. Sandra Lopez) – Labor economist  
     - Scientist 3 (Dr. Aisha Patel) – Public finance  
     - Critic (Prof. Halima Mensah) – Philosopher  

---

## 7. Troubleshooting & Tips

- **Missing Commands?**  
  - Run `/admin_sync password:"ADMIN_PASSWORD"` to refresh slash commands.  
- **Long Transcripts?**  
  - The bot may truncate large messages. Use repeated `/lab transcript_view` or break the discussion into multiple rounds.  
- **Forgot Syntax?**  
  - **`/help`** or **`/help command:"some_command"`** at any time.  

---

### Enjoy Your Research!
A Lab Session is your container to store agents, meetings, and transcripts. Quickstart creates one for you automatically, or you can set it up manually with `/lab start`. Once you’ve built or auto-generated agents, you can revisit them in future by reopening the same lab. Leverage multiple rounds, parallel runs, and critics for a truly multidisciplinary view!
