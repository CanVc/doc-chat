# Project Brief - DocChat

## The Problem

Technical teams waste hours searching through scattered documentation. The answer exists somewhere - in a PDF, a Word file, a Markdown page, an internal wiki - but finding it means opening five tabs, skimming three documents, and still not being sure.

Existing AI-powered solutions like Notion AI or Confluence AI are SaaS products. They are expensive, require a subscription, and send your data to third-party servers. For small companies handling sensitive internal documentation, that's a dealbreaker.

## The Solution

DocChat lets a team upload their documentation and chat with it. Ask a question, get a direct answer, see exactly which document it came from.

It runs on the company's own infrastructure. No subscription. One command to deploy, then provider and model setup from the app Settings page.

Document storage and vector indexing stay local by default. If OpenAI is configured, relevant excerpts are sent to OpenAI at ingestion and query time. For fully local usage, use Ollama with local models.

## Who It's For

Small to mid-size companies with internal documentation they want to make searchable - without paying for SaaS or compromising on data privacy. Someone on the team needs to be technical enough to run a Docker container. That's the only requirement.

## Why It Matters

Knowledge trapped in documents is knowledge wasted. DocChat turns a pile of files into something you can have a conversation with.

## Assumptions

- A team member can deploy and maintain Docker/Compose on an internal machine.
- The app is protected at the network level (VPN, private subnet, reverse proxy, firewall), since v1 has no built-in authentication.
- An admin can configure the LLM provider and models from the Settings page.
- Full data locality requires local models (Ollama). If OpenAI is used, relevant document excerpts are sent to OpenAI.