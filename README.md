Below is a concise but comprehensive guide to using the TheraLab Discord bot. The guide covers how to create and manage AI-powered research sessions (“Lab Sessions”), add Agents with different roles, run multi-agent discussions (“Team Meetings”), and retrieve transcripts. Use the examples as a starting point and adapt parameters to fit your needs.

---

## 1. Getting Started

### 1.1 Installation and Permissions
- Invite the bot to your Discord server or channel.
- Ensure the bot has permission to create slash commands (Manage Guild permission).

### 1.2 Command Overview
To see all available commands or get more details:
- **`/help`**: Shows general help for all commands.
- **`/help command:"command_name"`**: Shows specific help for a command (e.g., `"/help command:\"quickstart\""`).

---

## 2. Quickstart Workflow

The fastest way to create a new session, auto-generate multiple agents, and begin a brainstorming session is to use **`/quickstart`**:

**Command:**  
```
/quickstart topic:"Your topic" 
             [agent_count:3] 
             [include_critic:true] 
             [public:false] 
             [live_mode:true] 
             [rounds:3] 
             [speakers_per_round:null]
```
**Key Parameters**  
- `topic` (required): What you want the team to discuss.  
- `agent_count`: Number of Scientist agents to generate. Default is `3`.  
- `include_critic`: Whether to include a Critic agent. Default is `true`.  
- `public`: Make session visible to others. Default is `false`.  
- `live_mode`: Stream responses as they are generated. Default is `true`.  
- `rounds`: Number of conversation rounds. Default is `3`.  
- `speakers_per_round`: Number of agents to speak each round. By default, all agents speak.

**Example:**  
```
/quickstart topic:"How can CRISPR gene editing be used to treat cystic fibrosis?" 
            agent_count:2 
            include_critic:true 
            rounds:2
```
This command will:
1. End any existing active session for you.
2. Create a new session with a Principal Investigator, N Scientists, an optional Critic, and a Tool Agent.
3. Automatically start a team meeting with multiple discussion rounds.

---

## 3. Manual Workflow

If you want more control over session setup (choosing agent roles, etc.), you can create and manage everything step-by-step using the `/lab` group commands.

### 3.1 Create or Reopen a Lab Session

1. **Start a Session**  
   ```
   /lab start title:"Session Title" 
              [description:"Session purpose"] 
              [is_public:false]
   ```
   - Creates a new active session and ends any previous one.
   - `is_public` controls whether others can view the session.

2. **Reopen a Session**  
   ```
   /lab reopen session_id:"1234" [confirm:false]
   ```
   - Brings a previously ended session back to active status.  

3. **List Sessions**  
   ```
   /lab list [limit:10] [include_closed:true]
   ```
   - Shows up to `limit` sessions.  
   - `include_closed` can filter out or include ended sessions.

4. **End a Session**  
   ```
   /lab end [session_id:"ID"] 
   ```
   - If no `session_id` is provided, ends the user’s current active session.  
   - Session transcripts remain accessible unless deleted on the backend.

---

### 3.2 Manage Agents

1. **Create an Agent**  
   ```
   /lab agent_create agent_name:"Dr. Smith" 
                     [expertise:"Quantum Physics"] 
                     [goal:"Improve quantum entanglement research"] 
                     [role:"Scientist"] 
                     [model:"openai"]
   ```
   - Adds a new AI agent to the current session. Roles can be “Principal Investigator”, “Scientist”, or “Critic” (and optionally “Tool Agent”).

2. **Update an Agent**  
   ```
   /lab agent_update agent_name:"Dr. Smith" 
                     [expertise:"Advanced quantum physics"] 
                     [goal:"Expand entanglement approaches"] 
                     [role:"Principal Investigator"] 
                     [model:"anthropic"]
   ```
   - Changes agent properties (expertise, goal, role, or underlying LLM model).

3. **Delete an Agent**  
   ```
   /lab agent_delete agent_name:"Dr. Smith"
   ```
   - Removes the agent from the session.

4. **List Agents**  
   ```
   /lab agent_list
   ```
   - Shows all agents in the current session with their roles, expertise, goals, and models.

---

### 3.3 Conduct a Team Meeting

**Start a Meeting**  
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
- `agenda`: The main question or theme.  
- `rounds`: Number of times each agent speaks.  
- `agent_list`: Comma-separated agent names to include (otherwise all from the session).  
- `live_mode`: Stream messages in real-time.  
- `speakers_per_round`: How many agents speak each round (default is all agents).

**Example:**  
```
/lab team_meeting agenda:"Optimizing CRISPR gene editing" rounds:2 agent_list:"PI,Scientist1,Critic"
```

**End a Meeting**  
```
/lab end_team_meeting [meeting_id:"1234"] 
                      [end_all_parallel:true] 
                      [force_combined_summary:false] 
                      [is_public:false]
```
- Ends the specified or most recent meeting.  
- `end_all_parallel` also ends any parallel runs if multiple were started.

---

### 3.4 View Meeting Transcripts

1. **List Meeting Transcripts**  
   ```
   /lab transcript_list
   ```
   - Shows meetings (in the active session) along with their IDs and agendas.

2. **View a Transcript**  
   ```
   /lab transcript_view meeting_id:"12345"
   ```
   - Displays the conversation by round.  

---

## 4. Admin Commands

**`/admin_sync`**  
```
/admin_sync [global_commands:true] password:"YOUR_ADMIN_PASSWORD"
```
- Used by bot maintainers to clear and re-sync slash commands.
- **Warning**: Requires a valid admin password.

---

## 5. Usage Flow Examples

### 5.1 Minimal Step-by-Step (Manual Flow)
1. **Start a session**:  
   ```
   /lab start title:"CRISPR Gene Editing"
   ```
2. **Create agents**:  
   ```
   /lab agent_create agent_name:"PI" role:"Principal Investigator"
   /lab agent_create agent_name:"Scientist1" expertise:"Molecular Biology"
   /lab agent_create agent_name:"Scientist2" expertise:"Bioethics"
   /lab agent_create agent_name:"Critic" role:"Critic"
   ```
3. **Begin meeting**:
   ```
   /lab team_meeting agenda:"How to minimize off-target CRISPR edits?"
   ```
4. **After discussion, end meeting**:
   ```
   /lab end_team_meeting
   ```
5. **Review transcript**:
   ```
   /lab transcript_list
   /lab transcript_view meeting_id:"XYZ123"
   ```
6. **End the session**:
   ```
   /lab end
   ```

### 5.2 One-Command Flow
```
/quickstart topic:"Optimizing CRISPR for cystic fibrosis" 
            agent_count:2 
            include_critic:true 
            rounds:2
```
- Instantly sets up a Principal Investigator, 2 Scientists, a Critic, and a Tool Agent, then starts a 2-round brainstorming session.

---

## 6. Prompt Suggestions

Below are examples of complex, interdisciplinary research prompts that the bot can handle. These prompts are ideal for multi-agent perspectives:

1. **Biology (CRISPR-Based Gene Therapy)**  
   ```
   Research Question:
   How can CRISPR-based interventions be optimized to precisely correct disease-causing mutations in genes like CFTR (implicated in cystic fibrosis) or DMD (associated with Duchenne muscular dystrophy), while minimizing off-target edits, ensuring reliable delivery mechanisms (e.g., viral vs. non-viral vectors), and addressing the regulatory and ethical considerations of germline versus somatic applications?

   /lab team_meeting agenda:"CRISPR-based gene therapy optimization" rounds:2
   ```
   *(Or simply pass `topic:"CRISPR-based gene therapy optimization"` in a `/quickstart`.)*

2. **Economics/Philosophy (Universal Basic Income)**  
   ```
   Research Question:
   To what extent does implementing a Universal Basic Income (UBI) bolster social welfare without causing harmful inflation, labor force withdrawal, or unsustainable public debt, and how do arguments from moral and political philosophy (e.g., justice as fairness, autonomy, the nature of social contracts) inform the broader viability and desirability of UBI policies across different socio-economic contexts?

   Example Agents:
   Principal Investigator (Dr. Fiona Grant) - Interdisciplinary Economist
   Scientist 1 (Dr. Marcus Tan) - Macroeconomist
   Scientist 2 (Dr. Sandra Lopez) - Labor Economist
   Scientist 3 (Dr. Aisha Patel) - Public Finance Specialist
   Critic (Professor Halima Mensah) - Philosopher / Social Theorist
   ```

Use these prompts to maximize the bot’s multi-agent collaboration capabilities—each agent brings a unique viewpoint to the discussion, enriching the final outcome.

---

## 7. Troubleshooting & Tips

- If a command isn’t recognized or updated, use **`/admin_sync`** (if you have permissions) or re-invite the bot to refresh slash commands.  
- You can run `/help` at any time if you forget available parameters.  
- For extended sessions, consider using `/lab list` and `/lab transcript_list` to keep track of your progress.

---

### Happy Researching!
Use these commands to harness multiple AI perspectives in your Discord workspace. For detailed error messages or deeper debugging, consult your bot’s logs or ask your server admin for assistance.
