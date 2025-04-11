# create-autonomous-voice-ai-sales-agent

- Created at: 2025-04-10
- Created by: `🐢 Arun Godwin Patel @ Code Creations`

## Table of contents

- [Setup](#setup)
  - [System](#system)
  - [Installation](#installation)
- [Walkthrough](#walkthrough)
  - [Tech stack](#tech-stack)
  - [Build Instructions](#build-instructions)
    - [1. Setup Twilio](#1-setup-twilio)
    - [2. Build Vapi agent](#2-build-vapi-agent)
    - [3. Setup n8n](#3-setup-n8n)
    - [4. Initiate call](#4-initiate-call)

## Setup

### Services

1. Create a Twilio account [Twilio](https://www.twilio.com/)
2. Create a Vapi account [Vapi](https://vapi.ai/)
3. Create an n8n account or use a locally deployed instance of n8n: [n8n setup](https://n8n.io/)
4. Create a Google Drive account [Google Drive](https://drive.google.com/)

## Walkthrough

This tutorial will be using `n8n` as an orchestration layer to integrate with `Vapi`, which will provide an AI voice agent that uses `Twilio` to make a direct phone call. There will not be any code snippets required to complete this build, but I have included all of the prompts for the AI nodes that you'll need to setup the workflow.

### Tech stack

**Orchestration**

- Automation: `n8n`

**AI**

- Voice agent: `Vapi`

**Communication**

- Phone call: `Twilio`

**CRM**

- Google Drive: `Google Sheet`

### Build Instructions

#### 1. Setup Twilio

In order to build this end to end stack you need a way for the AI agent to make a phone call. We will be using Twilio to do this. You can sign up for a free account and get a phone number that you can use to make calls.

Once you have signed up for a Twilio account, you will need to create a new phone number. This phone number will be used to make the call to the user.

#### 2. Build Vapi agent

`Vapi` is a voice agent building platform that makes use of best in class technology to provide near human like calling experiences. This tutorial will not dive into how to optimally build an AI agent in Vapi, it is focused on integrating Vapi with n8n using a trigger to create an autonomous calling system.

Once you've created your voice agent and you've added your Twilio phone number to Vapi, we can start building our n8n workflow.

#### 3. Setup n8n

The n8n workflow is very simple, using just 2 nodes:

- `Google Sheets trigger`: This trigger will activate when data in our CRM is updated
- `HTTP request`: This node will make a HTTP POST request to the Vapi API to initiate a call

The HTTP POST request requires the following setup:

- _URL_: https://api.vapi.ai/call
- _Authentication_: None (we will pass it through the headers)
- _Headers_:
  - Content-Type: application/json
  - Authorization: Bearer {{ $json.vapi_token }}
- _Body_:

```json
{
  "phoneNumberId": "{{ $json.phone_number_id }}",
  "assistantId": "{{ $json.assistant_id }}",
  "customer": {
    "number": "+{{ $json.area_code }}{{ $json.phone_number }}"
  }
}
```

#### 4. Initiate call

The final step is to initiate the call. This can be done by activating the workflow in n8n and updating a row in the Google Sheet.

This completes the setup of our Telegram AI Agent, now you can test it by speaking to it through Telegram!

## Happy building! 🚀

```bash
🐢 Arun Godwin Patel @ Code Creations
```
