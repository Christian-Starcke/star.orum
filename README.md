# Star.Orum - Power Dialer Platform

## Overview

A custom-built power dialer solution for outbound calling with multi-step campaign functionality, call recording/transcription, and CRM integration. Built using a modern, low-cost tech stack.

## Tech Stack

### Core Platform
- **Cursor** - IDE and development environment
- **N8N** - Workflow automation and orchestration
- **GitHub** - Version control and repository management

### Telephony & Communication
- **Twilio** - Voice API, SMS, call recording, webhooks
- **Twilio Voice Insights** - Real-time call analytics and monitoring

### Data & Storage
- **Google Sheets** - Database (low-cost data storage)
- **Google Drive** - Call recording file storage
- **Google Appsheet** - User interface and mobile access

### AI & Processing
- **OpenAI/Claude APIs** - Call transcription, sentiment analysis, call insights
- **Twilio Media Streams** - Real-time audio streaming for live transcription

### Integration
- **Twenty CRM API** - CRM synchronization and data sync
- **Webhooks** - Real-time event handling (via N8N)

## Core Features

### 1. Power Dialer
- Outbound calling functionality
- Automated dialing workflows
- Call management interface

### 2. Multi-Step Campaigns
- Campaign creation and management
- Sequential call workflows
- Campaign tracking and analytics

### 3. Call Recording & Transcription
- Automatic call recording
- AI-powered transcription
- Call analysis and insights

### 4. CRM Integration
- **Twenty CRM** (Open Source) integration
- Post-call disposition sync
- Automatic data synchronization
- Similar to Orum's Salesforce integration pattern

## Architecture

The platform leverages N8N workflows to orchestrate:
- **Call Management**: Twilio Voice API for outbound calling and call control
- **Campaign Execution**: Multi-step dialing workflows with pacing and sequencing
- **Recording & Transcription**: Twilio call recording → Google Drive → AI transcription (OpenAI/Claude)
- **Data Flow**: Google Sheets as central data store → N8N workflows → Twenty CRM sync
- **Real-time Events**: Twilio webhooks → N8N → Google Sheets/Appsheet updates

## Data Management Strategy

**Rolling Window Approach** - To maintain Google Sheets performance while handling high call volumes:

- **Active Data**: Google Sheets maintains last 30 days of active call data
- **CRM Sync**: All call records sync to Twenty CRM (source of truth) post-call
- **Automated Cleanup**: N8N workflows automatically archive/delete records older than threshold after successful CRM sync
- **Benefits**: Keeps Sheets performant for active work while CRM maintains complete historical record

## Project Goals

Build a cost-effective, scalable power dialer solution that rivals commercial platforms like Orum, but tailored for the open-source Twenty CRM ecosystem.
