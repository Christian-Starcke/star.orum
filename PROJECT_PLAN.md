# Star.Orum - Project Plan

## MVP Scope (Phase 1)

### Core Functionality
- **Single-step outbound calling** - Manual dialing with click-to-call
- **Basic call recording** - Twilio call recording enabled
- **CRM activity sync** - Post-call activity sync to Twenty CRM with disposition and notes
- **Basic tracking** - Google Sheets for call logs and lead management

### Deliverables
- N8N workflow: Call initiation → Recording → CRM sync
- Google Sheets: Leads sheet + Calls log sheet
- Basic Appsheet interface for dialer
- Twenty CRM integration (activity sync with disposition and notes)

---

## Phases & Milestones

### Phase 1: MVP (Weeks 1-2)
**Goal**: Get basic outbound calling working with CRM sync

**Tasks**:
1. Set up Twilio account and phone number
2. Create Google Sheets data model (Leads, Calls)
3. Build N8N workflow: Webhook → Twilio Call → Record → Sync to CRM
4. Configure Twenty CRM API integration (activity sync with disposition and notes)
5. Basic Appsheet interface for dialer
6. Test end-to-end flow

**Success Criteria**:
- Can initiate outbound call from Appsheet
- Call is recorded in Twilio
- Call activity syncs to Twenty CRM (with disposition and notes)
- Call log appears in Google Sheets

---

### Phase 2: Campaign + Basic Analytics (Weeks 3-4)
**Goal**: Add single pre-determined campaign workflow and basic reporting

**Campaign Rules**:
- **Duration**: 4 weeks total
- **Frequency**: 2 calls per week (8 calls total per lead)
- **Schedule**: No calls on weekends, no calls on holidays
- **Execution**: Manual (like Orum) - when call step is due, user can execute the call

**Tasks**:
1. Create campaign tracking in Sheets
2. Build campaign step scheduler (2 calls/week, 4 weeks)
3. Add weekend/holiday exclusion logic
4. Create "Call Due" indicator in Appsheet
5. Manual call execution workflow
6. Build basic analytics queries in Google Sheets
7. Create simple reporting views in Appsheet
8. Track key metrics (calls made, completion rate, disposition breakdown, campaign progress)

**Success Criteria**:
- Campaign automatically schedules 8 call steps over 4 weeks
- Weekend/holiday exclusions working
- User can see when calls are due
- User can manually execute calls when due
- Basic analytics available in Appsheet
- Key metrics tracked and visible
- Campaign progress visible

---

### Phase 3: AI Call Summaries (Weeks 5-6)
**Goal**: Add AI-generated call summaries

**Tasks**:
1. Set up OpenAI API integration
2. Build call summary generation workflow (using call recording)
3. Store summaries in Google Sheets (Call Notes field)
4. Sync summaries to Twenty CRM as call notes
5. Integrate summary generation into call completion workflow

**Success Criteria**:
- AI-generated call summaries available post-call
- Summaries stored in Google Sheets and synced to CRM
- Summaries appear in Twenty CRM activity notes

---

## Core N8N Workflows

### Workflow 1: Call Initiation
**Trigger**: Appsheet button/webhook
**Flow**:
1. Receive call request (lead ID, phone number)
2. Validate lead status
3. Initiate Twilio call
4. Return call status to Appsheet

### Workflow 2: Call Completion & Sync
**Trigger**: Twilio webhook (call ended)
**Flow**:
1. Receive call completion webhook
2. Fetch call recording from Twilio
3. Store recording URL in Google Sheets
4. Generate AI call summary (Phase 3: OpenAI API)
5. Store summary in Google Sheets (Call_Notes field)
6. Sync call activity to Twenty CRM (disposition + notes/summary)
7. Update lead status in Sheets
8. Trigger cleanup workflow (if needed)

### Workflow 3: Campaign Step Scheduler
**Trigger**: Scheduled (daily check)
**Flow**:
1. Check leads in campaign
2. Calculate next call step based on campaign rules (2 calls/week, 4 weeks)
3. Exclude weekends and holidays
4. Update "Next_Call_Date" in Sheets
5. Mark calls as "Due" when date arrives
6. User manually executes calls when due (via Workflow 1)

### Workflow 4: Data Cleanup
**Trigger**: Scheduled (daily)
**Flow**:
1. Check call records older than 30 days
2. Verify CRM sync status
3. Archive/delete old records from Sheets
4. Log cleanup actions

---

## Data Models

### Google Sheets Structure

#### Sheet 1: Leads
| Column | Type | Description |
|--------|------|-------------|
| Lead_ID | Text | Unique identifier |
| First_Name | Text | |
| Last_Name | Text | |
| Phone | Text | |
| Email | Text | |
| Company | Text | |
| Status | Text | New/Contacted/Qualified/Lost |
| Campaign_ID | Text | Current campaign |
| Last_Call_Date | Date | |
| Next_Call_Date | Date | |
| CRM_Contact_ID | Text | Twenty CRM contact ID |

#### Sheet 2: Calls
| Column | Type | Description |
|--------|------|-------------|
| Call_ID | Text | Unique identifier |
| Lead_ID | Text | Reference to Leads |
| Campaign_Step | Number | Which call step (1-8) |
| Call_Date | DateTime | |
| Duration | Number | Seconds |
| Status | Text | Completed/No Answer/Busy/Failed |
| Disposition | Text | Interested/Not Interested/Callback/etc |
| Call_Notes | Text | AI-generated summary (Phase 3) |
| Recording_URL | Text | Twilio recording URL |
| CRM_Activity_ID | Text | Twenty CRM activity ID |
| Synced_To_CRM | Boolean | |

#### Sheet 3: Campaign (Single Pre-Determined Campaign)
| Column | Type | Description |
|--------|------|-------------|
| Lead_ID | Text | Reference to Leads |
| Campaign_Start_Date | Date | Campaign start date |
| Campaign_End_Date | Date | Campaign end date (4 weeks later) |
| Next_Call_Date | Date | Next scheduled call date |
| Call_Due | Boolean | Is call due for execution |
| Calls_Completed | Number | Number of calls completed (0-8) |
| Campaign_Status | Text | Active/Completed/Paused |

---

## Integration Points

### Twilio
- **Voice API**: Outbound calling
- **Call Recording**: Automatic recording
- **Webhooks**: Call status events

### Twenty CRM API
- **Activities**: Log call activities with disposition and notes
- **Authentication**: API key/token

### Google Sheets API
- **Read/Write**: Lead and call data
- **Appsheet**: Real-time updates
- **Triggers**: On edit events

### OpenAI API
- **Call Summaries**: Generate call summaries from recordings (Phase 3)

---

## Testing Strategy

### Phase 1 Testing
- [ ] Test Twilio call initiation
- [ ] Verify call recording works
- [ ] Test CRM activity sync (disposition + notes)
- [ ] Validate Google Sheets updates
- [ ] Test Appsheet interface

### Phase 2 Testing
- [ ] Test campaign step scheduling (2 calls/week, 4 weeks)
- [ ] Verify weekend/holiday exclusions
- [ ] Test "Call Due" indicator
- [ ] Validate manual call execution
- [ ] Test campaign progress tracking
- [ ] Test basic analytics queries
- [ ] Verify metrics calculation
- [ ] Test reporting views in Appsheet
- [ ] Validate campaign progress visibility

### Phase 3 Testing
- [ ] Test OpenAI API integration
- [ ] Verify call summary generation
- [ ] Test summary storage in Sheets
- [ ] Validate summary sync to CRM notes

---

## Technical Requirements

### N8N Setup
- N8N instance running (self-hosted or cloud)
- Webhook endpoints configured
- Credentials for all services stored
- Error handling workflows

### Twilio Setup
- Twilio account with phone number
- Webhook URLs configured
- Call recording enabled
- API credentials secured

### Google Workspace Setup
- Google Sheets created with structure
- Google Drive for recordings
- Appsheet app configured
- API credentials configured

### Twenty CRM Setup
- Twenty CRM instance running
- API access configured
- Custom fields defined
- Webhook endpoints (if needed)

---

## Risk Mitigation

### Performance Risks
- **Sheets slowdown**: Implemented rolling window cleanup
- **API rate limits**: Add retry logic and queuing
- **Call volume**: Monitor Twilio usage and costs

### Data Risks
- **CRM sync failures**: Retry mechanism + error logging
- **Data loss**: Regular backups of Sheets
- **Sync conflicts**: Last-write-wins with timestamps

### Integration Risks
- **API changes**: Version pinning and monitoring
- **Service outages**: Fallback workflows and alerts
- **Authentication issues**: Token refresh mechanisms

---

## Success Metrics

### Phase 1 MVP
- 100% call initiation success rate
- 100% CRM sync success rate
- <5 second call initiation time
- Zero data loss

### Phase 2 Campaign + Analytics
- Campaign steps scheduled correctly (2 calls/week, 4 weeks)
- Weekend/holiday exclusions working
- Manual call execution working
- Basic analytics functional and accurate
- Key metrics visible in Appsheet

### Phase 3 Call Summaries
- Call summaries generated successfully
- Summaries sync to CRM notes
- Summary quality acceptable

---

## Next Steps

1. **Set up development environment**
   - Configure N8N instance
   - Set up Twilio account
   - Create Google Sheets structure
   - Configure Twenty CRM API access

2. **Build Phase 1 MVP**
   - Start with Workflow 1 (Call Initiation)
   - Test with single call
   - Add Workflow 2 (Call Completion)
   - Integrate CRM sync
   - Build Appsheet interface

3. **Iterate and improve**
   - Gather feedback
   - Optimize workflows
   - Add error handling
   - Document processes
